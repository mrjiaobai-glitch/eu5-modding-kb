# 参考资料：社区模组框架 CMF（Community Mod Framework）

> **来源**：[EU5 Wiki: Community Mod Framework](https://eu5.paradoxwikis.com/Community_Mod_Framework)，`action=raw` 抓取 2026-10-30。
> **性质**：外部资料，**未安装实测**；作者/仓库：`Europa-Universalis-5-Modding-Co-op\community-mod-framework`（Steam Workshop id `3692202776`）。
> **体量**：约 6 分钟

## 一、CMF 是什么
跨 mod 的**共享框架**：共享 mod 菜单（CMM）、自定义行动栏、动态警报、工具触发器、大厅横幅、Mod 行动日志、on_action 钩子；配套 **CMT 工具包**（可视化编辑器 / 工坊上传 / 翻译）。**没有依赖 mod 时"隐形"**。

## 二、接入（唯一入口）
```text
# in_game/common/scripted_effects/your_effects.txt
your_mod_register = { … }

# in_game/common/on_action/your_on_actions.txt
cmf_on_mod_registration = { on_actions = { your_mod_on_register } }

your_mod_on_register = {
	effect = { your_mod_register = yes }   # 可拆成多个 effect
}
```
CMF 在**每位人类玩家开局**与**打开 mod 菜单**时触发该 on_action。

## 三、主要能力
| 能力 | 用法要点 |
|---|---|
| **CMM 设置菜单** | 注册开关/滑条/下拉/按钮/文本；有可视化编辑器；设置存 `variable_map(cmm|…)` |
| **行动栏** | `cmf_add_action_bar_element = { element = <root_key> }` / `cmf_remove_action_bar_element`；点击走 `cmf_on_callback`（`var:cmf_callback` = 元素 flag）；隐藏：`GetVariableSystem.Set('cmf_hide_action_bar','yes')` |
| **动态警报** | `cmf_show_alert = { alert = <root_key> }`、`cmf_remove_alert`、`cmf_is_alert_active`（触发器）；**需 country scope**；左键点击自动移除；点击走 `cmf_on_callback` |
| **大厅横幅** | `cmf_register_lobby_banner = { mod_id = your_mod }`（挂在 `cmf_on_banner_registration` 下） |
| **Mod 行动日志** | `cmf_log = { action = <flag> }`、`cmf_log_with_args`（arg1/arg2）、`cmf_log_with_scope_arg(s)`、`cmf_clear_log` |
| **共享钩子** | `on_game_start_after_lobby(_human_country)`、`on_game_load(_after_lobby)(_human_country)`、`cmf_on_country_transfer`（带 `scope:old_country`/`new_country`）、`cmf_yearly/monthly_human_country_pulse` |
| **触发器** | `is_host`（单机恒真）、`cmf_can_use_unrestricted_tools`（仅主机+开启"无限制工具"）；`cmf_is_mod_active = { mod_id = … }`、非 CMM mod 用 `cmf_register_mod` |
| **消警告** | `cmf_suppress = { v = 变量或 flag }`（放在 `always = no` 死分支里） |
| **GUI 宏** | `[Nand(Foo,Bar)]`、`[Nor(...)]`、`[Xor(...)]` |
| **顶层控件覆盖** | 已抽取 `foreign_country_lateralview` / `government_lateralview` / `ingame_topbar` / `location_window` / `outliner_entries` / `single_unit_window` / `technology_lateralview` 的定义，覆盖时只需写顶层控件 |

## 四、警报/按钮的本地化键
- 警报：`<root_key>_color`（blue/orange/red/red_war/black/yellow/green/purple）、`_icon`（如 `@advance!`）、`_name`、`_tooltip`
- 行动栏：`_color`、`_icon`、`_name`、`_tooltip`

## 五、条件可见 / 依赖弹窗
- 任何 CMM 设置或行动栏元素可配一个**同名 scripted GUI**（`is_shown` 控可见、`is_valid` 控灰色），注册：`cmf_register_scripted_gui = { element = … }`。
- **依赖检查弹窗**：从 releases 下载 `cmf-dependency-check.zip` 解压进 mod 目录；缺 CMF 时提示启用/订阅。

## 六、和本库的关系
- 这是**跨 mod 互操作**层；与 `reference\tools-community.md` 的 CMT（模板/上传/翻译）是配套两件套。
- 若你的 mod 不要求与其他 mod 共存设置，可直接不用 CMF。
- 原版脚本基础：`guides\scripting-core.md`、`reference\script-semantics.md`
