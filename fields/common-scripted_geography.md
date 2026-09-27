# common/scripted_geography（脚本化地理集合）

来源：`in_game\common\scripted_geography\scripted_geography.info`（**1,685 B**）+ 3 个数据文件（15 KB）实查

## 字段

```
<地理集合名> = {
    area = { <area 键…> }
    province_definition = { <预设省份键…> }
    region = { … } / sub_continent = { … } / continent = { … } / location = { … }
}
```

**定位**：把**任意层级的**地理对象（location / province_definition / area / region / sub_continent / continent）打包成一个**可引用的命名词条**——info 的话："groupings of geography (locations, province_definitions, areas etc.) that can include any of the hierarchical components"。

原版实例（info 原文）：

```
borneo_geography = {
    area = { north_borneo_area  south_borneo_area }
    province_definition = { riau_islands_province }
}
```

## 三件配套能力（info 全部列出）

### ① 专属脚本列表

```
scripted_geography:borneo_geography = {
    every_location_in_scripted_geography = { … }   # 遍历集合内所有地点
    every_area_in_scripted_geography = { … }       # 遍历集合内的 area（含其下层级所覆盖的）
}
```

### ② 判定触发器

```
scope:target_location = { is_in_scripted_geography = scripted_geography:borneo_geography }
scope:target_country  = { has_presence_in = scripted_geography:borneo_geography }
```

`is_in_scripted_geography` 可用在 **province_definition / area / region / sub_continent / continent** 六种作用域上（info 明确）。

### ③ 界面与本地化

- `[ShowScriptedGeographyName(Arg0)]` 在本地化里显示集合名；
- **若要在游戏里可见，集合键必须有 loc 键**（info 结尾："If scripted geography is to be used visibly in game, it should have `<key>` localized"，例：`borneo_geography: "Borneo"`）。

## 原版实测（3 数据文件 / 15 KB）

kb 的 `vanilla\vanilla-map-and-geography.md` 已统计：**27 个地理包**，含长城、大运河、英苏尼德兰等历史区域；6 个作用域都能用 `is_in_scripted_geography`。

## 审查要点

- 集合内的键**必须是真实地理 id**（来自 `map_data\definitions.txt` 的层级树）；写错不报错，只是集合为空。
- 要在地图上/界面上显示，**必须有同名 loc 键**。
- `has_presence_in` 与 `is_in_scripted_geography` 的作用域不同（前者是**国家**，后者是**地理对象**），别混用。
- 未在 readme 中说明：本类目**没有 readme**，只有 info；集合能否嵌套其它集合未文档化。
