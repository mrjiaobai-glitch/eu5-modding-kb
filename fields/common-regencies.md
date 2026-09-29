# common/regencies（摄政）

> **一句话**：摄政的五个字段与原版 15 种摄政实测，含兜底空位期与四组与继承法配套的选举摄政。
> **什么时候看**：新增摄政、写新继承法要配选举期摄政，或要查兜底空位期逻辑时翻这篇。
> **体量**：57 行 · 约 3 分钟通读

来源：`in_game\common\regencies\readme.txt`（172 B，5 字段）+ 15 个数据文件 **15 种摄政**

## 字段（readme 声明）

```
<regency id> = {
    start_effect = { <effects> }
    end_effect = { <effects> }
    allow = { <triggers> }
    modifier = { <modifiers> }
    internally_assigned = yes/no
}
```

## 原版实测（15 种 / 15 文件，一文件一摄政）

| 文件 | id | 说明 |
|---|---|---|
| `zz_default.txt` | `interregnum` | **兜底空位期**：`allow = { always = yes }`，`start_effect` 里随机挑一个 `can_become_a_regent = yes` 的角色 `set_regent`，挑不到就 `create_character = { age = 35 }` 现场造一个（并按 `heir_selection:tribal_matrilineal` 决定性别） |
| `1_nobles_regency.txt` | `nobles_estate_regency` | 阶层摄政 |
| `2_clergy_regency.txt` | `clergy_estate_regency` | |
| `3_burghers_regency.txt` | `burghers_estate_regency` | |
| `4_peasants_regency.txt` | `peasants_estate_regency` | |
| `5_cabinet_head_regency.txt` | `cabinet_head_regency` | 内阁首脑 |
| `9_overlord_regency.txt` | `overlord_regency` | 宗主 |
| `10_consort_regency.txt` | `consort_regency` | 配偶 |
| `11_subject_regency.txt` | `subject_regency` | 附属国 |
| `12_lordship_of_ireland_regency.txt` | `lordship_of_ireland_regency` | 专属 |
| `00_fratricide_succesion.txt` | `fratricide_succesion_regency` | 与同名继承法配套 |
| `00_judicial_election.txt` | `judicial_election_regency` | |
| `00_mamluk_succesion.txt` | `mamluk_succesion_regency` | |
| `00_papal_election.txt` | `papal_election_regency` | **唯一带 `end_effect` 的一个** |
| `00_republican_election.txt` | `republican_election_regency` | |

| 字段 | 出现 | 率 |
|---|---|---|
| `start_effect` / `allow` / `modifier` | 15 | 100% |
| `internally_assigned` | 4 | 27%（`consort_regency` / `subject_regency` / `cabinet_head_regency` / `overlord_regency`） |
| `end_effect` | 1 | 7%（`papal_election_regency`） |

本地化：`<id>` = 名称、`<id>_desc` = 描述（`main_menu\localization\<lang>\regencies_l_simp_chinese.yml:13`，`interregnum` = "空位期"）。原版 15 个全部有键。

## 审查要点

- `modifier` / `allow` / `start_effect` 原版 100% 出现——虽然是"可选字段"，实际是**必备结构**；只有 `end_effect` 和 `internally_assigned` 真的可选。
- `internally_assigned = yes` 的 4 个都是"由体系内部指派摄政者"的场合（配偶/宗主/附属国/内阁首脑）；改这几个要连带考虑由谁任命。
- `zz_default.txt` 的 `interregnum` 是**兜底**：新增摄政若不覆盖某种局面，玩家会掉进空位期，所以 `start_effect` 里必须有兜底造人逻辑（原版就靠 `create_character`）。
- 摄政与继承法成对出现（`fratricide_succesion` / `judicial_election` / `mamluk_succesion` / `papal_election` 四对同名）——写新继承法时通常要配一个摄政，否则选举期无人执政。
- 相关 trigger：`is_regent`（`trigger_localization\character_triggers.txt:340`）、`can_become_a_regent`。
- 未在 readme 中说明：本地化键格式、trigger/effect 作用域。
