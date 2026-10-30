# 参考资料：定局位置（settlement position）修改指南

> **来源**：[EU5 Wiki: Settlement position modding guide](https://eu5.paradoxwikis.com/Settlement_position_modding_guide)（`action=raw` 抓取，2026-10-30；页面标记 **1.0**，与 1.4.5 可能有差异）。
> **性质**：外部资料，**未实测**。
> **体量**：约 4 分钟

## 一、取新坐标
1. 开 debug mode，进游戏选中目标 **location**。
2. 控制台输入 **`copy_mouse_pos`**，**先别回车**；把鼠标移到想要的落点，再按回车 → 坐标进剪贴板（形如 `8475.943 3.1205864 6065.1533`）。
3. 控制台 `Ctrl+V` 可粘贴查看。

## 二、写覆盖坐标
大多数地点的定局位置是自动生成的，自定义坐标写在：
```
in_game\gfx\map\locators_override\locators_override.txt
```
条目格式（**一个地点要写两条**：`type = city` 管名称/圆点，`type = vfx` 管建筑模型）：
```
45 = {
	position = {8475.943 3.1205864 6065.1533}
	province = zalavar
	type = vfx
}
46 = {
	position = {8475.943 3.1205864 6065.1533}
	province = zalavar
	type = city
}
```
重进游戏后定局位置即改变。

## 三、消掉 locator 报错（不做出错但不致命）
日志会提示：
> Locator transform overrides modified. Make sure to update them with the command **[MapObjects.GenerateGameLocators city]** … 同理 vfx

处理：
1. 控制台分别执行 `MapObjects.GenerateGameLocators city` 和 `MapObjects.GenerateGameLocators vfx`；
2. 生成文件在 `Documents\Paradox Interactive\Europa Universalis V\`：`generated_locators_city` / `generated_locators_vfx`；
3. 目标文件是 `in_game\gfx\map\map_objects\generated_map_object_locators_city.txt` / `…_vfx.txt`；
4. **不能直接改名覆盖**：生成文件缺 layer 设置（第 6 行），要么把内容整段粘进原文件、要么粘贴后把第 6 行改成 `layer="vfx_layer"`（vfx）或 `layer="cities_layer"`（city）。

## 四、和本库的关系
- 地图层级/`map_data`：`vanilla\vanilla-map-and-geography.md`。
- 地图对象生成层（`content_source\map_objects\`）：`vanilla\vanilla-dlc-and-assets.md` §六。
- 控制台命令：`reference\console-commands.md`（`copy_mouse_pos`、`MapObjects.*` 都在里面）。
