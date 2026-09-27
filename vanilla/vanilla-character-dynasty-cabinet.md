# 原版解析：角色 · 王朝 · 内阁（vanilla character, dynasty & cabinet）

版本基准：EU5 1.3.x。全部结论来自游戏本体文件，路径相对 `<game>\`。

| 类目 | 规模 | 权威 |
|---|---|---|
| `in_game\common\traits\` | 9 文件 / **147 个特质**（9 类别） | `_traits.info` |
| `common\heir_selections\` | 5 文件 / **44 种继承法** | `00_heir_selections.info`（约 30 字段） |
| `common\regencies\` | **15 种摄政** | `readme.txt`（只有 5 个字段） |
| `common\child_educations\` | **6 种教育** | `readme.txt`（7 字段） |
| `common\character_interactions\` | **34 个交互** | `readme.txt` |
| `common\cabinet_actions\` | **73 个行动**（adm **35** / mil 21 / dip 17） | `readme.txt`（109 行） |
| `common\genes\` / `common\ethnicities\` | 11 文件 418KB / 54 文件 **629KB** | `_genes.info` |
| `common\death_reason\` | 3 文件（含 `02_life_expectancy.txt` 3.5KB） | 无 |
| `common\designated_heir_reason\` | 1 文件 | 无 |
| `common\artist_types\` / `artist_work\` | **13** 类型 / **21** 艺术品类型 | 各有 readme |
| **`main_menu\setup\start\05_characters.txt`** | **2.4MB**（`character_db`） | — |
| **`main_menu\setup\start\04_dynasties.txt`** | 238KB（`dynasty_manager`） | — |
| 常量 | **`NCharacter`（`defines` 1513–1573）** | — |
| 脚本面 | 角色 **103 触发器 / 41 效果**；王朝 7 / 3（`trigger_localization\character_triggers.txt` 等） | — |

> ⚠️ **`common\` 里没有 characters / dynasties 类目**——角色与王朝是**开局数据**（`character_db` / `dynasty_manager` 逐条定义具体人物与家族），不是"类型模板"。改"有哪些人"要去 `main_menu\setup\start\`，改"人怎么运作"才在 `common\`。

## 术语对照（中文译名与内部名）

| 内部名 | 游戏内中文 | 说明 |
|---|---|---|
| `character` | 角色 | 有 abilities / traits / culture / religion 的个人 |
| `adm` / `dip` / `mil` | 行政 / 外交 / 军事能力 | 各 0–100，相加 = `total_ability` |
| `trait` | **特质** | 9 类别；`modifier` 仅在"身份 = 类别"时生效 |
| `dynasty` / `dynasty_head` | **宗族** / 宗族领袖 | 共同祖先与血缘的角色群 |
| `ruler` / `consort` / `heir` | 统治者 / 配偶 / 继承人 | — |
| `heir_selection` / `heir_selection_score` | **继承法** / 继承人分数 | 44 种继承法 |
| `regency` / `regent` | **摄政** / 摄政者 | 无合法统治者或继承人年幼时 |
| `cabinet` / `cabinet_seats` / `cabinet_member` | **内阁** / 内阁席位 / 内阁成员 | 席位由 `government_size` 决定 |
| `cabinet_action` | 内阁行动 | 73 个；效果按完成度缩放 |
| `education` | 教育 | 3 岁起可选的儿童教育 |
| `artist` / `work_of_art` | 艺术家 / 艺术品 | 艺术品给主流文化加"文化影响" |
| `legitimacy` | 正统性 | 衡量人口与阶层对统治的接受度 |

## 一、角色

### 1.1 三能力与年龄阶段（`NCharacter`）

| 常量 | 值 | 说明 |
|---|---|---|
| `MAX_ABILITY_VALUE` | **100** | 单能力上限（词条：新生儿从 0 开始，随成长提高） |
| `MONTHLY_STAT_INCREASE_CHANCE_FOR_CHILD` | **55** | 儿童期每月 +1 的概率（1d100） |
| `MAX_INFANT_AGE` / `MAX_CHILD_AGE` / `MAX_ADOLESCENT_AGE` / `ADULT_AGE` | 2 / 10 / 15 / **16** | 年龄阶段；16 岁成年后才能当统治者/摄政/入内阁/任将 |
| `MAX_CHARACTER_AGE` | **100** | 硬上限（到岁必死） |
| `OLD_AGE_MONTH_CHANCE` | **3.0** | 老年死亡月骰（100 面骰） |
| `EXPLORER_EXTRA_LIFE` | 15 | 探险家额外寿命 |
| `INCOMPETENT_TOTAL_ABILITY_THRESHOLD` / `INCOMPETENT_AGE_THRESHOLD` | **50** / 10 | 三能力总和低于 50 且年龄 ≥10 → 弹"无能"警报 |
| `ATTRIBUTE_IMPORTANCE_MULTIPLIER` | 4 | 属性重要度的总乘数 |

**属性重要度**（AI 评估角色价值时按身份加权）：统治者 **1** / 继承人 0.9 / 统治者其他子女 0.8 / 继承人其他子女 0.7 / 其他角色 0.5。

三能力的官方语义（词条）：`adm` = 组织事务·掌控经济·维持国家；`dip` = 说服他人 + **受欢迎程度**；`mil` = 战术理解 + 艰难抉择。

### 1.2 随机角色生成偏向

从 POP 抽新角色时的倍率（`NCharacter`）：

| 常量 | 值 |
|---|---|
| `RANDOM_CHARACTER_CHANCE_PRIMARY_CULTURE_MULTIPLIER` | **80** |
| `..._ACCEPTED_CULTURE_MULTIPLIER` | 50 |
| `..._TOLERATED_CULTURE_MULTIPLIER` | 20 |
| `..._CULTURE_OPINION_MULTIPLIER` | 1（与主流文化对该文化的**好感**相乘；同文化视为"亲密"=3） |
| `..._RELIGION_OPINION_MULTIPLIER` | 1（同上，按宗教好感） |

即：**主流文化人口被抽中的概率是相容文化的 4 倍**，再乘文化与宗教好感——这就是"为什么你的宫廷里全是本族人"。

### 1.3 婚姻与生育

| 常量 | 值 |
|---|---|
| `AI_MIN_MARRIAGE_AGE` | 19 |
| `AI_MAX_FEMALE_MARRIAGE_AGE` | 30 |
| `DYNASTY_HEAD_MARRY_AGE` / 目标年龄区间 | 25 / **20–32**（宗族领袖的 AI 择偶窗口） |
| `MAX_PREGNANCY_AGE` / `PREGNANCY_AGE_THRESHOLD` | **40**（硬门）/ 35 |
| `PREGNANCY_CHANCE` | **170**（1000 面骰） |
| `PREGNANCY_FERTILITY_MIN/MAX_REDUCTION` | 12 / 25 |

婚姻的**外交侧数值**（联姻收益、`royal_ties` 接受度、后续配偶收益 ×0.5、统治者权重 ×2）见 `vanilla\vanilla-diplomacy.md` §二；婚姻交互三档：`marry_noble`（贵族通婚）/ `marry_lowborn`（低门第）/ `tribal_arrange_marriage`（部落包办）。

### 1.4 肖像与基因（`genes\` + `ethnicities\`）

- 规模：`genes` 11 文件 **418KB**、`ethnicities` 54 文件 **629KB**——**美术向但机制向 mod 也用得上**。
- `_genes.info`（自称 "very incomplete"）给出三类结构：
  - `color_genes`：`hair_color = { index = 0  color = hair  blend_range = { 0.4 0.6 } }`——**`blend_range` 决定在"显性/隐性父母之间取哪个比例"**（`{0 0}` = 完全取显性方，`{0.3 0.7}` = 取两者之间 30%~70%）。
  - `decal`：`type = skin/paint`——skin 贴花在肤色**之前**（用 skin-diffuse+normal），paint 在肤色**之后**（用 paint-diffuse+property）；含 `atlas_pos` 与 `alpha_curve`（按基因强度控制贴花透明度）。
  - `ugliness_feature_categories = { chin mouth }`——**供 traits 的 `ugliness_portrait_extremity_shift` 使用**（特质影响肖像极值）。
- 角色侧还有 `ethnicities\`（54 文件）决定外貌分布。

### 1.5 死因（`death_reason\`，3 文件）

- `00_hardcoded.txt`：`battle` / `battle_sea` / `siege` / `marching` 等**引擎触发**的死因，可带 `possible_parameter = location`（死亡地点参与本地化）。
- `01_content.txt`：内容侧死因。
- `02_life_expectancy.txt`：寿命/自然死亡单独一档（3.5KB）。
- 另有 `designated_heir_reason\00_standard.txt`（指定继承人的理由标签）。

## 二、特质（`traits\`，147 个 / 9 类别）

### 2.1 类别分布

| 文件 | 类别 | 数量 | 文件 | 类别 | 数量 |
|---|---|---|---|---|---|
| `00_ruler.txt` | ruler | **44** | `05_child.txt` | child | 10 |
| `01_general.txt` | general | 19 | `06_religious_figure.txt` | religious_figure | 10 |
| `02_admiral.txt` | admiral | 13 | `07_cabinet.txt` | cabinet | **21** |
| `03_artist.txt` | artist | 12 | `08_health.txt` | health | 13 |
| `04_explorer.txt` | explorer | 5 | | | |

### 2.2 字段权威（`_traits.info`）

```
trait_name = {
    category = ruler/general/admiral/artist/explorer    # 默认 ruler
    flavor = <trait flavor>
    allow = { }          # 成为有效选项的额外条件
    modifier = { }       # ★ 仅当角色"当前身份 = 该 trait 的 category"时生效
    chance = { }         # MTTH 风格：base = N + modifier = { factor = M <条件> }
                         # 结果经 GetCeiling() 取整 → 小于 1 的 factor 不会削弱几率（排除请用 allow）
    max_number_of_birth_siblings = 2   # 仅健康类：兄弟姐妹中已有 2 人得过 → 不再做出生判定
}
```

**三条容易踩的**：① `modifier` 与身份绑定（给 ruler 特质写效果，卸任即失效）；② `chance` 的取整行为（想降低概率要用 `allow` 排除，不能靠 <1 的 factor）；③ 健康类的"兄弟姐妹上限"字段是**原版唯一用到它的地方**。

### 2.3 获取途径（`NCharacter`）

| 常量 | 值 | 说明 |
|---|---|---|
| `CHILD_TRAIT_AGE` | 3 | 3 岁起可获儿童特质 |
| `YEARS_OF_RULING_FIRST_TRAIT` / `_SECOND_` / `_THIRD_` | **1 / 10 / 25** | **在位越久解锁越多统治者特质** |
| `CABINET_TRAIT_GAIN_CHANCE` | 2 | 内阁成员每月 2/100 |
| `LEADER_COMBAT_TRAIT_GAIN_CHANCE` | 0.5 | 将领：战斗中获得的传统 ×0.5 决定骰 |

疾病特质（`bubonic_plague_trait` 年死亡 0.55 / `smallpox_trait` 0.35 + 恢复特质 `pockmarked_trait`）在 `08_health.txt`，机制见 `vanilla\vanilla-hazards-and-environment.md` §5.1。

## 三、王朝（dynasty）

### 3.1 定义在开局数据里（`main_menu\setup\start\04_dynasties.txt`）

```
dynasty_manager = {
    default_dynasty = { name = { name = default_dynasty } home = vattnajokull }   # 兜底纹章用
    bjalbo_atten_dynasty = {
        name = { name = bjalbo_atten_dynasty }
        dynasty_name_type = location      # 姓氏来源类型（地名/父称…，与文化侧的 dynasty_name_type 对应）
        home = linkoping                  # 宗族发源地
        male_names   { name_birger name_eric name_magnus name_waldemar }
        female_names { name_catherine name_christine name_ingeborg name_rikissa }
    }
}
```

### 3.2 角色侧字段（`character_db`，`05_characters.txt`）

```
swe_birger_jarl = {
    first_name = { name = name_birger }
    culture = swedish
    religion = catholic
    adm = 97  dip = 54  mil = 91          # 三能力，0–100
    birth_date = 1210.1.1   death_date = 1266.10.21
    birth = vadstena                       # 出生地点
    ruler_trait = conqueror                # 可多条
    ruler_trait = cruel
    dynasty = bjalbo_atten_dynasty
    tag = SWE                              # 归属国
}
```

> ⚠️ **`05_characters.txt` 第一行的官方警告**："Remember to write **sons and daughters ALWAYS after their parents** to avoid crashes"——改开局人物时的第一条纪律。

### 3.3 机制与脚本面

- 词条：宗族 = **共同祖先与血缘**的角色群；其中最显赫者成为 **`dynasty_head` 宗族领袖**（AI 会按 `DYNASTY_HEAD_MARRY_AGE = 25` / 目标 20–32 岁为其择偶）。
- 脚本面很薄：**王朝 7 触发器 / 3 效果**——王朝机制实际靠**继承法 + 联姻 + 共主邦联**承载（后两者见外交篇）。
- 文化侧联动：`languages\` 的 `dynasty_names`（436 条语言有王朝名库）+ 文化的 `dynasty_name_type`（`descendant` / `patronym`，见文化与宗教篇）——**姓氏怎么拼由文化+语言决定，具体有哪些家族由 setup 决定**。

## 四、继承（`heir_selections\`，**44 种 / 5 文件**）

### 4.1 清单

| 文件 | 数量 | 代表 |
|---|---|---|
| `monarchy.txt` | **18** | cognatic / absolute_cognatic / salic / semi_salic primogeniture、partition_inheritance、**fratricide_succesion**（兄弟相残）、unigeniture、mamluk_succesion、youngest_son_of_ruler、tanistry_elective、elective_succession、dynastic_elective_succession、favorite_son_elective_succession、**union_of_crowns_succession**、austrian_cognatic_primogeniture、matrilineal_salic_law、judicial_election、byzantine_succession |
| `republic.txt` | 13 | oligarchic_elective、**republic_4_year_terms / republic_2_year_terms**、lottery_election、veche_selection、doge_election、diarchic_election、bank_selection、pirate_elective、peasant_elective、military_dictatorship、signoria_selection、podesta_elective |
| `theocracy.txt` | 6 | bishopric_elective、theocratic_elective、grandmaster_elective、abbot_elective、**abbess_elective_female**、papal_election |
| `specialized.txt` | 4 | admiralty_regime_heir_selection、general_heir_selection、guru_election、matrilineal_non_exclusive |
| `tribal.txt` | 3 | tribal_oldest_male、tribal_matrilineal、ritual_selection |

### 4.2 字段族（`00_heir_selections.info`）

| 组 | 字段 |
|---|---|
| 族谱遍历 | `traverse_family_tree`、`allow_extended_family`（含侄辈）、`include_ruler_siblings` |
| 候选范围 | `all_in_country`、`all_in_overlord`、`all_in_dynasty`、`include_other_countries`（跨国候选；**留空则整段跳过以省性能**）、`candidate_country`（选举候选国） |
| 资格 | `allow_children`（默认 no）、`through_female`、`allow_female`（默认 **no**）/ `allow_male`（默认 yes，"Sorry, but....history"）、`allow_foreign_ruler`（默认 yes）、`allowed_estates`（候选只能来自这些阶层）、`ignore_ruler`（选举 yes / 君主制 no）、`use_mothers_dynasty` |
| 选举 | `use_election`、`show_candidates`、`max_possible_candidates`、`term_duration`（月，默认 0） |
| 门与结算 | `potential` / `allowed` / **`locked`（为真时不能换继承法）** / `heir_is_allowed`、**`succession_effect`**（root = country；`scope:old_ruler` / `scope:new_ruler`）、`custom_tags` / `show_tags_in_ui` / **`custom_eligibility_rules`**（自定义解释键）、**`calc` 与 `sibling_score`**（候选人打分）、`cached`（性能） |

**打分用的是 `character_age` 而不是 `age_in_years`**——官方示例注释点明："同岁但生日不同也能分出先后"。

**词条**：`heir_selection_score`（**继承人分数**）衡量角色在某国继承中的受推崇程度，"通常分数最高的角色在继承发生时成为新统治者"；**多数继承法在继承前就定好继承人，另一些走选举**（`use_election`）；可用哪些继承法受**政体**等因素影响。

## 五、摄政（`regencies\`，**15 种**）

**触发**（词条原文）：政府**没有合法统治者**，或**继承人只是小孩**；君主制下**配偶可当摄政**，否则摄政是"与最强大阶层甚至宗主相关的角色"。

| 类别 | 类型 |
|---|---|
| 兜底 | `zz_default` |
| 按最强阶层 | `1_nobles_regency` / `2_clergy_regency` / `3_burghers_regency` / `4_peasants_regency` |
| 特殊身份 | `5_cabinet_head_regency`（**内阁首脑**）、`10_consort_regency`（配偶）、`9_overlord_regency`（宗主）、`11_subject_regency`、`12_lordship_of_ireland_regency` |
| 继承法专用（选举期） | `00_fratricide_succesion`、`00_judicial_election`、`00_mamluk_succesion`、`00_papal_election`、`00_republican_election` |

**字段只有 5 个**（readme 全文）：`start_effect`、`end_effect`、`allow`、`modifier`、`internally_assigned`。
关联交互：`make_regent_ruler`（摄政转正）。

## 六、内阁（`cabinet_actions\`，**73 个行动**）

### 6.1 席位与规模

| 机制 | 说明 |
|---|---|
| **内阁席位 `cabinet_seats`** | 由 `government_size` 决定；**革新逐步 +1**（`government_size = 1` 散落在 `advances\` 各时代） |
| 角色自然生成量 | `BASE_AMOUNT_FOR_COUNTRY = 1.25 × 内阁规模` |
| 三个状态修正 | `blocked_from_cabinet`、`cannot_be_removed_from_from_cabinet`、`is_head_of_cabinet` |
| 行动的能力分布 | **adm 35 / mil 21 / dip 17**（能力类型决定用哪个属性加值） |

### 6.2 ⚠️ 核心公式：readme 与本体冲突（差 10 倍）

readme 原文："所有修正都乘以 **1 + (有效能力 + 内阁效率) × 0.05**（`CABINET_ACTION_SKILL_MODIFIER` in defines）"——但 `defines` 里实际是：

```
CABINET_ACTION_SKILL_MODIFIER = 0.005
```

**以 defines 为准（0.005）**。这是本体 readme 与 defines 直接冲突的少数实例之一。

### 6.3 字段族（readme 109 行）

| 组 | 字段 |
|---|---|
| 能力与门 | `ability = adm/dip/mil`、`potential` / `allow`（root = country）、`is_finished`（root = country，scope:target = province） |
| 选目标 | `select_trigger`（与交互/和约同一套完整字段族：`looking_for_a` / `source` / `source_flags` / `column` / `map_mode` / `pre_evaluation_*` / 缓存三件套） |
| 时间与进度 | `allow_multiple`（可否同时进行多个）、`years/months/weeks/days`（**修正按完成度缩放**）、`progress` |
| **社会价值** | `societal_values`——**内阁行动可以直接推动社会价值轴** |
| 三作用域修正 | `country_modifier` / **`province_modifier`（注意是"省"）** / `location_modifier`；均为 **scaled + triggered**（`scale = <script>`、`potential_trigger = <trigger>`），修正值可引用脚本值 |
| 钩子 | `on_activate` / `on_fully_activated` / `on_deactivate`——**root = cabinet**，`scope:actor` = country、`scope:target`… |
| AI | `ai_will_do` |

**最小实例**（`stabilize_country.txt` 全文）：

```
stabilize_country = {
    ability = mil
    country_modifier = { stability_investment = cabinet_stability_investment }   # 值来自脚本值
    progress = { scope:actor = { value = stability } }
}
```

### 6.4 效率、成本与阶层联动

| 组 | 键 |
|---|---|
| 效率 | `character_cabinet_efficiency`（角色侧）/ **`country_cabinet_efficiency`（国家侧，与前者是两个键）**、`hire_for_cabinet_efficiency`、`cabinet_trait_impact_modifier` |
| 成本 | `set_cabinet_member_cost_modifier`、`replace_cabinet_member_cost_modifier`、`head_of_cabinet_promotion_cost_modifier`；价格来自 `prices\00_hardcoded.txt`：`set_cabinet_member = scaled_gold 1.0 / max_scale 100`、`replace_cabinet_member = stability 5`、`set_cabine_action = 免费` |
| **阶层 × 内阁（3 类 × 8 阶层）** | `*_estate_allowed_in_cabinet`、`*_estate_blocked_from_cabinet`、`*_estate_power_from_cabinet`；总开关 **`BASE_ESTATE_POWER_FROM_CABINET = 0.25`** |
| 性别门 | `block_male_cabinet` / `block_female_cabinet`、`allow_male_cabinet` / `allow_female_cabinet`、`ignore_gender_block_cabinet` |
| **内容开关** | `allow_cabinet_*` 系列修正（`naval_focus` / `soldiers_as_workforce` / `reduced_paperwork` / `assimilate_area` / `diplomatic_corps`）——**这些内阁行动必须靠修正解锁** |
| AI 相关 | `AI_CROWN_ESTATE_CABINET_MEMBER_FACTOR = 1.5`、`AI_ESTATE_BLOCKED_FROM_CABINET_UTILITY = -0.25`、`AI_PERFORMANCE_CABINET_ACTION_*`（地点抽样 20 / 省 5 / 区 1、每 36 月更新） |

## 七、角色交互（`character_interactions\`，**34 个**）

**readme 里三个必须先看的开关**：`on_other_nation`（能否对**他国**角色用，默认 **no**）、`on_own_nation`（本国，默认 **no**）、`is_consort_action`（**配偶专用**）——默认两个都是关的，忘了开就"点了没反应"。

其余字段与国交互同族：`message` / `sound` / `potential` / `allow` / price 四件套（`price` / `price_modifier` / `payer` / `payee`）/ `select_trigger` / `ai_tick` / `ai_will_do` / `effect` / `cooldown` / `show_message` / `show_message_to_target` / `show_in_gui_list`；比国交互多一个 **`allow_null_trigger`**（配合 `allow_null` 决定"空目标"是否入选）。

**34 个交互反映的玩法**：

| 类别 | 交互 |
|---|---|
| 继承与储位 | `appoint_as_heir`、`favor_heir`、`abdicate` |
| 婚姻 | `marry_noble`、`marry_lowborn`、`tribal_arrange_marriage` |
| 处置 | `execute_character`、`banish_character`、`pardon`、`seppuku`、`d008_mutilations` |
| 任官与宫廷 | `assign_governor`、`assume_fort_command`、`grant_cabinet_right`、`promote_to_head_of_cabinet`、`make_regent_ruler`、`move_children_to_court`、`ennoble`、`invite_prince`/`dismiss_prince` |
| 宗教与修会 | `take_the_vows`、`pap_reassign_cleric`、`choose_theologian_duelist`、`resign_as_grand_master`、`order_of_chivalry_actions` |
| 艺术 | `commission_art`、`dismiss_artist` |
| 专属内容 | `aclla_distribution`、`adapt_culture`、`appoint_dragoman`、`prefection`、`D008_assign_despot`、`D008_compose_strategikon` |

## 八、教育 · 艺术家 · 艺术品

### 8.1 儿童教育（`child_educations\`，**6 种**）

**⚠️ 本类目用"名字独占一行、`{` 写在下一行"的写法**（全 `common` 只有 3 个目录这样写：`child_educations` 6 处、`resolutions` 3 处、`biases` 1 处）——按"`key = {` 同行"的规则解析会**漏掉全部条目并把嵌套块误判为顶层**（实测会得出 16 而非 6）。

```
balanced_education =
{
    allow = { }                      # scope = character
    modifier = { character_child_education = 0.5 }
}
```

| 条目 | 要点 |
|---|---|
| `balanced_education` | `character_child_education = 0.5` |
| `administrative_education` | `character_child_education = 0.25` + **`character_adm_child_education = 0.5`** |
| `diplomatic_education` / `military_education` | 同上，分别指向 dip / mil |
| `expensive_in_depth_education` | 贵价深度教育（另有 `price_to_select` / `price_to_deselect`） |
| `orthodox_education`（D008） | `potential`（需 DLC + 东正教）、`allow`（须是**自治牧首区领袖**）、`modifier` `character_child_education = 0.8`、`country_modifier` **`monthly_religious_influence = -0.1`**、`on_education_start_effect`（可把角色**改宗**为宗主宗教） |

字段权威（readme）：`modifier`（角色修正）/ `country_modifier` / `price_to_select` / `price_to_deselect` / `potential` / `allow` / `on_education_start_effect`；`EDUCATION_AGE = 3`（3 岁起可选）。

### 8.2 艺术家与艺术品（`artist_types\` 13 种 / `artist_work\` 21 种）

**艺术家类型 13**：painter / sculptor / composer / writer / architect / philosopher / jurist / scientist / iconographer / metalsmith / calligrapher / storyteller / doctor。
字段（`artist_types\readme.txt`）：`potential`（哪些国家能有这种艺术家）+ `modifier`（**在宫廷期间施加的国家修正，挂在艺术家角色的 character modifier 上，死亡/解雇即移除**；同类型多个艺术家会**叠加**）。本地化键固定为 **`ARTIST_TYPE_NAME_<id>`** / **`ARTIST_TYPE_DESC_<id>`**。
**注意**：作品偏好**不在这里写**，而是写在各艺术品类型的 `allow = { artist_type = X }` 上。

**艺术品类型 21**：painting / miniature_painting / icon / scripture / statue / motet / symphony / hymnal / chronicle / novel / poem / play / mansion / monument / palace / temple_tower / treatise / weapon / regalia / prayer_book / oral_traditions。
字段（`artist_work\readme.txt`）：`captured`（能否被夺取）、`allow`（root = character；**写 `artist_type = X`**）、`location_modifier`、`country_modifier`、**`religion_scale_modifier`（作用于整个宗教！）**。

**机制意义**（词条原文）：艺术品完成后**直接增加艺术家所在国主流文化的 `cultural_influence`**（即文化战争的"攻击力"，见文化与宗教篇 §2.5）；艺术品**存放在某地点，该地被敌军占领时可能被摧毁或夺取**；持有期间持续给**威望与文化传统**。

## 九、耦合表

| 接到哪一层 | 具体 |
|---|---|
| 阶层（法律与阶层篇） | 内阁成员出身阶层 → `*_estate_power_from_cabinet`（总开关 `0.25`）；`*_estate_allowed/blocked_from_cabinet` 决定哪些阶层能入阁；阶层满意度影响角色 |
| 文化宗教（文化与宗教篇） | 角色的 culture / religion / 语言进随机生成偏向与好感；`dynasty_name_type` + `languages` 的 `dynasty_names` 决定姓氏；宗教人士（muslim_scholar / guru）、骑士团（15 个）、教廷封圣；D008 教育可改宗 |
| 外交（外交篇） | 王室联姻数值、`royal_ties` 接受度、共主邦联（因联姻共主）、`regency` 与摄政国；`send_officers` 等条约 |
| 战争（战斗与战争篇） | `general` / `admiral` 特质、将领取特质靠战功、军事能力进战斗（`BATTLE_WIN_CHANCE_GENERAL_MIL_FACTOR = 0.25`）；君王死亡 → 摄政/继承战争 |
| 科技时代（科技与时代篇） | 革新给 `government_size`（内阁席位）、外交容量与范围、`allow_cabinet_*` 解锁 |
| 疾病（自然环境篇） | 疾病给健康特质（腺鼠疫/天花/麻子）、`character_mortality_chance` |
| 正统性 | `legitimacy` 词条：人口与阶层对统治的接受度；低则叛乱、可能被换统治者 |

## 十、Mod 改造建议（可改 vs 硬编码）

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| 加特质 | `common\traits\<新文件>.txt` + 本地化 | `category` 决定"哪个身份才吃 modifier"；想降概率用 `allow` 而非 <1 的 factor |
| 加继承法 | `common\heir_selections\<新文件>.txt` | `calc` / `sibling_score` 用 `character_age`（含天数）；`locked` 可禁止切换 |
| 加摄政类型 | `common\regencies\<新文件>.txt` | 只有 5 个字段；谁当摄政靠 `allow` + `internally_assigned` |
| 加儿童教育 | `common\child_educations\<新文件>.txt` | **注意"名字独占一行"的写法**；修正键 `character_child_education` / `character_<能力>_child_education` |
| 加内阁行动 | `common\cabinet_actions\<新文件>.txt` | 修正按完成度缩放；`ability` 决定属性加值；**别忘了 `allow_cabinet_*` 之类的解锁修正（或用其它 `allow` 条件）** |
| 加角色交互 | `common\character_interactions\<新文件>.txt` | **必须显式写 `on_own_nation` / `on_other_nation`**（默认都是 no） |
| 加艺术家/艺术品 | `common\artist_types\` / `artist_work\` | 本地化键 `ARTIST_TYPE_NAME_/DESC_<id>`；作品偏好写在艺术品侧的 `allow` |
| 改开局人物 / 家族 | `main_menu\setup\start\05_characters.txt`（2.4MB）/ `04_dynasties.txt` | ⚠️ **子女必须写在父母之后**，否则崩溃；文件很大，改前备份 |
| 改年龄/生育/寿命/属性成长 | `NCharacter`（`defines` 1513–1573） | 全局生效 |
| 改外貌遗传 | `common\genes\` + `ethnicities\` | `blend_range` 控制子代在双亲之间的取值区间 |
| 改内阁规模 | `government_size`（修正 + 革新） | 席位数量与角色生成量都随之变化 |

**硬编码边界**：能力成长骰与年龄死亡骰的判定、继承分数的最终排序与选举流程、摄政的指派逻辑、`government_size` 与席位 UI 的绑定、肖像基因的渲染管线、内阁效率公式的合成（可改 `CABINET_ACTION_SKILL_MODIFIER` 与两个 efficiency 修正，公式在引擎）。

## 十一、中文检索键

**概念**（`game_concepts_l_simp_chinese.yml`）：`game_concept_character` 角色（:480）、`game_concept_ruler` 统治者（:86）、`game_concept_heir` 继承人（:495）、`game_concept_consort` 配偶（:490）、`game_concept_dynasty` **宗族**（:498）、`game_concept_dynasty_head` 宗族领袖、`game_concept_trait` **特质**（:1423）、`game_concept_ability` **能力**（:440）、`game_concept_adm` / `dip` / `mil` 行政/外交/军事能力（:775/779/783）、`game_concept_heir_selection` **继承法** + `heir_selection_score` **继承人分数**、`game_concept_regency` **摄政**（:1307）/ `regent` 摄政（:1470）、`game_concept_cabinet` **内阁**（:894）、`game_concept_cabinet_seats` **内阁席位**、`game_concept_cabinet_action` **内阁行动**（:886）、`game_concept_education` 教育（:1802）、`game_concept_marriage` 婚姻（:984）、`game_concept_royal_marriage` 王室联姻（:980）、`game_concept_artist` 艺术家（:899）、`game_concept_work_of_art` 艺术品（:912）、`game_concept_legitimacy` 正统性（:918）、`game_concept_prestige` 威望（:796）、`game_concept_court_language` 宫廷语言（:455）。

**界面**（`in_game\gui\`）：**`shared\character_tooltips.gui`（58KB）**、**`dynasty_tree_lateralview.gui`（55KB，宗族树）**、`character_lateralview.gui`（47KB）、`character_header.gui`、**`shared\cabinet_cards.gui`（22KB）**、`shared\portraits.gui`、`trait_item.gui`、`select_heir_selection.gui`、`panels\organization\marriage_union.gui`、`attribute_columns\character.gui`（10.8KB）/ `heir_selection.gui`（16KB）/ `cabinet.gui` / **`cabinet_action.gui`（19.5KB）**。

**本地化键前缀**：`ARTIST_TYPE_NAME_<id>` / `ARTIST_TYPE_DESC_<id>`（艺术家类型）；特质、继承法、内阁行动、教育均为 **id 同名键**（见 `tools\loc-keys.md`）。

**关联字段档**：`fields\` 侧的 **`common-traits.md`**（147 个特质全字段表）、**`common-heir_selections.md`**（44 种继承法）、**`common-regencies.md`**（15 种摄政）、**`common-child_educations.md`**（6 种教育）、**`common-death_reason.md`**（51 个死因 + `possible_parameter` → `DEATH_REASON_*` 键后缀规则）、**`common-genes-ethnicities.md`**（基因与 60 个族群）、`common-character_interactions.md`、`common-cabinet_actions.md`、`common-artist.md`、`common-avatars.md`、`common-estate_privileges.md`。

**上游（政体决定人选规则）**：`government_types` 的 `heir_selection` 白名单决定**这个政体能选哪些继承法**（君主国 23 种 / 共和 16 / 神权 9 / 汗国 3 / 部落 5），`default_character_estate` 决定角色默认阶层，`government_size` 决定内阁席位——详见 `vanilla\vanilla-government-and-reform.md`。
