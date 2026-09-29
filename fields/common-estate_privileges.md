# common/estate_privileges（阶层特权）

> **一句话**：阶层特权字段：适用阶层、可用门槛、生效时长与三类缩放修正，以及撤销已实施特权的条件字段。
> **什么时候看**：写阶层特权，或核对阶层引用与实施时长时翻这篇。
> **体量**：86 行 · 约 4 分钟通读

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

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\estate_privileges\` 全量 **7 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、原版在用、readme 未声明的字段

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `content_priority` | 17 | 3 | 800（3）、200（3）、400（3）、700（2）、100（2） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`estate`**（7 种）：nobles_estate（89）、burghers_estate（56）、clergy_estate（42）、peasants_estate（32）、tribes_estate（16）、dhimmi_estate（13）、cossacks_estate（13）
- **`content_priority`**（9 种）：800（3）、200（3）、400（3）、700（2）、100（2）、300（1）、500（1）、600（1）、900（1）
- **`months`**（1 种）：3（1）

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `days` | 0 次（7 档） | **有**（写在别的类目） |
| `on_fully_activated` | 0 次（7 档） | **有**（写在别的类目） |
| `province_modifier` | 0 次（7 档） | **有**（写在别的类目） |
| `weeks` | 0 次（7 档） | 全库也没有 → 疑似废弃字段 |
| `years` | 0 次（7 档） | **有**（写在别的类目） |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `ai_weight` | 4 | 1 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `global_nobles_estate_power` | 93 | country_modifier |
| `nobles_estate_target_satisfaction` | 83 | country_modifier |
| `global_burghers_estate_power` | 58 | country_modifier |
| `has_or_had_tag` | 57 | limit、OR、potential、any_overlord_or_above |
| `NOT` | 50 | allow、custom_tooltip、scottish_clans、autonomous_scottish_clans |
| `burghers_estate_target_satisfaction` | 48 | country_modifier |
| `has_variable` | 48 | OR、custom_tooltip、autonomous_scottish_clans、potential |
| `global_clergy_estate_power` | 45 | country_modifier |
| `OR` | 42 | allow、custom_tooltip、potential_trigger、potential |
| `clergy_estate_target_satisfaction` | 38 | country_modifier |
| `peasants_estate_target_satisfaction` | 33 | country_modifier |
| `global_peasants_estate_power` | 32 | country_modifier |
| `has_unlocked_estate_privilege_trigger` | 31 | potential、allow |
| `potential_trigger` | 25 | location_modifier、country_modifier |
| `monthly_towards_decentralization` | 24 | country_modifier |
