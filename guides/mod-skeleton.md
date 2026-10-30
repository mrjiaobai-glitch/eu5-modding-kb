# Mod 骨架（mod-skeleton）

> **一句话**：本机 mod 目录实查出的骨架：目录结构、metadata.json 注册、文件命名与编码规则，附 DLC 包结构对照。**EU5 没有游戏启动器**（与 EU4/CK3/VIC3 不同，连 Steam 直接进游戏），mod 由**游戏内 Mod 管理器**启用。
> **什么时候看**：新建 mod 时最先看——定目录、写 metadata.json、命名文件都在这一步。
> **体量**：73 行 · 约 4 分钟通读

mod 目录（本机示例）：`%USERPROFILE%\Documents\Paradox Interactive\Europa Universalis V\mod\`（EU5 **无启动器**，mod 在游戏内 Mod 管理器启用，启用状态存 `playsets.json` 的 `orderedListMods`）。

## 目录结构（真实 mod 实查）

```
mod\<mod名>\
├── .metadata\
│   ├── metadata.json      # 游戏注册（必需；无启动器，游戏内 Mod 管理器直接读）
│   └── thumbnail.png      # 预览图
├── in_game\               # 游戏内覆盖/新增
│   ├── common\<类目>\     # 只放要改的类目
│   ├── events\            # 事件文件
│   ├── gui\               # （可选）
│   ├── localization\<lang>\  # 语言文件（见下）
│   └── setup\             # （可选）国家等
└── main_menu\
    ├── common\            # static_modifiers / modifier_type_definitions / modifier_icons / named_colors 等
    ├── gfx\  gui\  setup\ # （可选）
    └── localization\<lang>\
```

## metadata.json（真实样例：刀锋与王座）

```json
{
	"name":	"刀锋与王座",
	"id":	"blades_and_thrones",
	"version":	"0.2.1",
	"supported_game_version":	"1.3.11",
	"short_description":	"佣兵、常备军与国家政治的联动机制",
	"tags":	["Events", "Military", "Gameplay"],
	"relationships":	[],
	"game_custom_data":	{
	}
}
```

## 文件组织规则

1. **只放要改的文件**：同路径同名 = 覆盖/合并原版（见 `merging.md`）；不存在的路径 = 新增。原版没有的类目子目录直接新建即可。
2. **加载顺序 = 文件名**：`00_` 前缀最先加载，数字越小越先；同数字按文件名序。原版惯例：`00_default.txt` / `0_xxx.txt` / `1_xxx.txt` / `2_xxx.txt` / `3_xxx.txt`（如 unit_types：0_tribal → 1_uniques_for_age_x → 2_unlocked_through_tech → 3_special）。⚠️ **实测更正（2026-10-30）**：**后加载的同名条目不会覆盖先加载的，而是被丢弃**（`error.log`：`Duplicated key … will not be created from file: …`），且文件处理顺序**不是** ASCII 升序。所以 `zzz_` 前缀**不能**用来压原版同键；要改原版条目请用 `REPLACE:`（见 `merging.md` §八，含与 wiki 的差异表）。
3. **本地化文件命名**：`<主题>_l_<lang>.yml`，主题建议用 mod 名前缀（真实 mod 用 `zzz_blades_and_thrones_l_english.yml`）。
4. **语言目录**：mod 可放 `in_game\localization\<lang>\` 或 `main_menu\localization\<lang>\`——**两处都会加载，但不必互为镜像**（43 个真实 mod 实测：22 个只放 main_menu、7 个只放 in_game、7 个两者都用且无同路径同名文件；详见 `localization.md`）。同键时**先处理者胜、后到者被丢弃**（2026-10-30 实测）。⚠️ **与 wiki 不同**：wiki 说"后加载的键不覆盖、必须放 `replace/` 目录"；实测**普通目录即可覆盖**（简中 `EXIT` 已验）。键仍建议不重复定义（见 `merging.md` §八）。
5. **编码**：yml 必须 UTF-8 **BOM**；txt 建议 BOM。
6. **改名/删除原版文件不可取**：用合并前缀或整体覆盖。

## DLC 包（官方 mod 形态）

`game\dlc\D008_fate_of_the_phoenix\`：in_game + loading_screen + main_menu 三分区（767 / 69 / 361 档，共 1,200 档 / 523.5 MB）+ `D008_fate_of_the_phoenix.dlc.json`（元数据，字段同 metadata.json 风格）+ thumbnail.dds + checksum_manifest.txt。`dlc\D000_shared\` 展示最小共享包：`main_menu\common\dlc\`、`main_menu\localization\dlc\<lang>\`。

> **⚠️ 两条与 mod 直接相关的实测**：①**DLC 只新增、不覆盖基础包**（D008 的 1,200 档与基础包 **0 路径碰撞**），官方给内容上锁用的是**基础包文件里的 `has_dlc = "<dlc 名>"`**（368 处 / 66 个基础包文件：d008 275/44 + d009 93/25）；②`common\dlc\<pack>.txt`（13 字段）**只有 DLC 目录里有**，mod **不能**靠它注册自己，注册仍走 `metadata.json`。详见 `vanilla\vanilla-dlc-and-assets.md`。

## 发布 mod 时要碰到的三个文件（第八轮补齐）

| 文件 | 作用 |
|---|---|
| **`main_menu\create_mod_tags.json`**（2,286 B / 104 行） | **Steam 创意工坊的分类标签表**：`{"tags":[{"LocKey":"ADVANCEMENTS_TAG","SteamRef":"Advancements"}]}`——发布时选的标签来自这里 |
| `loading_screen\localization\jomini\pdx_mod_dlc_manager\cw_ugc_dlc_l_english.yml`（865 B） | **上传/playset 的界面报错文案**（Workshop 不可用、mod 名长度 3–60、路径含非法字符、playset 自动排序…）——上传失败时对照它找原因 |
| `main_menu\gui\mods_gui\mods_gui.gui`（**101,518 B**） | mod 管理界面本体（playset 排序、依赖提示）；改界面行为要动它 |

> 另有 `main_menu\checksum_manifest.txt`（345 B / 27 行）= 目录级校验清单；`loading_screen\vfs_skip_files.config`（97 B）= **告诉 VFS 跳过哪些文件**（改了不生效时的嫌疑对象）。全表见 `guides\game-layout.md` 的"根部散文件"节。

## 新国家/新地点注意

- 国家定义：`in_game\setup\countries\<地区>.txt`（格式见 `setup\countries\00_readme.info`：TAG = { color = hsv360{...} color2 male_regnal_names female_regnal_names description_category = administrative difficulty = 2 }，difficulty 1–5）。
- 新增国家还要考虑：`main_menu\common\coat_of_arms\`（纹章）、`flag_definitions\`（旗帜）、地图色、`common\country_ranks\`（等级）。
- 地点（location）由地图二进制决定（`in_game\map_data\`），脚本只能引用 `location:<id>`（地点 ID 列表在 `map_data\location_templates.txt` 或 setup 文件）。
