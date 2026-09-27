# gfx/map/map_modes（地图模式）

来源：`in_game\gfx\map\map_modes\readme.txt`

## 字段

```
<map_mode_id> = {
    color_mode = <mode>                # 颜色模式（不用自定义模式时）
    map_color = <script color>         # root = location, scope:actor/recipient 等
    secondary_map_color = <script color>  # 条纹色；同上作用域
    tooltip_key = <script string key>  # tooltip 本地化键；同上作用域
    small_map_names / medium_map_names / large_map_names = <文本>   # 地图上显示的文本
    small_tooltip_context / medium_tooltip_context / large_tooltip_context = <tooltip context>
    color_and_names_refresh_counters = <counter 列表>  # 触发颜色与名字刷新的计数器
    color_refresh_counters = <counter 列表>            # 仅触发颜色刷新
    refresh_colors_on_selection_change = <yes/no>
    fill_in_impassable = <yes/no>      # 填充不可通行地形
    enable_snow = <yes/no>
    enable_water_names = <yes/no>
    enable_rivers = <yes/no>
    use_highlight = <yes/no>
    use_fow = <yes/no>                 # 战争迷雾
    allow_allocate_hotkey = <yes/no>
    quick_action_slot = <integer>
    map_lines_mode = <...>             # 硬编码
    override_border_rendering = <...>
    map_markers = <...>                # 允许/禁止的地图标记
    flatmap_behaviour = <...>
    flatmap_gradient_parameters = <...>
}
```

## 审查要点

- 未在 readme 中说明：各枚举的合法取值。
