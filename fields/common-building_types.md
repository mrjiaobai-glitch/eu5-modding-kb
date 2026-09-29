# common/building_types（建筑类型）

> **一句话**：建筑类型的完整字段表：建造耗时、雇用规模与产出、可建范围与外国建筑条件、生产方式引用，以及各触发字段的作用域。
> **什么时候看**：新增或改动建筑、排查建造条件与修正作用域写错时翻这篇。
> **体量**：155 行 · 约 8 分钟通读

来源：`in_game\common\building_types\readme.txt`

## 属性

```
<building_type> = {
    build_time = <integer>                    # 建造时间（天）
    employment_size = <float>                 # 可雇用人数；1 = 1000 人
    output = <float>                          # 每级建筑产出
    is_foreign = <yes/no>                     # 能否建在外国领地
    in_empty = <empty/owned/any>              # empty=仅非自有 location；owned=仅自有；any=任何地方
    stronger_power_projection = <yes/no>      # 建在外国领地需比地主国更强的 power projection
    need_good_relation = <yes/no>             # 建在外国领地需与地主国关系良好
    conversion_religion = <religion>          # 该建筑把 pop 改信成的宗教
    pop_type = <pop type>                     # 雇用的 pop 类型
    category = <building category>            # 建筑类别 tag
    construction_demand = <goods>             # 建造所需商品
    possible_production_methods = { <production_methods> }  # 每槽位生产方式（可多个列表）
    unique_production_methods = { x = { } }   # 直接脚本进建筑的独特生产方式
    obsolete = <building type>                # 使哪种建筑过时
    price = <price>; destroy_price = <price>
    estate = <estate type>                    # 哪个阶层可建
    max_levels = <scripted integer>           # 最大等级；root = location, scope:owner, scope:builder
    international_organization_link = <IO type>  # 建筑恒归 IO 领袖、IO 被毁则销毁、IO 有钱则代付
    allow = <trigger>                         # 能否建造（root = location, scope:actor = 建造国）；"启用"检查
    location_potential = <trigger>            # 能否建造（root = location）；"可见"检查
    country_potential = <trigger>             # 国家能否建造（root = country）；"可见"检查
    international_organization_potential = <trigger>  # IO 能否建造（root = IO, scope:actor = 国家）
    can_destroy = <trigger>                   # root = location, actor = 摧毁者, building = 建筑
    is_indestructible = <yes/no>              # 按钮与效果都无法摧毁；remove_if 仍可删除
    remove_if = <trigger>                     # 是否自动销毁（root = building）
    capital_modifier = <modifier>             # 首都时施加于 location（×等级×goods access）
    capital_country_modifier = <modifier>     # 首都时施加于国家
    capital_to_overlord_modifier = <modifier> # 施加于宗主（仅建筑所有者是附庸时）
    foreign_country_modifier = <modifier>     # 施加于当前所有国（仅 is_foreign = yes）
    modifier = <modifier>; raw_modifier = <modifier>  # location；后者不缩放
    market_center_modifier = <modifier>       # 市场中心时施加于 location
    pop_size_created = <float>                # 新建筑创建 pop（从首都取；仅外国建筑）
    increase_per_level_cost = <percent>       # 每级成本增幅；0.5 = 每级贵 50%
    <location rank>: <yes/no>                 # 可建造的 location 等级
    on_built = { <effects> }; on_destroyed = { <effects> }
    always_add_demands = <yes/no>             # 未雇满也要求完整需求
    custom_tags = { <strings> }
    AI_ignore_available_worker_flag = <yes/no>  # AI 缺 pop 类型也会建造
    important_for_AI = <yes/no>; important_for_UI = <yes/no>
    audio_category = <string>                 # 事件名 "construction_building_<audio_category>_<audio_tier>"
    audio_tier = <int 1..6>                   # 越界在 PostReadInit 时 ERRORLOG；默认 1
}
```

## 审查要点

- `max_levels` 是 **scripted integer**（不是字面数字），作用域 root=location/scope:owner/scope:builder。
- 触发类字段作用域各异（allow 的 root=location；remove_if 的 root=building），写错作用域是高频错误。
- `is_foreign = yes` 才用 `foreign_country_modifier`；`in_empty` 三值语义不同，勿混用。
- `international_organization_link` 引用须存在，且该 IO 须允许链接建筑。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\building_types\` 全量 **44 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `is_special` | 224 | 9 | yes（224） |
| `expensive` | 168 | 14 | yes（168） |
| `startup_ramp_target` | 110 | 28 | rural_startup_ramp_target（32）、guild_startup_ramp_target（22）、manufactory_startup_ramp_target（21）、mills_startup_ramp_target（18）、workshop_startup_ramp_target（17） |
| `graphical_tags` | 51 | 10 |  |
| `forbidden_for_estates` | 47 | 11 | yes（47） |
| `is_mill` | 18 | 18 | yes（18） |
| `ai_foreign_ignore_naval_range` | 8 | 1 | yes（8） |
| `can_close` | 7 | 3 | no（7） |
| `own_or_overlord_relation_needed` | 6 | 1 | trade_access（6） |
| `ai_forbid_shutdown` | 6 | 3 | yes（6） |
| `want_foreign_pop_created` | 5 | 2 | no（5） |
| `is_village` | 4 | 1 | yes（4） |
| `destroyable_building` | 4 | 1 | yes（4） |
| `automation_build_allowed` | 3 | 1 | no（3） |
| `allow_wrong_startup` | 3 | 1 | yes（3） |
| `ai_ignore_maintenance` | 3 | 2 | yes（3） |
| `convert_on_ownership` | 2 | 1 | hre_imperial_armory_own（1）、hre_imperial_armory_foreign（1） |
| `AI_optimization_flag_coastal` | 2 | 2 | yes（2） |
| `lifts_fog_of_war` | 1 | 1 | yes（1） |
| `ai_unique_location_list` | 1 | 1 |  |
| `content_priority` | 1 | 1 | 100（1） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`category`**（15 种）：cultural_category（67）、government_category（60）、trade_category（45）、military_category（44）、defense_category（41）、consumer_goods_category（37）、basic_industry_category（34）、estate_category（30）、religious_category（25）、infrastructure_category（22）、rgo_building_category（20）、weapons_industry_category（17）、naval_category（10）、colonial_category（9）、village_category（4）
- **`pop_type`**（8 种）：burghers（157）、soldiers（81）、clergy（65）、laborers（64）、nobles（61）、peasants（26）、slaves（7）、tribesmen（4）
- **`is_foreign`**（2 种）：no（414）、yes（42）
- **`audio_tier`**（6 种）：2（119）、3（115）、1（76）、4（75）、5（42）、6（20）
- **`city`**（2 种）：yes（422）、no（21）
- **`town`**（3 种）：yes（391）、no（34）、setup_only（4）
- **`megalopolis`**（2 种）：yes（422）、no（2）
- **`rural_settlement`**（3 种）：yes（161）、no（90）、setup_only（2）
- **`is_special`**（1 种）：yes（224）
- **`expensive`**（1 种）：yes（168）
- **`startup_ramp_target`**（5 种）：rural_startup_ramp_target（32）、guild_startup_ramp_target（22）、manufactory_startup_ramp_target（21）、mills_startup_ramp_target（18）、workshop_startup_ramp_target（17）
- **`forbidden_for_estates`**（1 种）：yes（47）
- **`estate`**（5 种）：nobles_estate（10）、peasants_estate（10）、burghers_estate（8）、clergy_estate（6）、tribes_estate（1）
- **`need_good_relation`**（2 种）：yes（30）、no（3）
- **`stronger_power_projection`**（2 种）：no（17）、yes（4）
- **`is_mill`**（1 种）：yes（18）
- **`price`**（11 种）：expand_rgo_farming（3）、orthodox_monastery_building（2）、miaphysite_monastery_building（2）、free_building（2）、hre_army_building（2）、expensive_estate_building（1）、expand_aqueduct_system（1）、calvinist_preachers_building（1）、lutheran_preachers_building（1）、build_hippodrome_price（1）、merchant_guild_chapel_price（1）
- **`always_add_demands`**（1 种）：yes（11）
- **`important_for_AI`**（1 种）：yes（10）
- **`AI_ignore_available_worker_flag`**（1 种）：yes（9）
- …另有 21 个枚举字段，见完整普查报告

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `audio_category` | 0 次（44 档） | **有**（写在别的类目） |
| `output` | 0 次（44 档） | **有**（写在别的类目） |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `on_construction_ended` | 1 | 1 |
| `on_construction_started` | 1 | 1 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `category` | 343 | naval_supplies_workshop_maintenance、fortress_rice_granary_maintenance、usa_national_mint_maintenance、rice_winery_manufactory_maintenance |
| `output` | 214 | guns_guild_lumber_maintenance、crude_quarry_maintenance、guns_workshop_iron_maintenance、fur_trim_guild_maintenance |
| `produced` | 214 | guns_guild_lumber_maintenance、crude_quarry_maintenance、guns_workshop_iron_maintenance、fur_trim_guild_maintenance |
| `debug_max_profit` | 213 | guns_guild_lumber_maintenance、crude_quarry_maintenance、guns_workshop_iron_maintenance、fur_trim_guild_maintenance |
| `always` | 179 | country_potential、can_destroy、location_potential、custom_tooltip |
| `tools` | 134 | shoen_quarries_maintenance、shipyard_maintenance、brewery_mill_maintenance、glass_guild_maintenance |
| `OR` | 129 | culture、any_pop、location_potential、location |
| `lumber` | 108 | scriptorium_maintenance、brewery_mill_maintenance、glass_guild_maintenance、north_sea_shipyards_maintenance |
| `NOT` | 83 | international_organization_potential、location_potential、root.owner、location |
| `custom_tooltip` | 68 | country_potential、location_potential、trigger_else、OR |
| `paper` | 67 | university_maintenance、scriptorium_maintenance、moscow_artillery_yard_maintenance、usa_national_mint_maintenance |
| `text` | 67 | custom_tooltip |
| `this` | 60 | NOR、any_culture_in_culture_group、location_potential、OR |
| `has_or_had_tag` | 52 | country_potential、OR、custom_tooltip、and |
| `owner` | 52 | NOT、location_potential、location、OR |
