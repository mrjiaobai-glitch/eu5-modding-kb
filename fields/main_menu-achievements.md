# main_menu/common/achievements（成就）

来源：`main_menu\common\achievements\readme.txt`

## 字段

```
<achievement_id> = {
    possible = { <triggers> }   # 开局时过滤，避免总检查
    happened = { <triggers> }   # 检查是否达成
}
```

## 分组（readme 声明）

- 把成就加进所选组（very easy / easy / medium / hard / very hard），文件在 `game\main_menu\common\achievement_groups.txt`。

## 审查要点

- 成就须加入 achievement_groups.txt 的某个组，否则不显示。
- 未在 readme 中说明：本地化键格式。
