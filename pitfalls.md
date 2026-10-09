# 实测坑速查（pitfalls）

> **一句话**：汇总 EU4→EU5 语法对照与实修踩过的坑：作用域、变量、合并结构、本地化 BOM、AI 生态、词条量级校准、审计脚本误报、统计偏误、PowerShell 静默故障，以及 1.4 关闭/移除的接口与字段。
> **什么时候看**：写完或审查 mod 出报错、加关键字条、写审计脚本、跨版本移植，或想确认某写法是不是坑时先查这里。
> **体量**：477 行 · 约 23 分钟通读
## 目录

- [一、EU4 语法混入（最常见）](#一eu4-语法混入最常见)
- [二、作用域坑（实测报错）](#二作用域坑实测报错)
- [三、变量坑](#三变量坑)
- [四、合并/结构坑](#四合并结构坑)
- [五、本地化坑](#五本地化坑)
- [六、AI 生态坑（观察者模式 160 年 0 触发教训）](#六ai-生态坑观察者模式-160-年-0-触发教训)
- [七、数量级/词条坑（实测）](#七数量级词条坑实测)
- [八、本知识库翻阅时的观察（原版状态）](#八本知识库翻阅时的观察原版状态)
- [九、死代码审计与存档分析要点（2026-09 实测补充）](#九死代码审计与存档分析要点2026-09-实测补充)
- [十、修正词条与数值校准（2026-09 实测）](#十修正词条与数值校准2026-09-实测)
  - [凭印象必写错的词条](#凭印象必写错的词条)
  - [符号语义反直觉（填数值最容易写反）](#符号语义反直觉填数值最容易写反)
  - [量级陷阱：`local_*` 与 `global_*` 不同档](#量级陷阱local_-与-global_-不同档)
  - [数值校准法（众数法）](#数值校准法众数法)
  - [⚠️⚠️ 出处的**种类不止一种**——"本体没有这个值"不能作为改值的唯一理由](#️️-出处的种类不止一种本体没有这个值不能作为改值的唯一理由)
  - [⚠️⚠️ 众数法不够：本体多数修正**不给裸数字**，给具名脚本值](#️️-众数法不够本体多数修正不给裸数字给具名脚本值)
  - [脚本值实测（`main_menu\common\script_values\default_values.txt`）](#脚本值实测main_menucommonscript_valuesdefault_valuestxt)
  - [存在性核对的正确范围（附带杀软坑）](#存在性核对的正确范围附带杀软坑)
  - [⚠️⚠️ 改既有值之前，先读**这个文件自己的审计注释**](#️️-改既有值之前先读这个文件自己的审计注释)
- [十一、审查脚本自身的误报（实测 7 次，全部是脚本 bug 不是 mod bug）](#十一审查脚本自身的误报实测-7-次全部是脚本-bug-不是-mod-bug)
- [十二、机制性质的读法（2026-09 补正：两次误判的同源病根）](#十二机制性质的读法2026-09-补正两次误判的同源病根)
  - [① 性质由脚本决定，勿从多数样本归纳：灾难 ≠ 惩罚机制](#-性质由脚本决定勿从多数样本归纳灾难--惩罚机制)
  - [② 穷举调用点再下"唯一/常规"结论：政策变更有八条途径](#-穷举调用点再下唯一常规结论政策变更有八条途径)
- [十三、统计本体数据时的工具偏误（2026-09：缩进锚点把必填字段判成可选 / 键名撞车把噪音当取值 / 搜索范围缺一半把"没扫"当成"没有"）](#十三统计本体数据时的工具偏误2026-09缩进锚点把必填字段判成可选--键名撞车把噪音当取值--搜索范围缺一半把没扫当成没有)
- [十四、PowerShell 静默故障（2026-10 实测 7 次：不是误报，是**恒为 0 / 丢数据 / 毁文件**）](#十四powershell-静默故障2026-10-实测-7-次不是误报是恒为-0--丢数据--毁文件)
  - [病根：大集合 + 管道/字典索引，在这个环境下会静默失效](#病根大集合--管道字典索引在这个环境下会静默失效)
  - [另一类：正则/语法的静默偏差](#另一类正则语法的静默偏差)
  - [最危险的一类：变量撞车导致破坏性写入（2026-10 实测，11 个文件被毁）](#最危险的一类变量撞车导致破坏性写入2026-10-实测-11-个文件被毁)
  - [强制自查规则（写进任何统计脚本）](#强制自查规则写进任何统计脚本)
- [十五、关闭/移除的接口与字段（2026-10 实查 1.4）](#十五关闭移除的接口与字段2026-10-实查-14)
- [十六、校验器自身失真（2026-10 两次实测：假阴性比假阳性更危险）](#十六校验器自身失真2026-10-两次实测假阴性比假阳性更危险)

> **⚠️ 最危险的是第十四节与第十六节**：十四是脚本静默返回 0 / 丢数据，把"漏了一大片"伪装成"全部覆盖"；十六是**连"用来发现漏没漏的那个校验"本身都在说谎**——它报"通过"，你就不再有第二次怀疑。

来源：`eu5-mod-review` 的实测记录（《刀锋与王座》2026-07/2026-08 实修，EU5 1.3.x；2026-10 于 1.4.0 测试版复核并新增第十五节）+ 本知识库翻阅游戏本体时的观察 + **工具与校验环节的自身故障记录（2026-10 新增第十六节）**。**EU5 ≠ EU4**，以下都是真实踩过的坑。

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

- **⚠️ 无 BOM 的 UTF-8 文件绝不能用 PowerShell 5.1 读写**：PS 5.1 对无 BOM 文件按 **ANSI(936/GBK)** 解码，乱码会被 `Set-Content` 永久烙进文件（`合同` → `鍚堝悓`）。**动手前查 `[System.Text.Encoding]::Default.WebName`**：`utf-8` = PS7 安全，`gb2312` = PS5.1 危险。永远用 `[System.IO.File]::ReadAllText/WriteAllText` + 显式 `UTF8Encoding`，不要用 `Set-Content`/`Out-File`。
- **注释已经乱码了怎么办**：分两类——**A 类（UTF-8 被当 GBK）可 GBK 回环 100% 还原**（实测 124/124 行），**B 类（含 PUA 码点 U+E000–U+F8FF）不可逆**、只能按语义重写。判定流程、抢救代码与方法论铁律见 `cases\encoding-corruption-2026-10.md`。
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
- **`is_subject_type = <mod 自定义类型>` 疑似恒真**（2026-09 实测，与 country_has_estate 同族）：无 overlord、无任何附庸关系的独立原版军队国通过了 `is_subject_type = mercenary_company` 检查（存档实证无 subject 字段、事件自身 trigger 也拦不住）；而原版类型（is_subject_type = colonial_nation 等）工作正常。**推论：对 mod 新增 subject type 的 is_subject_type 门槛可能全部失效**（行动/改革 potential、AI 排除列表等 100+ 处受影响）——规避法：创建时给国家打变量（set_variable = yes），门槛用变量检查。验证技巧："独立国对照法"——把怀疑恒真的触发器放到一个普通国家身上看是否误通过。**2026-10 实害案例**：《刀锋与王座》防 AI 军队国碎国的"双保险"（AI 列表 potential 的 `NOT = { is_subject_type ... }` + ai_will_do 的 OR 臂）被恒真翻转 → 列表恒假 + 全体独立国 -1000，**AI 两个多月从未执行过三个核心行动且毫无报错**——恒真类守卫务必用"独立国对照法"审计，NOT 包裹时危害翻倍。
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
| `stability_cost_modifier`／`global_unrest`／`global_trade_power`／`max_absolutism`／`global_monthly_devotion` | 均**不存在**。⚠️ **但别把这一行读成"unrest 类词条都没有"**——`local_unrest`（「本地叛乱度」，`category = location`）**确实存在**，本体众数 `−0.05`（14×），实有小值还有 `−0.025`(3×) / `−0.02`(2×)。不存在的只有 `global_unrest`。（2026-09 实测补正） |

### 符号语义反直觉（填数值最容易写反）

`antagonism_received_modifier` 与 `diplomatic_spending_cost` 都是 **`color=bad`** —— **正值是坏事、负值是好事**。

### 量级陷阱：`local_*` 与 `global_*` 不同档

曾拿 `town_rights` 的 `local_monthly_literacy = 0.05` 去校准国级 `global_monthly_literacy`，写出的值比本体众数高 5 倍。**国家级的量级必须用国家级用法校准**，`local_*` 的档位不能外推到 `global_*`。

### 数值校准法（众数法）

抽取该修正的**取值分布** → 取**众数** → 吸附到本体**实际出现过的精确值**（不要外推、不要取整到好看的数字）。

> ⚠️⚠️ **扫描范围必须两边都扫，只扫一半会得到"本体 0 处"这种假阴性**（2026-09 实测，我据此给用户提了错误的"换键"建议）：
>
> ```
> ✅ in_game\common\   ← building_types / laws / advances / government_reforms /
>                        estate_privileges / town_rights / auto_modifiers / generic_actions /
>                        situations / disasters / scripted_effects / religion / traits /
>                        international_organizations / parliament_types …
> ✅ main_menu\common\ ← static_modifiers\ 与 script_values\ ★最容易漏掉的一边★
> ```
>
> 实测翻车：`local_construction_speed` 只扫 `in_game\common\` → **"本体 0 处"**（据此建议换键）；
> 补上 `main_menu\common\static_modifiers\location.txt` 后 → **本体 9 个值**
> （`0.5 / −0.1 / 0.1 / −0.25 / 0.01 / −0.10 / 0.05 / −0.5 / 0.15`）。
> 同一次补扫还把 `local_build_buildings_efficiency` 从"无数据"变成 **0.2×11 众数**。
> **本体的 `static_modifiers` 不在 `in_game\common\` 下**——它和 `script_values` 一起住在 `main_menu\common\`。
>
> **判据**：拿到"某键本体 0 处"时，**先怀疑扫描范围，再怀疑键名，最后才信它是真的**。
> 与 §十三"区分 `0 = 原版从不用` 与 `0 = 正则写错`"是同一条要求的两种形态。

实测锚点：

| 修正 | 众数／依据 |
|---|---|
| `legislative_efficiency` | 0.1 |
| `stability_decay` | −0.00025（19×），最低只到 −0.005 |
| `global_monthly_literacy` | 0.01（28×） |

### ⚠️⚠️ 出处的**种类不止一种**——"本体没有这个值"不能作为改值的唯一理由

「每个值都要有本体出处」这条规矩**不完整**。出处有三类，**强弱不同**：

| # | 出处种类 | 例 | 强度 |
|---|---|---|---|
| 1 | **本体脚本实值**（吸附众数/实有值） | `local_unrest = -0.02` | 常用档。含义是"**别人这么写过**" |
| 2 | **游戏机制推导值**（按引擎公式/经济平衡算出来） | `local_monthly_food = 2.0`（按"无业农民的粮食产出"与"每月粮食消耗"的差额算出） | **比 ① 更强**。含义是"**这里就该是这个数**" |
| 3 | **场景／史实锚定值** | 按年代、地域、史实事件定的值 | 视论证强度 |

**踩法（2026-09，我又犯一次）**：看到 `local_monthly_food` 本体只有 1 处、值 `1.0`，就建议把用户的 `2.0` 降到 `1.0`——**忽略了 2.0 是算出来的**。用户点破后撤回。

**判据**：**改一个既有值之前，先问"这个数是不是算出来的"**，而不是先查本体有没有同值。
本体脚本实值只能证明"这个值**被允许**"，不能证明"这个值**是错的**"——尤其当那个实值来自**结构完全不同**的场景时
（本例的 `1.0` 与"该地点的就业结构"无关，而 `2.0` 正是按就业结构算的）。

**配套**：推导值应在注释里**写明推导过程**（用了哪两个量、怎么算的），否则下一个人（包括我自己）会再犯同样的错。

### ⚠️⚠️ 众数法不够：本体多数修正**不给裸数字**，给具名脚本值

**这是本项目最大的一次批量错值来源**（2026-09，两条法律 18 处值 + 一份设计文档全中）。出错写法：注释声称"本体 0.2(14×)"，而那句实际统计的是**某个具名脚本值出现 14 次**，不是"数值 `0.2` 出现 14 次"。两个后果：

1. **写出了本体根本不存在的裸数字**。实测：`global_monthly_food_modifier` 本体 **94 处全部是具名值，裸数字 0 处**；`global_production_efficiency` 裸数字只有 `0.025`(5×) 与 `0.05`(1×)。
2. **众数取错**——统计对象成了具名值的名字，不是数值。

**判据（三步）**
1. **先查裸数字有几处。** 若该键本体 0 处裸数字 ⇒ **必须写具名值**；此时写数字即使数值凑巧对，也是错的写法。
2. **具名阶梯是前缀式命名**：`<档>_<键>_bonus` / `_penalty`，**不是** `<键>_<档>_bonus`。⚠️ 所以 `Select-String -Pattern '^production_efficiency'` **一个都搜不到**——要搜 `production_efficiency`（不限行首）。
3. **档位词表**：`tiny / very_weak / weak / small / mild / medium / moderate / large / severe / huge / extreme / ultimate / radical / unique / catastrophic / apocalyptic / extinction`。每个键只用其中一段，**不是每键都有全套**。

**实测阶梯（`main_menu\common\script_values\default_values.txt`）**

| 键族 | 行 | 阶梯 |
|---|---|---|
| `monthly_food_productivity_*` | :515-533 | tiny .0025 / weak .025 / mild .05 / severe .075 / extreme .10 / ultimate .125 / **radical .165** / unique .25（惩罚侧一路到 −2.00） |
| `*_production_efficiency_*` | :856-868 | tiny .005 / small .01 / medium .025 / large .05 / huge .1（惩罚侧 −0.01…−0.5） |
| `*_tax_income_efficiency_*` | :928-938 | tiny .01 / small .02 / medium .03 / large .05 / huge .1（惩罚侧 −0.01…−0.2） |
| `monthly_prestige_*` | :39-52 | very_weak .02 / weak .05 / mild .1 / severe .25 / extreme .33 / ultimate .5 / radical 1.0 |
| `merchant_power_*` | :536-545 | weak 10 / mild 25 / severe 50 / extreme 75 / ultimate 100 |
| `liberty_desire_*` | :529-538 | −100 / −50 / −20 / −10 / −5 / +5 / +10 / +20 / +50 / +100（**唯一对称双梯**，负值才是"降低独立倾向"） |
| `societal_value_monthly_move` | :422 | 0.1；全族 :419-425 = min_scaling .01 / tiny .025 / minor .05 / **move .1** / large .2 / significant .33 / huge .5 |
| `monthly_food_productivity_*` 位置 | :515-522 | 只到 `unique_bonus = 0.25`——**0.2 这个数在食物键上根本不存在**，介于 radical(.165) 与 unique(.25) 之间 |

**副作用**：改成具名值后**数值本身也会变**（阶梯是离散的）。所以"改写法"与"改数值"往往是同一次改动，不要当成两件事。

**写完必做的 4 项复核**
1. 每个数值键：本体**裸数字几处**？0 处 ⇒ 必须换具名值。
2. 每个具名值：在 `default_values.txt` 里**确实存在**？（写错名字 = 静默失败）
3. 每个键的 `category` 是否 `country`？（不是 ⇒ 在 `country_modifier` / `overlord_modifier` 里**静默失效**）
4. 每个键是否 `boolean=yes`？（是 ⇒ 不能给数值）

**顺带：符号判据不能按正负号。** 删/改"负面修正"时必须按 **`color`** 判断，本体有反号键：
- `antagonism_received_modifier`、`diplomatic_spending_cost` = `color=bad` ⇒ **正值才是有害**
- `stability_decay` ⇒ **负值是好事**（衰减变慢）
- `antagonism_monthly_change_modifier` = `color=neutral` ⇒ 其负值 = 对抗度每月下降 = 好事
- `pop_join_rebel_threshold` ⇒ 负值是好事（入叛门槛变高）

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

### ⚠️⚠️ 改既有值之前，先读**这个文件自己的审计注释**

"扫描范围"不只指**扫哪些目录**，也包括**这个文件自己已经记过什么**。

实测（2026-10，审两个自建法律文件）：只扫 `laws\` 一个目录，据此判出"五处越界"并直接改掉；回滚时打印出的行里才发现**这两个文件自己带着一份 v4 数值返工记录**，逐值写着当时的本体计数与理由：

```
#   global_population_growth          = 0.001  次众数 13×（众数 0.0001/21× 过于微弱，
#                                              本条是"劝农"的招牌，刻意取高一档）
#   diplomatic_capacity               = 2      ⚠ **刻意超模**：众数是 1(57×)，2 仅 3×
#   antagonism_monthly_change_modifier = -0.05 本体 14 处，众数 0.02(4×)，-0.05 仅 2 处。
```

那些计数（`149×` / `120×` / `57×`）**远大于只扫 `laws\` 得到的数** ⇒ 窄口径覆盖了三处**有记录的刻意选择**，只能全部回滚。

**判据**

| 注释状态 | 动作 |
|---|---|
| 写了计数 | 默认不动；要动，先说明当年理由为何不成立 |
| 写了"刻意 / 超模 / 取高一档" | **不动**，除非设计意图本身变了 |
| 只写了"等于原值 / 形式已核" | **可以动** —— 它没核强度 |

**"注释核了 A 不等于核了 B"**：同日保留的两处里，`huge_tax_income_efficiency_bonus` 的注释只写了"原写裸数字；**值本身对**"——它核的是"具名值等于原数字"，**没核"huge 这个档该不该用"**。而 huge 在全本体只出现 **2 次**（large 61 / medium 58 / small 36）。
⇒ 读注释时必须看清**它到底核了什么**。

★ 顺带一条同日实测的**同类判据**：具名脚本值会**把量级藏起来**。"huge / radical" 听起来夸张，实际分别是 `0.1` / `0.165`，而它们是**阶梯的顶端档**——本体法律里从没用过 huge，food 最高只到 extreme（0.1）。**用具名值时必须回查它等于多少、以及那个档在本体被用过几次。**

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
| ④ | **叛军让步** `grant_benefits_to_estate` | 给阶层特权 + 满意度 + 政策 | `scripted_effects\rebel_negotiate_effects.txt:13`（7 个诉求调用） |
| ⑤ | 平时 UI 直接切换 | **100 稳定度 + 10 正义** | `prices\00_hardcoded.txt:90-97` |
| ⑥ | 事件 | 事件选项代价 | DHE：`flavor_TUR` 24 次、`flavor_ENG` 24 次、`flavor_FRA` 19 次… |
| ⑦ | 通用行动（虔诚改教法学派） | 行动价格 | `generic_actions\piety.txt:176-204`（9 次） |
| ⑧ | IO 政策投票 | `requires_vote` | `laws\readme.txt` |

议会只是**最常用的「无损」切换方式**（不花稳定度/金币，代价是先替阶层办议程）。

**读法**：`grep` 该效果在**全 `common` + `events`** 的调用点分布（如 `-Pattern "add_policy = "`），按调用点归纳途径，不要凭 UI 印象或单个文件就下"唯一/常规"结论。

**连带教训——术语混用会把途径一起搞错**：`law`（法律，**只能解锁/锁定，不存在"被修改"**）与 `policy`（政策，**可切换**）是两个概念，官方 readme 首行即写"A law is a container for one or more policies"。中文玩家口中的"改法律"实际永远是在改政策——写文档时跟着混用，就会顺手把"变更途径"也归错对象。

## 十三、统计本体数据时的工具偏误（2026-09：缩进锚点把必填字段判成可选 / 键名撞车把噪音当取值 / 搜索范围缺一半把"没扫"当成"没有"）

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

**同源变体：搜索范围缺一半——"本体 0 处"其实是"我没扫那儿"**（2026-09 实测，直接导致一次错误的换键建议）。

正则没错、缩进没错、键名也没错，**错在扫描的目录列表**。校准数值分布时只扫了 `in_game\common\`，漏掉 `main_menu\common\`——而本体的 **`static_modifiers\` 与 `script_values\` 都在 `main_menu\common\`，不在 `in_game\common\`**。

| 键 | 只扫 `in_game\common\`（**错**） | 补上 `main_menu\common\`（**对**） |
|---|---|---|
| `local_construction_speed` | "本体 0 处" ⇒ 建议换键 | 本体 **9 个值**：0.5 / −0.1 / 0.1 / −0.25 / **0.01** / −0.10 / **0.05** / −0.5 / 0.15 |
| `local_build_buildings_efficiency` | 4 处（0.2×2 / −0.1 / 0.3） | **0.2×11（众数）** / 0.4×4 / 0.3×4 / 0.5×3 |
| `local_migration_attraction` | 11 处 | **22 个值**，众数 0.1×25 |

**判据**：拿到"某键本体 0 处（或只有寥寥几处）"时，**先怀疑范围，再怀疑键名，最后才信它是真的**。
这条与本节其它变体是同一句话的三种说法：**"0" 永远先是方法问题的嫌疑对象**（§十"数值校准法"里也留了一份同样的警告）。

## 十四、PowerShell 静默故障（2026-10 实测 7 次：不是误报，是**恒为 0 / 丢数据 / 毁文件**）

**这类比第十一节危险得多**：第十一节是"报假错"，本节是"**报没问题**"。一次汉化排查中，同一批统计脚本连续 4 次返回 `0`，每次都像是"已全部覆盖"，实际是脚本在静默丢数据。

### 病根：大集合 + 管道/字典索引，在这个环境下会静默失效

| # | 写法 | 症状 | 正确写法 |
|---|---|---|---|
| 1 | `$dict[$key]` 取值（`Dictionary` 或 `Hashtable`，数万键规模） | **返回 `$null`**，不报错。比对 `$a[$k] -eq $b[$k]` 因此**恒为 True** → "冗余 = 覆盖总数"这种荒谬结果 | 遍历时用 `foreach ($kv in $dict.GetEnumerator())` 并只用 `$kv.Key` / `$kv.Value`；确需按索引取，先 `ContainsKey` 再用**小规模**字典分片 |
| 2 | `foreach ($k in $dict.Keys)` | **只迭代 1 次**（大字典截断） | `foreach ($kv in $dict.GetEnumerator())` |
| 3 | `... \| Where-Object { $_.Length -ge 2 -and $_ -notmatch '^[A-Z_]+$' }` | 条件明明为真却**匹配 0 条**（`[Our Kin]` 判为 0 个词） | 改显式 `foreach` + `if` 累加进 `List[string]`，不用管道 |
| 4 | 函数返回 `@{ KV = $h; File = $f }` 后取 `$r.KV` | 成员变成空，后续统计恒 0 | 不用函数封装，**在同一个脚本作用域里内联**两个 Hashtable |
| 5 | 把结果 `Cat $_`（自定义函数）传给 `Group-Object { }` | 函数名被当成**路径**解析，报"驱动器不存在"，输出全是垃圾 | 不用函数，直接在循环里算出分类列，`Export-Csv` 后用列名 `Group-Object` |
| 6 | 变量名 `$mod` / `$home` 等 | `$mod` 是 **PowerShell 自动变量**，赋值被**静默忽略** → 路径拼错 → 文件根本没装载 → 统计恒 0 | 用 `$modDir` / `$gameDir` 等非保留名；避免一切自动变量 |
| 10 | **`$x` 与 `$X` 同时使用**（循环变量 `$x` 累加进列表 `$X`） | PowerShell **变量名大小写不敏感** ⇒ 两者是同一个变量 ⇒ 循环最后一次赋值把列表**覆盖成字符串** ⇒ 循环后 `WriteAllLines($f, $X, …)` 把一个字符串当行数组写入 ⇒ **11 个文件各剩 1 行 61 字节，数据被毁** | **禁止**单字母变量名，尤其禁止仅大小写不同的名字并存。累加列表用 `$items`/`$linesOut`，循环元素用 `$item`/`$line`，名字必须语义化且互不为大小写变体 |

### 另一类：正则/语法的静默偏差

| # | 写法 | 症状 | 正确写法 |
|---|---|---|---|
| 7 | `$k -match '^A_\|^B_'`（或运算未分组） | 锚点只作用于第一个分支，匹配结果偏离预期 | `$k -match '^(?:A_\|B_)'` |
| 8 | 严格正则 `^\s+(K)\s*:\s*"(.*)"\s*$` 遇上**行尾注释** | 漏掉所有带注释的行，键数偏低 | 先确认目标文件是否有尾注释；有则用宽松式 `^\s+(K)\s*:\s*"`，值另取 |
| 9 | **用错列**——TSV 的 `Zh` 列里放的其实是官英原值，却去查另一张表 | 分类恒为 0（等价于"全都无需翻译"） | 分类前先 `Import-Csv` 打印 `PSObject.Properties.Name` 与前 3 行原文，**确认列义再写判据** |

### 最危险的一类：变量撞车导致**破坏性写入**（2026-10 实测，11 个文件被毁）

上面 1–9 条都只是"算错/算空"，源文件无恙；第 10 条会**真的改写文件**。批量改写 mod/本地化文件时，一次撞车就是成批数据损毁，且**不报任何错**。

**批量改写文件的强制流程**（任何语言脚本通用，不只 PowerShell）：

1. **先备份**：`Copy-Item` 整目录到临时位置，或确认已在 git 版本控制内。
2. **写临时文件，不直接覆盖**：结果先落 `xxx.tmp`，全部成功后再原子替换。
3. **写完立刻校验**（不校验 = 没写完）：断言 **文件数**、**每个文件的行数与字节数**、**关键键数量**符合预期；任一不符，**立即从备份回滚**，不要"先看看"。
4. **禁止用循环计数器/单字母名承载数据集合**：累加容器与循环元素必须是**语义化且互不为大小写变体**的名字。
5. **改写前先 dry-run**：先只打印"将要改哪些文件、各改成几行"，确认无误再执行写入。

### 强制自查规则（写进任何统计脚本）

1. **先证明基准量正确**：装载后立刻打印 `$集合.Count`，并**抽查 3 个已知键的实际取值**（`auch.gascon_dialect` 这类）。取值要能打印出预期的中文字符串，否则后面全部作废。
2. **结果出现 0 或"恰好等于总数"时，一律先怀疑脚本**，不要当成结论。本次 `0`（应为 545）、`0`（应为 1047）、`1430=1430`、`6797=6797` 全是脚本 bug。
3. **管道与显式循环各跑一次**，两者不一致则以显式循环为准。
4. **抽样人工核对**：统计出的"需翻译清单"里挑 5 条调出原文看，确认确实需要翻译（本次抽样才发现 `CATEGORY_HOSTILE_ACTIONS`、`$MEN$@manpower!` 这类**不该译**的标识符占了 2/3）。
5. **破坏性操作（force-push / 覆盖写 / 批量删除）永远不许和它的验证步骤链在同一个脚本里**：验证输出必须先回到执行者眼前、读过之后再发破坏指令。2026-10-09 实案：git 强推脚本把 fetch 检查与 `push -f` 连写，fetch 明明显示远程有 99 条真实历史，脚本照推——全靠推送前刚 fetch 过、对象还在本地才抢救回来。链式脚本里的"看起来没问题就继续"等于没检查。
6. **多行文本（提交信息等）不许当 PS 变量直接传命令行参数**——PS 会把多行字符串按数组展开成多个参数，命令报错又常被 `| Out-Null` 吞掉（同案：`git commit -m $msg` 静默失败、HEAD 原地不动，后续验证全建立在错误前提上）。多行消息一律写临时文件 + `git commit -F`。另：git 默认 `core.quotepath=true` 会把中文文件名输出成八进制转义，拿它回喂 `git rm` 必然失配——脚本前先 `git config core.quotepath false`。

## 十五、关闭/移除的接口与字段（2026-10 实查 1.4）

> 判定字段合法性时 **readme 与实际用法要互证，两边都可能说谎**：readme 会漏声明（`political_influence`），也会残留已删字段（`overlord_protects_external`）；原版文件会用 readme 没写的字段，也绝不碰它没开放的东西。

- **阶层私兵是原版阶层专属，自定义阶层不可用**：1.4 的 `private_army_per_pop` / `private_army_unit_categories` 只有 `nobles_estate` 与 `cossacks_estate` 携带（`estates\00_default.txt`），且 `estates\readme.txt` **完全未声明**这两个字段。`*_allowed_private_army` 修正虽覆盖 8 个原版阶层，但那是给原版阶层开私兵的特权门槛，不是自定义阶层的钩子。**替代路线**：自建 subject type 体系（有名字、有领土、可交互的可见实体，表现力强于隐藏单位式私兵）。
- **1.4 移除了 `overlord_protects_external` / `counts_as_external`**：1.3.11 合法（旧备份实证），1.4 全游戏 0 命中且 subject_types\readme.txt 不声明 → error.log 每形态 2 条 `Unexpected token`。**版本移除的字段没有语义替代**，只能删行并接受行为变化；跨版本移植务必全文件对 readme + 原版用量双重核查。
- **`prices\readme.txt` 字段表不全**：`political_influence` 未列出，但原版 `prices\08_estate_emergency.txt` 实际在用（阶层紧急行动 15-50 点）。**"readme 没有" ≠ "引擎不支持"——以原版实际用法为准**，与上一条合看：两个方向都要查。
- **每个 price 需要配套 `<price id>_cost_modifier` 修正类型**：引擎按命名约定到 modifier_type_definitions 找它，缺失则 `[price_database.cpp:121] Missing modifier type for price. <id>_cost_modifier`（每 price 一条）。原版标准结构：`color=bad / percent=yes / game_data={category=country}`。
- **`action_button_regular` / `card_list_action_button` 不是 widget 类型**（只是贴图名），写成类型报 `is not a valid widget/type/property`；行动按钮的正确类型是 `targetted_button_regular`，且它**不自带 text**——每个实例必须显式 `text = "<action key>"`，否则按钮标签空白。
- **`power_per_pop` 1.4 从 estates 移到 pop_types**：阶层政治力量改由 POP 岗位决定（阶层属性 → POP 属性）。自定义阶层要政治力量，把 `power_per_pop` 挂在自己的 pop_type 上，别再往 estate 文件里写。
- **事件选项底部信息栏不渲染 `\n`**（2026-10 截图实证）：138 字中文带 4 个 `\n` 仍连排成一条横带。长 tooltip 的解法是**缩短文本**（对齐同面板其他选项的量级），不是加换行——换行只在 `desc` 正文等段落渲染区有效。
- **1.4 存档是压缩二进制**（文件头 `SAV0303`）：旧"明文存档 grep 变量"分析法失效；验证存档状态改用 debug.log / error.log / 游戏内观察。
- **覆写即负债（2026-10 实害）**：REPLACE / REPLACE_OR_CREATE / 同名覆盖的复制源必须是**当前版本**——用旧版本复制件整块替换 = 把新版本行动**静默降级**（实测：1.3.11 复制的 extraordinary_taxes 丢了 1.4 新增的 `price` / `estate_interaction_action` / `icon`，零报错）。验收标准：改完与原版逐行 diff，**差异行数恰好等于意图改动数**。每次游戏大版本更新，清点 mod 全部覆写文件重验。
- **`looking_for_a = unit` 同时覆盖陆军与海军**：凡消耗/转化单位的行动，select_trigger 的 visible 必须显式 `is_army = yes` 或 `is_navy = yes`——实测 27 艘船被"遣散军队"按 `subunit_strength` 折算成士兵 POP 并拆船，**全程零报错**（引擎不质疑语义，只执行逻辑）。"日志干净"≠"没漏洞"，验收要故意做设计师没想到的操作。

## 十六、校验器自身失真（2026-10 两次实测：假阴性比假阳性更危险）

第十一节是"报假错"，第十四节是"静默丢数据"，本节最隐蔽：**校验本身给出"通过"的结论，而它是错的**——因为"通过"让人不再复查。同日两次实测：

| # | 现场 | 当时的结论 | 真相 |
|---|---|---|---|
| 1 | PowerShell 里读 UTF-8 文件，中文显示成 `寰锋媺…` 乱码 | **"文件被写坏了"**，一度判定 11 个 mod 文件损毁 | **文件本体完好**，是控制台按 GBK 解码 UTF-8 的**显示层**问题；换 Python 按 UTF-8 读，内容正常 |
| 2 | 用正则查 persona 里"是否残留硬编码路径"，返回 **0 行** | 报告**"清理干净 ✅"** | 校验串写成 `D:\\dsh-plugins`（双反斜杠），文件里是单反斜杠 ⇒ **模式永远匹配不到**，那个 0 是**假阴性**；改正后真实结果是 1 行（且那 1 行是必须写死的锚点定义） |

### 强制规则

1. **证明"不存在"之前，先证明"这个检查能发现存在"**（阳性对照 / positive control）：拿一个**已知应当命中**的样本喂给校验器，命中了才允许采信它报出的 0。第 2 次事故若有这一步，当场就能发现校验器是死的。
2. **匹配字面文本不要用正则**：用 `in` / `Contains` / 普通字符串查找。需要正则时记住反斜杠要跨**两层**转义（代码字符串层 + 正则层），层数最容易数错——`\\` 在代码里是"一个反斜杠字符"，在正则里才表示"匹配反斜杠"。
3. **分支锚点必须包组**：`^A_|B_` 的真实语义是"以 `A_` 开头"**或**"含 `B_`"，`^` 只管第一个分支；要写 `^(?:A_|B_)`（与第十四节第 7 条同源，此处按"校验器"视角重记）。
4. **"日志干净"≠"没问题"，"校验报 0"≠"事实为 0"**：与第十五节 `is_army/is_navy` 那条同理——引擎不质疑语义只执行逻辑，校验器也只执行你给的模式，不质疑模式是否写错。
5. **判断编码问题不要用控制台脸色**：用可显式指定编码的手段复核（`Get-Content -Encoding UTF8`、Python `open(..., encoding='utf-8')`、读字节头看 BOM）。

### 元教训

**校验器也要被验证。** 越是"证明没问题"的检查，越要先证明它有本事发现问题。
