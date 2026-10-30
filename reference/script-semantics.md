# 参考资料：脚本语义（触发器 / 效果 / 作用域怎么写）

> **来源**：[EU5 Wiki: Trigger](https://eu5.paradoxwikis.com/Trigger)（页标 **1.1**）、[Scope](https://eu5.paradoxwikis.com/Scope)（**pre-release**）、[Effect](https://eu5.paradoxwikis.com/Effect)（**1.2**），`action=raw` 抓取 2026-10-30。
> **性质**：外部资料（wiki 语义说明）；**页面版本落后于 1.4.5**，未逐条实测。与实测冲突时以 `<KB>\fields\` / 实跑为准。
> **与生成表的分工**：本档讲**怎么写/怎么读**；**有哪些**看 `reference\triggers.md` / `effects.md` / `scopes.md`（由游戏内 `script_docs` 生成，版本匹配）。
> **体量**：约 8 分钟

## 一、基本形制
- **两种形态**：**inline**（只接一个目标：scope link / script value / 对象键）与 **block**（接多个目标，如 scope link + script value）。
- **必须有 scope**：能跨 scope 用的标 `none/any`；否则只在对应 scope 里有效；有些 trigger/effect **会改变当前 scope**。
- trigger 读状态、effect 改状态；**同一套 scope 体系**。

## 二、比较运算符（trigger）
| 运算符 | 含义 | 在 `NOT` 里的反转含义 |
|---|---|---|
| `<` / `<=` | 严格小于 / 小于等于 | 大于等于 / 严格大于 |
| `=` / `!=` | 严格等于 / 不等于（可用于非数值） | 不等 / 等于 |
| `>` / `>=` | 严格大于 / 大于等于 | 小于等于 / 严格小于 |

- **`NOT = { }` 内的比较按反转义理解**。
- `=` 也用于非比较场合（赋值/语法要求），不要一律当比较。
- **value trigger**：文档里带 `Traits: <, <=, =, !=, >, >=` 的触发器可当"取值的 scope link"用（可放在 dot 链末尾参与比较）。

## 三、迭代器（list）——最容易混的一块
| 前缀 | 类型 | 语义 / 参数 |
|---|---|---|
| `any_` | **trigger** | 任一返回 scope 满足即真；`count`/`percent` 要求达到数量/比例；`count = all` 或 `percent = 1` = 全部满足；可加 `filter = { }` 限定参与判定的 scope |
| `every_` | effect | 对**所有**返回 scope 执行；`limit = { }` 收窄 |
| `ordered_` | effect | 按 `order_by = 值` 排序后取第 1 个；`position = int`（0 起）换位；`min`/`max` 限范围；`check_range_bounds = no` 关掉"范围越界"报错 |
| `random_` | effect | 对**一个随机**返回 scope 执行；`limit` 收窄；`weight = { … }` 加权 |

- ⚠️ **any_ 取反的坑（⚠️ 未实测，wiki 说法）**：`NOT = { any_x = { … } }` 默认等价于 `count = all`（即"所有 x 都不满足"才为真）；**若再显式写 `count = all`，语义又翻回来**（"存在一个不满足"）。写取反时别叠加 `count = all`；**原版无直接反例可静态验证**。
- `ordered_` 的 `min` 默认 0、`max` 默认 1；只给 `min` 时 `max` 视为列表长度。
- **取多个随机 scope**：① 多次 `random_` + 给已选中的 scope `set_variable = blocker_var`，下次 `limit = { NOT = { has_variable = blocker_var } }`；② 单个 `ordered_` + 用 `modulo` 造半随机排序（示例：`order_by = { value = total_population modulo = { 1 10 } } max = 10`），无需变量且不重复。

## 四、scope 与链式写法
- **base scope** = `root`；可另存 **saved scope**（`save_scope_as = 名`）→ 用 `scope:名` 引用。
- **dot 链式（scope stacking）**：`any_country = { capital.region = region:xxx }` 等价于嵌套两层。
- ⚠️ **PREV 语义会变（⚠️ 未实测，wiki 说法）**：链式写法里 `PREV` = **链开始前的 scope**（上例是 country），嵌套写法里 `PREV` = 上一层的 scope（上例是 ruler）——同一个 `PREV` 结果不同。原版 `PREV` 1,491 处 / `prev.` 268 处只能证明两种写法都存在，**不能证明语义差异**。
- **Event target**：contextual（依赖当前 scope，如 `capital`/`ruler`）或 specified（`类型:键`，如 `building_type:xxx`）；可作效果/触发器的目标。
- **scope 类型约 110+ 种**，完整清单见 `reference\scopes.md`（生成版 143 项，含值类型）。
- **能存变量的 scope（15 种，⚠️ 未验证，wiki 说法）**：cabinet / character / colonial_charter / country / culture / disaster / dynasty / international_organization / location / province / rebel / religion / active_situation / unit / war。原版 `set_variable` 写在事件/效果里，**静态无法按 scope 归属**；只能确认 `situations`(105)/`disasters`(64)/`IO`(32) 里有用法。

## 五、存在性：`?=` 运算符（关键防御写法）
scope 不存在时照常执行/检查会产生未定义行为，应先确认存在（`exists`、`has_capital` 等）。
- **trigger 里**：`scope:target ?= { gold >= 100 }` ≡ `AND = { exists = scope:target scope:target = { gold >= 100 } }`
- **比较里**：`ruler ?= scope:target` **只检查左侧**——`ruler` 不存在 → false；`scope:target` 不存在 → **报错**。两侧都查：`ruler ?= { scope:target ?= this }`。
- **effect 里**：`scope:target ?= { add_gold = 100 }` ≡ `if = { limit = { exists = scope:target } scope:target = { add_gold = 100 } }`。

## 六、effect 的 flow / log / variable 族
- 生成表里 effect 分**迭代器 / flow（条件与循环、tooltip）/ log（调试输出）/ variable** 四组，均可在任意 scope 使用；
- 具体名称见 `reference\effects.md`（1,662 条），**用 `| ` + 关键字 grep**：`if` / `random_list` / `while` / `tooltip` / `hidden_effect` / `log` 等。

## 七、常见坑清单
1. **scope 不匹配**：location 修正写进 country、character 效果写在 country——不报错但无效/未定义。
2. **`NOT` 里的比较要反转**（见 §二）。
3. **any_ 取反 + count = all 叠加**，语义翻回（见 §三）。
4. **scope 栈 PREV 语义漂移**（见 §四）——改链式写法时先核对 `PREV`。
5. **不判存在就直接操作**（见 §五）——养成 `exists` / `?=` 习惯。
6. **`=` 被当作比较**误读——非比较处的 `=` 只是赋值/语法。
## 八、与原版脚本对照（2026-10-30 静态核对）

> 方法：对 `game\in_game` + `game\main_menu` 共 3,559 个 `.txt` 扫描关键字，统计原版实际用法。

| 语义点 | 原版用量 | 结论 |
|---|---|---|
| `limit = {` | 18,931 | 常用 |
| `any_` / `every_` / `ordered_` / `random_` | 6,293 / 4,442 / 1,112 / 4,350 | 四类迭代器都在用 |
| `count =`（含 `all`） | 132 | 存在 |
| `percent =` | 22 | 存在 |
| `filter = {` | **1** | 存在（`formable_countries\00_formable_countries.txt:406`：`any_international_organizations_member_of` + `count = all` + `filter = { … }`）→ 与 wiki"filter 配合 count/percent"一致 |
| `weight = {`（random_ 加权） | 429 | 存在 |
| `order_by =` | 1,099 | 常用 |
| `position =` | 65 | 存在 |
| `check_range_bounds = no` | **581** | 常用（说明 min/max 越界报错是真问题） |
| `while = {` | 76 | 存在 |
| `random_list = {` | 626 | 存在 |
| `?=` | **13,834** | 原版主流写法（存在性防御） |
| `PREV` / `prev.` | 1,491 / 268 | 两种写法都有（大写常量、小写点链前缀） |
| `root` / `this` | 13,535 / 8,036 | 基础 scope 大量使用 |
| event target `类型:键` | 72,720（另有 `(类型:键)` 1,972） | 冒号形式确证 |

**scope 名单：wiki 落后，引擎为准**
- 引擎 `scopes.md` 生成 **143 种**；wiki 页只列 **120 种**；另有 **33 种** wiki 未列（多为 1.4.x/DLC 新增，如 `religious_order_*`、`calling`）。
- wiki 那 10 个"对不上"的名字已**逐条分类核实**（改名 / 非 scope / 上下文名 / 废弃）→ 见 **`reference\scope-aliases.md`**（差异台账，附原版用量证据）。
- **结论**：写脚本以 `reference\scopes.md` 为准；wiki 旧名只作搜索别名，先查台账。

**未能静态验证的 3 条**（保留为"wiki 说法"）：
1. `NOT = { any_x = { … } }` 默认等价 `count = all`（引擎语义，静态不可判）；
2. dot 链式下 `PREV` 语义变化；
3. "能存变量的 scope 15 种"——`set_variable` 出现在事件/效果里，**在定义文件里查不到归属**，无法静态核对。
