# common/artist_types 与 common/artist_work

来源：`in_game\common\artist_types\readme.txt`、`in_game\common\artist_work\readme.txt`

## artist_types（艺术家学科门类）

```
<artist_type_id> = {
    potential = { <triggers> }   # 国家作用域 trigger；省略则所有国家可用（用于按文化/宗教/advance 门槛）
    modifier = { ... }           # 艺术家任职宫廷期间应用的国家修正；作为角色修正加在艺术家身上，死亡/解雇时移除；支持全部标准国家修正字段
}
```

- 本地化键：`ARTIST_TYPE_NAME_<id>`（名称）、`ARTIST_TYPE_DESC_<id>`（说明）。
- 一个国家可同时雇用多种门类的艺术家；同门类修正叠加。
- 作品亲和性（艺术家偏好哪类作品）由 **artist_work 侧**的 `allow = { artist_type = <id> }` 定义，不在这里。
- `dismiss_artist` 角色交互把艺术家从宫廷移除（代价：威望招募）。

## artist_work（艺术品类型）

```
<work_of_art_type> = {
    captured = <yes/no>                    # 能否被夺取
    allow = { <trigger> }                  # 是否允许该类型；root = character
    location_modifier = <modifier>         # 施加于艺术品所在 location
    country_modifier = <modifier>          # 施加于艺术品所有者
    religion_scale_modifier = <modifier>   # 施加于整个宗教
}
```

## 审查要点

- artist_types 的 `modifier` 是"角色修正"（施加于艺术家角色），artist_work 的三类 modifier 作用域不同（location/country/religion）——按目标检查作用域与键。
- artist_work 的 `allow` 中 root = character（不是 country）。
- 本地化前缀 `ARTIST_TYPE_NAME_`/`ARTIST_TYPE_DESC_` 缺失会显示 raw key。
- 未在 readme 中说明：artist_work 的本地化键格式。
