# 实测坑速查（pitfalls）

> **一句话**：汇总 EU4→EU5 语法对照与实修踩过的坑：作用域、变量、合并结构、本地化 BOM、AI 生态、词条量级校准、审计脚本误报与统计偏误。
> **什么时候看**：写完或审查 mod 出报错、加关键字条、写审计脚本，或想确认某写法是不是坑时先查这里。
> **体量**：263 行 · 约 12 分钟通读

来源：`eu5-mod-review` 的实测记录（《刀锋与王座》2026-07/2026-08 实修，EU5 1.3.x）+ 本知识库翻阅游戏本体时的观察。**EU5 ≠ EU4**，以下都是真实踩过的坑。

## 一、EU4 语法混入（最常见）

| EU4 写法 | EU5 正确写法 |
|---|---|
| `ROOT` / `PREV` | `root` / `prev` |
| event_target / global_event_target | 无此概念；用 `save_scope_as = xxx` + `scope:xxx` |
| 本地化 `KEY:0` | 无 `:0` 后缀 |
| 事件选项 `weight = N` | `ai_chance = { base = N modifier = {...} }` 或 `ai_will_select = { <script math> }` |
| province_event | 无；用 `type = location_event` |
| `change_variable = { name = x value = N }` | `change_variable` 只有 add/subtract/multiply/divide/modulo；设值用 `set_variable` |

## 二、作用域坑（实测报错）

1. **save_scope_as 的具名 scope 不跨嵌套 effect 调用**（报 "Scoped object is not valid"）——effect 内复用须开头防御性重存：`scope:actor = { save_scope_as = xxx }`。
2. **random_subject 的 limit 用 has_variable 定位附庸不可靠**（原版 44 处用例无一配此组合）——用宗主保存的国家引用变量。
3. **every_owned_location 的 every_pop limit 勿再写 owner=root**（报 "Event target link 'owner' returned an invalid object"）——归属已由外层保证。
4. **mercenary scope 不能直接用 unit_location**（报 "Wrong scope: mercenary, expected unit"）——经 ordered_mercenary_sub_unit / every_mercenary_sub_unit 取子单位再取位置。
5. 事件选项里用 immediate 保存的 scope 做 title/desc 本地化是支持的，但**别在 title 里引用选项才有的 scope**。
6. **on_action 的 on_actions 列表引用带 trigger 的子块时，子块 trigger 可能不被评估**（2026-09 实测：landless_passage_pulse 挂在 monthly_country_pulse 的 on_actions，其 trigger 要求"军队型+流亡标记"，结果全部原版军队国（察合台等，无任何标记）都执行了 effect、收到专属事件）。原版同文件子块（on_papal_opinion_added）trigger 正常 → 疑似"跨文件合并块引用的子块"才触发此问题。**对策：关键身份/条件门槛内聚到事件自身 trigger + 调用处 effect 内 if（双保险），不要只依赖 on_action 子块 trigger**。事件自身 trigger 实测可靠（加门槛后不再误弹）。

## 三、变量坑

- 用前必初始化（未初始化刷 "Failed to fetch variable ... due to not being set"）。
- **初始化与使用侧同 scope**（跨 scope 是两份变量）。
- tooltip 显示：`[Root.GetVariable('x').GetValue|V0]`。
- **`exists = var:X` 只宜用于"国家引用变量"**（变量里存 scope 时检查引用有效性）；**数值变量的存在性守卫要用 `has_variable = X`**——trigger 上下文用 `exists = var:<数值变量>` 实测守卫不生效（初始化被跳过 → 后续读取连锁报 "Failed to fetch variable" + "Event target link 'var' returned an unset scope"，error.log 单会话 670 次）。
- 索饷/计数类 tooltip 变量（GetVariable 渲染）在事件 **immediate 里做幂等兜底初始化**（`if NOT has_variable → set 0`），否则老档/无历史时悬停选项报 "Data error in loc string"。

## 四、合并/结构坑

- **levies 特化单位必须放文件顶部**（第一个匹配生效）；INJECT 追加会排在 fallback 后永远轮不到——整体覆盖文件、新条目插 fallback 前。
- **REPLACE 必须逐字段保留原版内容**（如 pop_types 的 soldiers 若只改 max_strength，会丢 has_cap/estate 关联/literacy_impact）。
- **事件不能 REPLACE/INJECT**——复制修改会产生无害 "Duplicated event ID"。
- on_action 同名块是合并语义，直接写同名块追加即可，别加 REPLACE。
- **顺序敏感块**（country_name_construction 等"第一个匹配生效"）同样不能用 INJECT。

## 五、本地化坑

- **无 BOM 的 yml 整文件被忽略/乱码**；编辑工具保存会剥 BOM，记得补回。**DSH 的 `write` 与 `edit` 对 `.txt`／`.yml`／`.json`／`.md` 一律剥 BOM**（2026-09 实测累计 7 次：`my_big_house.txt`、`strengthen.txt`、`metadata.json`、`设计说明.md`、两个法律文件、yml 补丁）——而 mod 的惯例是**全部带 BOM**，**每次编辑后必须查 BOM**（读前 3 字节 EF BB BF）。**这不是"偶尔会忘"，是"每次都会发生"**——不要靠记性，靠脚本复查。
- **注意区分**：技能库自身的 `.md`（`~\.dsh\skills\`）**不带 BOM**（已实测 5 个文件全无）。BOM 要求只针对 mod 目录下的文件。
- yml 键必须在语言头（`l_simp_chinese:`）下且缩进；顶格键会毁文件。
- 事件选项 name 写裸中文 → raw key；必须 `<ns>.<id>.a` 形式。
- 本地化**不需要双镜像**：`main_menu\localization\<lang>\` 与 `in_game\localization\<lang>\` 两处都会被加载，但实测 36 个 mod 里 22 个只放 main_menu、7 个只放 in_game、7 个两者都用（且无同路径镜像文件）——**"两侧键必须一致"是旧文档的误记**。同键冲突以加载顺序后者为准，键尽量不重复定义。

## 六、AI 生态坑（观察者模式 160 年 0 触发教训）

- 带 `is_human = yes` 门槛的事件 AI 永不触发。
- ai_will_do / ai_chance 给 -100 起步 = AI 永不选。
- **灾难防重复**：can_start 的 `NOT{xxx_resolved=yes}` 标记**绝不能在 on_end remove**——否则灾难无限重复、永久奖励反复领取（实测单国 3 个永久修正）。

## 七、数量级/词条坑（实测）

- `num_forts` 等计数词条注意量级单位（游戏内数值 ≠ 直觉值，以原版用法为准）。
- `country_has_estate` **恒真**（不是版本差异）：EU5 为每个国家创建所有阶层对象，该触发器只检查"对象是否存在" = 永远 yes。判"阶层实质存在"用 `"estate_power(estate_type:xxx)" > 0`——详见 `blades-and-thrones-2026-08.md` 第 1 节。
- **`is_subject_type = <mod 自定义类型>` 疑似恒真**（2026-09 实测，与 country_has_estate 同族）：无 overlord、无任何附庸关系的独立原版军队国通过了 `is_subject_type = mercenary_company` 检查（存档实证无 subject 字段、事件自身 trigger 也拦不住）；而原版类型（is_subject_type = colonial_nation 等）工作正常。**推论：对 mod 新增 subject type 的 is_subject_type 门槛可能全部失效**（行动/改革 potential、AI 排除列表等 100+ 处受影响）——规避法：创建时给国家打变量（set_variable = yes），门槛用变量检查。验证技巧："独立国对照法"——把怀疑恒真的触发器放到一个普通国家身上看是否误通过。
- 引擎按名识别 static modifier 是常态：`capital_in_*`（首都地形）、`ruler_/general_/admiral_/explorer_*`（角色属性）、`difficulty_*`/`low_aggression`（难度/AI）、`<estate>_tax_impact`（阶层拼接）、`<side>_progress_cabinet_efficiency`（价值观轴）等**零脚本引用但生效**——"零引用=死代码"审计必须豁免引擎概念名与 INJECT: 指向原版对象的合并文件（目标名只在原版出现）。
- **军队国（landless）碎国**问题：给无领土实体做机制时注意 `num_locations > 0` 类门槛。
- 社会价值轴 progress modifier 语义与直觉相反（正负号）——查原版 societal_values 用法。
- 原版事件也可能是报错源：`flavor_kor.32` 的 trigger `unit.unit_location.owner ?= ROOT` 在将军无部队时刷 "Invalid unit found in scope"（1.3.11）——玩高丽必现，与本 mod 无关，审 error.log 先看 "Script location" 归属。

## 八、本知识库翻阅时的观察（原版状态）

- **天气系统脚本侧留白**：`topography` 的 `weather_*_strength_change_percent` 全为 0（注释保留原设计值：flatland −0.08 / hills −0.4 / mountains −2 / mountain_wasteland −4 / ocean_wasteland +0.01），`on_storm_reached_location` 为空——风暴的实际效果是引擎硬编码，mod 只能生成（start_weather_system）与挂钩子。**注意这三个字段只在 `topography` 里，`vegetation` 没有**（详见 `vanilla\vanilla-hazards-and-environment.md`）。
- **疾病专用修正整片留白**：7 种疾病 × impact/抵抗/增长 共 **35 个修正键**（21 local + 14 national），原版只用了 2 个（`national_malaria_resistance_modifier`），而 `local_disease_resistance` 只有 1 处——而 readme 自己举例"医院可以带 `local_my_disease_impact_modifier = -0.9`"。做医疗/检疫类建筑与革新时可放心使用这批键，不会与原版内容冲突。
- **`defender` 地形骰子加成**：山脉 +2、丘陵/森林/林地/丛林/湿地 +1——防守方在恶劣地形有真实骰子优势，做地形平衡时注意。
- **is_garrison 全局唯一**：只有 1 个陆军类别可以有驻军标记（unit_categories readme）。
- **原版 mod 目录确认**：`Documents\Paradox Interactive\Europa Universalis V\mod\` 用 `.metadata\metadata.json` 注册（name/id/version/supported_game_version/short_description/tags），`supported_game_version` 如 "1.3.11"。

## 九、死代码审计与存档分析要点（2026-09 实测补充）

**死代码审计豁免清单**（"定义零引用" ≠ 死，以下类别豁免）：
- auto_modifier / disaster / regency / subject_type / societal_value：引擎按 potential_trigger/can_start 等驱动，定义即生效；
- static modifier 引擎概念名（见第七节）与 `<estate>_`/`<side>_` 拼接约定；
- `INJECT:/REPLACE:` 指向**原版对象**的合并文件（目标名只在原版出现——在 mod 内部找不到引用是正常的）；
- 纯玩家手动授予的对象（privilege 等，有 loc 即正常）；generic_action 的引用在 .gui；
- `*_display` 变量：被 .yml 的 `GetVariable('...')` 读取（脚本侧扫描要覆盖 .yml）。

**存档分析要点**（明文 .eu5，数百 MB）：
- 变量存 `flag=<名> data={ type=... identity=... }`；变量名可全文 IndexOf；
- **附庸关系不以 `subject_type=<文本>` 存储**——别用文本搜判断附庸/独立（overlord 也非文本字段）；
- 存档 metadata **不含启用 mod 列表**——验证"游戏跑的是哪个版本"用 error.log 的 Script location 行号对照（本地删过行 vs 旧版行号差）；
- 380MB 档禁止正则整档扫描（超时）；用 IndexOf 循环 + 命中点前 ~30 万字符回溯 `country_name="TAG"` 定位国家；
- 判断"国家是否被某机制转化过"看机制副作用（ai_personality 变化、country_type 变化）与变量残留——注意部分转化路径会 remove 标记，残留为 0 不能反证没转化过，要看不可逆副作用。

## 十、修正词条与数值校准（2026-09 实测）

### 凭印象必写错的词条

| 词条 | 陷阱 |
|---|---|
| `marriage_desirability` | **`category = character`** —— 不能用在 `country_modifier` 里（做"联姻"类政策时最容易踩） |
| `subject_not_obligated_to_join_war` | **`boolean = yes`** —— 不能给数值 |
| `casus_belli_creation_speed` | 本体**几乎无用例**（仅一个可疑的 `4.0`），量级无法校准 → 避开 |
| `global_manpower` | **不存在**；国级是 `global_manpower_modifier`（`00_modifier_types.txt:4069`），地方级才是 `local_manpower` |
| `global_army_tradition` | **不存在**；国级陆军传统是 `monthly_army_tradition` |
| `stability_cost_modifier`／`global_unrest`／`global_trade_power`／`max_absolutism`／`global_monthly_devotion` | 均**不存在** |

### 符号语义反直觉（填数值最容易写反）

`antagonism_received_modifier` 与 `diplomatic_spending_cost` 都是 **`color=bad`** —— **正值是坏事、负值是好事**。

### 量级陷阱：`local_*` 与 `global_*` 不同档

曾拿 `town_rights` 的 `local_monthly_literacy = 0.05` 去校准国级 `global_monthly_literacy`，写出的值比本体众数高 5 倍。**国家级的量级必须用国家级用法校准**，`local_*` 的档位不能外推到 `global_*`。

### 数值校准法（众数法）

从 `in_game\common\` 抽取该修正的**取值分布** → 取**众数** → 吸附到本体**实际出现过的精确值**（不要外推、不要取整到好看的数字）。实测锚点：

| 修正 | 众数／依据 |
|---|---|
| `legislative_efficiency` | 0.1 |
| `stability_decay` | −0.00025（19×），最低只到 −0.005 |
| `global_monthly_literacy` | 0.01（28×） |
| `global_population_growth` | 0.0001（42×） |
| `monthly_legitimacy` | 0.05 |
| `global_<estate>_estate_power` | ±0.1 |
| `embrace_institution_cost_modifier` | −0.10 |
| `subject_income_modifier` | 0.025（8×） |
| `antagonism_received_modifier` | −0.1（21×）；**正值全本体仅 +0.1 一例** |
| `monthly_prestige` | 0.1（89×） |
| `global_distance_from_capital_speed_propagation` | 0.1（44×），1637 档的 `unitary_administration` 用 0.05 |
| `global_manpower_modifier` | 众数 0.1，但 0.15 有 10 处用例 |
| `global_levy_recruitment_speed_modifier` | **本体最低档就是 0.1**（写 0.05 即超限） |

### 脚本值实测（`main_menu\common\script_values\default_values.txt`）

| 脚本值 | 值 |
|---|---|
| `societal_value_monthly_move` | 0.1（本体最常用漂移档） |
| `societal_value_minor_monthly_move` | 更小档（本体用于 minor 漂移） |
| `small_permanent_target_satisfaction` | 0.025 |
| `medium_permanent_target_satisfaction_penalty` | −0.05 |
| `diplomatic_reputation_mild_bonus` | 2 |

### 存在性核对的正确范围（附带杀软坑）

修正定义在 `main_menu\common\modifier_type_definitions\`（本机 **3 个文件、2437 条**）。核对时**只读这 3 个文件**——**不要 `Get-ChildItem -Recurse` 扫整个 Steam 游戏目录**，实测被 Windows 杀软拦下两次（脚本在 Steam 库目录递归读 30+ 文件 + 大量正则，触发文件扫描启发式）。同理，数原版数据时用**只读指定文件**的方式，别递归。

## 十一、审查脚本自身的误报（实测 7 次，全部是脚本 bug 不是 mod bug）

写自动审计脚本时，以下每一条都真的产生过假失败：

1. **比对本地化语言头要带尾冒号**——`^l_english$` 漏掉 `:` 会报 22 个假失败。正确：`^l_english:`。
2. **正则要先剥注释行再匹配**——否则会匹配到脚本自己的注释文本（注释里写了示例键名），报出"键缺失"。
3. **正则要显式加 `(?m)`**（PowerShell 默认不是多行模式），否则 `^` 只匹配整个字符串开头，命中数恒为 0 或 1。
4. **检查某字段"是否存在"时别搜原始文本**——会命中文件自己的注释（注释里写了 `potential = { ... }` 的示例）。先过滤 `$_.TrimStart() -notmatch '^#'`。
5. **PowerShell 里 `$k:` / `$id:` / `$l:` 会被解析成作用域变量** → 写 `${k}:` / `${id}` / `${l}`。
6. **`Select-String` 管道里 Match 对象没有 `.Path` 属性**（`$_.Matches | ForEach-Object { Split-Path $_.Path }` 静默输出空）→ 改用 `grep` 工具。
7. **数块时要按括号深度或 tab 层级**——只数 `name = {` 会把嵌套块（`country_modifier`、`estate_preferences`、`potential`、`OR`）一起算进去，政策数虚高约 4 倍。

**反面教训**：把这些脚本的结论当成"审计通过"之前，先拿一个**已知正确答案**的样本自测（例如故意写错一个键，看脚本报不报）。假阴性比假阳性危险得多。

## 十二、机制性质的读法（2026-09 补正：两次误判的同源病根）

**病根：从「多数样本」归纳「机制定义」。** 两次实测误判都出在这里——

### ① 性质由脚本决定，勿从多数样本归纳：灾难 ≠ 惩罚机制

原版 36 个灾难里多数 `modifier` 全负（汉化灾难五项月度资源 −0.25、宫廷与国家、王权斗争、自由渴望、帝国衰落），于是容易被归纳成"**灾难 = 国家衰退的结算**"——**错**。反例 `D008_fate_of_the_phoenix`（凤凰之命）：

- `modifier` 三键**两正一负**（`global_bureaucracy_maintenance_efficiency = 0.2`、`casus_belli_creation_speed_modifier = 0.2`、`hire_mercenary_premium_cost_modifier = 0.5`）
- 自带 **12 个专属行动**（`generic_actions\D008_fate_of_the_phoenix_actions.txt`），全部以 `disaster_type = disaster_type:fate_of_the_phoenix` + `disaster_is_active = yes` 为前置——**只有该灾难进行中才能用**
- `content_priority = 700`；`on_end` 按结果分支（领土 >145 且奥斯曼崛起未发生 → 复兴成功）
- 开局强制触发（`tag = BYZ` + `current_year < 1338`）

**它的真实身份是"内容包 / 改革菜单"，只是借了灾难的骨架。**

**机制同构性**（这才是定义）：灾难与局势结构完全一致（持续状态 + 修正 + 月钩子 + 事件链 + 结束判定），差别只在：

| 差异点 | 灾难 | 局势 |
|---|---|---|
| 作用范围 | 单国（root = country） | 跨国家/地区（root = situation） |
| 槽位 | `has_any_active_disaster = no` —— 每国 1 个 | 可并存 |

→ **灾难 = 作用域收缩到单国、槽位唯一的局势（"个人局势"）**，承载什么由脚本定。

**推论（可用于设计）**：`has_any_active_disaster = no` 的互斥锁 = **每国一份的稀缺槽位**，可用一个温和灾难占位，替目标国挡掉更恶性的灾难（**槽位护盾**）。

**读法**：判断某机制的性质前，先抽看它与邻近机制的 `modifier` / `on_*` **实际内容**；横向对比同类样本只用来找"常见形态"，不用来下定义。

### ② 穷举调用点再下"唯一/常规"结论：政策变更有八条途径

曾据 UI 印象写成"**议会是政策变更的唯一常规通道**"——**错**。改动永远是**政策（policy）**，途径至少八条：

| # | 途径 | 代价 | 出处 |
|---|---|---|---|
| ① | 议会请求 `ask_for_law_changes` | 议会支持度（**无损**） | `generic_actions\parliament.txt:592` |
| ② | 议会议程 `pa_change_policy` | 稳定度惩罚 | `parliament_agendas\00_common.txt:57` |
| ③ | 议会诉求 | 诉求自身条件 | `parliament_issues\01_country_specific...:516` |
| ④ | **叛军让步** `grant_benefits_to_estate` | 给阶层特权 + 满意度 + 政策 | `scripted_effects\rebel_negotiate_effects.txt:12`（7 个诉求调用） |
| ⑤ | 平时 UI 直接切换 | **100 稳定度 + 10 正义** | `prices\00_hardcoded.txt:90-97` |
| ⑥ | 事件 | 事件选项代价 | DHE：`flavor_TUR` 24 次、`flavor_ENG` 24 次、`flavor_FRA` 19 次… |
| ⑦ | 通用行动（虔诚改教法学派） | 行动价格 | `generic_actions\piety.txt:176-204`（9 次） |
| ⑧ | IO 政策投票 | `requires_vote` | `laws\readme.txt` |

议会只是**最常用的「无损」切换方式**（不花稳定度/金币，代价是先替阶层办议程）。

**读法**：`grep` 该效果在**全 `common` + `events`** 的调用点分布（如 `-Pattern "add_policy = "`），按调用点归纳途径，不要凭 UI 印象或单个文件就下"唯一/常规"结论。

**连带教训——术语混用会把途径一起搞错**：`law`（法律，**只能解锁/锁定，不存在"被修改"**）与 `policy`（政策，**可切换**）是两个概念，官方 readme 首行即写"A law is a container for one or more policies"。中文玩家口中的"改法律"实际永远是在改政策——写文档时跟着混用，就会顺手把"变更途径"也归错对象。

## 十三、统计本体数据时的工具偏误（2026-09：缩进锚点把必填字段判成可选 / 键名撞车把噪音当取值）

与 §十二 是**同一类病根的不同形态**：§十二 是"从**样本内容**归纳机制定义"，本条是"用**有偏的采样工具**归纳字段结构"。结论都是错的，且都错得很像真的。

**踩法与假结论**：用 `^\t` / `^name` 这类**缩进锚点**统计 `in_game\common\` 各目录的字段出现率，得出：

| 字段 | 缩进锚点（**错**） | 结论会写成 | 花括号深度解析（**对**） |
|---|---|---|---|
| `cultures` 的 `language` | 1882 / 2083 | "语言可选，约 90%" | **2087 / 2087 = 100%（必填）** |
| `cultures` 的 `culture_groups` | 2042 | "文化组覆盖 98%" | 2040 / 2087（**缺 47**） |
| `religions` 的 `group` | 243 / 293 | "50 个宗教没有组" | **293 / 293 = 100%** |
| `religions` 的 `definition_modifier` | 243 | "九成宗教用它" | **293 / 293 = 100%** |
| 文化总数 | 2083 | "2083 个文化" | **2087** |

**成因**：同一目录内**缩进风格不统一**。`cultures\argentinian.txt` 的 `cunco_culture`、`east_asia.txt` 的 `she_culture` 等 4 个文化写成了"**一个空格 + 名字 = {**"，顶格正则与 tab 正则都抓不到；反过来，带前导空格的**嵌套块**又会被 `^\s+` 正则误计入顶层。语言的 `dialects = { ... }` 子块、修正块 `modifier = { ... }` 都属后者。

**正确做法（二选一，都要用 `[ \t]*` 而非写死缩进）**：

1. **花括号深度扫描**：顺序扫描文本，命中 `(?m)^[ \t]*([A-Za-z_0-9]+)[ \t]*=[ \t]*\{` 后**整块跳到配对的 `}`**（深度归零处），继续从块后扫描——这样嵌套块天然被跳过，只统计顶层定义；块体内的字段即为该定义第 1 层字段。
2. 只做单字段核对时，用 `(?m)^[ \t]*key[ \t]*=`。

**同源变体：名字与 `=` 一行、左花括号另起一行**（2026-09 统计 `child_educations` 时再踩一次）。`common\child_educations\` 的真实写法是

```txt
balanced_education =
{                       # ← 名字和 = 在上一行，左花括号独占一行
	allow = { }
	modifier = { character_child_education = 0.5 }
}
```

**两种翻车方式**：

1. 用 `key = {` 单行正则去数 → 6 个教育被数成 **16** 个（块体内部的嵌套块被提到了顶层；早期一次更粗的统计甚至数成 0）。
2. 手写解析器只认「整行只是一个标识符」的变体（正则 `^([a-z_0-9]+)\s*$`）→ 因为行尾还挂着一个 `=`，同样一个都抓不到，返回 **0**。**必须把尾部 `=` 也允许掉**：`^([A-Za-z_0-9\.\-]+)\s*=?\s*$`，再往后看第一个非空行是不是 `{`。

全库 `common\` 里只有三处用这种写法（`child_educations` 6 处、`resolutions` 3 处、`biases` 1 处），所以它不是主流风格，但**足以让计数翻倍或归零**。判定办法同上：**总数对不上就是方法错了**（`child_educations` 目录只有 2 个数据文件、本体 readme 与命名都指向 6 个，数出 16 或 0 都该停下来）。

**先验证再下结论**：宣称"某字段必填/可选"之前，先跑一遍**全量统计**并核对总数（总数对不对是方法对不对的第一信号——文化总数 2083 vs 2087 就是警报）。

**业务结论（已核对）**：`cultures` 的 `language` **必填**（2087/2087；它同时供给名字库、通用/宫廷/市场/礼仪语言、同化加成），`culture_groups` **可选**（47 个孤立民族留空，如阿伊努/琉球/楚科奇/萨米/科普特/阿尔巴尼亚）；`religions` 的 `group`/`color`/`definition_modifier` 三个字段原版 100% 出现。

**同源变体：键名撞车——取值必须白名单过滤**（2026-09 统计事件字段时再踩一次）。

`in_game\events\` 里的 `type =` 并非都是事件类型：事件体内部到处是**同名异义**的键——`add_estate_privilege = { type = estate_type }`、`work_of_art_type`、`casus_belli`、各社会价值轴名……按 `^\s*type\s*=` 直接统计会得到 `estate_type` 643 / `work_of_art_type` 368 / `casus_belli` 360 这类**根本不存在的事件类型**。同理 `category =` 会数出 `estate 77` / `pretender 53` / `nationalist 29`（别处的同名键），而事件的合法取值只有 `disaster_event` / `situation_event` / `international_organization_event`。

**判据**：

1. 字段计数必须**限定花括号深度**（见上），或把正则锚定到**取值白名单**：`(?m)^\s*type\s*=\s*(country_event|location_event|unit_event|exploration_event|age_event)\s*$`。
2. **总数对账**是唯一可靠的哨兵：349 档共 7,470 个事件，逐类型相加必须等于 7,470（原版 country 7,413 + exploration 36 + location 20 + age 1 + unit 0）。
3. 区分"**0 = 原版从不用**"与"**0 = 正则写错**"：只有在取值范围被白名单钉死、且总量能对上时，`interface_lock` 0 处 / `hidden` 7 处才是真结论（已核对）。

