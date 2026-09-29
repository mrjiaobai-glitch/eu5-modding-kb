# common/prices（价格定义）

> **一句话**：价格定义的字段全表，列出可用于支付的各种资源与三个缩放钳制项，ID 以 price: 语法被引用。
> **什么时候看**：定义或引用 price:<id>、要确认某项成本能用哪些资源支付时翻这篇。
> **体量**：99 行 · 约 5 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\prices\` 全量 **7 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `legitimacy` | 13 | 3 | 10（6）、20（2）、25（1）、40（1）、30（1） |
| `ignore_inflation` | 12 | 1 | yes（12） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`stability`**（12 种）：10（31）、20（18）、50（16）、30（5）、5（5）、40（5）、15（2）、2（1）、25（1）、100（1）、1（1）、400（1）
- **`prestige`**（8 种）：10（22）、5（9）、25（5）、30（4）、20（4）、15（4）、50（4）、1（1）
- **`religious_influence`**（12 种）：20（11）、50（10）、10（8）、25（7）、30（4）、15（4）、100（3）、75（2）、80（1）、1（1）、5（1）、33（1）
- **`government_power`**（8 种）：5（11）、10（10）、50（4）、25（3）、20（3）、30（2）、-5（1）、15（1）
- **`manpower`**（9 种）：1（12）、0（6）、0.04（3）、0.02（3）、0.25（2）、0.01（2）、0.1（1）、0.250（1）、4（1）
- **`max_scale`**（9 种）：500（9）、1000（5）、300（3）、200（2）、400（2）、250（2）、350（1）、1500（1）、100（1）
- **`honor`**（7 种）：10（7）、30（3）、70（3）、40（2）、20（2）、50（2）、100（1）
- **`sailors`**（5 种）：1（10）、0.02（4）、0.01（2）、0.250（1）、0.25（1）
- **`legitimacy`**（7 种）：10（6）、20（2）、25（1）、40（1）、30（1）、5（1）、15（1）
- **`ignore_inflation`**（1 种）：yes（12）
- **`scaled_manpower`**（5 种）：0.1（2）、0.05（1）、12（1）、0.15（1）、0.01（1）
- **`righteousness`**（3 种）：10（4）、40（1）、20（1）
- **`scaled_sailors`**（4 种）：0.1（2）、0.15（1）、0.05（1）、0.01（1）
- **`war_exhaustion`**（3 种）：1（2）、-2（1）、2（1）
- **`karma`**（3 种）：10（2）、20（1）、-25（1）
- **`scaled_recipient_gold`**（2 种）：1（2）、2（1）
- **`navy_tradition`**（3 种）：5（1）、10（1）、20（1）
- **`yanantin`**（1 种）：-0.01（3）
- **`army_tradition`**（2 种）：5（2）、10（1）
- **`min_scale`**（2 种）：5（2）、25（1）
- …另有 2 个枚举字段，见完整普查报告

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `complacency` | 0 次（7 档） | **有**（写在别的类目） |
| `gold_per_pop` | 0 次（7 档） | 全库也没有 → 疑似废弃字段 |
| `harmony` | 0 次（7 档） | 全库也没有 → 疑似废弃字段 |
| `inflation` | 0 次（7 档） | **有**（写在别的类目） |
| `min` | 0 次（7 档） | **有**（写在别的类目） |
| `rite_power` | 0 次（7 档） | **有**（写在别的类目） |
| `self_control` | 0 次（7 档） | 全库也没有 → 疑似废弃字段 |
