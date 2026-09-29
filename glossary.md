# EU5 术语总表（glossary）

> **这是什么**：本库文档里出现过的**术语中英对照总表**，供人类读者查到不认识的词时翻阅。
> **来源**：全部条目汇总自本库既有文档（每条标出处）；**不含库外新知识**，未在库内出现的词一律不收。
> **给 agent**：不需要读本文件——直接读对应权威档更省 token。

版本基准 EU5 1.3.x。共 **132 条**：§一 69 · §二 35 · §三 14 · §四 14；「待核实」8 条，另「已订正」9 条（2026-09 按本体概念键逐词核对）。

## 一、核心概念（引擎与游戏机制）

| 内部名 / 英文 | 中文（游戏内译名） | 一句话 | 出处 |
| --- | --- | --- | --- |
| `age` | 时代 | 6 个，全局按日期推进 | `vanilla\vanilla-tech-and-age.md` 术语对照 |
| `institution` | 思潮 | 每时代 3 种；9 条传播通道，需接纳 | `vanilla\vanilla-tech-and-age.md` 术语对照 |
| `advance` | 革新 | 3178 条；国家同时只能研究 1 项 | `vanilla\vanilla-tech-and-age.md` 术语对照 |
| `research` / `research_progress` | 研究 / 研究进度 | 进度 = f(礼仪语言力量, 识字率, 教士满意度, 思潮数) | `vanilla\vanilla-tech-and-age.md` §四 |
| `embrace` | 接纳 | 接纳思潮后解锁对应革新 | `vanilla\vanilla-tech-and-age.md` 术语对照 |
| `liturgical_language` | 礼仪语言 | 国教的正式语言；天主教等固定不可改 | `vanilla\vanilla-tech-and-age.md` §四 |
| `language_power` | 语言力量 | 相对世界最强语言的百分比 | `vanilla\vanilla-tech-and-age.md` §四 |
| `rgo` | 原产 | 原料采集点，**不需要建筑**；官方正式名"原料生产" | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| `rgo_pops` | 劳工或奴隶 | 可从事原产的人群类型，只有 laborers + slaves | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| `raw_material` / `produced_goods` | 原材料 / 制成品 | 前者由原产产出，后者由建筑里的人群产出 | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| `production_method` | 生产方式 | 决定建筑的投入品与效率 | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| `establishment` | 投产度 | 达阈值前吞吐缩水，超过后给产出加成；默认关闭 | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| `employment_size` | 雇佣规模 | `1 = 1000 人`（readme 第 5 行） | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| `rural_settlement/town/city/megalopolis` | 乡村/集镇/城市/大都市 | 四个 `location_rank`，是建筑的可建造门槛 | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| `subsidy` | 补贴 | 亏损建筑由所有者每月补足 | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| `market` | 市场 | 市场中心 + 按市场接入度与市场保护覆盖的地点 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `market_center` | 市场中心 | 集中经济行为的地点，所有者可控制入市资格 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `market_access` | 市场接入度 | 地点到市场中心的距离；决定交割顺序 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `merchant_capacity` | 贸易容量 | 能在市场内进出口多少商品 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `merchant_power` | 贸易优势 | 供给有限时决定出口交割优先顺序（英文名 Trade Advantage） | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `trade_profit` / `trade_income` | 贸易利润 / 贸易收入 | 价差收益 / 卖货收入；按阶层权力分配 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `stockpile` | 商品储备 | 市场储备的商品量，用于缓冲价格 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `protectionism` | 市场保护 | 提高本地控制度，抵消市场吸引力 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `sound_toll` | 海峡通行费 | 特定海峡控制者向穿行贸易收费 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `maritime_presence` | 海上存在 | 沿岸海区的海上影响力，贸易优势的重要来源 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| domestic/trading/foreign/distant | 国内/贸易/外国/遥远市场 | 四类市场，按你与它的关系划分 | `vanilla\vanilla-trade-and-market.md` §三 |
| `continent` / `sub_continent` | 大陆 / 次大陆 | 9 个（含 4 个海洋大陆）/ 23 个 | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `region` / `area` | 区域 / 地区 | 82 个 / 805 个；静态层级树的中层 | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `province_definition` | 预设省份 | 4309 个；静态地图分组，定义在 `definitions.txt` | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `province` | 省份 | **运行时**：同一预设省份内由同一国家拥有的一组地点 | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `location` | 地点 | 最小地块，≈2.87 万；总是属于某个省份 | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `sea_zone` | 海区 / 海域 | 在 `default.map` 的 `sea_zones` 段（4821 个） | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `impassable_mountains` / `non_ownable` | 不可通行山地 / 不可拥有（走廊） | 1878 个 / 153 个（撒哈拉、叙利亚沙漠走廊等） | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `scripted_geography` | 脚本地理 | mod/事件用自定义地理包，可混合任意层级 | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `culture` / `culture_group` | 文化 / 文化组 | 一个文化可属多个组；文化组可选，语言必填 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `primary_culture` | 主流文化 | 代表国家身份认同 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `accepted_culture` / `tolerated_culture` | 已接纳文化 / 相容文化 | 三层地位的中上两层 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `dominant_culture` | 优势文化 | 地点/国家内规模最大的文化（**≠ 主导**） | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `cultural_unity` | 文化统一度 | 主流文化人口占比 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `cultural_tradition` / `cultural_influence` | 文化传统 / 文化影响 | 文化战争的防御力 / 攻击力 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `culture_war_power` | 文化战争力量 | 攻/防比；影响同化、整合、间谍网、围城 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `assimilation` | 同化 | 其他文化的人群转为主流文化 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `religion` / `religion_group` | 宗教 / 宗教组 | 293 个 / 29 个 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `religious_unity` | 宗教统一度 | 官方宗教人口占比 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `tolerance_own/heretic/heathen` | 容忍度（国教/异端/异教） | 三个独立数值；1 点 = 5% 满意度 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `religious_influence` | 宗教影响力 | 货币资源，仅 17 个宗教有 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `religious_aspect` | 宗教信条 | 可增删换的教义槽位 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `religious_school` / `sect` | 宗教学派 / 宗派 | 43 学派；宗派仅佛教系 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `god` / `omen` | 神 / 神谕 | 127 神祇；神谕有渐进实装期 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `holy_site` | 圣地 | 241 座，`importance` 1–5 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `canonization` / `saint` | 封圣 / 圣人 | 封圣花 75 宗教影响力 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `cardinal` / `curia` | 枢机 / 教廷 | 仅天主教；教廷是 IO 特殊地位 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `patriarch` | 牧首 | 东正教系，含自治牧首区 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `reform_desire` | 改革呼声 | 天主教内部冲突值 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `tithe` | 什一税 | 天主教的 IO 定期付款，`= 0.02` | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `movement` | 运动 | 宗教/文化在人口中的传播引擎 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `estate` | 阶层 | 国家内部利益集团（8 个） | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `estate_power` | 阶层力量 | 政府内政治力量；是**存量**概念 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `estate_satisfaction` | 阶层满意度 | 需求满足程度 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `estate_opinions` | 阶层观感 | 阶层**对他国**的观感 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `estate_privilege` | 阶层特权 | 提高阶层力量的特殊权利 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `crown_power` | 王室力量 | 被阶层力量总和削弱 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `law` / `policy` | 法律 / 政策 | 法律是容器，含多个政策；**同时只能选一个** | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `parliament` | 议会 | 阶层表达诉求的场所 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `parliament_agenda` | 议会议程 | 阶层提要求，解决它换支持 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `parliament_issue` | 议会诉求 / 议案 | 需表决的事项，未通过有损失 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `parliament_support` | 议会支持 | 通过诉求的意愿值 | `vanilla\vanilla-law-and-estate.md` 术语对照 |
| `disaster` | 灾难 | 作用域收缩到单国、槽位唯一的局势（"个人局势"） | `vanilla\vanilla-disaster-and-situation.md` §二 |
| `situation` | 局势 | 跨国家/跨地区的持续状态容器，可多个并存 | `vanilla\vanilla-disaster-and-situation.md` §二 |

## 二、最容易误译、最容易被 EU4 带偏的

这一节收"字面像 A、实际是 B"的词。**中文列写的是库内记录的译名**，`⚠` 标注库内记录与被引概念词条不一致者。

### 2.1 科技与制度

| 内部名 / 英文 | 中文（游戏内译名） | 一句话 | 出处 |
| --- | --- | --- | --- |
| `advance` | 革新 | **不是"科技"**；3178 条 | `vanilla\vanilla-tech-and-age.md` 术语对照 |
| `institution` | 思潮 | **不是"制度"**；每时代 3 种 | `vanilla\vanilla-tech-and-age.md` 术语对照 |
| ⚠ `devotion` | 奉献度 | 神权国的政府影响力；**不是"虔诚"** | `vanilla\vanilla-government-and-reform.md` 术语对照 |
| `establishment` | 投产度 | **不是"机构/建立"**；是建筑投产度 | `vanilla\vanilla-production-and-buildings.md` 术语对照 |

### 2.2 地图与人口

| 内部名 / 英文 | 中文（游戏内译名） | 一句话 | 出处 |
| --- | --- | --- | --- |
| `province` | 省份 | **运行时概念**，≠ 静态 `province_definition`（预设省份） | `vanilla\vanilla-map-and-geography.md` §二 |
| `area` / `region` | 地区 / 区域 | 两级不同：**地区在区域之下**（大陆→次大陆→区域→地区） | `vanilla\vanilla-map-and-geography.md` 术语对照 |
| `dominant_culture` | 优势文化 | 规模最大者；**≠ 主导文化** | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `tolerated_culture` | 相容文化 | **不是"被容忍的"**；三层地位的中层 | `vanilla\vanilla-culture-and-religion.md` 术语对照 |
| `rgo` | 原产 | 官方的正式名是"原料生产"，原产是缩写 | `vanilla\vanilla-production-and-buildings.md` 术语对照 |
| ⚠ `disease` | 疾病 / **疫病** | 库内术语对照写"疾病"，同篇正文与概念词条用"疫病" | `vanilla\vanilla-hazards-and-environment.md` 术语对照 |
| ⚠ `outbreak` | 疫情 | 指"一次具体爆发"（`disease_outbreak` 作用域） | `vanilla\vanilla-hazards-and-environment.md` 术语对照 |
| ⚠ `pop` / `pop_type` | POP / 社会阶级 | 库内术语对照写"POP 类型"；词条"社会阶级" | `vanilla\vanilla-pop.md` §二 |

### 2.3 贸易与外交

| 内部名 / 英文 | 中文（游戏内译名） | 一句话 | 出处 |
| --- | --- | --- | --- |
| `trade_company` | 贸易公司 | **是一种附庸类型**（`subject_types\trade_company.txt`），不是建筑 | `vanilla\vanilla-trade-and-market.md` 术语对照 |
| `union` / `personal_union` | 共主邦联 | **不是"联合"**；因王室联姻共主形成的条约类型，两个键同译名 | `vanilla\vanilla-diplomacy.md` 术语对照 |
| `spy_network` | 间谍网 | 渗透程度，是"地下外交行动"的货币 | `vanilla\vanilla-diplomacy.md` 术语对照 |
| `ai_disposition` | 国家态度 | **不是"性格"**；是该国目前对你的看法，会不断重估 | `vanilla\vanilla-ai.md` 术语对照 |
| `ai_personality` | 国家性格 | 8 种，驱动外交与军事行动的基本特性 | `vanilla\vanilla-ai.md` 术语对照 |
| `antagonism` | 敌意 | **EU5 版的"侵略扩张"**，0–1000 | `vanilla\vanilla-diplomacy.md` 术语对照 |
| `conquistador` | 征服者 | **既是动作也是附属国类型** | `vanilla\vanilla-colonization-and-exploration.md` 术语对照 |
| `colonial_charter` | 特许殖民地 | 国家想殖民**整个省份**时建立；特许殖民地将迁徙 | `vanilla\vanilla-colonization-and-exploration.md` 术语对照 |
| `merchant_power` | **贸易优势** | **不是"商人力量"**（英文名 Trade Advantage）；贸易篇 2026-09 已订正 | `vanilla\vanilla-trade-and-market.md` §六 |
| `merchant_capacity` | **贸易容量** | **不是"商人容量"** | `vanilla\vanilla-trade-and-market.md` §六 |
| `market_access` | **市场接入度** | **不是"市场准入"** | `vanilla\vanilla-trade-and-market.md` §三 |
| `protectionism` | **市场保护** | **不是"保护主义"**；条约开关 `lifts_trade_protection` 叫"取消市场保护" | `vanilla\vanilla-trade-and-market.md` §八 |
| `maritime_presence` | **海上存在** | **不是"海事存在"** | `vanilla\vanilla-trade-and-market.md` §六 |
| `stockpile` | **商品储备** | **不是"库存"** | `vanilla\vanilla-trade-and-market.md` §四 |
| `sound_toll` | **海峡通行费** | 不是"通行费"；豁免开关 `is_exempt_from_sound_toll` | `vanilla\vanilla-map-and-geography.md` §三 |
| `war_exhaustion` | **厌战度** | 不是"厌战" | `vanilla\vanilla-combat.md` §五 |
| `estate_opinions` | **阶层外交倾向** | **不是"阶层观感"**——是阶层**对他国**的观感 | `vanilla\vanilla-law-and-estate.md` §三 |

### 2.4 EU4 写法在 EU5 不成立

| 内部名 / 英文 | 中文（游戏内译名） | 一句话 | 出处 |
| --- | --- | --- | --- |
| `law` | 法律 | **只能解锁/锁定，不存在"被修改"**；可切换的是政策 | `pitfalls.md` §十二② |
| `event_target` | — | **无此概念**；用 `save_scope_as = xxx` + `scope:xxx` | `pitfalls.md` §一 |
| `ROOT` / `PREV` | — | 不存在；EU5 是 `root` / `prev` / `this` | `pitfalls.md` §一 |
| 本地化 `KEY:0` | — | 不存在 `:0` 后缀 | `pitfalls.md` §一 |
| 选项 `weight = N` | — | 不存在；用 `ai_chance` 或 `ai_will_select` | `pitfalls.md` §一 |
| `province_event` | — | 不存在；用 `type = location_event` | `pitfalls.md` §一 |

## 三、脚本词条（作用域 / 触发器 / 效果 / 修正）

| 内部名 / 英文 | 中文（游戏内译名） | 一句话 | 出处 |
| --- | --- | --- | --- |
| 作用域 scope | 作用域 | 主体对象；由类目或事件 `type` 决定 | `guides\scripting-core.md` §四 |
| `root` / `prev` / `this` | — | EU5 的作用域词；无 `ROOT`/`PREV` 大写变体 | `pitfalls.md` §一 |
| `save_scope_as` / `scope:xxx` | 具名作用域 | 保存后复用；**不跨嵌套 effect 调用** | `pitfalls.md` §二 |
| `?=` | 可选作用域 | 允许目标不存在；`?=` 块内是效果上下文 | `cases\blades-and-thrones-2026-08.md` §5 |
| `trigger` | 触发器 | 只读条件；与效果**不能混写** | `guides\scripting-core.md` §三 |
| `effect` | 效果 | 写操作；`limit` 里不得放效果 | `guides\scripting-core.md` §三 |
| `script_value` | 脚本值 | 公式/常量，任意需要数字处可引用 | `guides\scripting-core.md` §一 |
| `scripted_effect` / `scripted_trigger` | 效果宏 / 条件宏 | 支持 `$参数$` 文本替换 | `guides\scripting-core.md` §二 |
| `on_action` | 触发钩子 | 引擎事件挂载点；同名块是**合并**语义 | `guides\scripting-core.md` §四 |
| `modifier` | 修正 | 数值增减块；键必须在 `modifier_type_definitions` 注册 | `tools\review-checklist.md` §二 |
| `static_modifier` | 静态修正 | 引擎按名识别，零脚本引用也生效 | `pitfalls.md` §七 |
| `scaled & triggered modifier` | 缩放修正 | 用 `scale` + `potential_trigger` 动态算值 | `fields\common-bureaucracies.md` |
| `counters` / `variables` | 变量 | 用前必初始化；`exists = var:X` 不宜用于数值变量 | `pitfalls.md` §三 |
| `INJECT` / `REPLACE` | 追加 / 替换前缀 | 文件合并前缀；事件不能 REPLACE/INJECT | `guides\merging.md` §二 |

## 四、本地化与文件约定

| 内部名 / 英文 | 中文（游戏内译名） | 一句话 | 出处 |
| --- | --- | --- | --- |
| `metadata.json` | mod 描述符 | 注册 mod：name/id/version/supported_game_version/tags | `guides\mod-skeleton.md` §metadata.json |
| in_game / loading_screen / main_menu / dlc | 四区 | 游戏本体的四个区，mod 按区镜像 | `guides\game-layout.md` |
| `localization\<lang>\` | 本地化目录 | 主语言文件；任务链另有 `missions\` 子目录 | `guides\localization.md` |
| `main_menu\common\defines\` | 常量表 | `guides\defines.md` 是它的索引 | `guides\defines.md` |
| `l_simp_chinese:` | 语言头 | yml 首行；键须在语言头下且缩进 | `guides\localization.md` |
| BOM（`EF BB BF`） | 字节顺序标记 | **mod 的 yml 必须带**；无 BOM 整文件被忽略 | `pitfalls.md` §五 |
| `namespace` | 命名空间 | 事件 ID 形式 `<ns>.<数字>`，数字 1–9999 全局唯一 | `guides\event-making.md` 文件级规则 |
| loc 键 | 本地化键 | 名称即键；漏一个显示 raw key | `guides\localization.md` |
| raw key | 原始键名 | 本地化缺失时界面直接显示键名 | `pitfalls.md` §五 |
| `$…$` | 脚本参数 | `$PRICE$` / `$MONTHS|0$`（`|0` 取整），必须配对 | `guides\localization.md` |
| `[Root.GetName]` / `[country|e]` | GUI 表达式 | `|e` 是概念链接；照抄原版 yml 写法 | `guides\localization.md` |
| `game_concept_*` | 概念词条 | 中文名在 `game_concepts_l_simp_chinese.yml` | `vanilla\vanilla-map-and-geography.md` 中文检索键 |
| `scripted_tests` | 游戏内测试 | 用游戏自带测试验证效果是否生效 | `guides\testing.md` |
| `error.log` / `Script location` | 报错日志 | 带 `Script location` 的那行才能定位文件与行号 | `tools\error-log-decoder.md` |

## 待核实

库内找不到明确中文名证据、或库内记录与所引证据不一致的词。**中文列一律写 `—` 或标注`⚠`**。

| 词 | 缺什么证据 |
| --- | --- |
| `market_attraction` | 库内只在中文检索键里出现英文键名；概念键为**市场吸引力**（`game_concept_market_attraction`），正文尚未记中文名 |
| `cultural_view` / `cultural_opinion` | 库内术语对照的键名写 `cultural_view`，概念词条是 `cultural_opinion`（文化好感）——**键名需核对** |
| `weather_system` | 库内只给出内部名与机制说明，未记录中文译名，也未引对应概念键 |
| `presence` / `resistance` / `stagnation` | 库内只给出内部名与"存在度/抵抗/停滞"的说明，未见对应 `game_concept_*` 键 |
| `r0` / `mortality_rate` | 库内只给出内部名与"基本传染数/死亡率"的说明，未见对应概念键 |
| `NCharacter` / `NCombat` / `NPop` 等 | 库内只给出常量段名（N+系统名）与行号，未解释该命名约定本身 |
| `黄金` 的中文名 | 库内多处写"金/gold"（如"gold 250"），未记游戏内货币中文名（概念键为**杜卡特**） |
| `政治权力` / `正义` / `业力` | 库内在多篇里作代价货币使用，但未集中记录其概念键与中文名 |

## 已订正（2026-09，按本体概念键逐词核对）

**库内旧译与游戏本体概念表（`main_menu\localization\simp_chinese\game_concepts_l_simp_chinese.yml`）不一致**，已按本体订正正文与术语表：

| 内部名 | 旧译（库内） | 现用（本体概念键） | 影响文档 |
| --- | --- | --- | --- |
| `merchant_power` | 商人力量 | **贸易优势** | 贸易篇 / 殖民探索篇 / 法律阶层篇 / 附庸类型档 |
| `merchant_capacity` | 商人容量 | **贸易容量** | 贸易篇 / 法律阶层篇 / 天命篇 |
| `market_access` | 市场准入 | **市场接入度** | 贸易篇 / 生产建筑篇 |
| `protectionism` | 保护主义 | **市场保护**（条约开关 → "取消市场保护"） | 贸易篇 / 外交篇 |
| `maritime_presence` | 海事存在 | **海上存在** | 贸易篇 / 地图地理篇 |
| `stockpile` | 库存 | **商品储备** | 贸易篇 |
| `sound_toll` | 通行费 | **海峡通行费** | 贸易篇 / 地图地理篇 |
| `war_exhaustion` | 厌战 | **厌战度** | 战斗篇 / AI 篇 / defines 篇 |
| `estate_opinions` | 阶层观感 | **阶层外交倾向** | 法律阶层篇 |

> 另有两条同义不同译、**未改正文**（不是错译，只是与概念键用词不同）：`disease` 概念键为**疫病**（库内多写"疾病"）、`disease_outbreak` 为**疫病爆发**（库内多写"疫情/爆发"）；`establishment` 概念键为**投产度**（库内已用对）。
