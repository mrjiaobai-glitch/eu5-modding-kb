# common/recruitment_method（招募方式）

> **一句话**：招募方式的五个字段：strength、experience、build_time、army 与 default 默认标记。
> **什么时候看**：想改招募出来的部队强度、经验或建造耗时，或新增招募方式时翻这篇。
> **体量**：24 行 · 约 2 分钟通读

来源：`in_game\common\recruitment_method\readme.txt`

## 字段

```
<recruitment method id> = {
    strength = <float>
    experience = <float>
    build_time = <float>
    army = <yes/no>
    default = <yes/no>
}
```

## 审查要点

- 未在 readme 中说明：语义细节、本地化键格式。
