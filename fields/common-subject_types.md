# common/subject_types（附庸类型）

来源：`in_game\common\subject_types\readme.txt`

## 触发类字段（作用域各异，重点核对）

```
<subject_type> = {
  visible=<trigger>(root=overlord,target=subject); enabled=<trigger>(root=overlord,target=potential subject)
  visible_through_diplomacy=<trigger>(root=overlord,target=subject); enabled_through_diplomacy=<trigger>(root=overlord,target=potential subject)
  visible_through_treaty/enabled_through_treaty=<trigger>(可选;root=overlord,target,recipient=和约对象,war)
  creation_visible=<trigger>(root=overlord); subject_creation_enabled=<trigger>(root=overlord,target_province=潜在地理)
  release_country_enabled=<trigger>(root=overlord,target=potential subject)
  can_attack=<trigger>(root=subject type,overlord/subject/attacker/defender)
  can_rival/can_marry=<trigger>(root=subject type,overlord/subject/actor/recipient)
  allow_declaring_wars=<trigger>(root=subject type,scope:attacker,scope:defender)
  join_offensive_wars_always/auto_call/can_call=<trigger>(root=subject type,scope:actor=caller,scope:recipient=callee,scope:target)
  join_defensive_wars_always/auto_call/can_call=<trigger>(同上)
}
```

## 数值/枚举字段

```
  minimum_opinion_for_offer=<int>; type=<location/pop/building/army>
  overlord_modifier/subject_modifier=<modifier>; great_power_score_transfer=<float>; government=<government key>
  level=<int>(低等级自主度高;经验:3=可吞并附庸,0=名义附庸)
  can_be_annexed=<yes/no>(默认yes); annexation_speed=<float script value>(默认1)
  annexation_min_years_before=<int>; annexation_min_opinion=<int>; annexation_stall_opinion=<int>
  subject_pays=<price>(附庸每月付宗主); diplomatic_capacity_cost_scape=<float>(非 scriptvalue)
  subject_can_cancel/overlord_can_cancel=<yes/no>; will_join_independence_wars=<yes/no>
  fleet_basing_rights=<yes/no>; food_access=<yes/no>; use_overlord_laws=<yes/no>(共用法律政策)
  on_overlord_becomes_a_subject=<cancel_subjects/transfer_subjects/nothing>(默认nothing)
  annulled_by_peace_treaty=<yes/no>(默认yes); annullment_favours_required=<int>
  use_overlord_map_color/use_overlord_map_name=<yes/no>; only_overlord_culture/only_overlord_or_kindred_culture/only_overlord_court_language=<yes/no>
  can_overlord_recruit_regiments/_build_ships/_build_roads/_build_buildings/_build_rgos=<yes/no>
  overlord_share_exploration=<yes/no>; overlord_protects_external=<yes/no>(默认yes); overlord_protects_other_subjects=<yes/no>(默认no)
  counts_as_external=<yes/no>(默认no); can_be_force_broken_in_peace_treaty=<yes/no>; overlord_can_enforce_peace_on_subject=<yes/no>
  war_score_cost=<script value>; base_antagonism=<script value>(≤0 用代码计算值)
  monthly_favor_gain=<script value>(root=人情方,scope:overlord,scope:subject)
  diplo_chance_accept_subject/diplo_chance_accept_overlord=<tag=values 列表>(如 border_distance=-0.1)
  ai_wants_to_be_overlord/ai_wants_to_be_subject=<effect script>(scope:overlord,scope:subject)
  institution_spread_to_overlord/institution_spread_to_subject=<script value>
  has_overlords_ruler=<yes/no>(宗主统治者兼任附庸统治者;创建时宗主无统治者则附庸脱离联合进摄政)
  has_overlords_religion=<yes/no>; has_limited_diplomacy=<yes/no>; can_change_rank=<yes/no>; can_change_heir_selection=<yes/no>
  on_enable=<effect>(root=subject,future_overlord=overlord); on_disable=<effect>(root=subject,former_overlord=overlord)
  on_monthly=<effect>(root=subject)
}
```

## 审查要点

- 各 trigger root 不同（visible 是 overlord；can_attack 是 subject type）——混用作用域是高频错误。
- `type` 枚举：location/pop/building/army；`on_overlord_becomes_a_subject` 枚举：cancel_subjects/transfer_subjects/nothing。
- 未在 readme 中说明：本地化键格式（实测见 SKILL.md 第 6 节：顶层键即 loc 键 + `LEAD_<名>`/`AM_<名>`）。

## subject_pays 实现（属国每月付宗主，2026-09 实测）

- 字段写法：`subject_pays = <price 键>`（原版裸键引用，如 `subject_pays = subject_pays_vassal`）。
- price 定义在 `in_game\common\prices\`（原版 03_diplomacy.txt 顶部）：`scaled_gold = 0.2` = 附庸月收入 20% 自动转宗主；可配 `scaled_manpower`/`scaled_sailors` 同时抽人力/水手；`ignore_inflation = yes` 防通胀缩放。原版比例：vassal 0.2 / colonial 0.025(+水手人力 0.1) / tributary 0.2(+0.05+0.05) / march 0.1 / trade_company 0.5 / pronoia 0.2。
- mod 自定义：新建 `in_game\common\prices\zzz_*.txt` 定义自有 price（scaled_gold 比例或 `gold = N` 固定额）→ subject type 里改指。
- 注意与 overlord_modifier 的 `monthly_gold_income`（拨款/收费修正）是两套独立机制，可并存（拨款 + 缴税双轨）。

## 四种"殖民 / 海外"附属国（2026-09 实测）

| | `colonial_nation` 殖民领 | `conquistador` 征服者 | `dominion` 自治领 | `trade_company` 贸易公司 |
|---|---|---|---|---|
| level | 1 | 1 | **3** | 1 |
| 纳贡 | `subject_pays_colonial` | `subject_pays_vassal` | `subject_pays_vassal` | `subject_pays_trade_company` |
| 可否吞并 | **`can_be_annexed = no`** | 可（20 年 + 好感 150） | — | **no** |
| 外交容量系数 | 0.5 | **0.1** | 0.5 | 0.5 |
| 列强分转移 | 0.75 | 0.5 | 0.5 | 0.75 |
| 政体 | republic | — | — | republic |

- **`colonial_nation` 的关键标记是 `is_colonial_subject = yes`**——殖民地联邦 IO、殖民革命局势、`merge_colonies`、`send_people_to_the_colonies` 全都用它来识别"殖民附属国"；写自定义殖民附属国类型时**必须带上这个字段**。
- 殖民领另有：宗主兼任统治者 = no、`will_join_independence_wars = yes`、`shares_exploration_with_overlord = yes`、`merchants_to_overlord_fraction = 0.33`（商人力量上交）、`can_change_heir_selection = no`；`subject_creation_enabled` / `release_country_enabled` 都要求目标省份 `is_overseas_for_owner = yes`。
- 机制全貌见 `vanilla\vanilla-colonization-and-exploration.md` §6。
