# common/formable_countries（可成立国家）

> **一句话**：可成立国家字段：等级与必需地点比例、离谱度规则筛选、可见与可点条件、成立效果，以及名称旗帜等标识项。
> **什么时候看**：加可成立国家，或核对成立条件与名称旗帜等标识齐备时翻这篇。
> **体量**：98 行 · 约 5 分钟通读

来源：`in_game\common\formable_countries\readme.txt`

## 字段

```
<formable_id> = {
    level = <int>                    # 国家等级；玩家/AI 只能成立更高等级的国家
    required_locations_fraction = <float>  # 需要的 location 比例；默认 1.0（100%）
    capital_required = <yes/no>      # 必需地点列表是否必须包含当前首都；默认 no
    potential_requires_own = <yes/no>  # 是否必须拥有列表中的 location 才能在列表看到；默认 yes
    rule = <historical/plausible/fantasy>  # 游戏规则中"多离谱"的筛选；默认 historical
    potential = { <trigger> }        # 能否看到按钮；root = 当前国家
    allow = { <trigger> }            # 能否点按钮；root = 当前国家
    form_effect = { <effect> }       # 点击成立时发生；root = 当前国家
    name = <string>
    flag = <string>
    adjective = <string>
    tag = <string>
    color = <map color>              # 若未设置则保留旧国家地图色
    continents = { ... }             # 必需大陆列表
    sub_continents = { ... }
    regions = { ... }
    areas = { ... }
    locations = { ... }
}
```

## 审查要点

- `rule` 枚举：historical/plausible/fantasy。
- `name`/`flag`/`adjective`/`tag` 四者齐备；adjective 通常是 loc 键形式（如 EXA_ADJ）。
- 未在 readme 中说明：本地化键格式。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\formable_countries\` 全量 **1 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、取值白名单（本体出现过的值 + 次数）

- **`level`**（5 种）：2（65）、3（52）、1（23）、4（19）、5（3）
- **`rule`**（3 种）：historical（118）、plausible（35）、fantasy（9）
- **`capital_required`**（2 种）：no（9）、yes（4）
- **`content_priority`**（1 种）：800（6）
- **`potential_requires_own`**（1 种）：no（3）

### 二、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `content_priority` | 6 | 9 |

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `example_f` | 0 次（1 档） | 全库也没有 → 疑似废弃字段 |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `provinces` | 23 | 1 |
| `ai_will_do` | 1 | 1 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `culture` | 193 | OR、potential、allow、AND |
| `NOT` | 83 | custom_tooltip、region:iberia_region、OR、potential |
| `OR` | 81 | any_international_organization、trigger_else、potential、c:LIT |
| `owns` | 72 | AND、allow、OR |
| `limit` | 66 | else_if、trigger_if、every_international_organizations_member_of、every_ownable_location_in_province_definition |
| `if` | 54 | form_effect、if |
| `set_country_rank_effect` | 31 | form_effect、if |
| `religion.group` | 20 | AND、potential、allow、OR |
| `religion` | 17 | potential、allow、OR |
| `custom_tooltip` | 15 | trigger_if、allow、OR |
| `text` | 15 | custom_tooltip |
| `always` | 15 | custom_tooltip、potential、allow |
| `tag` | 15 | OR |
| `culture.language` | 14 | potential、allow、OR |
| `has_culture_group` | 14 | culture、potential、OR |

### 六、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `content_priority` | 6 | 9 |
