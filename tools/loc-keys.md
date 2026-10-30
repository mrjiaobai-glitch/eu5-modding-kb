# 审查清单辅助：本地化键（loc 键）规则

> **一句话**：本地化键的形态总表：事件、任务、音乐、修正、概念等各族的键前缀，以及判「缺键」必守的两条与子目录规则。
> **什么时候看**：审查本地化或排查界面 raw key 时，先在这里对照键形态，再按末节定核键范围。
> **体量**：58 行 · 约 3 分钟通读

> 本文件不是 readme 提炼内容，而是 SKILL.md 第 6 节检查清单的按需加载辅助表（源自人工实测经验，含与 readme 声明的交叉核对）。审查本地化前加载。

## 事件键

`<namespace>.<id>.title / .desc / .a / .a.tt`（`.historical_info` 也在用；选项键是 `.a` / `.b` / `.c`…）

## 任务键（在 `main_menu\localization\<lang>\missions\` 子目录里）

- 任务链 5 键：`<链名>` / `<链名>_DESCRIPTION` / `<链名>_CRITERIA_DESCRIPTION` / `<链名>_BUTTON_TOOLTIP` / `<链名>_BUTTON_DETAILS`
- 任务节点 3 键：`<节点名>` / `<节点名>_desc` / `<节点名>_tt`
- `select_trigger` 的 `name = "…"` / `none_available_msg_key = "…"` 也是 loc 键（原版实例 `mission_select_market` / `mission_no_markets`，值里可写 `$no_valid_markets$` 复用别的键）
- 触发器 tooltip：`HAS_ENABLED_MISSION_TRIGGER` / `HAS_UNLOCKED_MISSION_TASK_TRIGGER`（+ `NOT_` / `FIRST_` / `THIRD_` 变体，见 `trigger_localization\scripted_triggers.txt:187/193`）

## 音乐键（在 `main_menu\localization\<lang>\music_player_gui\music_player_l_<lang>.yml` 子目录里）

- 曲名键 = **`music_player_tracks` 里的条目名**（= wwise 事件键，如 `MusicPlayer_01_Overture_I_Genesis`）
- 三件套：`<曲名>` / `<曲名>_flavour`（简介）/ 人名键（`composer` / `performer` / `soloist` 字段的值本身也是 loc 键）
- 详见 `vanilla\vanilla-audio-and-fonts.md` §一.3

## 名称即 loc 键（顶层键名 = loc 键，漏一个显示 raw key）

- static_modifier → `STATIC_MODIFIER_NAME_<名>` + `STATIC_MODIFIER_DESC_<名>`（前缀形式；category 枚举见 main_menu-static_modifiers.md）
- **修正键（modifier_type_definitions）→ `MODIFIER_TYPE_NAME_<键>` + `MODIFIER_TYPE_DESC_<键>`**（英文 loc 各 2,507 / 2,477 条；**新注册一个修正键就必须补这两个**，见 `fields\main_menu-modifier_type_definitions.md`）
- **概念词条（game_concepts）→ `game_concept_<名>` + `game_concept_<名>_desc`，且 `alias` 里每个别名各要一套**（英文共 3,058 键；`[<名>|e]` 链接的注册表，见 `fields\main_menu-game_concepts.md`）
- auto_modifier → `AUTO_MODIFIER_NAME_<名>`
- casus_belli → 原版常见 `cb_<名>` + `cb_<名>_desc`；readme 声明键为 `<id>` / `<id>_PROV` / `<id>_desc`——**以原版 loc 实际键为准**
- 自定义外交关系 → `<名>_relation` + `<名>_relation_desc`（模板 food_access_relation）
- wargoal 的 `war_name = "XXX"` → 独立 loc 键（如 EXPEL_LANDLESS_WAR_NAME）
- reform / subject_type / estate / law / societal_value / AI 人格 / disaster / regency / scripted_country_names 块名 → 顶层键即 loc 键（subject_type 另需 `LEAD_<名>` / `AM_<名>`）
- artist_type → `ARTIST_TYPE_NAME_<名>` + `ARTIST_TYPE_DESC_<名>`
- trait → `<名>` + `desc_<名>` + `<名>_die_desc`（`traits_l_*.yml`；原版 147 个特质 147 个 `desc_*` 齐全）
- heir_selection / regency / child_education → `<名>` + `<名>_desc`（分别在 `government_l_*` / `regencies_l_*` / `character_l_*`；三者原版均 100% 有键）
- death_reason → **`DEATH_REASON_<名>` + 按 `possible_parameter` 顺序的参数组合后缀**（`disease` 需 4 个键：`DEATH_REASON_disease` / `_disease_location` / `_disease_disease_outbreak` / `_disease_location_disease_outbreak`；原版 51 个 id ↔ 87 个键）。文本用 `[SCOPE.sLocation('location').GetName]` 取参数
- designated_heir_reason → `HEIR_REASON_<名>`（全在 `character_l_*.yml`，7/7）
- country_description_category → `country_description_category_name_<键>` + `country_description_category_desc_<键>`
- generic_action / resolution → `<action_tag>` + `<action_tag>_desc`（无前缀）；消息键 `PERFORM_<Key>_ACTION` / `WE_PERFORM_<Key>_ACTION` / `OTHER_PERFORMS_<Key>_ACTION` / `ACTION_<Key>_PERFORMED_ON_US`

## 不需要 loc 的类型（不要误报）

- modifier 类型名（modifier_type_definitions，引擎处理）
- rebel demand 名（vanilla 全无）

## 占位符与跨引用

- 占位符配对：`[Root.GetName]` 方括号、`[Root.GetVariable('x').GetValue|V0]`、`$KEY$` 引用存在。
- 跨 mod 引用原版/1644 的 loc 键（如 civil_war_game_over）→ 标注"外部依赖"，不报错。

## 判「缺键」必守的两条（2026-09 实测）

1. **loc 键是全局的，不按目录归属**——某个键在"别的类目文件"里也可能已经定义。实例：特质 `arrogant` 的名称键**不在** `traits_l_simp_chinese.yml`，而在 `character_names_l_simp_chinese.yml:29880` 的人名词表里（`desc_arrogant` / `arrogant_die_desc` 都在 traits 文件里）。**只搜本类目文件会误报"缺键"**；反过来，改同名的人名词条会连带改掉特质名。
2. **DLC 内容的键在 DLC 目录**——基础包 loc 只有 5 个儿童教育，DLC 的 `orthodox_education` 只在 `game\dlc\D008_fate_of_the_phoenix\main_menu\localization\`。核键范围要包含 `dlc\*\main_menu\localization\<lang>\`。
3. **子目录也算**——任务键不在 `main_menu\localization\<lang>\` 顶层，而在 `missions\` 子目录里；只 grep `*.yml` 顶层会误报任务全缺键。
