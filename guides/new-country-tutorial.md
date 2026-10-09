# 制作教程：从零加一个国家（new-country-tutorial）

> **一句话**：以雅典为样例、把新增国家拆成第 0 到第 7 步的端到端教程，含 2,340 个国家块的真实字段分布与开局模板层用法。
> **什么时候看**：第一次加国家时按这七步顺序做，做到哪一步就翻对应小节与自检清单。
> **体量**：143 行 · 约 7 分钟通读

> **E2E 教程**：把"新增一个可开局的国家"拆成可执行步骤。**每步都给原版实例与权威档**；不确定的一律标 `[存疑]`。
> 素材来自原版实查（2026-09）：以 **ATH（雅典）** 为贯穿样例，它出现在 6 个文件里，**没有一处是多余的**。

## 第 0 步：挑 TAG，并确认没被占用

- TAG = **3 个大写字母**；原版有 2,340 个国家块（`setup\countries\`）。
- 占用检查（三处都要查）：`in_game\setup\countries\*.txt`（国家定义）、`main_menu\common\flag_definitions\00_flag_definitions.txt`（旗帜列表名）、`main_menu\localization\<lang>\country_names_l_<lang>.yml`（`<TAG>:` 与 `<TAG>_ADJ:`）。

## 第 1 步：国家定义 → `in_game\setup\countries\<地区档>.txt`

权威：`00_readme.info`（**243 B**，注意扩展名是 `.info`）+ `fields\setup-countries.md`。ATH 的真实块（`balkans.txt:17`）：

```txt
ATH = {
    color = map_athenian              # 或 rgb { … } / hsv360 { … }
    color2 = rgb { 41 188 84 }
    culture_definition = catalan      # ★ 注意字段名带 _definition
    religion_definition = catholic
}
```

**原版 2,340 个国家块的真实字段分布**（不是 readme 的模板）：

| 字段 | 用到的国家数 | 说明 |
|---|---|---|
| `color` | 2,340（100%） | 地图色（可用 `map_<名>` 具名色） |
| `culture_definition` / `religion_definition` | 各 2,337 | **主文化与国教**（字段名都带 `_definition`） |
| `color2` | 1,848 | 副色 |
| `description_category` | 67 | 引用 `common\country_description_categories\` |
| `difficulty` | 65 | 1–5 |
| `is_historic` | 55 | 历史国家标记 |
| `unit_color0/1/2` | 各 46 | 兵模配色 |
| `male_regnal_names` / `female_regnal_names` | **0** | ⚠️ readme 模板里有，但**原版没有任何国家在用**——照抄 readme 会写出没人用的字段 |

国家名/形容词**不在这里**：去 `country_names_l_<lang>.yml`（第 4 步）。

## 第 2 步：领土与开局状态 → `main_menu\setup\start\10_countries.txt`

文件顶层是 `countries = { … }`，里面每个 TAG 一块。ATH（`:12491`）：

```txt
ATH = {
    own_control_core = {                  # 领土：直接列 location id
        athens megara thebes atalanti livadeia gravia
    }
    starting_technology_level = 3
    include = "expl_mediterranean"        # ★ 开局模板（第 2b 步）
    include = "expl_silk_road_west"       # ★ 开局模板（第 2b 步）
    include = "catholic_monarchy"
    government = {                        # 也可以直接写，见模板字段
        type = monarchy
        heir_selection = cognatic_primogeniture
    }
}
```

- 领土标记枚举（kb 已记）：`own_control_core` / `own_control_integrated` / `own_control_conquered` / `own_control_colony` / `own_core` / `own_conquered`。
- location id 必须真实存在（`map_data\location_templates.txt` 的 28,573 条 → 见 `vanilla\vanilla-map-and-geography.md`）。
- 权威字段档：`fields\setup-countries.md`（同档覆盖 `in_game\setup\countries`）；`10_countries.txt` 本身**没有 readme**，字段以原版实测为准。

## 第 2b 步：★ 开局模板层 → `main_menu\setup\templates\`（**205 档，kb 此前零覆盖**）

**这是新国家真正的"省事开关"**：`10_countries.txt` 里 `include = "<模板名>"` 出现 **5,256 次 / 192 个去重目标**——原版每个国家都靠模板拼出开局，而不是手写十几行。

模板能带的东西（205 档顶层字段频次实测）：

| 字段 | 档数 | 内容 |
|---|---|---|
| `government` | **168** | 政体 + 继承法 + **议会** + **13 条社会价值观初值**（如 `centralization_vs_decentralization = 40`） |
| `starting_technology_level` | 123 | 开局科技等级 |
| `include` | 57 | **模板可嵌套**（`expl_*` 常被政体模板 include） |
| `discovered_areas` / `discovered_regions` / `discovered_provinces` | 39 / 33 / 25 | **开局"已知海域/地区/省份"**（探索知识） |
| `court_language` | 14 | 宫廷语言 |
| `country_rank` | 10 | 国家等级 |
| `currency_data` / `religious_school` / `scholars` | 各 1 | 少见字段 |

命名约定（按类目挑，别自己发明）：

- `expl_<地区>` —— 探索知识（`expl_mediterranean` / `expl_silk_road_west` / `expl_china` …）
- `catholic_` / `muslim_` / `indian_hindu_` / `indian_muslim_` / `east_asia_` / `subsaharan_` / `gaelic_` / `turkish_beylik` / `russian_principality` … —— **宗教/文化区 + 政体**
- 后缀限定：`_no_coast`（内陆）、`_not_present`（该宗教尚未存在）、`_no_censor`、`_tribesmen`、`_no_auxilium` …
- 用到的 Top：`expl_western_europe` 334 / `expl_mesoamerica` 318 / `expl_northern_europe` 277 / `catholic_monarchy_no_coast` 192 / `amerindian_tribe` 178 / `japanese_clan` 146。

**做法**：先 `include` 一个政体模板 + 1–3 个 `expl_*`，再看还缺什么用直写字段补（直写会**覆盖**模板值，顺序按文件内先后）。`test_template.txt` 是原版的测试模板，可当最小样例读。

## 第 3 步：（可选）POP 与思潮分布 → `main_menu\setup\start\06_pops.txt` / `08_institutions.txt`

⚠️ **这两个文件是按 location 索引的，不是按 TAG**（`ath = { define_pop = { type = clergy size = 0.007 culture = picard religion = catholic } }` / `ath = { feudalism = yes }`）。也就是说：**加了国家必须保证它的地点有 POP 定义**，否则地点是空的；这一步与"加国家"是两件事，但开局体验取决于它。

- `06_pops.txt` **5 MB**，逐地点逐 POP（`define_pop = { type size culture religion }`）；权威见 `vanilla\vanilla-pop.md`／POP 篇。
- `08_institutions.txt` 662 KB，逐地点标记已接纳的思潮。

## 第 4 步：本地化 → `main_menu\localization\<lang>\country_names_l_<lang>.yml`

```yml
 ATH: "Athens"
 ATH_ADJ: "Athenian"
```

- **两个键**（名 + 形容词）；11 语言各一份。
- 文件必须 **UTF-8 BOM**（`pitfalls.md` §五：无 BOM 整文件被忽略）。
- 键形态全表见 `tools\loc-keys.md`。

## 第 5 步：国旗与家徽 → `coat_of_arms` +（可选）`flag_definitions`

**实测确认的机制**（ATH 就是活例）：**`flag_definitions` 里没有该 tag 的列表时，引擎直接拿 tag 当 COA 键**——所以最小做法是：

1. 在 `main_menu\common\coat_of_arms\coat_of_arms\pre_scripted_countries.txt` 加一个**与 TAG 同名**的 COA 键（`ATH = { … }`，在 `:2309`）：

```txt
ATH = { # Athens
    pattern = "pattern_solid.dds"
    color1 = "white"
    color2 = "blue"
    color3 = "red_secondary"
}
```

2. （按需）在 `flag_definitions\00_flag_definitions.txt` 加 `ATH = { flag_definition = { coa = ATH  priority = 1 } }` ——**只有需要"按条件换旗/附庸角标"时才需要**（ATH 原版就没有）。

字段权威：`fields\main_menu-coat_of_arms.md`、`fields\main_menu-flag_definitions.md`；五层模型见 `vanilla\vanilla-heraldry-and-flags.md`。

## 第 6 步：自检清单

| 检查 | 怎么看 |
|---|---|
| 国家出现在选择界面 | 名称键 `TAG` 是否在**11 语言**里都有（当前语言缺 → 显示 raw key） |
| 有领土且不是荒地 | `10_countries.txt` 的 location id 是否真实；`own_control_core` 拼写 |
| 政体/继承法/议会对 | 模板是否 include 成功（拼错模板名**不报错**、只是没效果） |
| 科技等级与已知海域对 | `starting_technology_level` / `discovered_*` 是否被模板带进来 |
| 旗子对 | COA 键名是否**与 TAG 逐字一致**（大小写）；`pattern`/贴图名是否存在于 `gfx\coat_of_arms\patterns\` |
| 地图色对 | `color` 是否为合法具名色或 rgb/hsv 块 |
| error.log | 有无 `missing` / raw key（见 `tools\error-log-decoder.md`） |

## 第 7 步：相关档

`fields\setup-countries.md`（国家定义字段）· `vanilla\vanilla-map-and-geography.md`（location id 与开局数据层）· `vanilla\vanilla-heraldry-and-flags.md`（旗帜五层）· `tools\loc-keys.md`（键形态）· `guides\mod-skeleton.md`（mod 骨架与 metadata.json）· `tools\review-checklist.md`（发布前审查）
