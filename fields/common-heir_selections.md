# common/heir_selections（继承法）

> **一句话**：继承法字段：候选范围、选举参数、四类触发块与两条互斥的计分路线，另列说明文件未载而原版在用的三个字段。
> **什么时候看**：写继承法、选计分路线，或核对任期单位与资料片回退字段时翻这篇。
> **体量**：131 行 · 约 6 分钟通读

来源：`in_game\common\heir_selections\00_heir_selections.info`（4 318 B，31 个字段）+ 5 个数据文件 **44 种继承法** 的实际用法

## 字段（readme 声明，按用途分组）

```
<heir_selection id> = {
    # ① 候选范围（谁能进池）
    traverse_family_tree = yes      # 是否走家族树计世系
    depth_first = yes               # （readme 未列）家族树按深度优先遍历
    allow_extended_family = yes     # 走家族树时是否纳入侄甥辈
    include_ruler_siblings = yes
    all_in_country = yes            # 全国内角色
    all_in_overlord = yes           # 全宗主国内角色
    all_in_dynasty = yes            # 现统治者宗族全体
    allow_children = yes            # 未成年可否为候选（默认 no）
    allow_female / allow_male = yes # 默认 no / yes
    through_female = yes            # 女性是否能传递继承权
    use_mothers_dynasty = yes       # 是否按母方宗族计算
    allow_foreign_ruler = no        # 已是他国统治者可否候选（默认 yes）
    ignore_ruler = yes              # 计算时排除现统治者（选举 no、王朝 yes）

    # ② 选举
    use_election = yes
    max_possible_candidates = <int> # 选举候选上限
    term_duration = <月>            # 任期（默认 0 = 终身）
    show_candidates = yes

    # ③ 触发块
    potential = { … }               # 是否出现在界面
    allowed = { … }                 # potential 满足后的进一步条件
    locked = { … }                  # 为真则不能切走本法
    include_other_countries = { … } # root = 被考察国, scope:target = 发生继承的国家
    candidate_country = { … }       # 同上（选举用）
    heir_is_allowed = { … }         # root = character, scope:target = country
    allowed_estates = { nobles_estate clergy_estate }  # 空 = 不限阶层

    # ④ 计分
    calc = { … }                    # 选举型：逐候选人打分（root 可能不存在 → 写 tooltip 分支）
    sibling_score = { … }           # 王朝型：家族树遍历时逐同胞打分

    # ⑤ 元数据 / 运行时
    modifier = { … }                # （readme 未列）生效期间的国家/角色修正
    succession_effect = { … }       # root = country, scope:old_ruler, scope:new_ruler
    custom_tags = { <strings> }     # 自定义标识串
    show_tags_in_ui = yes/no
    custom_eligibility_rules = { <strings> }  # 可本地化的自定义资格说明
    cached = no                     # 是否集中缓存使用本法的国家列表
    setup_fallback = <其它继承法 id>  # （readme 未列）本法不可用时的回退
}
```

## 原版实测（44 种 / 5 文件）

| 文件 | 个数 |
|---|---|
| `monarchy.txt` | 18 |
| `republic.txt` | 13 |
| `theocracy.txt` | 6 |
| `specialized.txt` | 4 |
| `tribal.txt` | 3 |

### 两条互斥的计分路线（本次统计最有用的一条结论）

| 路线 | 个数 | 代表 |
|---|---|---|
| **只 `calc`**（选举型：逐候选人打分） | 30 | `elective_succession`、`papal_election`、`doge_election`、`lottery_election` |
| **只 `sibling_score`**（王朝型：家族树逐同胞打分） | 12 | `cognatic_primogeniture`、`salic_law`、`partition_inheritance`、`byzantine_succession` |
| 两者都有 | 2 | `union_of_crowns_succession`、`matrilineal_non_exclusive` |

- `depth_first = yes` 只有 4 个：`cognatic_primogeniture`、`absolute_cognatic_primogeniture`、`austrian_cognatic_primogeniture`、`union_of_crowns_succession`。
- `use_election = yes` 与 `term_duration` **都是同样那 18 个**（`fratricide_succesion`、`elective_succession`、`dynastic_elective_succession`、`judicial_election`、`oligarchic_elective`、`republic_4_year_terms`、`republic_2_year_terms`、`lottery_election`、`veche_selection`、`doge_election`、`diarchic_election`、`pirate_elective`、`peasant_elective`、`military_dictatorship`、`signoria_selection`、`podesta_elective`、`guru_election`、`papal_election`）；`term_duration` 值只有三种：**0（16 个，= 终身）/ 24（2 年）/ 48（4 年）**。
- `allowed_estates` 41/44；**空的 3 个**（`veche_selection`、`admiralty_regime_heir_selection`、`general_heir_selection`）表示不限阶层。
- `heir_is_allowed` 40/44；缺的 4 个是 `fratricide_succesion`、`judicial_election`、`veche_selection`、`papal_election`。
- `potential` 35/44；无 `potential` 的 9 个多为共和国/神权选举与部落法。

字段出现率（全部 44 个）：

| 字段 | 出现 | 率 |
|---|---|---|
| `allowed_estates` | 41 | 93% |
| `heir_is_allowed` | 40 | 91% |
| `potential` | 35 | 80% |
| `calc` | 32 | 73% |
| `allow_female` / `ignore_ruler` | 28 | 64% |
| `through_female` | 26 | 59% |
| `include_ruler_siblings` / `locked` | 23 | 52% |
| `term_duration` / `allow_children` / `use_election` | 18 | 41% |
| `max_possible_candidates` | 17 | 39% |
| `sibling_score` | 14 | 32% |
| `succession_effect` / `traverse_family_tree` | 13 | 30% |
| **`modifier`** | 11 | 25% |
| `all_in_country` | 11 | 25% |
| `custom_tags` | 8 | 18% |
| `allow_foreign_ruler` / `show_candidates` | 7 | 16% |
| `all_in_dynasty` | 6 | 14% |
| **`depth_first`** | 4 | 9% |
| `custom_eligibility_rules` | 3 | 7% |
| `allow_male` | 3 | 7% |
| `use_mothers_dynasty` / `allowed` | 2 | 5% |
| **`setup_fallback`** / `cached` / `include_other_countries` / `candidate_country` | 各 1 | 2% |
| `allow_extended_family` / `all_in_overlord` / `show_tags_in_ui` | **0** | readme 有、原版不用 |

## readme 未列出、但原版在用的 3 个字段

| 字段 | 原版实况 |
|---|---|
| `modifier = { … }` | 11 处，**内容全是 `care_about_producing_heirs = yes`**（都在 `republic.txt` 的任期选举法上）——AI 是否在意生育继承人的开关 |
| `depth_first = yes` | 4 处，家族树**深度优先**遍历（长子系优先于旁系），只在长子继承家族出现 |
| `setup_fallback = cognatic_primogeniture` | 1 处（`byzantine_succession`）：该法带 `potential = { has_dlc = "d008_fate_of_the_phoenix" … }`，**没 DLC/条件不满足时回退到指定继承法** |

`custom_tags` 原版只用了两个值：`cannot_be_transferred`、`empty_heir_religion_law`。

## 本地化

`<heir_selection id>` = 名称、`<id>_desc` = 描述（`main_menu\localization\<lang>\government_l_simp_chinese.yml`，如 `salic_law` / `salic_law_desc`）。原版 44 个**全部有键**。

## 审查要点

- **先决定走哪条计分路线**：王朝型写 `sibling_score`（并配 `traverse_family_tree` / `depth_first`），选举型写 `calc`（并配 `use_election` / `max_possible_candidates` / `term_duration`）。只写 `calc` 而没开 `use_election`、或反之，都会出现"继承人不按预期产生"。
- `calc` / `sibling_score` 的 `add = { desc = "…" value = … }` 需要**两条分支**：readme 示例里第二条是 `not = { exists = root }`（通用 tooltip 用），漏掉它会导致界面 tooltip 取值异常。
- `modifier` 在 readme 里根本没写（readme 的字段清单止于 `custom_eligibility_rules`），改用 `care_about_producing_heirs` 之外的类型前先确认该修正类型在本体 `modifier_type_definitions` 存在。
- DLC 门控的继承法必须给 `setup_fallback`，否则未持有 DLC 的存档切到该法时没有退路（原版做法就是这么写的）。
- `allowed_estates` 留空 = 不限阶层；填了就要引用真实 estate id（`nobles_estate` 等）。
- 未在 readme 中说明：本地化键格式、`term_duration` 单位是**月**（原版值 24/48 即 2 年 / 4 年）、以及上文 3 个未列字段。
