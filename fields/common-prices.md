# common/prices（价格定义）

> **一句话**：价格定义的字段全表，列出可用于支付的各种资源与三个缩放钳制项，ID 以 price: 语法被引用。
> **什么时候看**：定义或引用 price:<id>、要确认某项成本能用哪些资源支付时翻这篇。
> **体量**：49 行 · 约 3 分钟通读

来源：`in_game\common\prices\readme.txt`

## 字段

```
<price id> = {
    scaled_manpower = <float>
    scaled_sailors = <float>
    scaled_gold = <float>
    scaled_recipient_gold = <float>
    gold_per_pop = <float>
    manpower = <float>
    sailors = <float>
    gold = <float>
    stability = <float>
    war_exhaustion = <float>
    inflation = <float>
    prestige = <float>
    army_tradition = <float>
    navy_tradition = <float>
    government_power = <float>
    karma = <float>
    religious_influence = <float>
    purity = <float>
    honor = <float>
    doom = <float>
    rite_power = <float>
    yanantin = <float>
    complacency = <float>
    righteousness = <float>
    harmony = <float>
    self_control = <float>
    min = <float>         # 修正可累加的最小量
    min_scale = <float>   # 修正前必须支付的最小量
    max_scale = <float>   # 修正前必须支付的最大量
}
```

## 审查要点

- 被其他类目引用时用 `price:<price_id>` 语法——ID 拼写错误静默失败。
- 未在 readme 中说明：无。
