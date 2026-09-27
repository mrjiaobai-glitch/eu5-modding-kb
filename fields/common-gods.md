# common/gods（神祇）

来源：`in_game\common\gods\readme.txt`

## 字段

```
<god_id> = {
    religion = <religion key>        # 指定神适用的多个宗教/宗教组；两种格式：
    group = <group key>              #   格式1: religion = { religion = <键> name_key = <该宗教下神的名字 loc 键> }
                                     #   格式2: religion = <键>（名字就是神 id tag 本身时）
    potential = <trigger>            # root = country
    allow = <trigger>                # root = country
    years / months / weeks / days = <int>  # 完全生效时间；修正按比例缩放
    on_activate = <effect>           # root = country
    on_fully_activated = <effect>    # 100% 时
    on_deactivate = <effect>         # 移除时（root = country）
    country_modifier / province_modifier / location_modifier = <scaled & triggered modifier>
    is_female = <yes/no>             # UI 中显示 God 还是 Goddess；脚本用 is_god_female trigger 判断
}
```

## 效果

- `add_god = <god id>`、`remove_god = <god id>`

## 审查要点

- `religion`/`group` 两种写法（块形式带 name_key；裸键形式名字即 id）——块形式漏 name_key 则无名字。
- 未在 readme 中说明：本地化键格式。
