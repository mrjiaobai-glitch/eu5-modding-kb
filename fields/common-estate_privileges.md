# common/estate_privileges（阶层特权）

> **一句话**：阶层特权字段：适用阶层、可用门槛、生效时长与三类缩放修正，以及撤销已实施特权的条件字段。
> **什么时候看**：写阶层特权，或核对阶层引用与实施时长时翻这篇。
> **体量**：144 行 · 约 7 分钟通读

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

## 阶层私兵特权（1.4 新系统，2026-09 实查）

**机制要点**：把"阶层武装"从**数值加成**升级为**阶层真的拥有并维持军团**。开关不是特权本身，而是一个国家修饰符 `*_estate_allowed_private_army`；特权只是把这个开关打开、并顺带抬高该阶层影响力。阶层侧的数据（规模、兵种）在 `in_game\common\estates\00_default.txt`，见 `fields\common-estates.md`。

### 两代"贵族私兵"不是一回事（重点：名字一样）

| | 老：`noble_armies` | 新：`noble_private_armies_privilege` |
| --- | --- | --- |
| 位置 | `nobles_estate.txt:708` | `nobles_estate.txt:1827` |
| 可用国家 | **仅法/朝**（`potential = has_or_had_tag = FRA / KOR`） | **任何国家**（`allow = { }` 空） |
| 关键效果 | `nobles_estate_levy_size = 0.5`、`monthly_political_influence_gain_modifier = -0.10`、`monthly_towards_aristocracy`、`global_nobles_estate_power = 0.50`、`nobles_estate_satisfaction_decay = 0.02` | **`nobles_estate_allowed_private_army = yes`**、`global_nobles_estate_power = 1.0` |
| 本质 | 纯数值（征召兵 +50%），没有实体部队 | 阶层**自费**组建并维持私人军团 |
| 中文名 | 贵族私兵（`estate_l_simp_chinese.yml:476`） | 贵族私兵（同名，`:736`） |

哥萨克版完全同构：`cossack_private_armies_privilege`（`cossacks_estate.txt:2`）→ `cossacks_estate_allowed_private_army = yes` + `global_cossacks_estate_power = 1.0`。

### 开关是"全阶层通用"的（mod 接口）

`main_menu\common\modifier_type_definitions\00_modifier_types.txt:18596–18652` 定义了 **8 个** `*_estate_allowed_private_army`（`boolean = yes`）：贵族、市民、教士、农民、哥萨克、王室、迪米、部落。**原版只给贵族与哥萨克配了特权**，其余六个只有开关——想给别的阶层开私兵，必须**同时**在该阶层的 `estates\00_default.txt` 块里补 `private_army_per_pop` 与 `private_army_unit_categories`（否则没有额度与兵种）。

### 玩家的操作面：征用私兵

| 项 | 内容 | 出处 |
| --- | --- | --- |
| 动作 | `press_estate_private_regiments_action`「征用私兵」（阶层应急互动一族） | `generic_actions\estate_emergency_actions.txt:980` |
| 前提 | 持有贵族私兵或哥萨克私兵特权 | 同上 `potential` |
| 可点条件 | 任一阶层 `estate_unraised_regiment_count >= 1`（有**尚未征召**的私兵） | 同上 `allow` |
| 冷却 | `press_estate_private_regiments_cooldown` **3 年** | 同上 `cooldown` |
| 代价 | `add_legitimacy = legitimacy_mild_penalty` + 该阶层满意度下降 | 同上 `effect`；文案 `actions_l_simp_chinese.yml:323` |
| 效果 | `press_estate_private_regiments = { estate = X }` —— 把该阶层**全部**私人军团收编进你的军队 | `effect_localization\estate_effects.txt` |
| AI | `ai_tick = daily`、`ai_tick_frequency = 180` | 同动作定义 |

### 阶层侧的三态与 UI

- 三个触发器：`estate_total_regiment_count` / `estate_raised_regiment_count`（**服役中**）/ `estate_unraised_regiment_count`（**尚未征召**）；见 `trigger_localization\estate_triggers.txt:57–72`（中文：`triggers_l_simp_chinese.yml:8501–8503`）。
- 面板显隐由 `Estate.IsPrivateArmyAllowed` 控制（`in_game\gui\government_lateralview.gui:355`，且"永远忠诚"的阶层不显示）；提示框走引擎的 `Estate.GetPrivateArmyInfo`（`main_menu\gui\shared\estate_tooltips.gui:109`）。
- 玩家可见标签：**「私人军队」**、**「私人军团：$CURRENT$/$MAX$」**、**「私人军队维护费：$VAL$」**（`interfaces_l_simp_chinese.yml:1026 / 3912 / 3913`）；部队提示里 `SUBUNIT_OWNING_ESTATE` 也标为"私人军队"——私兵连队是**独立归属的子单位**。

### 1337 开局谁已经拿着它

`noble_private_armies_privilege`：**DAN、NOV、PSK、VYT、FRA**（`main_menu\setup\1337\10_countries.txt:277 / 2098 / 3351 / 3439 / 15370`）；`cossack_private_armies_privilege`：**KIE**（`:43376`）；老特权 `noble_armies`：**FRA、KOR**（`:15373 / 24759`）。

### 政治面（设计意图）

给阶层武装 = **拿军事红利换阶层权力**（`global_*_estate_power +1.0`）：DHE 里把它当威胁处理——朝鲜「禁止私兵」事件可直接 `revoke_estate_privilege = estate_privilege:noble_armies`（`events\DHE\flavor_KOR.txt:2883/2897`）、土耳其要摆脱贵族私兵、黑死病事件里贵族趁机要求保留小规模私兵。

### 未在数据文件中说明（别猜）

- 私兵上限的**确切公式**：数据只给 `private_army_per_pop`（贵族/哥萨克均为 0.1）与兵种权重，取整与合成规则在引擎侧。
- **维护费金额**：文案与 GUI 都说阶层自费，但常量不在数据文件里。
- **"服役中/未征召"的切换时机**（是否战时自动动员）：全库无脚本 raise 它们 → 引擎自动，条件未说明。
- `levy_noble_armies` / `levy_noble_armies_act`（"征用贵族私兵服役"，声称 `CATEGORY_INFLUENCE_ACTIONS`）**只有本地化、脚本里无定义** → 疑似改名前的残留或未安装 DLC 内容，**别照它写 mod**。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\estate_privileges\` 全量 **7 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **口径**：字段 = 顶层块内的 ``key =``；已排除 readme 以 ``<模式>`` 声明的键、以及本体修正注册表（``modifier_type_definitions``，2,437 键）内的修正名。

### 一、取值白名单（本体出现过的值 + 次数）

- **`estate`**（7 种）：nobles_estate（96）、burghers_estate（67）、clergy_estate（47）、peasants_estate（32）、tribes_estate（18）、cossacks_estate（14）、dhimmi_estate（13）
- **`content_priority`**（9 种）：100（3）、800（3）、200（3）、400（3）、700（2）、500（1）、600（1）、300（1）、900（1）
- **`months`**（1 种）：3（1）

### 二、该用哪些修正（本体在这个类目里实际用过，前 1）

| 修正名 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `content_priority` | 18 | 9 |

### 三、readme 声明、但本类目内原版 0 使用

> ⚠ 只代表"本类目没用"，**不等于这个字段没意义**——同名字段常被别的类目使用。

| 字段 | 本类目 | 全库其它类目 |
| --- | --- | --- |
| `days` | 0 次（7 档） | **有**（出现在 11 个类目） |
| `on_fully_activated` | 0 次（7 档） | **有**（出现在 3 个类目） |
| `province_modifier` | 0 次（7 档） | **有**（出现在 1 个类目） |
| `weeks` | 0 次（7 档） | 全库也没有 → 疑似废弃字段 |
| `years` | 0 次（7 档） | **有**（出现在 23 个类目） |

### 四、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `ai_weight` | 4 | 1 |

### 五、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `global_nobles_estate_power` | 100 | country_modifier |
| `nobles_estate_target_satisfaction` | 92 | country_modifier |
| `global_burghers_estate_power` | 69 | country_modifier |
| `has_or_had_tag` | 59 | potential、any_overlord_or_above、limit、OR |
| `burghers_estate_target_satisfaction` | 58 | country_modifier |
| `NOT` | 51 | potential_trigger、autonomous_scottish_clans、trigger_if、limit |
| `has_variable` | 51 | autonomous_scottish_clans、OR、NOT、potential |
| `global_clergy_estate_power` | 49 | country_modifier |
| `OR` | 47 | potential_trigger、?、potential、custom_tooltip |
| `clergy_estate_target_satisfaction` | 47 | country_modifier |
| `has_unlocked_estate_privilege_trigger` | 37 | potential、allow、NOT |
| `peasants_estate_target_satisfaction` | 34 | country_modifier |
| `global_peasants_estate_power` | 32 | country_modifier |
| `potential_trigger` | 26 | country_modifier、location_modifier |
| `monthly_towards_decentralization` | 26 | country_modifier |

### 六、引擎脚本命令/通用键（出现在 ≥5 个类目，不是本类目的字段 schema）

| 键 | 次数 | 出现在多少个类目 |
| --- | --- | --- |
| `content_priority` | 18 | 9 |
