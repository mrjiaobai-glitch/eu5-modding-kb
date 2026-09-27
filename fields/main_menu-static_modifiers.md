# main_menu/common/static_modifiers（静态修正）

来源：`main_menu\common\static_modifiers\readme.txt`（注意：位于 main_menu，不在 in_game）

## 字段

```
<static_modifier_key> = {
    game_data = {
        category = <location/country/unit/character/religion/mercenary/province/internationalorganization/rebel/dynasty>
        decaying = <yes/no>              # 默认 no；随时间变弱
        remove_if = <none/religion/culture>  # 默认 none；宗教/文化改变时自动移除
    }
    <modifiers>
}
```

## 审查要点

- `category` 是枚举（九选一）。
- 本地化键：`STATIC_MODIFIER_NAME_<名>` + `STATIC_MODIFIER_DESC_<名>`（见 SKILL.md 第 6 节）。
- 未在 readme 中说明：无。
