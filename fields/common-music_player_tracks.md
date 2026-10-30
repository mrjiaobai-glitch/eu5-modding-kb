# common/music_player_tracks（音乐播放器曲目）

> **一句话**：音乐播放器曲目条目：三个可选字段、条目名必须是 wwise 事件键，以及曲名与简介键的本地化写法。
> **什么时候看**：加曲目、改曲名与简介文案，或要确认新增音乐需同时动哪几处时翻这篇。
> **体量**：57 行 · 约 3 分钟通读

来源：`in_game\common\music_player_tracks\music_player_tracks.info`（**533 B**）+ `00_music_player_tracks.txt`（10.9 KB）实查

## 字段

```
<wwise event 键> = {
    composer  = <loc 键>     # 可选
    performer = <loc 键>     # 可选
    soloist   = <loc 键>     # 可选
}
```

**条目名 = wwise 事件键**（info 原文："All new songs added on wwise, should be added by their **event key**"）——不是曲名，也不是文件名。

info 原文示例：

```
MusicPlayer_01_Overture_I_Genesis = {
    composer  = Mattias_Henndlund
    performer = Mattias_Henndlund
    soloist   = Mattias_Henndlund
}
```

## 必需的本地化（info 明确要求）

```
MusicPlayer_01_Overture_I_Genesis:          "Overture I Genesis"      # 曲名
MusicPlayer_01_Overture_I_Genesis_flavour:  "This song is..."         # 简介
Mattias_Henndlund:                          "Mattias Henndlund"       # 作者/演奏者名
```

文件位置：`main_menu\localization\<lang>\music_player_l_<lang>.yml`（info 说的是 `music_player_l_english.yml`）。

## 原版实测

| 项 | 值 |
|---|---|
| 数据文件 | `00_music_player_tracks.txt` **10.9 KB**（唯一）/**82 曲**；DLC 另有自己的档（`dlc\D008_…\in_game\common\music_player_tracks\D008_music_player_tracks.txt` 1.3 KB） |
| 字段实测 | `composer` 82/82、`performer` 82/82、**`soloist` 仅 13** |
| info | `music_player_tracks.info` 533 B |
| 界面 | `gui\` 的 `music_player` 相关文件（主菜单域另有 `main_menu\localization\music_player_gui\` 11 个文件） |

## 审查要点

- **条目名必须是 wwise 事件键**——写曲名不会播放（info 首行即强调）。
- 三个可选字段（`composer` / `performer` / `soloist`）的值是**本地化键**，不是自由文本；三个键都要在 loc 文件里存在。
- **曲名键 + `_flavour` 键**成对（info 示例），漏掉简介键则界面缺一行文案。
- 新增音乐要同时动三处：wwise 工程（引擎外）、本目录的条目、`music_player_l_<lang>.yml`。
- **机制与"能改到哪一步"见 `vanilla\vanilla-audio-and-fonts.md` §一**：替换曲目可行（按 `Init.txt` 的 ID 换 `Media\<ID>.wem`），**新增曲目必须回 Wwise 重打包 `.bnk`**；按文化配乐在 `main_menu\music\audio_culture_types\`。
- 未在 readme 中说明：本类目**没有 readme**，只有 533 B 的 info；曲目与播放时机的绑定（何时放哪首）未在数据侧文档化。
