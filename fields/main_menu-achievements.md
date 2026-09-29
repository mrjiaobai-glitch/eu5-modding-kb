# main_menu/common/achievements（成就）

> **一句话**：成就的两个触发器字段（possible 与 happened），以及把成就加入成就组文件的分组要求。
> **什么时候看**：加成就、要写达成条件或决定它归入哪个难度组时翻这篇。
> **体量**：26 行 · 约 2 分钟通读

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
