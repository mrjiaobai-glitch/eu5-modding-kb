# 法律设计与落地（common/laws 制作层）

来源：2026-09《尼泊尔王公自用平衡》实修（EU5 1.3.11）+ `in_game\common\laws\` 全目录逐块实查。

**字段权威见 `common-laws.md`**（`laws\readme.txt` 提炼）。本文只写 **readme 没写、但实查确认** 的东西，以及"怎么设计一条法律"的方法。

---

## 一、readme 未声明但确认可用的字段

| 字段 | 位置 | 说明 |
|---|---|---|
| `law_category` | law 块 | UI 分类显示。**实机确认生效**：`law_category = administrative` 显示为「行政法」。**不需要 `<法律键>_CATEGORY` loc 键**（本体部分老法律有该键做覆盖，做新法律不必） |
| `estate_preferences` | policy 块 | 政策靠山（列阶层名）。readme 完全未提，但本体大量使用（`administrative_system`、`cabinet_laws`、`02_distribution_of_power.txt`） |
| `monthly_towards_<轴>` | policy 的 `country_modifier` 内 | 社会价值轴漂移；值习惯用 script value `societal_value_monthly_move` |
| `potential = { current_age_or_later = { age = age_N } }` | policy 块 | 按时代解锁**该条政策**（不是整条法律） |

本体 30 个法律文件里只有 **3 个**用到 `current_age_or_later`（`01_common.txt`、`03_estate_laws.txt`、`23_lordship_of_ireland.txt`），**没有任何法律在法律级用 `age =`** —— 时代门控是"可用但非主流"的写法。

## 二、本地化：名称即键，没有独立字段

- **法律块名** → `<块名>` + `<块名>_desc`
- **政策块名** → `<政策名>` + `<政策名>_desc`

漏一个就显示 raw key。`common-laws.md` 里写的"未在 readme 中说明：本地化键格式"，就是这条。

## 三、解锁链：三处必须同步，漏一处**静默失效**

`unlock_law_effect = { type = X }` 内部是 `set_variable = { name = unlocked_law_$type$ }`，变量名由 `$type$` 派生。所以改法律键名时要同步**三处**：

1. 法律块名（`in_game\common\laws\<文件>.txt`）
2. 同文件 `potential = { has_unlocked_law_trigger = { type = X } }`
3. **调用侧**——通常在**另一个文件**里，如 `government_reforms\<文件>.txt` 的 `on_activate = { unlock_law_effect = { type = X } }` ← **最容易漏**

只改一边 → 名字对不上 → **法律永远解不开，而且不报错**。

**双调用有本体依据**：一条效果的 `on_activate` 里可以连调多次 `unlock_law_effect`。参照 `in_game\common\formable_countries\00_formable_countries.txt:2546-2551`——拜占庭内容解锁时连调两次 `unlock_government_reform_effect` + 四次 `unlock_policy_effect`。（`unlock_law_effect` 与 `unlock_policy_effect` 在 `scripted_effects\country_effects.txt` 里结构完全相同，只差 `law:`/`policy:` 与变量前缀。）

> 附带一条本体 readme 的自相矛盾：readme 警告「`custom_description` 的 `text` 键不可与 effect 键同名」，而 `unlock_law_effect` **正是这样定义的**。它是本体自身写法且被调用数百次，实际工作正常——该警告针对的是丢失 subject/object/value 拾取的场景。

## 四、政策的通行配方（三件套 + 时长）

```
<policy> = {
    country_modifier = { <收益...> <硬代价...> <monthly_towards_* = societal_value_monthly_move> }
    estate_preferences = { <阶层> }
    months = 1        # 或 years = 2（本体大法律主流）
}
```

本体大法律（`administrative_system`、`cabinet_laws`）几乎条条如此。

**大法律不塌缩的机制不是削弱数值，而是让每个选项 = 一个具体动作 + 一批支持者。** 反过来说：如果一条政策的身份只是它的价值观漂移方向（去掉 `monthly_towards_*` 就只剩一堆可互换的百分比），那整条法律读起来就是"价值观加速器"——这是设计错误，不是平衡问题。

**漂移是副产物，不是身份。** 允许某些政策不带漂移。

## 五、池子该开多大：本体通用池只有 5~9 条

⚠ **别被大数字骗了。** 逐块数准（2026-09）：

| 法律 | 总政策 | **通用** | tag 专属 |
|---|---|---|---|
| `legal_code_law` | 42 | **5** | **37**（英国普通法、沙里亚、罗斯真理、巴伐利亚法典、大明律、经国大典、蒙古法、塞尔维亚、波兰、立陶宛…） |
| `administrative_system` | 24 | **9** | 15（诺夫哥罗德 pyatina、奥斯曼 eyalet、阿拉贡 merindad、纳瓦 calpixque、骑士团 tongues、印度 rajya/poligar/zabt、拜占庭 pronoia 系列…） |

**本体的"大法律"是靠 tag 门控堆出来的，通用池一律小而精。** 推论：

- 做**通用法律**（无条件解锁）→ 池子开 **5~9 条**，靠"每条更重"制造分化
- 想开大池子 → **必须**用 `potential` 做 tag / culture / 宗教 / 时代门控

**数政策时的两个坑**：
- 政策的 `unique = yes` 在**两个 tab**，法律级的才在一个 tab。用 `^\tunique = yes` 数会只命中法律级、把 tag 专属政策全漏掉。
- 有些政策块 opener 带行尾 `#` 注释（`01_legal_system.txt:432, 451, 496, 590, 619`），用 `\{$` 锚定会静默漏掉 → 用 `\s*\{`。
- 交叉验证用 `^\t\t(years|months|weeks|days) = `（政策级时长字段）；但**不是每条政策都有时长字段**（`traditional_law_policy` 就没有），以块计数为准。

## 六、时代阶梯：本体自己就这么干

`in_game\common\laws\01_common.txt:589-972` 的 `administrative_system` 是范本——**1337 档全是去中心化选项**：

| 政策 | 解锁 | 效果关键词 |
|---|---|---|
| `autonomous_councils` | **1337** | 立法效率、**去中心化** |
| `central_councils` | **1337** | 内阁效率、集权、贵族化 |
| `local_courts_administration` | **1337** | **去中心化**、入叛门槛 −0.03、**农村控制 −0.05** |
| `feudal_administration` | **1337** | **去中心化**、粮食 +0.05 |
| `confederal_union` | 1437 | 去中心化、附庸忠诚 +10 |
| `federal_state` | 1537 | 全体满意度、去中心化 |
| `absolute_rulership` | **1637** | **王室权力 +0.1**、绝对主义 |
| `unitary_administration` | **1637** | **距离传播 +0.05**、集权 |

**时代起始年**：`age_1_traditions` 1337 ／ `age_2_renaissance` 1342 ／ `age_3_discovery` 1437 ／ `age_4_reformation` 1537 ／ `age_5_absolutism` 1637 ／ `age_6_revolutions` 1737。

**三条设计含义**：

1. **1337 年"行政体系"的选择本质是"你打算把多少权力让给地方"**——`local_courts_administration` 直接给**负的** `global_max_rural_control`。
2. 把绝对主义时代的举措（土地清册、常设税、国家驿传、王室权力增长）放到 1337 是**断代错误**。全本体只有 `absolute_rulership` 给 `global_crown_estate_power`，且锁在 1637。
3. **数值上的"弱一档"就是断代**，不一定需要加门控。同一个修正，1337 给 0.05、1637 给 0.1，本身就是时代差异。

## 七、内容断代：三筛规则

写**无条件解锁**的通用法律时，每条政策必须同时过三筛：

| 筛 | 要求 |
|---|---|
| **一、开局年做得到** | 不需要常设官僚、常备军、常设税 |
| **二、任何文明都有** | 不能是某个文明的专有制度 |
| **三、是一件具体的政府动作** | 不是价值观方向 |

二三两筛的交集只有一个东西：**普遍政府职能**。

> **关键区分：「专名」要禁，「职能词」不禁。**
> `常平仓`是汉代某具体机构的名字（专名，**禁**）；"粮仓"是全人类都有的职能（职能词，**不禁**）。
> `科举`是中国专有制度（**禁**）；"用人"是普遍职能（**不禁**）。
> 等级会议／封建封臣／修道院／巡回法庭是拉丁基督教世界的专有制度（**禁**）；"议事""司法""扶植宗教"是普遍职能（**不禁**）。

## 八、命名：三条禁令

通用法律的命名最容易连错三次，**每次都换一种错法**：

| 错法 | 例子 | 为什么错 |
|---|---|---|
| **汉典专名** | 清丈田亩、编户齐民、设立常平仓、开科取士、通商惠工 | 《汉书》典故、汉代具体机构名、中国专有制度 |
| **现代公文构词** | 户口**登记**、邮驿**体系**、**奖励**工商、攻势**外交** | `体系`／`登记`／`奖励`／`XX外交` 都是 19 世纪末才进中文的构词 |
| **文明专名** | 召集等级会议、委政封臣、修道院庇护、巡回审判 | 拉丁基督教世界的制度，元朝／德里苏丹国／马里帝国／阿兹特克一个都对不上 |

**本体自己划的线**：

- **通用法律一律用中性制度词**：本乡自治、巡回法庭、封建行政、集权管理、联邦国家、林业条例、航海法案、保护商路、选贤任能
- **专名一律门控**：六部、科举制度、`examination_system_policy_chinese` / `_korean` / `_vietnam`（中国/朝鲜/越南）；锁国与开国（日本）；八大臣内阁（马拉塔）

**规则**：政策名 = **一类制度或一项工程的通称**。不做典故、不做对偶、不用特定文明的官制／典籍术语。描述用现代短句直述，不用四六骈文。

## 九、撞名检查（新增政策**必做**）

**显示名与键名都要查**，对照本体 `main_menu\localization\simp_chinese\` 全目录（键名方面该目录的 `laws_and_policies_l_simp_chinese.yml` 收全部法律与政策显示名）。

实测撞到两个：

| 初拟 | 本体已有 | 后果 |
|---|---|---|
| 保护商路 | `protect_trade_routes: "保护商路"` | 两条不同法律里出现同名政策，UI 上是两个「保护商路」 |
| 举贤任能 | `meritocratic_recruitment_policy: "选贤任能"`（只差一字） | 同屏混淆 |

**只查显示名不查键名是不够的**——键名撞了会直接覆盖本体本地化。

## 十、审查要点（法律专属）

- 政策里改国级用 `country_modifier`；误用 `province_modifier` / `location_modifier` 会静默变成地点级。
- `unique = yes` 是法律级字段，含义与其他类目的 `unique` 一致（排他）。
- 引用 payment / special status / parliament_type 的 IO 政策须确认对应类目存在（见 `common-laws.md`）。
- 政策块 opener 若带行尾注释，**注释内容不参与解析**，但会干扰审查脚本的 `\{$` 锚定。
- **`law_category = foreign_policy` 在国家作用域没有本体先例**——本体唯一那条 `sc_relations_with_hre` 是国际组织作用域（`potential` 里是 `international_organization_type`）。类别显示大概率正常（`law_category` 字段生效已在行政法上实机确认），但属于**未实机验证**项，审查时标 `[存疑]`。
