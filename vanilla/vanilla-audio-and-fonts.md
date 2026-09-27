# 原版解析：音频层与字体层（vanilla audio & fonts）

版本基准：EU5 1.3.x。**本篇为什么存在**：这两层服务的人群窄（音乐/字体 mod），但 **kb 此前对它们几乎零覆盖**，而实查下来它们**都不是"黑盒"**——音频有文本映射表与可脚本编辑的设置档，字体就是 ttf/otf + 文本定义。**它们不是"做不了"，只是优先级低于脚本层。**

| 层 | 位置 | 规模 |
|---|---|---|
| 音频设置 | `loading_screen\sound\audio_settings.txt` | 1,963 B（引擎 / 6 档案 / VCA 总线） |
| 音频包 | `loading_screen\sound\banks\windows\` | **10 个 `.bnk`** + `Media\*.wem` **1,225 个** + 文本元数据（`Init.txt` 644 行等） |
| 地图环境音 | `loading_screen\sound\map\ambience\` | 11 个文本档 + 3 张 sensor 掩码 PNG + 官方 `sensorgen.py` |
| 常驻音效 | `loading_screen\sound\persistent_objects\` | 1 档 / 1.4 KB |
| 曲目表 | `in_game\common\music_player_tracks\` | `00_music_player_tracks.txt` 10.9 KB / **82 曲** + info 533 B |
| 曲目文案 | `main_menu\localization\music_player_gui\music_player_l_<lang>.yml` | english **25.5 KB**（11 语言） |
| 按文化配乐 | `main_menu\music\audio_culture_types\00_audio_culture_types.txt` | 703 B / **8 个文化原型** |
| 原声开发文件 | `main_menu\music\mp3\` | 7 个 mp3 / 70 MB（**不是游戏运行时读的**） |
| 字体 | `loading_screen\fonts\` | **289 档 / 541 MB**：`ttf` 250 + `otf` 24 + **6 个 `.font` 定义**（跨三区）+ 每族 `OFL.txt` |
| 文本样式 | `loading_screen\gui\textformatting.gui` | 704 行（`textformatting = { }` + `template text_*`）；`fonttemplates.gui` 仅 1.5 KB 且多为注释示例 |

---

## 一、音频：四件套 + 可改点

### 1. 引擎与总线：`sound\audio_settings.txt`

```txt
sound_engine = "wwise"

audio_profiles = {                       # 玩家可选的音频档案（首项 Off）
    { name = "PDX_Off"          display_text = "SETTING_AUDIO_OPTION_OFF" }
    { name = "PDX_HomeCinema"   display_text = "SETTING_AUDIO_OPTION_HOMECINEMA" }
    { name = "PDX_Headphones" / "PDX_NightMode" / "PDX_Speakers" / "PDX_TV" … }
}

vcas = {                                 # VCA = wwise 音量总线，setting 是引擎侧音量键
    { name = "VCA_Master" setting = "volume.bus:/"            display_text = "SETTING_Master" }
    { name = "VCA_Music"  setting = "volume.vca:/MUSIC"       display_text = "SETTING_Music" }
    { name = "VCA_Ui"     setting = "volume.vca:/UI"          display_text = "SETTING_UI" }
    …
}
```

**可改**：加/改音频档案、加 VCA 总线（配套 `SETTING_*` loc 键）——纯文本，加载期读取。

### 2. 包与媒体：`sound\banks\windows\`

| `.bnk` | 体量 | 内容 |
|---|---|---|
| `sb_world_media.bnk` | **821 MB** | 世界/环境音媒体（最大） |
| `sb_ui_media.bnk` | 14.9 MB | UI 音效 |
| `sb_music_media.bnk` | 3.1 MB | **音乐媒体** |
| `sb_world_logic.bnk` | 3.1 MB | 世界音逻辑 |
| `sb_music_logic.bnk` | 375 KB | 音乐逻辑 |
| `sb_ui_logic.bnk` | 144 KB | UI 逻辑 |
| **`sb_music_media_D008.bnk`** | 66 KB | **DLC 音乐包**（D008 自己的曲目媒体） |
| `Init.bnk` 26 KB / `sb_logic.bnk` 5.7 KB / `MasteringSuite.bnk` 2 KB | | 引擎/母带 |

- `Media\` 下 **1,225 个 `.wem`**，**文件名 = Wwise ID**（如 `975949960.wem`）。
- **`Init.txt`（644 行）是文本映射表**：`ID / Name / Wwise Object Path`，例：
  `583705497  mus_boredom  \Music\mus_boredom` —— **事件名 ↔ ID ↔ 媒体**这条链在文本里能追（其中 `\Music\` 路径 25 条；完整事件表仍在 `.bnk` 内）。
- 另有 `ProjectInfo.json/xml`、`PlatformInfo.json/xml`、`PluginInfo.json/xml`、`MasteringSuite.txt`、`sb_logic.txt`（都是 wwise 工程元数据，**版本敏感**）。

### 3. 曲目表与文案（可脚本化的那一半）

`in_game\common\music_player_tracks\00_music_player_tracks.txt` —— **82 曲**，条目名 = **wwise 事件名**：

```txt
MusicPlayer_01_Overture_I_Genesis = {
    composer  = Hakan_Glante                    # loc 键（80 曲有）
    performer = Norrkoping_Symphony_Orchestra   # loc 键
    soloist   = <loc 键>                        # 仅 13 曲
}
```

info（533 B）原文要点：**新曲必须按 wwise 事件键登记**，可选 composer/performer/soloist，并**同步 `music_player_l_<lang>.yml`** 的三类键（曲名 / 曲名 `_flavour` / 人名）。文案在 `main_menu\localization\music_player_gui\music_player_l_<lang>.yml`（english 25.5 KB，小语种 ~0.9 KB）。

### 4. 按文化配乐：`main_menu\music\audio_culture_types\`

```txt
european_sfx     = { priority = 100   culture_tag = european_gfx }
indian_sfx       = { priority = 110   culture_tag = indian_gfx }   # 优先级更高的先匹配
east_asian_sfx / african_sfx / middle_east_sfx / north_american_sfx / south_american_sfx …
```

**这就是"哪个文化放哪套音乐/环境音"的机制**（8 个原型，`culture_tag` 指向 gfx 文化标签）。`main_menu\music\mp3\` 的 7 个 mp3（70 MB，`pdx_caesar_hg_demo_*`）是**原声开发文件**，游戏运行时不读。

### 5. 地图环境音（文本可改的一层）

`sound\map\ambience\`：`audio_parameter_groups.txt` / `audio_parameter_limits.txt` / `sensor_patterns.txt`（5.4 KB）/ `terrain_ambience_layers.txt` + `_default` / `sound_alias_bank.txt` / `map_object_ambience.txt` / `map_object_visibility_settings.txt` + `sensor_mask_default*.png`（3 张）+ **官方 `sensorgen\sensorgen.py`（1,574 B，用 PIL 把图片转成 sensor 掩码）**。
`sound\persistent_objects\00_audio_persistent_objects.txt`（1.4 KB）登记常驻音效对象。

---

## 二、字体：两层定义 + 语言映射

### 1. 文件与定义分布

**6 个 `.font` 定义档，按区各管一段**：

| 档 | 区 | 体量 | 管什么 |
|---|---|---|---|
| `loading_screen\fonts\loading_screen_fonts.font` | loading_screen | 31 KB / 835 行 | 主字体组（`fontfiles`） |
| `loading_screen\fonts\cw_fonts.font` | loading_screen | 15 KB / 548 行 | 另一套字体组（OpenSans 系） |
| `loading_screen\fonts\loading_screen_fonts_headers_serif.font` | loading_screen | 1.3 KB | **headers 专用**（`headers_serif` 激活时由 `FontRegister` 读） |
| `loading_screen\fonts\loading_screen_fonts_headers_sans_serif.font` | loading_screen | 1.9 KB | headers 无衬线变体 |
| `in_game\fonts\in_game_fonts.font` | in_game | 37 KB / 884 行 | 游戏内字体设置 |
| `main_menu\fonts\main_menu_fonts.font` | main_menu | **428 B** / 28 行 | 主菜单字体设置 |

字体本体：`loading_screen\fonts\<字族>\*.ttf|otf`，13 个字族（CormorantGaramond 11 / MapNamesFonts 12 / NotoSans 19 / NotoSansJP 11 / NotoSansKR 11 / NotoSansMono 38 / NotoSansSC 11 / NotoSerif 75 / NotoSerifDisplay 73 / NotoSerifJP 8 / NotoSerifKR 8 / NotoSerifSC 8），**每族带 `OFL.txt`**（SIL 开源字体授权文件）。

### 2. 两层语法

```txt
# 第一层：fontfiles = "字体文件组"（带【语言 → 有序文件列表】的映射）
fontfiles = {
    name = "NotoSans-Regular"
    always_load = no
    group = {
        languages = { "l_english" "l_french" "l_german" "l_russian" "l_spanish" "l_polish" "l_braz_por" "l_turkish" }
        files = { "fonts/NotoSans/NotoSans-Regular.ttf"
                  "fonts/NotoSansSC/NotoSansSC-Regular.ttf"      # ← 有序 = 回退链
                  "fonts/NotoSansJP/NotoSansJP-Regular.ttf"
                  "fonts/NotoSansKR/NotoSansKR-Regular.ttf" }
    }
    group = { languages = { "l_simp_chinese" }  files = { … } }      # 每种语言单独一组
    group = { languages = { "l_korean" }        files = { … } }
    group = { languages = { "l_japanese" }      files = { … } }
}

# 第二层：font = "样式名 → 字体组"（GUI/文本样式引用的是这里的 name）
font = {
    name = "StandardGameFont_HeadersSerifSetting"
    fontstyle = { style = regular  fontfiles = "NotoSans-Regular" }
    fontstyle = { style = bold     fontfiles = "NotoSans-Medium" }
}
```

**原版注释里的约定**（`cw_fonts.font` 头 3 行）：每个字体要说明用途；不用的字体删掉或注释；**字体名应按用途命名、而不是源文件名**（例：`StandardGameFont_HeadersSerifSetting`）。

实测：`l_simp_chinese` / `l_japanese` / `l_korean` / `l_russian` / `l_polish` / `l_braz_por` 各出现在 **37 个 `.font`** 里、`l_turkish` 32 个、**`l_arabic` 0 个**（无阿拉伯语支持）。

### 3. 文本样式在 `textformatting.gui`，不在 `fonttemplates.gui`

- `loading_screen\gui\textformatting.gui`（**704 行**）：`textformatting = { … }` 块 + `template text_common_template` / `text_single_template` / `text_multi_template` / `text_default_text_template`（**改字号/颜色/换行行为在这里**）。
- `loading_screen\gui\fonttemplates.gui` 只有 **1,517 B**，且**绝大部分是注释掉的示例**（`FontSmall` / `FontNormal` / `FontButton` / `FontHeading1/2` 全在注释里），活着的只有 `FontDefaultColor`（颜色）与 `FontHeading3`。**别以为字体模板都在这**。

---

## 三、可改点与硬边界

| 想改什么 | 动哪里 | 注意 |
|---|---|---|
| **替换已有曲目** | 按 `Init.txt` 找到事件名对应的 Wwise ID → 替换 `Media\<ID>.wem` | `.wem` 是编码格式，替换文件要么同格式、要么走 wwise 转换；**不要改文件名** |
| **新增一首曲目** | ❌ **脚本侧做不到**：新事件要登记进 wwise 工程并重打包 `.bnk`（`sb_music_media.bnk` 等） | 唯一硬边界；纯脚本 mod 只能"替换" |
| 曲目名/作者/简介 | `in_game\common\music_player_tracks\*.txt`（条目名 = wwise 事件键）+ `music_player_l_<lang>.yml`（曲名 / `_flavour` / 人名三类键） | 名字键与 `_flavour` 成对；11 语言 |
| 加/改音频档案、音量总线 | `sound\audio_settings.txt`（`audio_profiles` / `vcas`） | 配套 `SETTING_*` loc 键 |
| 让某文化用另一套音乐/环境音 | `main_menu\music\audio_culture_types\`（8 原型 + `priority` + `culture_tag`） | `culture_tag` 指向 gfx 文化标签（见 `gfx\interface\graphical_cultures\`） |
| 地图环境音规则 | `sound\map\ambience\*.txt`（参数组/上限、sensor 图案、地形层、alias bank） | sensor 掩码图可用官方 `sensorgen.py` 生成 |
| 常驻音效 | `sound\persistent_objects\00_audio_persistent_objects.txt` | — |
| **换 UI 字体** | 丢 ttf/otf 到 `loading_screen\fonts\<族>\` + 在对应 `.font` 的 `fontfiles` 里改 `files`（或新增 `fontfiles` + `font`） | 守 **OFL** 再分发条款（原版字体都带 `OFL.txt`）；改名后要同步所有引用该 `font` 名的地方 |
| **给新语言加字体** | 该语言在 `.font` 的 `group.languages` 里补 `l_<lang>` + 有序 `files`（回退链） | 语言代码见 `guides\localization.md`（11 种） |
| 改字号/颜色/换行 | `loading_screen\gui\textformatting.gui` 的 `template text_*` | 不是 `fonttemplates.gui`（那里基本是注释） |
| 加字族 | 新目录 + 对应 `.font` 里的 `fontfiles`/`font` 声明 | 字体名按**用途**命名（原版注释明文要求） |

**硬编码**：wwise 引擎本身（`sound_engine = "wwise"`）；`.wem` 的编码/解码；事件注册表（在 `.bnk` 内）；字体的栅格化与回退顺序由引擎按 `files` 列表执行；`.bnk`/`ProjectInfo` 等元数据版本敏感，**跨版本改会失效**。

---

## 四、中文检索键

音频：`SETTING_Master` / `SETTING_Music` / `SETTING_UI`（音量总线文案）、`SETTING_AUDIO_OPTION_*`（音频档案文案）、`music_player_l_<lang>.yml`（曲名 / `_flavour` / 人名）。
字体：`.font` 里的 `fontfiles`（文件组）与 `font`（样式名）；`textformatting.gui` 的 `template text_*`；`OFL.txt`（授权）。
相关档：`fields\common-music_player_tracks.md`（曲目字段权威）· `vanilla\vanilla-gui.md`（界面层）· `guides\localization.md`（语言代码与 BOM）· `guides\game-layout.md`（三区结构）
