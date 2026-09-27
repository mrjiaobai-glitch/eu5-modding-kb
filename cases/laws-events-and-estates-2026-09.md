# 法律/改革撤销 · 事件自检 · 阶层力量（EU5 1.3.x 实测）

来源：《刀锋与王座》2026-09 复核轮（修 BOM/乱码轮 + 全量静态自检 + 设计复核）。
全部为**可跨 mod 复用的引擎事实**，非该 mod 专有内容。字段权威仍以 readme 提炼为准，
本文件补充 readme 未说明的引擎行为。与 `blades-and-thrones-2026-08.md` 配套。

---

## 1. 法律/改革的 potential 会被引擎周期复查，失效即整条撤销 ⚠️ 高危

**机制**：引擎**周期性复查** `law` / `government_reform` 的 `potential`；一旦失效，
**整条法律连同已选中的政策一起被自动撤销**（不是隐藏、不是保留，是移除）。

**实测事故**：军制法律的 `potential` 里写了 `estate_power(estate_type:士兵阶层) > 0`。
而 EU5 **军队即 soldiers POP**——所以"军队全灭 / 整军遣散 / 组建军团转移全部部队 /
出租整军"都会让这个阶层力量归零，等价于"国家没有任何军队"：

> 法律消失 → 重建军队后法律回来 → 再选 → 再被撤 → 玩家观感 = **"法律反复被自动撤销"**

**修复手法**（最小改法）：
```
potential = {
    OR = {
        "estate_power(estate_type:xxx)" > 0
        has_policy = <本法律的任一政策>     # 已持有政策的国家，法律永久有效
    }
}
```
语义：已生效的内容保留，门控只挡"从未选过 + 无阶层"的纯平民国家。
也可以**直接删掉 law 级 potential**（原版 `army_doctrine_law` 就没有）。

### 铁律

**law / reform 的 `potential` 只放"几乎不变"的条件** —— 政体、宗教、改革、tag。
**绝不放会随局势波动的动态值**（兵力、阶层力量、金币、战争状态）。

- "**想隐藏 UI**" → 用 `potential`
- "**想防止被撤销**" → 必须保证**已生效后 potential 恒真**

证例：原版 `feudal_de_jure_law` 等以 `government_type` 为 potential——政体一变法律即消失，
证明"引擎自动撤销"是标准行为而非 bug。

### 附带噪音：AI 政策频繁切换

`AI_PERFORMANCE_POLICIES_MONTHS_BETWEEN_UPDATES`（defines）= AI 每 N 月按
`wants_this_policy_bias` 重选政策。若几个政策的 bias **值域重叠且随战争/收入波动**，
AI 会频繁换政策，观感上像"反复改法/撤销"。
压制法：给 bias 加"当前政策惯性"（`if has_policy = <当前> add 大值`）或拉开差异化。

---

## 2. 阶层力量（estate power）：两个反直觉特性

### 2.1 阶层力量是**存量**概念，不是"该职业的政治地位"

`power_per_pop` 是"每个 POP 折算多少政治分量"。推论：

- **贵族/市民/农民的 power 随人口增长**；
- **士兵/常备军的 power 只随军队规模增长**（EU5 军队即 soldiers POP）。

⇒ 有地国家越大，纯军事阶层的**相对**分量天然被稀释。做长期机制时，
"军事阶层越来越弱"是结构趋势，不是数值没调好。

### 2.2 给"占满全国人口的阶层"设高 `power_per_pop` 会自相矛盾

`power_per_pop` 的设计意图是表达"这个职业在**众多社会集团中**的政治权重"。
若某阶层**就是全国人口**（无地军队国的士兵阶层 = 全军 + 全部 POP），
它的力量会被推到极值（→1），并因阶层 `rival` 关系把**王室力量挤压到 0**。

用《刀锋与王座》的忠诚公式验算（系数见其平衡表）：
`忠诚 = 0.5 + (满意度-0.5)×0.3 + 士兵力量×0.1 + 王室力量×0.2 + ...`
→ `×0.1` 项拿满、`×0.2` 项归零 = **忠诚恒高**。

**这与"某阶层力量上升 = 政治风险"的设计意图方向相反**——军队吞掉国家，
反而成了忠诚的保证。

### 设计结论（可复用）

给**无地军队国 / 军政合一实体**设计时，**不要**用"职业阶层"模型套整国人口：

- 该实体里**不存在**"军队 vs 王室"的可分离对立面（谁政变谁？统治者就是军队）；
  政变/哗变/军政府这类机制的前提不成立；
- 应把军队阶层**当人口基数（兵力/人力/征召）**处理，而把政治张力**上移为政权内部派系**
  （老兵 vs 新兵、军官团 vs 士兵、军阀 vs 忠顺派）——人格/派系系统比阶层系统贴合；
- 阶层 `power_per_pop` 的高值**只对"众多集团之一"的实体有意义**。

### 顺带：招牌循环可能不适用

"征兵 → 战后遣散 → 阶层崛起 → 退伍安置 → 阶层回落"这个循环**要求土地与农民**
（遣散后转回农民）。无地国**没有土地、没有农民**，`change_pop_type = pop_type:peasants`
没有落点 ⇒ 循环退化，遣散/安置机制产出的只是"没有政治意义的人口数字"。
给无地形态做机制前先问一句：**这个循环在这个形态里成立吗？**

---

## 3. 事件触发路径的语义差（控制台 vs 脚本）

- **控制台 `event <id>` 是强制触发，跳过 `trigger`**；
- 事件级防重复（`trigger = { NOT { xxx_resolved = yes } }`）**只对脚本触发生效**；
- 游戏内路径（灾难 `on_start` 等）会检查 `trigger`；**脚本触发不满足 trigger 时运行 `on_trigger_fail`**
  （注意：**on_action 触发的失败不会运行 `on_trigger_fail`**，见 `common-events.md`）。

⇒ 所以"控制台还能重复触发"是**预期调试行为，不是 bug**。
⇒ 防重复要**双保险**：事件层 `trigger` 挡脚本路径 + 灾难层 `can_start` 挡游戏内路径。

### 选项预览会执行选项体副作用 ⚠️

EU5 悬停事件选项会**执行选项体的效果**（但**不执行 `set_variable`**）——
写在选项体（非 `hidden_effect`）的这些效果会**每次悬停重复执行**：

`create_character`、`set_new_ruler`、`trigger_event_*`、`add_pop` / `add_pop_size`、
`remove_reform` / `add_reform`、`change_country_type`、`set_personality`、`every_owned_location`

症状：预览卡顿 + 事件堆积导致无响应。
**修复：副作用效果一律包 `hidden_effect`**（预览不执行隐藏效果）。

推论（对 `add_pop` 特别重要）：**`add_pop` 的 `size` 不能依赖 actor 预置变量**——
UI 预览只跑效果体、不执行 `set_variable`，取到的是 0 或直接报错。
需要预置值时改在子单位层直接换算。

---

## 4. 事件自检清单（写/审事件必跑）

1. **括号平衡**：数每个事件文件的 `{` `}`，diff 必须为 0。
2. **ID 唯一**：`namespace.integer`（`0 < integer < 10000`），mod 内不重复。
3. **loc 完整性**：脚本引用的 **title / desc / 选项 name / `.tt` / historical_info** 键必须都在 yml 中（镜像两侧）。
4. **`outcome`** 取值 `positive` / `neutral` / `negative`（**默认 neutral**；这是音频方向）。
   ⚠️ 只有这三个是合法值——**不要**用 good/bad，也**不要**因为技能旧表述把 `negative` 判成错误。
5. **条件文本**用 `first_valid` + `triggered_desc`（**不是** `if/else_if`），最后一个无 `trigger` 兜底。
6. **`fire_only_once`** 只用于真一次性事件；循环事件用冷却变量（`set_variable` + `months = N`）。
7. **调试事件**：编号 `.99` + `orphan = yes` + `is_human` 门槛。
8. **复制原版事件文件里的 scripted effect 时必须一并复制或改名**，防止覆盖原版同名 effect。
9. **具名 scope** 不跨嵌套 effect 调用——被 effect 内部复用时开头防御性重存。

## 5. 审查脚本自身：可复用的静态复核

### 5.1 loc 键引用审计：区分"变量名"与"loc 键"

扫 `title|desc|name|custom_tooltip|historical_info = <标识符>` 时，会大量命中**变量名/脚本值名**，
造成假失败（实测一次 29 个"缺失键"**全是**变量与引擎词条）：

| 命中 | 真实身份 |
|---|---|
| `name = mercenary_mutiny_company_refusals` | 变量名（`name =` 在 set/change_variable 里） |
| `custom_tooltip = civil_war_game_over` | **原版 loc 键**（引擎词条）→ 在原版 yml 里，不在 mod yml |
| `desc = ..._military_crisis_value` | script_value 名 |
| `title = <变量名>` | 变量 |

⇒ **判定规则**：`<ns>.<数字>.xxx` 形式才是 mod 自有 loc 键；纯标识符优先当变量/脚本值/原版键，
并**回查原版 yml** 再断言缺失。

### 5.2 条件文本的 desc 不存在是**正确的**

像 `blades_and_thrones_loyalty.1` 这种用 `desc = { first_valid = { triggered_desc ... } }` 的事件，
**没有** `<id>.desc` 这个基键，只有 `.desc.critical` / `.desc.low` / `.desc.mid` / `.desc.base`。
审计脚本必须解析 `first_valid/triggered_desc` 结构，**不能只查 `<id>.desc`** ——否则全量假失败。

### 5.3 局部变量名会被当成 loc 键

`name = xxx` 在 `set_variable` / `create_character` / `select_trigger` / `casus_belli` 里
都是**变量/参数名**，不是 loc 键。审计前先按**所在块类型**过滤。

### 5.4 乱码检测与 BOM 复核（一次跑完全库）

```powershell
# BOM：读前 3 字节，EF BB BF 才算有
$b=[System.IO.File]::ReadAllBytes($f); $hasBom = ($b[0] -eq 0xEF -and $b[1] -eq 0xBB -and $b[2] -eq 0xBF)
# 乱码：双重编码残留常见字（可扩展）
'[\uFFFD]|闂|婵|濠|鐎|閸|缂|閻|鈧|鍕|濡'
```
**注意**：`Get-Content -Encoding Byte` 在 PowerShell 7 不可靠，
请用 `[System.IO.File]::ReadAllBytes` / `WriteAllText`。

**乱码可逆性判定**：先看有没有 `U+FFFD`（有 = 信息已丢）。
**没有 U+FFFD 也不代表可逆**——实测 222 个乱码字符无一个 U+FFFD，
但 UTF-8 → GBK 往返回环后产生**新的乱码**，说明信息仍已丢失，**只能重建不能还原**。
重建时按**代码语义**逐行重写注释，并核对注释描述的数值与 `script_values` 实际值一致。

### 5.5 改注释也要复核缩进

用脚本批量替换注释行时，若在脚本语言里写 `\t` 字面量而该语言**不解释转义**，
会写入**字面的反斜杠 + t** 而不是制表符（实测一次 23 行）。
替换后必须**抽查缩进**（把 tab 显形比对），并**数括号 / 验结构**确认没动到代码。
