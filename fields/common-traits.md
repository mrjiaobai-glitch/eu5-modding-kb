# common/traits（特质）

来源：`in_game\common\traits\_traits.info`（790 B）+ 9 个数据文件 **147 个特质** 的实际用法
> **注意**：本文件在 `in_game\common\traits\`（不在 `main_menu\common\`）。

## 字段（readme 声明）

```
<trait id> = {
    category = ruler/general/admiral/artist/explorer   # 谁能用；readme 称默认 ruler
    flavor = <trait flavor>                            # 见 common/trait_flavor
    allow = { <character triggers> }                   # 成为该特质的额外条件
    modifier = { <character modifiers> }               # ⚠️ 仅当角色身份 == category 时生效
    chance = {                                         # MTTH 式出现概率
        base = N
        modifier = { factor = M <condition> }
    }
    max_number_of_birth_siblings = 2                   # Health 专用；默认 2
}
```

## 原版实测（147 个 / 9 文件）

| 文件 | 个数 | `category` |
|---|---|---|
| `00_ruler.txt` | 44 | ruler |
| `07_cabinet.txt` | 21 | cabinet |
| `01_general.txt` | 19 | general |
| `02_admiral.txt` | 13 | admiral |
| `08_health.txt` | 13 | health |
| `03_artist.txt` | 12 | artist |
| `05_child.txt` | 10 | child |
| `06_religious_figure.txt` | 10 | religious_figure |
| `04_explorer.txt` | 5 | explorer |

字段出现率（花括号深度解析）：

| 字段 | 出现 | 率 | 备注 |
|---|---|---|---|
| `category` | 147 | 100% | 原版无一依赖 readme 的"默认 ruler" |
| `modifier` | 147 | 100% | 可为空块 `{ }`（如 `smallpox_trait`） |
| `allow` | 75 | 51% | 可为空块 |
| `custom_tags` | 64 | 44% | readme 未列 |
| `flavor` | 63 | 43% | personality 41 / education 9 / government_approach 7 / interests 6 |
| `chance` | 27 | 18% | **只出现在两类**：`05_child.txt` 10/10、`07_cabinet.txt` 17/21；其余 7 个类目的特质都没有 `chance`（靠出生骰/战斗骰或脚本给予） |
| `is_bad` | 19 | 13% | readme 未列 |
| `yearly_chance_of_remove` | 11 | 7% | readme 未列 |
| `chance_on_birth` | 7 | 5% | readme 未列 |
| `chance_after_battle` | 6 | 4% | readme 未列 |
| `yearly_chance_to_die` | 3 | 2% | readme 未列 |
| `upgrades_to` | 2 | 1% | readme 未列 |
| `recovery_trait` / `juvenile_form` / `max_number_of_birth_siblings` | 各 1 | 1% | 前两个 readme 未列 |

## readme 未列出的 9 个字段（原版都在用，写 mod 常需要）

| 字段 | 语义（原版实例） |
|---|---|
| `is_bad = yes` | 标记负面特质（AI/UI 判定） |
| `custom_tags = { … }` | 自由标签，**可多个空格分隔**；原版取值：`negative` 19 / `administrative` 10 / `diplomatic` 4 / `military tactical` 4 / `economical` 4 / `administrative tolerant` 3 / `diplomatic social intrigue` 3 … |
| `chance_on_birth = 0.0003` | **出生时**获得概率（health 类主力渠道；`hunchback` 0.00015 最低、`sickly` 0.15 最高） |
| `chance_after_battle = 0.01` | **战斗后**获得概率（`disfigured` 0.05 最高） |
| `yearly_chance_of_remove = 0.25` | 每年**自然消失**概率 |
| `yearly_chance_to_die = 0.25` | 每年**因此致死**概率（与上者并存即"要么好、要么死"） |
| `recovery_trait = pockmarked_trait` | 痊愈后**替换**为该特质（`smallpox_trait`：0.65 消失 / 0.35 死亡 / 活下来得麻脸） |
| `upgrades_to = blind` | 恶化**升级**目标（`one_eyed` → `blind`、`scarred` → `disfigured`） |
| `juvenile_form = eunuch` | **未成年形态**（`castrated` 在儿童期以 `eunuch` 呈现） |

`max_number_of_birth_siblings` 是 readme **已列**的字段（默认 2），但唯一改动它的原版例子值得记：`sickly` 设为 **1**（配合 `chance_on_birth = 0.15` + `yearly_chance_of_remove/to_die = 0.25`，实现"约 50% 五年内夭折，活下来的恢复正常"）。

## `category`：readme 已陈旧

readme 只声明 5 个值 `ruler/general/admiral/artist/explorer`（且称默认 ruler），**原版实际 9 个**——另加 `cabinet` 21 / `health` 13 / `child` 10 / `religious_figure` 10。本地化侧同样有 9 组 `TRAIT_CATEGORY_*` / `TRAIT_WHEN_*`（`traits_l_simp_chinese.yml:2-23`，另有第 10 个 `TRAIT_CATEGORY_CHARACTER`）。

**`modifier` 的生效条件**（readme 原文最关键一条）：只有当**角色当前身份与该特质的 `category` 相同**时修正才生效——`ruler` 类特质挂在内阁席位上等于白写。

## `trait_flavor`（`in_game\common\trait_flavor\00_default.txt`，201 B）

```
personality = { color = rgb { 63 125 199 } }        # 性格（原版 41 个特质）
government_approach = { color = rgb { 119 70 168 } } # 治国取向（7）
interests = { color = rgb { 193 117 61 } }          # 兴趣（6）
education = { color = rgb { 80 161 115 } }          # 教育（9）
```

`flavor` 只决定 **UI 配色分组**，原版只有这 4 个值；未定义的 flavor 无颜色可显示。

## 本地化（原版实测）

- `<trait id>` = 名称、`desc_<trait id>` = 描述、`<trait id>_die_desc` = 角色去世时的追述（`main_menu\localization\<lang>\traits_l_simp_chinese.yml`）
- 147/147 都有 `desc_*`；**唯一名称缺口的处理值得注意**：`arrogant`（`07_cabinet.txt:371`，cabinet 类）在 traits 文件里**没有** `arrogant:` 键，只有 `desc_arrogant` + `arrogant_die_desc`——名称实际由 `character_names_l_*.yml:29880` 的**人名词表条目**提供（键是全局的，所以游戏内显示正常）。
  → 审查启示：**判断"缺 loc 键"必须全库搜，不能只看本类目的 loc 文件**；反之，改了同名的人名词条也会连带改掉特质名。

## 脚本面（原版 common\ 内引用计数）

| 写法 | 计数 | 示例 |
|---|---|---|
| `has_trait = <id>`（**不带 `trait:` 前缀**） | 38 | `has_trait = drunkard` |
| `has_trait = trait:<id>` | 0 | 原版不用 |
| `add_trait = trait:<id>` | 20 | `add_trait = trait:castrated`（`d008_mutilations.txt:59`） |
| `add_trait = { … }`（带 first/global/third 变体） | 1 | `character_effects.txt:106` 声明其 loc 变体 |
| `remove_trait` | 1 | — |

配套 trigger（`trigger_localization\character_triggers.txt`）：`has_trait`(:260)、`has_trait_category`(:267)、`num_of_traits_of_category`(:273)——后两个可直接按**类别**而非具体特质写条件。

## 审查要点

- **特质 id 必须与 `modifier` 语义匹配 `category`**：角色身份 ≠ category 时修正静默失效，是"改了没效果"的第一大原因。
- `chance` 结果会被 `GetCeiling()` **向上取整**（readme 原文），所以 `factor < 1` 不会降低概率；排除条件要写 `allow`。
- `upgrades_to` / `recovery_trait` / `juvenile_form` 引用的**必须是已存在的 trait id**（原版 `one_eyed`→`blind`、`scarred`→`disfigured`、`smallpox_trait`→`pockmarked_trait`、`castrated`→`eunuch`）；写错 id 时升级链断在一个不存在的特质上。
- `custom_tags` 是自由文本标签（原版 `military tactical` 这类双词值说明**一个键可空格式多值**），不要当成枚举。
- 未在 readme 中说明：`flavor` 的取值清单（在 `trait_flavor\`）、`category` 的完整 9 值、以及本文"readme 未列出的 8 个字段"。
