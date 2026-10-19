# 本地化制作（localization）

> **一句话**：本地化文件的放置位置、UTF-8 BOM 与语言头格式、11 种语言代码，以及事件、任务、修正、交互各类键的前缀规则与占位符写法。
> **什么时候看**：写 loc 文件或排查界面显示 raw key 时，对照本文的位置、格式与键前缀规则。
> **体量**：105 行 · 约 5 分钟通读
> **最后校验**：EU5 1.4.1（Steam buildid 25905365）/ 2026-10-13 —— 新增「界面表达式的版本迁移」一节

## 文件位置（实查修正）

| 位置 | 内容 | mod 用法 |
|---|---|---|
| `main_menu\localization\<lang>\` | **原版主语言文件**（english 114 个 yml：actions_l_english.yml、advances_l_english.yml、buildings_l_english.yml…） | mod 可放这里 |
| `loading_screen\localization\<lang>\` | languages.yml、load_tips_l_<lang>.yml | 启动界面文本 |
| `loading_screen\localization\jomini\` | savegame_gui_l_<lang>.yml、cw_ugc_dlc_l_<lang>.yml | 存档界面 |
| `main_menu\localization\<lang>\missions\` | **任务链/任务节点文案**（4 个 yml：`generic_missions_l_english.yml` 93 KB 等；11 语言都有该子目录） | 任务 mod 放这里 |
| `in_game\localization\jomini\script_system\` | **引擎触发词条** trigger_system_l_<lang>.yml（11 语言） | 只读参考（查词条存在性） |
| `in_game\localization\<lang>\` | **本体此处没有 mod 式语言文件**（整个 `in_game\localization\` 只有上面那 11 个引擎词条） | **mod 可放这里**（实测 36 个 mod 里 7 个只用此处、7 个与 main_menu 并用） |
| `dlc\*\main_menu\localization\dlc\<lang>\` | DLC 语言（dlc_localizations_l_<lang>.yml） | 官方 mod 示范 |

语言代码：english / french / german / spanish / simp_chinese / korean / japanese / polish / russian / turkish / braz_por（11 种）。

**mod 的本地化放哪**：`main_menu\localization\<lang>\` 或 `in_game\localization\<lang>\` **都行，两处都会被加载**。

- 实测口径（2026-09，创意工坊 36 个带 yml 的 mod）：**22 个只放 `main_menu\`、7 个只放 `in_game\`、7 个两者都用**；而"两者都用"的 mod 里**没有一对同路径同名的文件**（两处放的是不同内容）。→ **不存在"必须双镜像、两侧键必须一致"的要求**（旧版文档写过这条，已按实测订正）。
- 原版自身：`main_menu\localization\` **5,835 个 yml**（主体，事件/任务/概念词条全在这里，原版事件键 `ages_of_eu.1.title` 就在 `main_menu\localization\english\advances_l_english.yml`）；`in_game\localization\` 只有 **11 个**，全是引擎内部词条 `jomini\script_system\trigger_system_l_<lang>.yml`（只读参考）。
- 同键冲突时**按加载顺序后者覆盖**（`zzz_` 前缀 = 后加载，见 `merging.md`）；**键名不重复定义最稳**。

## 文件格式

```yml
l_english:                     # 语言头（必须首行）
 ACTION_COST: "Spend $PRICE$ to gain the following effects:"
 call_jihad: "Call $jihad$"
 call_jihad_desc: "Choose an infidel country to bring down the might of Allāh upon."
 call_jihad.tt: "A new [GetInternationalOrganizationType('jihad').GetName] is created against the selected [country|e]"
```

- **必须 UTF-8 BOM**（无 BOM 整个文件被忽略/乱码；编辑工具常剥 BOM，保存后要补回）。
- 语言头必须在**第 1 行**：`l_english:` / `l_french:` / `l_german:` / `l_spanish:` / `l_simp_chinese:` / `l_korean:` / `l_japanese:` / `l_polish:` / `l_russian:` / `l_turkish:` / `l_braz_por:`（本体 5,835 个 `simp_chinese` yml 中 5,829 个头在第 1 行，其余 6 个前 8 行内找不到头）。
  - ⚠ 实测创意工坊 mod 里有人在头前放注释行（`# earth content. remove this f...`），共 4 个 mod、约 40 个文件这么写；**引擎是否容忍未验证**——本体从不这么写，自用 mod 建议照本体放第 1 行。
- 键**必须缩进一个空格**写在语言头之下；值用双引号包裹；`#` 注释。
  - 实测口径（2026-09）：本体 `main_menu\localization\simp_chinese\` 全部 yml 的键行 **152,165 行全部缩进**，**顶格键 0 例**——顶格会被解析成新的顶层块，导致该文件键全部失效。
  - 曾有一版文档把这条写成"键顶格（不缩进）"，与同页示例和本体实测都相反，**2026-09 按本体全量实测订正**（`tools\kb-self-audit.md` §九 第 2 条）。
- 键内可用 `"` 转义、`\n` 换行。

## 键前缀规则（名称即键，漏一个显示 raw key）

- 事件：`<namespace>.<id>.title / .desc / .historical_info / .a / .b / .tt`
- 任务（`missions\` 子目录）：链 5 键 `<链名>` / `<链名>_DESCRIPTION` / `<链名>_CRITERIA_DESCRIPTION` / `<链名>_BUTTON_TOOLTIP` / `<链名>_BUTTON_DETAILS`；节点 3 键 `<节点名>` / `<节点名>_desc` / `<节点名>_tt` ← `guides\event-making.md` §任务、`fields\common-missions.md`
- 静态修正：`STATIC_MODIFIER_NAME_<键>`、`STATIC_MODIFIER_DESC_<键>`（main_menu\common\static_modifiers）
- 自动修正：`AUTO_MODIFIER_NAME_<键>` / 科技 `ADVANCE_NAME_<键>` / 建筑 `BUILDING_NAME_<键>`（见 `tools\loc-keys.md` 全表）
- 国家交互/角色交互：`<交互键>`、`<交互键>_desc`、`<交互键>.tt`（actions_l_english.yml 里 call_jihad 系列就是例子）
- 游戏规则：`rule_<键>`、`setting_<键>`、`setting_<键>_desc`（`_game_rules.info` 明示）

## 占位符

- 脚本参数：`$PRICE$` / `$MONTHS|0$`（`|0` 表示取整），必须配对存在，否则显示原文。
- GUI 表达式：`[Root.GetName]`、`[country|e]`（概念链接）、`[GetInternationalOrganizationType('jihad').GetName]`、`[ShowReligionGroupAdjective('muslim')]`——照抄原版 yml 的写法。

## 界面表达式的版本迁移（汉化 mod 每次本体更新必做重扫）

loc 值里的 `[Scope.Method]` / `@[…]!` 图标语法**不是字符串装饰，而是引擎运行时调用的方法**。方法被改名或移除后，**旧写法不会报错，只会显示不出来或露出原始标记**。官中不会跟着官英重扫，所以本体小版本一更新，官中就冒出一批失效调用——这正是汉化 mod 该修的，也是本 mod 译文自身会背上的债。

**1.4.1 实查（基准：Steam buildid 25905365，`game\loading_screen` 时间戳 2026-10-13）**

| 旧写法（≤1.4.0） | 1.4.1 官英改用 | 官英残留 | 官中残留 |
|---|---|---|---|
| `@[COUNTRY.GetFlag]!` | `[COUNTRY.GetFlagIconWithNoTooltip]` | **0 处**（`.GetFlag]` 全树 0） | **44 处**（35 个键） |
| `[scope:host_lord.GetName]` | `[host_lord.GetName]` | **0 处** | 7 处 |
| `[current_location.GetProvince.GetName]` | `[current_location.GetName]` | — | — |
| `ShowCharacterInteractionName('favor_heir')` | `ShowGenericActionName('favor_heir')` | `interfaces`/`government_l_english.yml` | — |
| `…GetGovernment.GetRuler.GetName` | `…GetGovernment.GetRulerOrRegent.GetName` | `flavor_mos.3.tt2` | — |

同一版还把宗教修会的 `GetPlayerPower` / `GetPlayerLoyalty` 体系整体换成 **standing**：新增 `RELIGIOUS_ORDER_STANDING_*`、`zeal_*_hint` 系列键，旧 power/loyalty 方法在官英中**一处不剩**。

**普查方法（本库标准做法，脚本见汉化 mod `_work\sweep-141-*.ps1`）**

1. 全量导出官英键→值（`main_menu\localization\english` 递归 + `dlc\**\*_l_english.yml`），同样导出官中；两份快照对上一版做 diff，得 **NEW / CHANGED / REMOVED** 三类增量——**不要只补新词**，CHANGED 里藏着表达式迁移。
2. **逐键**把本 mod 译文（以及官中原文）的 `[令牌]` 与 `$占位符$` 集合，对**同键官英**取差集：**多出来的令牌 = 引擎不再提供的调用/变量**，必然显示异常。这一步能同时抓出错译（如官中把 `#R left unchecked…#!!` 写成 `！` 收尾）与失效方法。
3. 判定"某方法是否还活着"用**全树计数**：官英 0 处使用 + 官中仍在用 = 已被移除（比"查 readme"可靠，loc 方法没有 readme）。
4. 差集比对要**规范化**：`[X.GetName|Y]` 与 `[X.GetName]` 只差颜色格式，属正常；`[war|e]` 与 `[war_leader|e]` 才是不同令牌。不规范化就会产生上千条噪声（实测 1539 条 → 去噪后 42 条真问题）。

**惯例（不要擅自"补译"）**

- **DLC 显示名官中保留拉丁原文**：`fate_of_the_phoenix_key` / `across_the_pillars_key` 官中值 = 官英原文 `#italic …#!`。译成中文反而与原版不一致，**跳过**；但 `<dlcname>_dlc_desc` 官中是译的，要译。
- `<X>_activate_desc` 官中固定句式「获取Premium Edition，或从商店单独购买以启用！」（"Premium Edition" 不译）——沿用，别自创。

## 检查清单

1. 文件 BOM ✔ 语言头正确 ✔
2. 每个引用的键都存在（事件 title/desc/选项/tt、修正名、交互名…）
3. 占位符配对
4. 同键在 main_menu 与 in_game 两侧（若两侧都放了文件）语义一致
5. 中文用 `l_simp_chinese` 文件，别写进 l_english（会显示英文原文给中文玩家）
6. **每次本体更新**：重跑「界面表达式的版本迁移」一节的逐键差集普查，覆盖本 mod 自己的全部译文（不只是新增词条）

## 未译池：官中"有键但值没译"（与"表达式失效"并列的另一类）

判定游戏里露英文，要**同时**查两件事：① 表达式失效（上一节）；② **键存在但值仍是整句英文**。只查新增键会整类漏掉（1.4.1 实测：新增键 238 条之外，还有 1399 条这类）。

**筛出"真需翻译"的四层判据**（缺一层就有成倍噪声，详见 `pitfalls.md` §十八）：剥 `[...]`/`$…$`/`#标记`/`@图标!` → 再剥**整段 `#TOOLTIP:…#!` 指令串** → 剔除"值是另一个键名"的全大写标识符与 `:TOKEN,` → 剩余英文词数 ≥2 才算待译。

**哪些不该译**（官中自己也保留）：`_CATEGORY` 等指向别的键的值、公司/工作室名（`#credits_studio PARADOX TINTO#!`）、DLC 显示名、人名；曲名按"**英文描述性标题意译、非英语原语标题保留、括号内乐器/职务译出**"处理；`_flavour`/`_facts` 正文一律译。

**写入门禁（每条都要过）**：与本版本官英同键的 `[令牌]`/`$占位符$`/标记集合**必须完全相等**；另加"值内不许裸双引号、剥标记后不许残留英文词"。旧清单不能直接当任务表——先用"该键是否仍被脚本引用"验活（EU5 事件/决议显式写 loc 键名，可 grep `in_game\`），并给这个检查做**阳性对照**。
