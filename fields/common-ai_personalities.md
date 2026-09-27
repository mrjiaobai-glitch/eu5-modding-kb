# common/ai_personalities（AI 国家性格）

来源：**本体无 readme**——文件名 `in_game\common\ai_personalities\00_ai_personalities.txt`（5 229 B）自带的文件头注释即权威，字段表由该文件 8 个性格的实际用法反推。

## 字段

```
<personality id> = {
    default = yes            # 默认性格（原版只有 ai_balanced 带；未分配性格的国家用它）
    color = hsv { H S V }    # 地图模式/界面配色（原版一律 hsv）
    modifier = {             # AI 行为修正块（普通修正类型，可叠加）
        <ai modifier type> = <值>
    }
}
```

## 三层语义（文件头注释原文归纳）

| 层 | 含义 | 原版用到的键 |
|---|---|---|
| **Willingness 意愿** | 多 eager 地找仗打 | `aggressiveness_modifier`、`carefulness_modifier`、`win_war_chance_threshold`、`ai_months_between_wars`、`ai_require_cb_for_war` |
| **Tolerance 忍耐** | 为目标肯吃多少痛 | `war_declaration_stab_hit_tolerance`、`war_declaration_war_exhaustion_tolerance`、`coalition_strength_tolerance`、`expected_warscore_modifier`、`peace_offer_fairness`、`peace_offer_negotiation_power` |
| **Priorities 优先** | 看重什么资源与结果 | `*_importance_modifier`（manpower/stability/control/defence/diplomacy/trade/gold/institution/religious_unity/societal_value）、`bias_for_*_policies`、`ai_opinion_bias`、`subjugation_preference_modifier`、`dynastic_acquisition_preference_modifier`、`ai_force_annexation_modifier`、`mercenary_units_preference_modifier`、`unintegrated_land_expansion_penalty_modifier`、`revoke_privileges_importance_modifier`、`ai_stability_target_modifier`、`ai_government_power_target_modifier` |

文件头另有一句关键约束：这些修正**与特质、社会价值观的 AI 修正叠加**（"stack additively with trait and societal value modifiers"）——所以性格不是唯一变量，同一性格在不同特质/价值下表现不同。

## 原版实测（8 种）

| id | 中文 | 关键数值 |
|---|---|---|
| `ai_balanced` | 平衡（`default = yes`） | `carefulness +0.05`、`win_war_chance_threshold +0.05`、`ai_months_between_wars 6` |
| `ai_aggressive` | 侵略 | `aggressiveness +0.3`、`carefulness −0.2`、间隔 **−12**、`coalition_strength_tolerance +0.25`、`ai_force_annexation +0.5`、`dynastic_acquisition −0.7` |
| `ai_expansionist` | 扩张 | `aggressiveness +0.2`、间隔 −6、`subjugation_preference +0.15`、`bias_for_colonialist_policies +0.15` |
| `ai_defensive` | 防御 | `aggressiveness −0.2`、`carefulness +0.25`、`defence_importance +0.25` |
| `ai_cautious` | 谨慎 | `carefulness +0.3`、`win_war_chance_threshold 0.15`、间隔 **+18**、`ai_require_cb_for_war = yes`、`war_declaration_stab_hit_tolerance −2` |
| `ai_opportunistic` | 投机 | `gold_importance +0.2`、`trade_importance +0.15`、`peace_offer_negotiation_power +0.15` |
| `ai_isolationist` | 孤立 | `aggressiveness −0.25`、间隔 **+24**、`diplomacy_importance −0.2`、`bias_for_isolationist_policies +0.3` |
| `ai_friendly` | 友善 | `peace_offer_fairness +0.25`、`diplomacy_importance +0.25`、`ai_force_annexation −0.5`、`dynastic_acquisition_preference_modifier 1.0` |

8 个性格合计引用 **35 个修正类型**，全部定义在 `main_menu\common\modifier_type_definitions\00_modifier_types.txt`（该文件 2393 个修正类型里，**48 个带 `ai=yes`** 标记为 AI 专用）。

## 分配规则

`main_menu\common\game_rules\00_game_rules.txt:467` 的 `ai_personalities` 规则：默认 `ai_personalities_historical`，另有三档随机（`ai_personalities_random` / `_random_per_age` / `_random_per_ruler`）。概念词条（`game_concepts_l_simp_chinese.yml:2097`）原文：「性格在游戏开局时设置，只能通过事件或游戏规则进行更改。」

## 本地化

性格名与描述为 **id 同名键**：`ai_aggressive` / `ai_aggressive_desc`（`main_menu\localization\<lang>\ai_personalities_l_simp_chinese.yml`，原版 8 种全有）。同文件另有 `AI_PERSONALITY_MODIFIERS`（"行为修正"）与地图模式键 `ai_personality_mapmode`。

## 审查要点

- `color` 用 `hsv { … }`（不是 rgb），漏掉只影响地图模式着色。
- `default = yes` 只能有一个；新增性格不会自动被分配，要改的是游戏规则或事件。
- 修正名必须存在于 `modifier_type_definitions`——`ai_yes` 标记的 48 个 + `ai_require_cb_for_war` + `ai_opinion_bias` 是安全区，写别的普通修正会生效但**不是 AI 行为语义**。
- 未在 readme 中说明：本类目**根本没有 readme**，字段与语义以本文件 + 文件头注释为准。
