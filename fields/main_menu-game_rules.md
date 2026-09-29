# main_menu/common/game_rules（游戏规则）与 scripted_lists（自定义脚本列表）

> **一句话**：游戏规则的字段与 flag 清单，附脚本化列表的 base 与 conditions 写法及本地化三件套。
> **什么时候看**：加游戏规则选项、要用 flag 禁成就或锁生产方式时翻这篇。
> **体量**：72 行 · 约 4 分钟通读

来源：`main_menu\common\game_rules\_game_rules.info`（**1,373 B**）+ `00_game_rules.txt`（487 行 / **30 条规则**）；`main_menu\common\scripted_lists\scripted_lists.info`（549 B）

## game_rules 字段（info 全表）

```
<rule 键> = {
    default = <setting 名>            # 默认档

    <setting 名> = {
        apply_modifier = <类别>:<修正键>   # 给匹配类别的角色套修正；类别：player / ai / all
                                          # 例：player:very_easy（对应 main_menu\common\static_modifiers\difficulty.txt）
        defines = {                       # ★ 激活时【覆盖引擎常量】，格式同 defines 文件
            NArmy = { MOVEMENT_SPEED = 10 }
        }
        flag = <flag 键>                   # 特殊行为标记（见下）
    }
}
```

**本地化键三件套**（info 原文）：`rule_<key>`（规则名）、`setting_<key>`（档位名）、`setting_<key>_desc`（档位描述）。

## flag 清单（info 原文）

| flag | 作用 |
|---|---|
| `blocks_achievements` | 该档下**不能获得成就** |
| `lenient_ai` / `harsh_ai` | AI 宽容 / 严苛（info 标 `???`——官方自己未说明） |
| `low_ai_aggression` / `high_ai_aggression` | AI 攻击性（同上标 `???`） |
| `no_subject_flags` | 附属国旗帜不叠加宗主旗角 |
| `no_subject_map_color` | 附属国不共享宗主地图色 |
| `no_ahistorical_formable_countries` | 禁止成立非历史可成立国家 |
| **`disable_<production_method 键>`** | 该生产方法**任何情况下不可启用** |
| **`force_<production_method 键>`** | 该生产方法**强制启用且不可切走** |

## 原版实测（30 条规则 / 487 行）

flag 使用频率：**`flavour_rule` 39** · `general_rule` 29 · **`blocks_achievements` 17** · `multiplayer_rule` 15 · `ai_personalities_random` 3 · `proficiency_novice` 2 · `task_rewards_disabled` 2 · `rename_locations_allowed` 1。

> ⚠️ **`defines` 覆盖在原版用得是 0 次**——能力存在但**没有任何原版样例**。想用 `defines` 覆盖（例如在规则里改 `NCombat`）只能照 info 的格式自己试，调试时留意多人哈希与存档兼容（见 `guides\defines.md`）。
>
> 已确认的原版规则例（`00_game_rules.txt`）：`ai_personalities`（默认 `ai_personalities_historical`，另有三档随机）、`ai_colonisation_rule`（默认 `all_countries_colonize`）、`ai_exploration_rule`（默认 `western_exploration_only`）。

## scripted_lists 字段（info 全表）

```
<列表名> = {
    base = <脚本列表>          # 只写 any_/random_/every_/ordered_ 之后的部分，如 "country"、"character"
    conditions = { <triggers> }  # 施加到列表元素的全部条件
}
```

定义后**当普通脚本列表使用**（info 例子）：

```
adult_character = { base = character  conditions = { is_adult = yes } }
every_adult_character = { add_adm = -5 }        # 可直接用 every_/any_/random_/ordered_ 前缀
```

## 审查要点

- **flag 是行为开关而不是文案**：加 `blocks_achievements` 会立刻禁成就——发布前想清楚（`vanilla` 里 17 处用它）。
- `apply_modifier` 的修正键必须来自 `main_menu\common\static_modifiers\`（如 `difficulty.txt` 的 `difficulty_ai_hard`），且类别前缀只能是 `player` / `ai` / `all`。
- `defines` 覆盖**原版零使用**：改常量会牵动多人哈希与存档兼容。
- `scripted_lists` 的 `base` 只写后缀（`character` 而**不是** `every_character`）；定义好的列表在脚本里**必须带前缀**调用。
- 未在 readme 中说明：两个类目**都没有 readme**，只有各自的 `.info`。
