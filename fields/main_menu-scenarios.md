# main_menu/common/scenarios（推荐开局卡）

> **一句话**：推荐开局卡的字段：国家、纹章键、玩法取向与熟练度档，并强调 flag 是纹章键不是 tag。
> **什么时候看**：加主菜单推荐开局卡、要配旗帜或核对玩法取向前缀大小写时翻这篇。
> **体量**：46 行 · 约 3 分钟通读

来源：**无 readme**——`00_scenarios.txt`（**1 KB / 10 个场景**）实查，**文件头一行注释**说明取旗方法。

## 它是什么

主菜单"**推荐开局**"卡片：每个场景指向一个国家 + 一张纹章旗 + 玩法取向 + 熟练度档。

```txt
# You can use the "coa" console command to get the flag for a particular country.

TUR_scenario = {
    country   = TUR                  # 国家 tag
    flag      = TUR_scenario         # ★ COA 键（不是 tag！——去 coat_of_arms 里找同名键）
    player_playstyle   = MILITARY    # 玩法取向（大写）
    player_proficiency = NOVICE      # 熟练度档
}

CAS_scenario = {
    country = CAS
    flag    = CAS_castile_leon
    player_playstyle   = MILITARY
    player_proficiency = EXPERIENCED
}
```

## 原版实测（10 个场景）

| 字段 | 出现 | 说明 |
|---|---|---|
| `country` | 10/10 | 国家 tag |
| `player_playstyle` | 10/10 | 大写枚举（原版见 `MILITARY` 等；同一枚举也用于 `scriptable_hints` / `missions` 的 `player_playstyle`，那边是小写） |
| `player_proficiency` | 10/10 | 原版见 `NOVICE` / `EXPERIENCED`（**与游戏规则里的 `proficiency_*` 旗标同一套档位**，见 `fields\main_menu-game_rules.md`） |
| `flag` | 8/10 | **COA 键**；缺省时用 tag 当 COA 键（同 `flag_definitions` 的兜底规则） |

## 审查要点

- **`flag` 是 COA 键、不是 tag**：写 `flag = TUR` 只有在 `coat_of_arms` 里恰好有 `TUR` 键时才成立；原版有两条场景直接用 tag、其余用 `X_scenario` 之类的**专用旗**。取值规则见 `vanilla\vanilla-heraldry-and-flags.md`。
- 需要 COA 键存在（`main_menu\common\coat_of_arms\coat_of_arms\`，4,566 个键）。
- `player_playstyle` / `player_proficiency` 是**大写枚举**——与小写用法混用是常见错误（两者都可能静默不生效）。
- 未在 readme 中说明：本类目**没有 readme**；枚举的完整取值清单、卡片排序规则、场景是否会覆盖游戏规则里的熟练度默认值（`mission_packs_enabled_rule` 的 `proficiency_expert` 旗标）均未文档化。
