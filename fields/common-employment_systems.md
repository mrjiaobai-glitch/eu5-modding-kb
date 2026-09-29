# common/employment_systems（就业系统）

> **一句话**：就业系统字段：国家修正、决定建筑先拿到雇员的优先级数值，以及 AI 选择使用哪个系统的评估值。
> **什么时候看**：调建筑雇员的分配顺序，或给就业系统加修正时看。
> **体量**：27 行 · 约 2 分钟通读

来源：`in_game\common\employment_systems\readme.txt`

## 用途

决定哪些建筑先获得雇员。

## 字段

```
<employment_system_id> = {
    country_modifier = <modifier>       # 使用该系统时国家获得的修正
    priority = <script value>           # 建筑优先级（数字大先处理：10 → 2 → 1）
    ai_will_do = <script value>         # AI 如何选择使用哪个系统
}
```

## 审查要点

- `priority` 数字越大越优先。
- 未在 readme 中说明：本地化键格式。
