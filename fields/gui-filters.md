# gui/filters（脚本化过滤器）

> **一句话**：脚本化过滤器的字段全表与三个本地化键，含 exclusive_group 必须组内全写等硬性约束。
> **什么时候看**：给列表加筛选器、要配置标签分组与数值范围显示格式时翻这篇。
> **体量**：73 行 · 约 4 分钟通读

来源：`in_game\gui\filters\readme.txt`（**2,212 B——GUI 层唯一有实质内容的官方文档**）+ 18 个数据文件（27 KB）实查

## 字段（readme 全表）

```
<filter_tag> = {
    scope = <scope type>            # 该过滤器可操作的对象类型
    trigger = <trigger>             # 提供的 scope 对象是否通过过滤器
    range = {                       # 提供数值范围选择
        min = <值>
        max = <值>
        step = <值>
        format = <string>           # 显示文本格式
    }
    tag = <string>                  # 管道分隔的标签列表（"a|b|c"）；视图 .gui 用 WithFilterTags('x|y|z') 暴露交集
    group = <integer>               # 同一 UI 背景内的分组
    exclusive_group = <integer>     # 单选按钮组；**组内每个过滤器都必须写**；选中一个则取消上一个
    invert = <yes/no>               # 默认排除，勾选后包含
    enabled_at_start = <yes/no>     # 初始勾选状态
    hidden_in_searchbar = <yes/no>  # 仍生效并出现在侧边菜单/自动补全，但搜索栏不显示启用芯片
    group_sorting = <...>           # 组内排序；目前仅字母序
}
```

## 原版实测（18 文件 / 按对象类型分）

`05_location.txt`（3,955 B 最大）· `06_country.txt` · `07_unit.txt` · `08_subunit.txt` · `09_character.txt`（4,445 B）· `11_pop.txt` · `16_culture.txt` · `17_religion.txt` · `20_province.txt` · `28_rebel.txt` · `29_trade.txt` · `31_goods.txt` · `34_mercenary.txt` · `37_international_organization.txt` · `42_building.txt`（1,894 B）· `57_cabinet_action.txt` · `58_building_type.txt` + `readme.txt`。

**文件序号 = 作用域编号**（与 `common\attribute_columns\` 的编号体系一致，便于两边对照）。

实例（`06_country.txt`）：

```
country_only_within_diplomatic_range = {
    scope = country
    tag = diplomacy
    enabled_at_start = yes
    group = 1
    trigger = { exists = scope:target  scope:target = { within_diplomatic_range = root } }
}
```

作用域约定：**root = 玩家（Palyer 侧）**，**`scope:target` = 被筛选的对象**（原版文件头注释："root is player / scope:target is the country to filter"）。

## 本地化（readme 明确要求三个键）

| 键 | 用途 |
|---|---|
| `search_filter_<tag>_name` | 过滤器名 |
| `search_filter_<tag>_desc` | 描述 |
| `search_filter_<tag>_format` | 使用 `range` 时数字的显示格式 |

## 语义（readme 原文要点）

- **带子项的列表**：任一子项通过过滤器，则整个项通过（"if one of the sub-items pass the filter, the entire item pass it"）；之后引擎可再改写该项的数据或布局（如建筑宏建造器）。
- 检查顺序：**搜索栏过滤器 → 标签 → 分组**。
- **`tag` 省略 = 对所有视图可见**——readme 原文警告："OMITTING tag entirely makes the filter universally visible across every view of this scope — use only for filters that genuinely belong everywhere; **most filters should declare at least one tag**"。
- `hidden_in_searchbar` 的用途（readme 原文）：面板已经给了专用按钮（如 `estates_filter_button`）时，避免同一个功能出现两次。

## 审查要点

- **`exclusive_group` 必须组内每个过滤器都写**，漏一个就退化成普通多选（readme 全大写强调："MUST BE IN EVERY FILTER IN THE GROUP"）。
- 省略 `tag` 的过滤器会出现在所有视图——滥用是常见错误。
- `range` 的 `format` 只影响显示；真正的过滤逻辑仍在 `trigger` 里。
- 三个 loc 键缺任何一个，界面上就是 raw key。
- 未在 readme 中说明：无（本类目文档是 GUI 层最完整的）。
