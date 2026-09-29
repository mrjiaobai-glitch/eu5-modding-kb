# common/cultures、culture_groups、languages、religions 等（文化·宗教字段库）

> **一句话**：文化宗教字段库：文化、文化组、语言、语族、宗教、学派、宗教人士与骑士团八类，逐字段标注原版出现率与必填判断。
> **什么时候看**：改文化、宗教、语言名字库或学派骑士团，判断字段必填还是可选时看。
> **体量**：196 行 · 约 9 分钟通读

覆盖来源：**本档是本库唯一"非 readme 来源"的字段文档**——文化有 `in_game\common\cultures\00_cultures.info`、文化组有 `00_culture_groups.info` 两个官方示例文件（下称 .info），而**宗教、语言、学派、骑士团没有任何 readme/.info**，字段表全部来自**对原版实查**（括号内为该字段在原版的出现数 / 总数，可作为"必填 or 罕见"的判断依据；统计用花括号深度解析，见 `pitfalls.md` §十三）。

相邻 readme 已覆盖的系统见另档：`common-religion.md`（religious_aspects / factions / focuses）、`common-gods.md`、`common-holy_sites.md`、`common-movements.md`、`common-town_rights.md`。

## cultures（文化，2087 个 / 53 文件）

权威：`cultures\00_cultures.info`

```
my_culture = {
    language = my_language        # language or dialect —— 必填（2087/2087）
    color = map_ENG
    tags = { catalan_gfx swedish_gfx european_gfx }
    country_modifier = { }        # 该国主流文化为此文化时
    location_modifier = { }       # 该地点优势文化为此文化时
    character_modifier = { }      # 角色文化为此文化时
    opinions = { danish_gfx = enemy }   # enemy/negative/neutral/positive/kindred
    culture_groups = { polish_group slavic_group }   # 可多、可省（47 个省略）
    suppress_no_pops_error = yes  # 默认 no；历史/未来文化允许启动时无 POP
}
```

| 字段 | 出现数 / 2087 | 说明 |
|---|---|---|
| `language` | **2087（100%）** | **必填**：指向 `common/languages` 的键；缺则无名字库（角色/王朝/地名生成依赖它） |
| `color` | 2087（100%） | named color |
| `tags` | 2087（100%） | gfx 标签，如 `{ catalan_gfx european_gfx }` |
| `opinions` | 2074 | 键可为**文化名**或 **gfx 标签**（.info 例子用的是 `danish_gfx`）；值五档 |
| `culture_groups` | 2040（97.7%） | **可选**；多组时约定"越独特越靠前"；组本身不列成员 |
| `use_patronym` | 49 | yes/no，名字带父称 |
| `goods_demand_modifier` | 7 | 该文化 POP 的商品需求 |
| `dynasty_name_type` | 5 | `descendant` / `patronym`（`british.txt:101/165`） |
| `country_modifier` / `location_modifier` / `character_modifier` | 各 4 | 三作用域修正 |
| `active` | 1 | `active = no` = 休眠文化（`italian.txt:320` 的 `roman_culture`） |
| `noun_keys` / `adjective_keys` | 各 1 | 文化名构词表（`british.txt:30/36`，由 adder/falcon/baron、black/red/yellow 组合） |
| `suppress_no_pops_error` | 0 | 仅见于 .info（原版无使用） |

## culture_groups（文化组，209 个 / 1 文件）

权威：`culture_groups\00_culture_groups.info`（全文只有三个修正块）

```
culture_group = {
    country_modifier = { }     # 主流文化属于该组时
    location_modifier = { }    # 优势文化属于该组时
    character_modifier = { }   # 文化属于该组时
}
```

`in_game\common\culture_groups\00_culture_groups.txt` 只有 1 个文件；组名惯例 `*_group`（如 `slavic_group`、`mapudungun_group`）。

## languages（语言与方言，528 个 / 53 文件）

无 readme。字段实测（按出现数）：

| 字段 | 出现数 | 说明 |
|---|---|---|
| `male_names` | 446 | 男名列表（空格分隔或换行） |
| `color` | 439 | — |
| `female_names` | 439 | 女名列表 |
| `dynasty_names` | 436 | 王朝名 |
| `lowborn` | 436 | 平民名（无王朝时用） |
| `family` | 295 | 家族名 |
| `dialects` | 31 | **子块**：方言可继承/覆盖父语言 |
| `character_name_short_regnal_number` | 23 | 短称号序号写法 |
| `character_name_order` | 23 | 名/姓顺序 |
| `location_prefix` | 18 | 地名前缀 |
| `patronym_suffix_son` / `_daughter` | 15 / 13 | 父称后缀 |
| `patronym_prefix_son` / `_daughter` | 10 / 10 | 父称前缀（含 `_vowel` 变体） |
| `location_suffix` | 6 | 地名后缀 |
| `fallback` | 4 | 缺名字时的回退语言 |
| `location_prefix_vowel` | 4 | 元音前的前缀变体 |
| `ship_names` | 3 | 舰名 |
| `dynasty_template_keys` | 3 | — |
| `descendant_prefix(_female/_male)` / `descendant_suffix(_female/_male)` | 各 2 | 后代称谓 |
| `location_prefix_elision` / `_ancient` / `_ancient_vowel` | 2 / 1 / 1 | 地名省音与古体 |
| `first_name_conjoiner` / `require_genitive_location_names` / `patronym_suffix` | 各 1 | 连接词 / 属格地名 / 统一父称 |

**硬性检查**：define `NCulture|MINIMUM_NAMES_PER_LANGUAGE = 10`——"男名或女名低于 10 个就报错"。

## language_families（语族，56 个 / 1 文件）

只有 `color`；用于同化加成（`ASSIMILATION_SAME_LANGUAGE_FAMILY_MODIFIER = 0.1`）与运动的 `required_language_families`。

## religions（宗教，293 个 / 29 文件）

**无 readme / 无 .info** —— 下表全为实查。

| 字段 | 出现数 / 293 | 说明 |
|---|---|---|
| `group` | **293（100%）** | 宗教组键（`common/religion_groups`） |
| `color` | **293（100%）** | — |
| `definition_modifier` | **293（100%）** | 该宗教的国家级修正（含 `monthly_religious_influence`、`maximum_religious_influence`、`can_have_monasteries`、`country_allow_canonization`、`cannot_declare_no_cb_wars_on_religion_head` 等） |
| `religious_aspects` | 268 | **信条槽位数**（`max_religious_aspects`），不是开关；catholic 无此字段 → 信条行动不可用 |
| `opinions` | 248 | 对其它宗教的五档观感 |
| `tags` | 135 | gfx/内容标签（如 `protestant`，被 `is_protestant_religion` 检查） |
| `language` | 26 | **礼仪语言**（`church_dialect` 等） |
| `has_religious_influence` | 17 | 启用宗教影响力货币 |
| `unique_names` | 16 | 专属名 |
| `enable` | 12 | **启用日期**；宗教改革系全部 `9999.1.1`（由局势 `enable_religion` 启用） |
| `custom_tags` | 9 | — |
| `has_canonization` | 7 | 封圣系统 |
| `has_karma` | 6 | 业力（佛教系 + 达摩系） |
| `goods_demand_modifier` / `clergy_goods_demand_modifier` | 5 / 4 | 商品需求（全体 / 教士） |
| `max_sects` | 5 | 宗派槽位（佛教系） |
| `max_religious_figures_for_religion` | 5 | 宗教人士上限 |
| `ai_wants_convert` | 4 | AI 改宗倾向（`= no` 用于阻止"非新教"宗教主动传教） |
| `has_autocephalous_patriarchates` | 4 | 自治牧首区 |
| `religious_school` | 3（可重复行） | 可用学派列表（ibadi 18 / shia 16 / sunni 21 行） |
| `has_patriarchs` | 2 | 牧首 |
| `culture_locked` | 2 | 绑定文化（以色列系） |
| `tithe` | 1 | 什一税（catholic `0.02`） |
| `has_cardinals` | 1 | 枢机（catholic） |
| `has_religious_head` | 1 | 有宗教领袖 |
| `important_country` | 1 | 宗教领袖国（catholic = PAP） |
| `use_icons` | 1 | 圣像系统（orthodox，配 `religious_icon_power_modifier`） |
| `needs_reform` | 1 | 改革呼声系统（catholic） |
| `religious_focuses` | 1 | 焦点列表（tonal，配 `RELIGIOUS_FOCUS_COST = 100`） |
| `num_religious_focuses_needed_for_reform` | 1 | 完成多少焦点触发改革 |
| `factions` | 1 | 派系（佛教系，配 `common/religious_factions`） |
| `saints_concept` | 1 | 圣人概念（`folk_european`） |
| `has_omens` | 1 | 神谕（`folk_european`） |
| `has_avatars` | 1 | 化身（达摩系） |
| `has_yanantin` | 1 | 二元互补（`folk_peruvian`） |
| `has_honor` / `has_purity` | 各 1 | 荣誉 / 洁净（佛教系） |

## religion_groups（宗教组，29 个 / 1 文件）

| 字段 | 出现数 / 29 | 说明 |
|---|---|---|
| `color` | 29 | — |
| `convert_slaves_at_start` | 28 | 开局奴隶是否随主改宗 |
| `allow_rgo_slave_demand` | 20 | 允许原产使用奴隶（写在各组自己的 `modifier` 里 20 次） |
| `modifier` | 20 | 组级国家修正 |
| `allow_slaves_of_same_group` | 3 | 同组成员可否为奴 |
| `clergy_goods_demand_modifier` | 3 | 教士商品需求（如基督教组 `wine = 10`） |
| `goods_demand_modifier` | 1 | 组级商品需求（穆斯林组把 `wine/beer/liquor` 设为 0） |

## religious_schools（学派，43 个 / 6 文件）

无 readme。结构（`hinduism.txt`）：

```
samkhya_school = {
    color = rgb { 54 13 0 }
    enabled_for_country   = { religion = religion:hindu }    # 国家可用条件
    enabled_for_character = { religion = religion:hindu }    # 角色可用条件
    modifier = { tolerance_heathen = 0.5 tolerance_heretic = 0.5 }
}
```

6 个文件：`hinduism.txt`（3 学派）/ `jain.txt` / `ibadi.txt` / `shia.txt` / `sufi.txt` / `sunni.txt`；**只有 ibadi / shia / sunni 三个宗教用 `religious_school = X` 重复行挂载**（共 43 个学派被引用）。效果 `set_school_opinion`，触发器 `school_opinion` / `has_religious_schools`。

## religious_figures（宗教人士，2 个定义 / 2 文件）

```
muslim_scholar = {
    enabled_for_religion = { group = religion_group:muslim }   # 或 religion = <id>
}
```

仅 `00_muslim.txt`（`muslim_scholar`）与 `01_hindu.txt`（`guru`，`this.group = religion_group:dharmic`）。
配套：宗教的 `max_religious_figures_for_religion`、修正 `number_of_allowed_religious_figures`、价格 `invite_religious_figure_same_school`（`scaled_gold 0.2 / max_scale 250`）与 `..._different_school`（`0.4 / 500`）、`dismiss_religious_figure`（稳定 10）、define `RELIGIOUS_FIGURE_CHANCE_OF_MOVING = 0.01`。

## chivalric_orders（骑士团，15 个 / 3 文件）

无 readme。结构（`01_historical_orders.txt`）：

```
order_of_saint_george = {
    potential = { has_or_had_tag = HUN }
    country_modifier = { nobles_estate_levy_size = 0.1 }
    character_modifier = { mil = 15 monthly_prestige = 0.05 ... }
    character_eligible = { is_adult = yes is_female = no }
}
```

文件：`01_historical_orders.txt` / `02_german_societies.txt` / `03_western_orders.txt`；`potential` 可用 `always = no` 关掉某团。

## 审查要点

- **文化必须写 `language`**（原版 2087/2087）；`culture_groups` 可省（原版缺 47 个，全是孤立民族）。
- **`opinions` 的键允许是 gfx 标签**（`.info` 示例 `danish_gfx = enemy`），不要当成笔误删掉。
- **`religious_aspects = N` 是槽位数**：写了 `religious_aspects = 0` 或省略，`add/change/remove_religious_aspect` 三个通用行动的 `potential`（`religion.max_religious_aspects > 0`）就不通过。
- **`enable = 9999.1.1` 是新教系的正常写法**——不是假数据，别"顺手修正"成历史日期；那会切断 `situation:reformation` 的启用链。
- **`religious_school` 是重复行**（不是列表块）：`religious_school = X` 写多行；只在 `sunni`/`shia`/`ibadi` 三处出现。
- 宗教缺 `group` 会掉出所有组级机制（组修正、奴隶规则、商品需求、组观感）；原版 293 个全部有 `group`。
- `languages` 每个语言至少 10 个男名/女名，否则 `MINIMUM_NAMES_PER_LANGUAGE` 报错。
- 宗教/文化/语言的**本地化键**：与 id 同名（见 `tools\loc-keys.md`）；文化组、学派、宗教人士同理。
