# 从这里开始（START HERE）

> **这是什么**：给**人类**的上手路线。本库有 148 篇文档，别从索引表刷起——按下面的路线走。
> **三条路**：① 从没做过 mod → 读第一节，按顺序走；② 已经有具体任务 → 直接查第二节的表；③ 只是遇到不认识的词 → 翻 `glossary.md`。
> **给 agent**：不用读本文件——直接读 `README.md` 的常见任务速查与 `INDEX.md` 的全量映射，更省 token。

## 一、零基础路线（约 2 小时，按顺序读）

| # | 读什么 | 大概耗时 | 读完你能做到 |
| --- | --- | --- | --- |
| 1 | `guides\game-layout.md` | 10 分钟 | 知道 EU5 的四个区（`in_game` / `loading_screen` / `main_menu` / `dlc`）、mod 该放哪、同名文件怎么合并 |
| 2 | `guides\mod-skeleton.md` | 10 分钟 | 建出一个 launcher 认得的空 mod（`.metadata\metadata.json` 注册） |
| 3 | `guides\merging.md` | 10 分钟 | 知道什么时候用 INJECT、什么时候必须 REPLACE、为什么顺序敏感的块不能 INJECT |
| 4 | `guides\event-making.md` + `fields\common-events.md` | 30 分钟 | 写出一个能进游戏、标题描述都正常显示的事件 |
| 5 | `guides\localization.md` | 10 分钟 | 知道 EU5 本地化**必须 UTF-8 BOM**、键怎么起名、为什么要双镜像 |
| 6 | `guides\testing.md` + `tools\error-log-decoder.md` | 20 分钟 | 读懂 `error.log`，并把报错定位到自己的文件与行号 |
| 7 | `pitfalls.md` | 30 分钟 | 把高频坑先过一遍（EU4→EU5 差异、变量、合并、灾难重复） |

走完这 7 步，再照 `guides\new-country-tutorial.md`（做一个新国家）或 `guides\new-mission-pack-tutorial.md`（做一条任务链）走一遍，"从零到进游戏"就闭环了。

## 二、按任务查

| 我想…… | 按这个顺序读 | 关键点 / 最容易踩的坑 |
| --- | --- | --- |
| 写一个事件 | `guides\event-making.md` → `fields\common-events.md` → `tools\loc-keys.md` | `namespace` 必须定义、ID 1–9999 全局唯一；`title`/`desc` 键是 `<ns>.<id>.title` 这种形式；事件的 `type` 五选一 |
| 加一条法律／政策 | `guides\law-design.md` → `fields\common-laws.md` | **解锁链三处必须同步**（漏一处静默失效）；`potential` 只放几乎不变的条件（引擎会周期复查，失效即整条撤销）；新增前**显示名与键名都要查撞名** |
| 加一个建筑／生产方式 | `fields\common-building_types.md`、`fields\common-production_methods.md` → `vanilla\vanilla-production-and-buildings.md` | 先认清原产（RGO）与建筑的接口：原产**不需要建筑**，制成品才走 `production_methods` |
| 加／改科技与思潮 | `fields\common-advances.md` → `vanilla\vanilla-tech-and-age.md` → `guides\defines.md` | `advance` 是**革新**、`institution` 是**思潮**——不是"科技/制度" |
| 做一个新国家 | `guides\new-country-tutorial.md` → `vanilla\vanilla-setup-data.md` → `fields\setup-countries.md` | 开局数据分三层：国家定义 / 开局模板（**首选**，`include = "<模板名>"`）/ `setup\start` |
| 做一条任务链 | `guides\new-mission-pack-tutorial.md` → `fields\common-missions.md` → `vanilla\vanilla-events-and-missions.md` | 本地化键数量固定：链 5 个、节点 3 个 |
| 改地图 / 开局人口 | `vanilla\vanilla-map-and-geography.md` → `vanilla\vanilla-setup-data.md` | 换地图位图时 `locations.png` 与 `named_locations\00_default.txt` 的键必须一致，否则地点认不出来 |
| 调数值平衡 | `guides\defines.md` → `vanilla\` 对应篇 → `pitfalls.md` 第十节 | 新增修正值前先抽本体取值分布、取众数、吸附到**实际出现过的精确值**，别凭印象写 |
| 审查自己的 mod | `tools\review-checklist.md` → `tools\error-log-decoder.md` → `pitfalls.md` | 按"错误 / 警告 / 建议 / 存疑"分级出报告；引用类 ID 逐个核对存在性见 `tools\audit-ids.md` |
| 排错（崩溃 / 报错 / 中文乱码） | `tools\error-log-decoder.md` → `guides\testing.md` | 先判断报错归属（原版还是自己），`error.log` 里带 `Script location:` 的那行才是定位 |
| 搞清某机制能不能改 | `vanilla\` 对应篇（23 篇，篇末都标了"可改 vs 硬编码"）→ `guides\defines.md` | 例：天气的风暴实际效果是硬编码，只能改生成与挂钩点 |
| 改本地化 / 修乱码 | `guides\localization.md` → `tools\loc-keys.md` | BOM 是 EU5 特有的硬要求；键必须缩进在语言头下；放 `main_menu\` 或 `in_game\` 都行（**不必互为镜像**） |

## 三、按你的角色

- **只想改数值的玩家**：`guides\defines.md`（常量索引）→ `pitfalls.md` 第十节（数值分布查法）→ `vanilla\` 对应篇确认这个值是不是硬编码
- **要做内容（事件 / 法律 / 建筑 / 任务 / 国家）**：第二节的表，每条都是一条最短路径；两篇 E2E 教程是最好的起点
- **要审查或排错**：`tools\review-checklist.md`（清单化自检）+ `tools\error-log-decoder.md`（报错归属）+ `cases\`（别人踩过的实测坑）
- **想搞懂引擎怎么运作**：`vanilla\` 23 篇——每篇讲清一个系统由哪些文件组成、关键常量在哪、哪些能改、哪些写死在引擎里

## 四、新手最容易翻车的 5 件事

1. **本地化文件没有 BOM** —— EU5 要求 UTF-8 **BOM**，没有 BOM 整个文件被忽略或显示乱码（`guides\localization.md`）
2. **新增内容撞名** —— 法律／政策／建筑／改革的**显示名与键名都要**对照本体 `main_menu\localization\simp_chinese\` 查一遍，只查显示名会覆盖本体本地化（`guides\law-design.md` 第九节）
3. **凭 EU4 记忆写** —— EU5 换了 jomini 引擎：没有 `ROOT/PREV`、没有 `event_target`、没有 `KEY:0`、事件选项没有 EU4 式 `weight`（`pitfalls.md` + 本库"铁律"）
4. **在顺序敏感的块上用 INJECT** —— 追加会排到 fallback 之后轮不到，这类块（如 `levies` 特化单位、`country_name_construction`）只能整体覆盖文件（`guides\merging.md`）
5. **事件不写 `namespace` 或 ID 撞车** —— ID 必须 `namespace.integer` 且全局唯一；改原版事件要用新文件 + 原版 namespace 复制修改（`guides\event-making.md`）

## 五、这个库为什么"长这样"

本库是**给 agent 与检索用的高密度参考**，不是教程体：文档里大量是表格与速记式表达（字段名 / 取值 / 作用域 / 坑），为的是让 AI 一次加载就能核对字段，而不是从头读到尾。

人类读者的正确打开方式是：

- **按任务跳**（上面的表），不要顺序通读 `fields\` 与 `vanilla\`
- 每篇 `fields\` 文档末尾通常有「审查要点」，那是最值得看的几行
- 想找某个词先查 `glossary.md`；想找某个类目先查 `guides\systems-map.md`
- 全量索引在 `INDEX.md`（148 篇逐档映射），需要"按图索骥"时再翻
