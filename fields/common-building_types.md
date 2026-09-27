# common/building_types（建筑类型）

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
