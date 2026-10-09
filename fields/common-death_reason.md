# common/death_reason（死因）+ common/designated_heir_reason（指定继承人理由）

> **一句话**：死因与指定继承人理由两档：随机掷骰池、触发与权重、可携带参数，以及参数顺序决定本地化键后缀的命名规则。
> **什么时候看**：加死因、配本地化键组合，或查继承人理由空块键时翻这篇。
> **体量**：86 行 · 约 4 分钟通读

来源：**本体没有 readme**——字段由 4 个数据文件反推（`in_game\common\death_reason\00_hardcoded.txt` 544 B、`01_content.txt` 887 B、`02_life_expectancy.txt` 3 502 B、`designated_heir_reason\00_standard.txt` 193 B）

## death_reason 字段

```
<death_reason id> = {
    random = yes                    # 是否进「随机死因」掷骰池
    trigger = { <character triggers> }   # 谁可能这样死（角色作用域；地点条件写 location ?= { … }）
    weight = <script value>         # 相对权重；可为常数或 value/subtract/multiply 算式
    possible_parameter = location   # 该死因可携带的参数（可多行）：location / killer / disease_outbreak
}
```

## 原版实测（**51 个**死因 / 3 文件）

| 文件 | 个数 | 性质 |
|---|---|---|
| `00_hardcoded.txt` | 9 | 只有 `possible_parameter` + 注释 `# hardcoded`：由**引擎**在对应事件点触发（`battle` / `battle_sea` / `siege` / `marching` / `in_port` / `at_sea` / `camp` / `disease` / `country_annexed`） |
| `01_content.txt` | 18 | 多为空块或只带参数：**不进随机池**，用于处死/献祭等特定场合（`seppuku` / `sacrifice` / `their_own_exploding_cannon` / `cannibalism` / `beheading` / `execution` / `slicing` / `guillotine` / `assassination` / `strangling` / `self_defence` / `burned_alive` / `poison_arrow` / `combat` / `killed_by_janissaries` / `stray_fire` / `hanged_by_mob` / `poisoned`）。原版脚本里 `death_reason` **0 次引用**，可见这些也是由引擎在对应交互/事件路径内部取用，而不是脚本按 id 指定 |
| `02_life_expectancy.txt` | 24 | **全部** `random = yes`：寿终/意外死亡掷骰池（`old_age` / `choking_accident` / `sporting_accident` / `fell_down_stairs` / `burst_ulcer` / `pneumonia` / `sudden_illness` / `food_poisoning` / `hunting_accident` / `heart_failure` / `fever` / `dysentery` / `jousting` / `farming_accident` / `mining_accident` / `alcoholism` / `froze` / `suicide` / `drowning_river` / `drowning_sea` / `starvation` / `wild_animal` / `vanished` / `sickly_death`） |

字段出现率：`random` 24/51（47%）· `weight` 23/51（45%）· `possible_parameter` 20/51（39%）· `trigger` 14/51（27%）。
**只有 `02_life_expectancy.txt` 里的条目带 `random = yes`**，其余 27 个走引擎的硬编码触发路径（原版脚本里 `death_reason` 0 次引用）。

> ⚠️ **脚本侧零引用**：全库 `.txt` / `.info` / `.md` 里搜 `death_reason` 命中 0 次，只有本地化键（`DEATH_REASON_*`）。这一层是**引擎记录与展示**用的；想让角色"以某死因死亡"要走交互/效果的内建路径（如 `d008_mutilations` 这类交互），不是给角色挂字段。

## `possible_parameter` → 本地化键后缀（本类目最容易踩的坑）

原版 **51 个 id ↔ 87 个 `DEATH_REASON_*` 键**，多出的 36 个正是参数组合键。规则：

**键名 = `DEATH_REASON_<id>` + 按 `possible_parameter` 声明顺序追加 `_<参数名>`，取「有 / 无」的全部组合。**

| 死因 | 声明的参数 | 需要准备的键（4 个） |
|---|---|---|
| `disease` | `location`、`disease_outbreak` | `DEATH_REASON_disease` / `_disease_location` / `_disease_disease_outbreak` / `_disease_location_disease_outbreak` |
| `assassination` | `location`、`killer` | `DEATH_REASON_assassination` / `_assassination_location` / `_assassination_killer` / `_assassination_location_killer` |
| `combat` | `location`、`killer` | 同上四连 |
| `battle` | `location` | `DEATH_REASON_battle` / `_battle_location` |

参数名就是**作用域名**，文本里这样取用（`character_l_simp_chinese.yml:85-88`）：

```yaml
DEATH_REASON_disease_location_disease_outbreak: "[CHARACTER.GetHeSheFormal|U]在[SCOPE.sLocation('location').GetName]不幸染上[SCOPE.sDiseaseOutbreak('disease_outbreak').GetName]去世。"
```

还有一个**只有 loc 键、数据里没有**的 `exploding_cannon`（数据侧叫 `their_own_exploding_cannon`）——说明除数据文件外引擎还有内置死因。

## 权重写法（`02_life_expectancy.txt` 实例）

| 死因 | trigger 关键条件 | weight |
|---|---|---|
| `old_age` | `age_in_years >= 60` | `{ value = age_in_years  subtract = 60  multiply = 4 }`（60 岁以后**每岁 +4**） |
| `sporting_accident` | `age_in_years < 50`、男性、`has_estate = estate_type:nobles_estate` | `10 - mil × 0.1` |
| `jousting` | 男性、<50、贵族、基督教、**时代 = age_1_traditions / age_2_renaissance** | `10 - mil × 0.1` |
| `farming_accident` / `mining_accident` | 非将军/提督/探险家、平民、`location ?= { raw_material ?= { goods_method = farming/mining } }` | `10 - mil × 0.1` |
| `froze` | `location ?= { winter_level = severe }` | `20 - mil × 0.1 - location.development × 0.1` |
| `wild_animal` | `location ?= { is_land = yes  development < 20 }` | `20 - mil × 0.1 - development × 0.5` |
| `drowning_river` / `drowning_sea` | `has_river` / `is_coastal` | `1 - mil × 0.01` |
| `starvation` | `location ?= { province ?= { is_starving = yes } }` | **无 weight**（原版唯一一个既无 weight 也非硬编码的条目） |
| `sickly_death` | `has_trait = sickly` | **1000**（病弱特质直接把随机死因权重拉满） |
| `alcoholism` | `has_trait = drunkard` | 10 |
| `vanished` | — | 0.1 |

可见两条渠道互不替代：**特质侧** `yearly_chance_to_die`（每年固定死亡概率，见 `fields\common-traits.md`）+ **死因侧** `weight`（随机掷骰的相对权重）。

## designated_heir_reason（指定继承人理由）

```
<reason id> = { }     # 空块，纯本地化键：HEIR_REASON_<id>
```

原版 7 个，全部是空块、全部有中文键（`character_l_simp_chinese.yml:164` 起）：`married_into_ruling_dynasty`（嫁入统治者宗族）、`preferred_heir_of_ruler`、`designated_as_appanage`、`prince_in_exile`、`corruler`（共治者）、`sibling_of_ruler`、`infant_emperor`。用途是给界面解释"此人为何是继承人"。

## 审查要点

- **新增死因必须一次写全 loc 键**：基键 + 每个参数组合键，缺哪个组合就在对应情形显示 raw key（原版 51 → 87 就是这么来的）。
- `trigger` 是**角色作用域**（`age_in_years` / `is_female` / `has_estate` / `has_trait`），涉及地点一律 `location ?= { … }`（`?=` 允许角色无地点时不炸）。
- `weight` 可以是常数、算式或省略；原版省略的两类分别是"引擎硬编码触发"和 `starvation`（脚本触发）。**别照抄 `starvation` 当模板**。
- `possible_parameter` 允许多行（原版 `disease` 两行）；**改声明顺序会同时改变 loc 键名顺序**（`_location_killer` ≠ `_killer_location`）。
- 未在 readme 中说明：**本类目没有 readme**，全部字段语义以本文件为准。
