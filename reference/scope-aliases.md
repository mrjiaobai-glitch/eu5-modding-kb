# 参考资料：wiki 与引擎的 scope 名差异台账

> **用途**：拿 wiki 旧名去搜脚本/查文档时，先在这里对一下**实际叫什么、还是不是 scope**，避免照 wiki 名字写出无效脚本。
> **权威**：以 `reference\scopes.md`（`script_docs` 生成，EU5 1.4.5，143 种）为准；本表只记录**wiki 与引擎不一致的条目**及证据。
> **来源**：[EU5 Wiki: Scope](https://eu5.paradoxwikis.com/Scope)（pre-release）vs 引擎 `event_scopes.log`；证据为对 `game\in_game` + `game\dlc` 的静态扫描（2026-10-30）。
> **体量**：3 分钟

## 一、处置分类

| 分类 | 含义 | 处理 |
|---|---|---|
| **改名/旧拼写** | 同一个东西，wiki 用了旧名或拼错 | 文档里写引擎名，保留 wiki 名作**别名**便于搜索 |
| **非 scope** | wiki 把它列进 scope，但它其实是**事件目标键 / 数据块键 / 字段值** | 不当 scope 用；在 `fields\` 对应类目里查 |
| **缺文档** | 引擎在特定上下文提供该 scope 名，但 `event_scopes.log` 没列 | 以脚本实际用法为准，标注"上下文 scope" |
| **无使用** | 引擎表/脚本都查不到 | 视为废弃，不采用 |

## 二、差异明细

| wiki 名 | 引擎/实际 | 分类 | 证据（原版用量） |
|---|---|---|---|
| `any/none` | `none/any` | 记法 | 脚本里 `any/none`、`none/any` 均 **0** 处——纯文档记法，不是脚本 token |
| `bool` | `boolean` | 改名/旧拼写 | `boolean` **931** 处；`bool` 仅 14 处（零散旧写法） |
| `integer_flag` | `flag` | 改名 | `integer_flag` **0** 处；引擎 scope 表里是 `flag` |
| `subunit` | `sub_unit` | 旧拼写 | `sub_unit` **16** 处 vs `subunit` **6** 处 |
| `land_ownerhip_rule` | `land_ownership_rule` | **wiki 拼写错误** | 引擎表 `land_ownership_rule` |
| `rebel` | 引擎 scope 类型 = `rebels`；`scope:rebel` 是 **on_action 上下文提供的名字** | 改名 + 上下文 | `scope:rebel` **9** 处，全在 `in_game\common\on_action\_hardcoded.txt`（注释：`scope:target = rebel country`） |
| `religion_group` | 是**事件目标键**（`religion_group:christian`），不是 scope；scope 类型用 `group` | **非 scope** | `religion_group:` **1,181** 处，全部是 `键:值` 形式 |
| `government_type` | 是**事件目标键**（`government_type = government_type:tribe`） | **非 scope** | 1,316 处均为 `government_type = government_type:X` |
| `goods_demand` | 是**数据块键**（`goods_demand = { … }`） | **非 scope** | `goods_demand = {` **7** 处，在 `common\age` 等定义里 |
| `cardinal` | 是 `religious_figure:cardinal` 的**类型值** / 建筑字段；无 `scope:cardinal` | **非 scope** | `scope:cardinal` **0**；`cardinal = {` / `create_cardinal` **29** 处 |

## 三、引擎有、wiki 没列的 33 种
`ai_personality`、`ambition`、`ambition_definition`、`bureaucracy`、`bureaucracy_type`、`calling`、`chivalric_order`、`colonial_charter_goal`、`demand`、`expedition`、`expedition_type`、`government`、`group`、`imperial_circle`、`industry_promotion`、`industry_type`、`invalid`、`land_ownership_rule`、`movement`、`movement_definition`、`none/any`、`omen`、`rebels`、`religious_order`、`religious_order_type`、`scripted_geography`、`sub_unit`、`town_rights`、`town_rights_type`、`unit_formation_preference`、`unit_template`、`boolean`、`flag`。

> 多数与 1.4.x / DLC 新增相关（如 `religious_order_*`、`calling` 对应 D009）；wiki 未更新。**用 `scopes.md` 查**。

## 四、长期规则
1. **引擎生成物优先**（`scopes.md` / `triggers.md` / `effects.md`）；wiki 名字只作**别名**。
2. 发现新的不一致 → **加一行到本表**（附证据：路径:行 或用量），再改文档。
3. wiki 页更新后重跑一次比对（本表日期 + 引擎版本号），过期条目删掉。
4. 本表**不收录**"wiki 与引擎都同意"的名字，避免膨胀。
