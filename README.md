# EU5 Modding 知识库（eu5-modding）

> **这是什么**：EU5（Europa Universalis V / jomini 引擎）模组**制作 + 审查**的中文知识库——16.2 万字、151 篇文档，全部基于游戏本体文件（EU5 1.3.x）实查，**不凭 EU4 经验推测**（EU5 与 EU4 脚本体系不通用）。
>
> **怎么用**：
> - **有 AI agent**（Claude Code / DSH / Codex…）：`git clone https://github.com/mrjiaobai-glitch/eu5-modding-kb.git` → 把 `skills\eu5-mod-review\` 复制进技能目录 → 直接说"按这个库给我写 / 审 EU5 mod"（技能自动定位本库，见 `skills\README.md`）
> - **查资料**：从下方「目录结构」进对应部分——`fields\` 字段库（本体 74 份 readme 逐条提炼）/ `vanilla\` 原版机制解析（哪些能改、哪些引擎硬编码）/ `guides\` 制作指南 / `pitfalls.md` 实战坑速查
> - **不用 AI**：`fields\` 101 档本身就是本体 readme 的中文逐条提炼，可直接当速查手册
>
> **规模**：151 篇 · 1,390 KB · 18,015 行 · 162,273 汉字 · 5,168 行表格 · 294 个代码块 · MIT

> **定位**：Europa Universalis V（jomini 引擎）模组制作与审查的**知识总库**——独立于任何技能，可被技能、对话或其他知识库引用。
> **本仓库**：本仓库即该知识库本身；配套 agent 技能随库附带于 `skills\eu5-mod-review\`（安装见 `skills\README.md`）。
> **来源**：74 份游戏本体 readme 权威提炼 + 制作层实查 + 机制逆向解析 + 实战记录。只记录本体实际声明的内容；未声明的字段一律标注"未在 readme 中说明"，**禁止凭 EU4 经验补充**。
> **路径基准**：本 README 中所有文档路径均相对于本目录（本仓库根 `eu5-modding-kb\`）。

## 目录结构

```
eu5-modding-kb\
├── README.md          人类入口：定位 + 常见任务速查（本文件）
├── START-HERE.md      新手路线与按任务导览（给人看的）
├── glossary.md        术语总表（中英对照 + 常见误译）
├── INDEX.md           全量索引（全部文档的逐档映射表）
├── fields\            字段库：74 份 readme 提炼 + 实查补缺的类目字段权威（101 档）
├── guides\            制作指南：骨架/合并/事件/脚本/法律/本地化/测试 + **E2E 教程（新国家 / 新任务链）**（12 档）
├── vanilla\           原版机制解析：天灾人祸与自然环境/地图地理/外交/战斗与战争/POP/天命/科技时代/贸易市场/生产建筑/文化与宗教/法律阶层议会/灾难局势/国际组织IO/角色王朝内阁/AI/政体改革官僚/殖民与探索/事件与任务/DLC与美术资源/纹章与旗帜/音频与字体/开局数据/界面GUI（23 档）
├── cases\             实战案例：真实 mod 的实测记录（2 档）
├── tools\             查表工具：ID 族总表 / loc 键前缀全表 / 审查清单 / error.log 解码表 / 本库数字体检（5 档）
├── pitfalls.md        实战坑速查（EU4→EU5 语法对照等，最高频查阅）
└── skills\            附带技能：eu5-mod-review（SKILL.md + references\README.md）
```

## 从这里开始（按你要做的事找）

| 我想…… | 先看这几篇 | 备注 |
| --- | --- | --- |
| 跑通第一个能进游戏的改动 | `guides\mod-skeleton.md` → `guides\game-layout.md` → `guides\localization.md` → `guides\testing.md` | 骨架 → 放哪 → 中文 BOM → 验证，四步闭环 |
| 写一个事件 | `guides\event-making.md` → `fields\common-events.md` → `tools\loc-keys.md` | 权威格式 + 字段 + 本地化键 |
| 加一条法律／政策 | `guides\law-design.md` → `fields\common-laws.md` | **必做撞名检查**（`law-design.md` 第九节） |
| 加建筑／生产方式 | `fields\common-building_types.md`、`fields\common-production_methods.md` → `vanilla\vanilla-production-and-buildings.md` | 先看生产篇认清 RGO 与建筑的接口 |
| 做新国家 / 新任务链 | `guides\new-country-tutorial.md` / `guides\new-mission-pack-tutorial.md` | 两篇端到端教程，从零到进游戏 |
| 改地图 / 开局数据 | `vanilla\vanilla-map-and-geography.md`、`vanilla\vanilla-setup-data.md` | 三层开局数据（国家定义 / 模板 / start） |
| 审查或修自己的 mod | `tools\review-checklist.md` → `tools\error-log-decoder.md` → `pitfalls.md` | 清单化自检 + 报错归属判断 |
| 某机制到底能不能改 | `vanilla\`（23 篇机制解析，篇末都标了可改点与硬编码边界） | 例如天气风暴实际效果是硬编码 |
| 查某个脚本类目的字段 | `guides\systems-map.md` → `fields\` 对应档 | 类目地图先定位入口文件与 readme |

> 更细的路线（零基础按顺序读哪几篇、每步耗时、常见坑）见 **`START-HERE.md`**；术语与常见误译见 **`glossary.md`**。
> **全量索引**（全部文档的逐档映射表）见 **`INDEX.md`**。

## 用法

- **写 mod 某类目** → 先查 `guides\systems-map.md` 定位入口文件与 readme，再加载 `fields\` 对应字段文档核对字段，最后照本体样例写
- **审查 mod** → 配合 `eu5-mod-review` 技能的检查清单（本库是它的知识层）；引用类 ID 核对见 `tools\audit-ids.md`
- **改机制** → 先读 `vanilla\` 对应篇（搞清文件组成/数值位置/可改点/硬编码边界）
- **踩坑查询** → 先翻 `pitfalls.md` 与 `cases\`

---

## 铁律

1. **一切以游戏本体为准**：写任何词条前先 grep 游戏本体确认存在/用法，禁止凭 EU4 记忆补。
2. **EU5 ≠ EU4**：无 ROOT/PREV、无 event_target、无 `KEY:0`、选项无 weight、作用域词 root/prev/this。
3. **字段不确定 → 查 readme**：73 个 `readme.txt` + `_script_values.info` + `on_actions.info` + `_game_rules.info` 是官方权威说明，都在游戏本体里。
4. **命名先查撞名**：新增法律／政策／建筑／改革前，**显示名与键名都**对照本体 `main_menu\localization\simp_chinese\` 查一遍。通用（无条件解锁）内容禁汉典专名、禁现代公文构词、禁文明专名——详见 `guides\law-design.md` 第八、九节。
5. **数值先查分布**：新增修正值前先抽取本体该修正的取值分布、取众数、吸附到实际出现过的精确值，禁止凭印象写数——详见 `pitfalls.md` 第十节。
6. 制作完成后用 `eu5-mod-review` 技能的审查流程自检（技能入口：本仓库 `skills\eu5-mod-review\SKILL.md`）。
