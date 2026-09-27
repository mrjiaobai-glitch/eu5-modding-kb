# error.log 解码表（报错串 → 病因 → 首查点）

> **来源**：本库的实测记录（`pitfalls.md`、`cases\blades-and-thrones-2026-08.md`、`guides\scripting-core.md`）与官方 `.info` 原文。**没有跑过游戏就没有新条目**——补条目时必须注明"报错串原文 + 来源 + 版本"，不要把猜测写成事实。
>
> **本档不做的事**：不列引擎全部报错串（没有全表）；不解释与 mod 无关的原版报错（见 §一 的归属法）。

## 一、先做归属判断（否则会白改）

1. **看 `Script location:` 行**——它给出 `文件:行号`。若指向**原版文件**，先怀疑是原版自身或"你改了行号导致对不上"。
   - 实战：1.3.11 有一条高丽必现的报错**与本 mod 无关**，先看归属再动手。
2. **区分报错阶段**：
   - **加载期**（拼写/引用/括号）→ 多为 `missing effect` / `missing trigger` / raw key；
   - **运行期**（作用域/变量/界面）→ 多为 `Failed to fetch variable` / `unset scope` / `Invalid scope types`。
3. **存档 metadata 不含启用 mod 列表**——验证"游戏跑的是哪个版本"，用 `Script location` 的行号与本地文件对照（行号差 = 版本不一致）。

## 二、报错串 → 病因 → 首查点

| 报错串（原文） | 典型病因 | 首查点 | 来源 |
|---|---|---|---|
| `Failed to fetch variable ... due to not being set` | 变量**用前没初始化**（读一个从未 `set_variable` 的变量）；或用错的 scope 里读（跨 scope 是两份变量） | 使用侧之前是否初始化；初始化与使用**是否同 scope** | `pitfalls.md` §三 |
| `Event target link 'var' returned an unset scope` | 同上，且常与上一条**连锁出现**（数值变量的存在性守卫写成了 `exists = var:X` → 守卫不生效、初始化被跳过） | 数值变量**改用 `has_variable = X`** 守卫；`exists = var:X` 只宜用于"国家引用变量" | `pitfalls.md` §三（实测单会话 670 次） |
| `Data error in loc string` | 本地化串里引用了**未初始化的变量**（老档/无历史时悬停选项即报） | 在事件 `immediate` 里做**幂等兜底初始化**（`if NOT has_variable → set 0`） | `pitfalls.md` §三 |
| `Duplicated event ID` | **无害**：改了原版事件的常见后果（新文件 + 原版 namespace + 复制修改） | 不用改；但**事件不能 REPLACE/INJECT**，只能复制修改 | `pitfalls.md` §四、`guides\merging.md` |
| `missing effect`（大量刷） | 参数化 `scripted_effect` 的 **`$参数$` 没传**（或拼错参数名） | 调用处是否把该 effect 声明的全部 `$…$` 都给了；例如 `my_arg_$type$` 那类"参数拼进键名"的写法最易漏 | `fields\common-scripted_effects.md`、`guides\scripting-core.md` |
| `missing trigger`（大量刷） | 参数化 `scripted_trigger` 的 `$参数$` 未传 | 同上 | `fields\common-scripted_triggers.md` |
| `Invalid scope types for event ...` | 在**错误的作用域**里访问对象：如 estate scope 不能在 location 作用域内访问；或 `?=` 可选块内写了裸 trigger（那里是 effect 上下文） | 该效果/触发器的合法作用域表；`?=` 块内的上下文 | `cases\blades-and-thrones-2026-08.md` §5 |
| raw key（界面直接显示键名，如 `my_mod.1.title`） | 引用的 **loc 键不存在**：键写错、漏键、忘 BOM、放在错误的位置，或键在 DLC / `missions\` 子目录里 | `tools\loc-keys.md` 的"判缺键三原则"（键全局、含 DLC、含子目录） | `pitfalls.md` §五 |

## 三、启动即崩 / assert（目录与文件级）

| 现象 | 病因 | 首查点 |
|---|---|---|
| 删掉空壳目录后启动 assert | **`policies` / `mission_task_defs` / `scripted_rules` 是空壳·勿删**（info 原文：目录为空但删掉会 assert；任务项其实在 `missions\` 里） | `guides\systems-map.md` 对应行 |
| yml 整个文件被忽略 / 中文乱码 | loc 文件**无 BOM**（或编辑工具保存时剥了 BOM） | 读前 3 字节是否为 `EF BB BF`；**每次编辑后都要查**（`pitfalls.md` §五） |
| 改了没效果但无报错 | **静默失效类**：ID 前缀写反（见 `tools\audit-ids.md`）、修正键未注册、`ai_is_valid` 未开、`scripted_guis` 漏 `.End`、`using` 指向不存在的库控件、条件写在了不生效的字段（`potential` vs `allow`） | `tools\review-checklist.md` 的"语义陷阱"与"引用完整性"两节 |

## 四、取证与复现手段（本库已有）

- **脚本测试**：`in_game\common\tests\`（`year` / `success` / `failure` / `end_year` / `success_child`，日志行格式见 `fields\common-tests.md`）。
- **调试事件**：`events\debug\qa_debug.txt` 的 `orphan = yes` + `trigger = { always = no }` 模板；`000_johan_debug.txt` 是**实体自检**事件（`mission:` / `goods:` / `road_type:` 等实体引用清单）。
- **观察者模式**：验证 AI 行为（160 年 0 触发的教训见 `pitfalls.md` §六）。
- **版本/改动定位**：用 `Script location` 行号对照本地文件行号。

## 五、相关档

`guides\testing.md`（测试流程）· `tools\review-checklist.md`（可机械判定项）· `tools\audit-ids.md`（ID 与写法核对）· `tools\loc-keys.md`（键存在性）· `pitfalls.md`（坑速查）· `cases\`（真实复盘）
