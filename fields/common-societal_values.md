# common/societal_values（社会价值观）

来源：**本体无 readme**——`in_game\common\societal_values\00_default.txt`（499 行 / **17 条轴**）实测反推；推动机制在 `main_menu\common\static_modifiers\country.txt`

## 字段

```
<left_side>_vs_<right_side> = {
    left_modifier = { <modifiers> }        # 100% 出现：靠左时的一整包修正
    right_modifier = { <modifiers> }       # 100% 出现：靠右时的一整包修正
    age = <age key>                        # 18%：从某时代起才存在
    allow = { <trigger> }                  # 18%：轴是否启用
    opinion_importance_multiplier = <float>  # 18%：AI 对他国该轴位置的观感权重
    content_priority = <int>               # 6%
}
```

## 原版 17 条轴

```
centralization_vs_decentralization        集权 / 分权
traditionalist_vs_innovative              传统 / 创新
spiritualist_vs_humanist                  虔诚 / 人文
aristocracy_vs_plutocracy                 贵族 / 财阀
serfdom_vs_free_subjects                  农奴 / 自由民
mercantilism_vs_free_trade                重商 / 自由贸易
belligerent_vs_conciliatory               好战 / 和解
quality_vs_quantity                       质量 / 数量
offensive_vs_defensive                    进攻 / 防御
land_vs_naval                             陆权 / 海权
capital_economy_vs_traditional_economy    资本 / 传统经济
individualism_vs_communalism              个人 / 集体
outward_vs_inward                         外向 / 内向
sinicized_vs_unsinicized                  汉化 / 未汉化
absolutism_vs_liberalism                  专制 / 自由主义
mysticism_vs_jurisprudence                神秘主义 / 法理
latinization_vs_hellenization             拉丁化 / 希腊化
```

两包修正的实例（集中在集权/分权轴）：

| 侧 | 修正 |
|---|---|
| 左（集权） | `global_crown_estate_power +0.5`、`global_distance_from_capital_speed_propagation +0.2`、`subject_loyalty −20`、`annexation_speed_modifier +0.33`、`control_importance_modifier +0.2` |
| 右（分权） | `global_estate_target_satisfaction` 提升、`global_estate_satisfaction_recovery +0.002`、`subject_loyalty +30`、`annexation_speed_modifier −0.33`、`control_importance_modifier −0.1` |

传统/创新轴还挂了 `cultural_tradition_modifier`、`embrace_institution_cost_modifier`、`bias_for_scholar_policies`、以及**思潮抗性** `national_lutheranism_movement_resistance_modifier` ——社会价值观与"思潮传播"（`common\movements\`）双向耦合。

## 焦点标签（`*_focus`）：34 个，**只有本地化键**

`government_l_<lang>.yml` 里有 35 个 `*_focus` 键（34 = 17 轴 × 2 侧 + 1 个杂项 `cab_naval_focus`），例如 `centralization_focus` / `decentralization_focus` / `spiritualist_focus` / `humanist_focus` / `plutocracy_focus`。

⚠️ **它们在 `common\` 与 `main_menu\common\` 里都没有定义块**——是**纯本地化标识**，被 `government_reforms`（51 处）、`laws`、`cabinet_actions`、`missions`、`advances` 当作"社会价值取向"引用（如改革的 `societal_values = { humanist_focus }`）。**查 focus 是否存在必须搜 yml**。

## 推动机制：`societal_value_push_*` 衰减静态修正

原版在 `main_menu\common\static_modifiers\country.txt` 定义 **30 个**：

```
societal_value_push_humanist = {
    game_data = { category = country  decaying = yes }
    monthly_towards_humanist = societal_value_huge_monthly_move
}
```

- `decaying = yes`（衰减型）+ 每月朝某一侧推进 → **改革、法律、事件推动社会价值观都是挂这个修正**
- 本地化键：`STATIC_MODIFIER_NAME_societal_value_push_humanist`（"Pressão Social por …" / "Societal Push for …"）
- 移动量级由 `script_values` 提供（原版如 `societal_value_minor_monthly_move` / `societal_value_huge_monthly_move`）

## 相关常量与触发器

| 常量 | 值 | 含义 |
|---|---|---|
| `SOCIAL_VALUE_REQUIREMENT_FOR_REFORM` | **50** | 改革要求在该轴上达到的位置 |
| `GOVERNMENT_REFORM_SOCIETAL_VALUE_REQUIREMENT_FORECAST_IN_MONTHS` | 12 | 界面预告窗口 |
| `..._FORECAST_CUTOFF_IN_MONTHS` | 24 | 预告上限 |

相关修正键：`societal_value_importance_modifier`（AI 重视度）、`bias_for_*_policies`（政策偏好，17 轴对应 11 组 bias 键）、各轴的 `monthly_towards_*`。

## 审查要点

- 轴的**内部名就是 `A_vs_B` 全串**（`change_societal_value = { type = centralization_vs_decentralization … }` 这样引用），写半截名不生效。
- `left_modifier` / `right_modifier` **两边都要给**（原版 100%）——只写一边会导致另一侧毫无效果。
- 推动社会价值**不要直接改数值**：正确做法是定义/复用一个 `societal_value_push_*` 衰减修正（`decaying = yes`），否则效果不会随时间消退。
- 焦点标签（`*_focus`）不在数据侧定义；mod 新增焦点只需补 loc 键，但**引用它的改革/法律要有对应的判定逻辑**。
- 未在 readme 中说明：**本类目没有 readme**；轴的取值范围、移动速率合成、界面显示阈值均在引擎侧。
