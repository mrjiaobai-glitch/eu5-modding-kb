# common/building_types（建筑类型）

> **一句话**：建筑类型的完整字段表：建造耗时、雇用规模与产出、可建范围与外国建筑条件、生产方式引用，以及各触发字段的作用域。
> **什么时候看**：新增或改动建筑、排查建造条件与修正作用域写错时翻这篇。
> **体量**：166 行 · 约 8 分钟通读

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

> **数据源**：`in_game\common\building_types\` 全量 **45 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `is_special` | 243 | 10 | yes（243） |
| `expensive` | 184 | 14 | yes（184） |
| `startup_ramp_target` | 110 | 28 | rural_startup_ramp_target（32）、guild_startup_ramp_target（22）、manufactory_startup_ramp_target（21）、mills_startup_ramp_target（18）、workshop_startup_ramp_target（17） |
| `graphical_tags` | 53 | 10 |  |
| `forbidden_for_estates` | 49 | 11 | yes（49） |
| `is_mill` | 18 | 18 | yes（18） |
| `want_foreign_pop_created` | 11 | 4 | no（11） |
| `ai_forbid_shutdown` | 9 | 5 | yes（9） |
| `can_close` | 8 | 4 | no（8） |
| `ai_foreign_ignore_naval_range` | 8 | 1 | yes（8） |
| `own_or_overlord_relation_needed` | 7 | 2 | trade_access（7） |
| `is_village` | 4 | 1 | yes（4） |
| `automation_build_allowed` | 4 | 1 | no（4） |
| `destroyable_building` | 4 | 1 | yes（4） |
| `ai_ignore_maintenance` | 3 | 2 | yes（3） |
| `allow_wrong_startup` | 3 | 1 | yes（3） |
| `convert_on_ownership` | 2 | 1 | hre_imperial_armory_foreign（1）、hre_imperial_armory_own（1） |
| `lifts_fog_of_war` | 1 | 1 | yes（1） |
| `ai_unique_location_list` | 1 | 1 |  |

### 二、取值白名单（本体出现过的值 + 次数）

- **`pop_type`**（8 种）：burghers（165）、soldiers（85）、clergy（69）、nobles（68）、laborers（65）、peasants（29）、slaves（7）、tribesmen（4）
- **`category`**（15 种）：cultural_category（70）、government_category（67）、trade_category（52）、military_category（46）、defense_category（44）、consumer_goods_category（37）、basic_industry_category（34）、estate_category（30）、religious_category（29）、infrastructure_category（23）、rgo_building_category（20）、weapons_industry_category（17）、naval_category（10）、colonial_category（9）、village_category（4）
- **`is_foreign`**（2 种）：no（440）、yes（43）
- **`audio_tier`**（6 种）：2（124）、3（121）、4（85）、1（76）、5（47）、6（20）
- **`city`**（2 种）：yes（448）、no（22）
- **`town`**（3 种）：yes（416）、no（35）、setup_only（4）
- **`megalopolis`**（2 种）：yes（448）、no（2）
- **`rural_settlement`**（3 种）：yes（168）、no（91）、setup_only（2）
- **`is_special`**（1 种）：yes（243）
- **`expensive`**（1 种）：yes（184）
- **`startup_ramp_target`**（5 种）：rural_startup_ramp_target（32）、guild_startup_ramp_target（22）、manufactory_startup_ramp_target（21）、mills_startup_ramp_target（18）、workshop_startup_ramp_target（17）
- **`forbidden_for_estates`**（1 种）：yes（49）
- **`estate`**（5 种）：nobles_estate（11）、peasants_estate（9）、burghers_estate（8）、clergy_estate（6）、tribes_estate（1）
- **`need_good_relation`**（2 种）：yes（30）、no（3）
- **`stronger_power_projection`**（2 种）：no（17）、yes（4）
- **`is_mill`**（1 种）：yes（18）
- **`price`**（11 种）：expand_rgo_farming（3）、free_building（2）、orthodox_monastery_building（2）、miaphysite_monastery_building（2）、hre_army_building（2）、expand_aqueduct_system（1）、expensive_estate_building（1）、calvinist_preachers_building（1）、lutheran_preachers_building（1）、build_hippodrome_price（1）、merchant_guild_chapel_price（1）
- **`AI_ignore_available_worker_flag`**（1 种）：yes（12）
- **`important_for_AI`**（1 种）：yes（11）
- **`want_foreign_pop_created`**（1 种）：no（11）
- …另有 20 个枚举字段，见完整普查报告

### 三、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `content_priority` | 1 | 9 |

### 四、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `ai_construct_weight` | 0 次（45 档） | 全库也没有 → 疑似废弃字段 |
| `ai_destroy_weight` | 0 次（45 档） | 全库也没有 → 疑似废弃字段 |
| `audio_category` | 0 次（45 档） | **有**（出现在 1 个类目） |
| `output` | 0 次（45 档） | **有**（出现在 2 个类目） |

### 五、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `on_construction_started` | 1 | 1 |
| `on_construction_ended` | 1 | 1 |

### 六、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `category` | 360 | shipyard_maintenance、bajang_ratu_maintenance、fine_cloth_mill_maintenance、dyesmill_maintenance |
| `icon` | 360 | shipyard_maintenance、bajang_ratu_maintenance、fine_cloth_mill_maintenance、dyesmill_maintenance |
| `icon_type` | 360 | shipyard_maintenance、bajang_ratu_maintenance、fine_cloth_mill_maintenance、dyesmill_maintenance |
| `produced` | 219 | millet_brewery_maintenance、alum_fine_cloth_workshop_maintenance、bavarian_beer_workshop_maintenance、wool_weavers_maintenance |
| `output` | 219 | millet_brewery_maintenance、alum_fine_cloth_workshop_maintenance、bavarian_beer_workshop_maintenance、wool_weavers_maintenance |
| `debug_max_profit` | 218 | millet_brewery_maintenance、alum_fine_cloth_workshop_maintenance、bavarian_beer_workshop_maintenance、wool_weavers_maintenance |
| `always` | 201 | location_potential、custom_tooltip、country_potential、can_destroy |
| `OR` | 144 | limit、custom_tooltip、any_pop、location_potential |
| `tools` | 140 | star_fort_maintenance、bund_maintenance、leather_livestock_workshop_maintenance、weapon_workshop_maintenance |
| `lumber` | 113 | stockade_maintenance、weapon_workshop_maintenance、trans_saharan_trade_outposts_maintenance、alum_dyes_guild_maintenance |
| `NOT` | 91 | remove_if、custom_tooltip、root.owner、allow |
| `this` | 78 | location_potential、custom_tooltip、OR、any_culture_in_culture_group |
| `paper` | 77 | funduq_maintenance、riksbank_maintenance、venetian_palaces_maintenance、porto_pisano_maintenance |
| `custom_tooltip` | 69 | allow、trigger_else、location_potential、OR |
| `text` | 68 | custom_tooltip |

### 七、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `content_priority` | 1 | 9 |
