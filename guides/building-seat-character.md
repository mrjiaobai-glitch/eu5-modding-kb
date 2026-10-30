# 配方：建筑绑定一个角色（seat a character）

> **一句话**：想让"某建筑一建成，就有个角色坐进去并带某种身份"，**不用等引擎内建**——`on_built` 是通用建筑字段，`set_up_cardinal` 之类的"选人"逻辑本身就是普通 scripted effect；你要做的只是**建筑 → on_built → 脚本挑/造角色 → 绑地点 + 给身份**。
> **什么时候看**：自定义"席位/驻所/圣职/学院院长/总督府"类建筑、做 mod 专属的"有名无实"角色位、或想改原版枢机座堂的行为时翻这篇。
> **体量**：121 行 · 约 6 分钟通读

来源：`in_game\common\building_types\readme.txt:40-41`（`on_built`/`on_destroyed` 为通用字段）、`building_types\unique_buildings.txt:996`（`seat_of_cardinal` 范例）、`scripted_effects\on_action_effects.txt:667`（`set_up_cardinal` 全文可抄）、`modifier_type_definitions\00_modifier_types.txt:4181`（`gives_cardinal` 是注册键）。路径均在游戏根目录下。**引擎侧边界见 §四。**

## 一、为什么这套逻辑可以照抄

原版 `seat_of_cardinal` 的全部"魔法"拆开看，**没有一处是引擎私有**：

| 环节 | 原版实现 | 是否 mod 可复用 |
| --- | --- | --- |
| 建筑钩子 | `on_built = { location = { set_up_cardinal = yes } }` | ✅ `on_built`/`on_destroyed` 是**通用建筑字段**（`building_types\readme.txt:40-41`），本体 5 个建筑文件在用 |
| 选人 | `owner = { random_character = { limit = { 存活/成年/男/教士/同宗教/非艺术家/非修会/非枢机 } } }` | ✅ 全是通用效果 |
| 兜底 | 挑不到就 `create_character = { religion=...; female=no; estate=estate_type:clergy_estate; age={40 65} }` | ✅ 通用效果 |
| 绑定 | `set_character_location = 该地点` + `set_character_religious_figure = religious_figure:cardinal` | ✅ 通用效果（`fields\common-scripted_effects.md`） |
| 身份 | `religious_figures\02_catholic.txt` 里一句 `cardinal = { … }` | ✅ 新增 figure 就是加一个文件 |
| 引擎旗标 | `raw_modifier = { gives_cardinal = yes }` | ✅ `gives_cardinal` 是**注册过的 boolean 修饰键**（`00_modifier_types.txt:4181`, category=location） |

结论：**"建筑 → on_built → scripted effect → 角色"这条链 100% 在脚本层可复制**；只有"枢机"这个身份背后**引擎算的统计**（`cardinal` scope、`num_cardinals`、教廷权威里的 `PA_CARDINALS`）属于"只能用、不能新造"。

## 二、最小模板

**① 建筑**（`common\building_types\my_buildings.txt`）：
```txt
my_seat = {
    is_special = yes
    max_levels = 1
    is_indestructible = yes
    can_close = no
    pop_type = clergy
    allow = { always = no }              # 只靠事件/动作授予
    modifier = { local_max_literacy = 1.0 }   # 随等级缩放；不缩放用 raw_modifier
    on_built     = { location = { my_seat_effect = yes } }
    on_destroyed = { location = { my_seat_cleanup = yes } }
}
```
> 若要接**原版枢机系统**（地图变黄、计入 `num_cardinals`、教皇权威 `PA_CARDINALS`）：加 `raw_modifier = { gives_cardinal = yes }`，并让 `on_built` 调原版 `set_up_cardinal`。

**② 效果**（`common\scripted_effects\my_seat_effects.txt`，location 作用域）：
```txt
my_seat_effect = {
    save_temporary_scope_as = seat_loc
    owner = {
        if = {
            limit = {
                any_character = {
                    is_alive = yes  is_adult = yes  is_female = no
                    has_estate = estate_type:clergy_estate
                    religion = this.religion
                    is_artist = no
                    is_religious_order_member = no
                    or = { is_religious_figure = no  religious_figure != religious_figure:cardinal }
                }
            }
            random_character = {
                limit = { is_alive = yes  is_adult = yes  is_female = no
                          has_estate = estate_type:clergy_estate
                          religion = this.religion  is_artist = no
                          is_religious_order_member = no
                          or = { is_religious_figure = no  religious_figure != religious_figure:cardinal } }
                save_temporary_scope_as = chosen
            }
        }
        else = {
            create_character = {
                religion = this.religion  female = no
                estate = estate_type:clergy_estate  age = { 40 65 }
                save_temporary_scope_as = chosen
            }
        }
    }
    if = {
        limit = { exists = scope:chosen }
        scope:chosen = {
            set_character_location = scope:seat_loc
            set_character_religious_figure = religious_figure:cardinal   # 或自定义 figure
        }
    }
}
```

**③ 清理**（拆建筑 / 换人前先摘身份，**只清身份不杀角色**）：
```txt
my_seat_cleanup = {                       # location 作用域
    if = {
        limit = { exists = scope:chosen } # 或原版写法：location 的 cardinal scope
        scope:chosen = {
            reset_character_religious_figure = {}
            reset_character_location = {}
        }
    }
}
```
> 原版就是这么做的：`remove_cardinal_variables = { reset_character_religious_figure = {}; reset_character_location = {} }`（`on_action_effects.txt:662`）。

## 三、换人 / 自定义身份
- **换人**：先对旧 occupant 跑上面 `reset_character_*`，再对新角色跑 `set_character_location` + `set_character_religious_figure`。参考原版 `appoint_dynasty_cardinal`（`generic_actions\catholic.txt:480`）——它正是"有旧枢机→`remove_cardinal_variables`；再给新角色定位+身份"。
- **自定义 figure**：在 `common\religious_figures\` 加文件，形如 `my_office = { enabled_for_religion = { this = religion:xxx }; move_countries = no; schools = no }`，再用 `set_character_religious_figure = religious_figure:my_office`。
- **谁算"坐在里面"**：用地点的变量/flag 记录（`set_variable` / `add_to_global_variable_list`），别指望引擎给你造 `exists = my_figure` 这种作用域。

## 四、三条引擎边界（必记）

1. **不能自造引擎作用域/计数**：`exists = cardinal`、`num_cardinals`、`total_cardinals`、`PA_CARDINALS` 都是引擎认"枢机人物/`gives_cardinal`"后给的。自定义身份想统计数量，**自己用变量/列表**做，再用普通触发器读。
2. **`set_up_cardinal` 是"天主教口径"**：写死挑**教士阶层** + 给 `religious_figure:cardinal`。跨宗教或自定义 figure 时**自己写效果**，别直接调它（可整体抄走改条件）。
3. **换人=先摘后给**：直接覆盖会留脏状态（旧角色仍挂 figure / 仍绑旧地点）。原版每次换人都先 `reset_character_religious_figure` + `reset_character_location`。

## 五、引擎侧未证

- `gives_cardinal` 加在**自定义**建筑上，是否 100% 等价触发原版那套"地点 `cardinal` scope + 地图着色 + 计入 `num_cardinals`"（原版只在 `seat_of_cardinal` 与一个政府改革里用过）——**建议实测**：建一个带该键的自定义建筑，看地图是否变黄、`num_cardinals` 是否增加。
- `set_character_religious_figure` 对**未被 `enabled_for_religion` 覆盖**的宗教是否报错/静默无效。
- 同一地点能否叠多个"席位类"建筑与多个绑定角色（原版座堂 `max_levels=1`，未见此限制是否通用）。

## 审查要点

- `on_built`/`on_destroyed` 里操作的**作用域是 location**（写 `owner = { … }` 才进国家）；`random_character` 必须在某个国家作用域里，别直接写在 location 上。
- `is_indestructible = yes` + `allow = { always = no }` 是"只能由事件/动作授予"的常用组合；否则玩家能直接造或拆。
- 拆建筑只用 `reset_character_*` 摘身份，**不要**顺手 `kill_character`（原版不杀）。
- 需要"可见/可交互"的话，再配一条 `generic_action`（参考 `request_cardinal_seat` 的 `allow`/`cooldown`/`price`/`select_trigger` 写法）。
