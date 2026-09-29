# common/rival_criteria（宿敌标准）

> **一句话**：宿敌标准：每个 tag 一个 enabled 块，用国家触发器限定该 tag 允许与谁结为宿敌，仅 AI 遵守。
> **什么时候看**：想限制或放开某国 AI 的宿敌选择范围时翻这篇。
> **体量**：27 行 · 约 2 分钟通读

来源：`in_game\common\rival_criteria\readme.txt`

## 字段

```
<tag> = {
    enabled = {
        <country triggers>   # 应用于潜在宿敌国家
    }
}
```

## 语义

- 决定该 tag 允许与谁结为宿敌；**只有 AI 遵守**这些标准——AI 只宿敌满足条件者。
- root = 定义的 tag（自身）；enabled 内的 trigger 施加于**潜在宿敌目标**。

## 审查要点

- 未在 readme 中说明：本地化键格式。
