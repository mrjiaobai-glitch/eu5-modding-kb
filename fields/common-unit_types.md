# common/unit_types（单位类型）

来源：`in_game\common\unit_types\readme.txt`

## 字段

```
<unit_type_id> = {
    age = <age>                          # 启用时代
    build_time = <int>
    upgrades_to = <another unit type>    # 可选；升级路径
    buildable = <yes/no>
    levy = <yes/no>                      # 能否用于征召
    default = <...>                      # readme 未说明取值
    construction_demand = <goods demand>
    maintenance_demand = <goods demand>
    use_ship_names = <yes/no>
    assault = <yes/no>                   # 能否强攻要塞
    bombard = <yes/no>                   # 能否轰击要塞
    auxiliary = <yes/no>                 # 视为辅助单位
    category = <unit_category>           # 所属类别
    location_trigger = { <location_triggers> }   # location 满足才可招募
    location_potential = { <location_triggers> } # location 满足才显示
    country_potential = { <country_triggers> }   # 国家满足才显示
    mercenaries_per_location = { pop_type = <pop type> multiply = <proportion> }
    limit = { <value calculations> } / <default_value>  # 该类型单位数量上限
    combat = { <topography/vegetation/climate/coastal/inland/river> }  # 特定地形伤害修正
    impact = { <topography/vegetation/climate/coastal/inland/river> }  # 特定地形移动速度修正
    copy_from = <unit_type>              # 复制模板全部数值，可后续再改
    gfx_tags = {}
    color = <color>                      # 覆盖视觉主色
    <combat modifiers>                   # 同 unit_categories 的三组修正
}
```

## 审查要点

- `category` 引用须在 common/unit_categories 存在；`copy_from`/`upgrades_to` 引用须存在。
- `combat`/`impact` 的键是地形枚举（topography/vegetation/climate/coastal/inland/river）。
- 未在 readme 中说明：本地化键格式。
