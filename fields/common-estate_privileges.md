# common/estate_privileges（阶层特权）

来源：`in_game\common\estate_privileges\readme.txt`

## 字段

```
<privilege_id> = {
    estate = <estate type tag>      # 适用的阶层
    potential = <trigger>           # 行动是否可能（root = country）
    allow = <trigger>               # 行动能否开始（root = country）
    years / months / weeks / days = <int>  # 完全生效时间；修正按完成比例缩放
    on_activate = <effect>          # 选择时（root = country）
    on_fully_activated = <effect>   # 100% 时（无延迟则立即）
    on_deactivate = <effect>        # 移除时（root = country）
    country_modifier = <scaled & triggered modifier>  # 施加于整个国家
    province_modifier = <scaled & triggered modifier> # 施加于省份
    location_modifier = <scaled & triggered modifier> # 施加于 location
    can_revoke = <trigger>          # 何时可以撤销已实施的特权
}
```

## 审查要点

- `estate` 引用须在 common/estate_types 存在。
- 未在 readme 中说明：本地化键格式。
