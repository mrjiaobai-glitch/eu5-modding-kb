# 本地化制作（localization）

## 文件位置（实查修正）

| 位置 | 内容 | mod 用法 |
|---|---|---|
| `main_menu\localization\<lang>\` | **原版主语言文件**（english 114 个 yml：actions_l_english.yml、advances_l_english.yml、buildings_l_english.yml…） | mod 可放这里 |
| `loading_screen\localization\<lang>\` | languages.yml、load_tips_l_<lang>.yml | 启动界面文本 |
| `loading_screen\localization\jomini\` | savegame_gui_l_<lang>.yml、cw_ugc_dlc_l_<lang>.yml | 存档界面 |
| `main_menu\localization\<lang>\missions\` | **任务链/任务节点文案**（4 个 yml：`generic_missions_l_english.yml` 93 KB 等；11 语言都有该子目录） | 任务 mod 放这里 |
| `in_game\localization\jomini\script_system\` | **引擎触发词条** trigger_system_l_<lang>.yml（11 语言） | 只读参考（查词条存在性） |
| `dlc\*\main_menu\localization\dlc\<lang>\` | DLC 语言（dlc_localizations_l_<lang>.yml） | 官方 mod 示范 |

语言代码：english / french / german / spanish / simp_chinese / korean / japanese / polish / russian / turkish / braz_por（11 种）。真实 mod 把语言文件放在 `in_game\localization\<lang>\`（如 `刀锋与王座\in_game\localization\simp_chinese\zzz_blades_and_thrones_l_simp_chinese.yml`）——两个位置都会被加载，**同键冲突时加载顺序后者胜**（镜像规则：键尽量两侧一致，见下）。

## 文件格式

```yml
l_english:                     # 语言头（必须首行）
 ACTION_COST: "Spend $PRICE$ to gain the following effects:"
 call_jihad: "Call $jihad$"
 call_jihad_desc: "Choose an infidel country to bring down the might of Allāh upon."
 call_jihad.tt: "A new [GetInternationalOrganizationType('jihad').GetName] is created against the selected [country|e]"
```

- **必须 UTF-8 BOM**（无 BOM 整个文件被忽略/乱码；编辑工具常剥 BOM，保存后要补回）。
- 语言头：`l_english:` / `l_french:` / `l_german:` / `l_spanish:` / `l_simp_chinese:` / `l_korean:` / `l_japanese:` / `l_polish:` / `l_russian:` / `l_turkish:` / `l_braz_por:`。
- 键顶格（不缩进）在语言头下；值用双引号包裹。`#` 注释。
- 键内可用 `"` 转义、`\n` 换行。

## 键前缀规则（名称即键，漏一个显示 raw key）

- 事件：`<namespace>.<id>.title / .desc / .historical_info / .a / .b / .tt`
- 任务（`missions\` 子目录）：链 5 键 `<链名>` / `<链名>_DESCRIPTION` / `<链名>_CRITERIA_DESCRIPTION` / `<链名>_BUTTON_TOOLTIP` / `<链名>_BUTTON_DETAILS`；节点 3 键 `<节点名>` / `<节点名>_desc` / `<节点名>_tt` ← `guides\event-making.md` §任务、`fields\common-missions.md`
- 静态修正：`STATIC_MODIFIER_NAME_<键>`、`STATIC_MODIFIER_DESC_<键>`（main_menu\common\static_modifiers）
- 自动修正：`AUTO_MODIFIER_NAME_<键>` / 科技 `ADVANCE_NAME_<键>` / 建筑 `BUILDING_NAME_<键>`（见 `tools\loc-keys.md` 全表）
- 国家交互/角色交互：`<交互键>`、`<交互键>_desc`、`<交互键>.tt`（actions_l_english.yml 里 call_jihad 系列就是例子）
- 游戏规则：`rule_<键>`、`setting_<键>`、`setting_<键>_desc`（`_game_rules.info` 明示）

## 占位符

- 脚本参数：`$PRICE$` / `$MONTHS|0$`（`|0` 表示取整），必须配对存在，否则显示原文。
- GUI 表达式：`[Root.GetName]`、`[country|e]`（概念链接）、`[GetInternationalOrganizationType('jihad').GetName]`、`[ShowReligionGroupAdjective('muslim')]`——照抄原版 yml 的写法。

## 检查清单

1. 文件 BOM ✔ 语言头正确 ✔
2. 每个引用的键都存在（事件 title/desc/选项/tt、修正名、交互名…）
3. 占位符配对
4. 同键在 main_menu 与 in_game 两侧（若两侧都放了文件）语义一致
5. 中文用 `l_simp_chinese` 文件，别写进 l_english（会显示英文原文给中文玩家）
