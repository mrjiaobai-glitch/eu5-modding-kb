# common/ai_scripted_expansion_score 与 ai_scripted_expansion_target

> **一句话**：AI 扩张评估的两类脚本：一个为战争目标加减分，一个用候选列表让 AI 看到本会忽略的国家，含各字段作用域。
> **什么时候看**：要让 AI 主动打某个国家、或排查扩张目标不出现时看。
> **体量**：76 行 · 约 4 分钟通读

来源：`in_game\common\ai_scripted_expansion_score\readme.txt`、`in_game\common\ai_scripted_expansion_target\readme.txt`

## ai_scripted_expansion_score（AI 评估未来战争时为扩张目标加减分）

```
<name> = {
    attacker_potential = <trigger>      # root = attacker；为 false 则分数不生效
    target_trigger = <trigger>          # root = target, scope:attacker = attacker；为 false 则分数不生效
    score = <script value>              # scope:attacker + scope:target；在乘数应用前加到最终分（与 EXPANSION_TARGET_SCORE_NEEDED_TO_PICK define 相关）
    multiplier = <script value>         # scope:attacker + scope:target；乘在整个最终分上
    never_attack = <trigger>            # scope:attacker + scope:target；为 true 则最终分恒为 0
}
```

## ai_scripted_expansion_target（让 AI 评估本会忽略的国家）

```
<name> = {
    attacker_potential = <trigger>      # root = attacker；为 false 则分数不生效
    candidate_list = <effect>           # root = attacker；用 add_to_list = source 把候选对象填进列表
    casus_belli = <casus_belli>         # root = attacker, scope:target = target；可留空让 AI 自选
    ignore_antagonism = <yes/no>        # 是否忽略已有的高敌意国家组；只在宣战时检查，不在割地时检查
    score = <script value>              # scope:attacker + scope:target；设定扩张目标分
    sort_value = <script value>         # scope:attacker + scope:target；排序用；留空则同 score
}
```

## 审查要点

- 作用域约定固定：root = attacker（score/target 文件中）、`scope:attacker`/`scope:target` 为脚本值可用作用域——写错作用域（如用 `root.xxx` 取 target 数据）是常见错误。
- 目标类 `casus_belli` 引用须存在。
- 未在 readme 中说明：本地化、默认值。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\ai_scripted_expansion\` 全量 **3 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、取值白名单（本体出现过的值 + 次数）

- **`casus_belli`**（1 种）：casus_belli:cb_gag_force_country_into_faction（2）

### 二、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `ignore_antagonism` | 0 次（3 档） | 全库也没有 → 疑似废弃字段 |
| `never_attack` | 0 次（3 档） | 全库也没有 → 疑似废弃字段 |

### 三、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `add` | 10 | sort_value、score、if、multiplier |
| `is_member_of_international_organization` | 6 | target_trigger、attacker_potential |
| `multiply` | 5 | sort_value、score |
| `limit` | 4 | every_neighbor_country、every_subject_or_below、if |
| `add_to_list` | 4 | every_neighbor_country、every_international_organization_member |
| `situation_is_active` | 4 | situation:guelphs_and_ghibellines |
| `situation:guelphs_and_ghibellines` | 4 | attacker_potential |
| `AND` | 3 | OR |
| `scope:target` | 2 | limit |
| `if` | 2 | score |
| `every_international_organization_member` | 2 | international_organization:ghibellines_io、international_organization:guelphs_io |
| `every_neighbor_country` | 2 | every_subject_or_below、candidate_list |
| `ai_personality` | 2 | AND |
| `is_neighbor_of` | 1 | limit |
| `this` | 1 | NOT |
