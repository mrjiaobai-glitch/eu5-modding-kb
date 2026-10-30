# 文件合并体系（merging）

> **一句话**：mod 与原版文件的合并规则：按文件名路径加 **六个**前缀（INJECT / REPLACE / TRY_* / *_OR_CREATE）控制，含 on_action、事件与本地化的特殊语义。**2026-10-30 起标注验证状态：实跑前凡标 ⚠️待实跑 的都是 wiki/引擎串旁证，不是本机实测。**
> **什么时候看**：要覆盖或追加原版内容时，先在这里查该用哪种前缀、能不能用。
> **体量**：88 行 · 约 4 分钟通读

EU5（jomini）中，mod 文件与原版文件的合并由**文件名路径 + 前缀**控制。这是 mod 制作者最常踩坑的环节。
> **🧪 2026-10-30 实跑结论（EU5 1.4.5，测试 mod `kb_load_order_test`）**
> 1. **同名文件 = 整文件覆盖**（不是按块合并）：mod 放同路径同名文件后，`error.log` **没有**重复键日志（VFS 同路径遮蔽，原文件其余块一并消失）。**对照实验**：同 mod 内换个文件名再定义重复 trait，立刻报 `Duplicated key navigator will not be created from file: common/traits/zz_kb_dup_test.txt:1`。→ 本库旧说"按块合并"**错**，wiki 对。
> 2. **本地化同键 = 后加载覆盖生效**：普通 `localization/<lang>/kb_test_*.yml` 覆盖了原版 `core_l_simp_chinese.yml` 的 `EXIT`（界面实测显示 `ZZZ_MOD_OVERRIDE_WORKS`）；`debug.log` 记 `Key 'EXIT' defined in multiple files: ...core_..., ...kb_test_...`。→ 本库旧说**对**，wiki"后加载不覆盖、须 replace 目录"**错**（`replace/` 仍可用但非必需）。
> 3. **同名条目 = 先处理者胜、后到者被丢弃**：游戏数据库键的日志原文是 `Duplicated key <键> will not be created from file: <文件>:<行>`——**后处理的那份不创建**。事件同 ID 也一样会被丢弃，日志格式为 `Duplicated event ID '闸' found. New Location: '<新文件>', Previous Location: '<先注册文件>'`。
> 4. **文件处理顺序不是 ASCII 升序**：两轮实测（2 文件 / 3 文件）中 `kb_aaa_test.txt` **始终是 "New Location"**（即最后处理），`kb_mmm/kb_zzz` 是 Previous；lexer 的 BOM 警告却按 aaa→zzz 出现（预扫描序≠解析序）。→ **不要靠 `zzz_` 之类文件名去赌条目归属**，要么用 `REPLACE:`，要么用新 namespace+新 ID。
> 5. `.txt` 不带 UTF-8 BOM 会被警告：`should be in utf8-bom encoding (will try to use it anyways)`（能跑；带 BOM 的不再警告，实测对照）。

## 一、同路径文件的合并语义

- **同名文件（同路径同文件名）= 整文件覆盖**（✅ 2026-10-30 实跑）：mod 放同路径同名文件后，原文件其余块**一并消失**，**不是**按块合并；`error.log` 无任何重复键日志即为证。→ 改原版文件**必须复制整份原文件再改目标块**，只写一个块会把其它块全丢掉。
- **不同文件名**：全部加载，**声明为文件名序**；**子目录文件排在父目录文件之后**（本体 `in_game\common\levies\readme.txt:27`「file name order first, then declaration order」只证明 levies 平局裁决用文件名序）。⚠️ **与 wiki 不同**：wiki 称"全部文件按 ASCII **升序**加载"；我们两轮实测（2 文件 / 3 文件）里 `kb_aaa_test.txt` **始终是最后被处理**的（"New Location"），即**不是升序**——见 §八。
- ⚠️ **同键重复的胜负待实跑**：引擎字符串（`gamedatabase.h`）为 `Duplicated key {} will not be created from file: {}`——**先加载者胜、后者不创建**；本库旧结论"后者覆盖前者"与此相反。在实跑确认前，**别依赖 `zzz_` 前缀去压原版同键**，改用 `REPLACE:`。

## 二、前缀（写在块名前面）

| 前缀 | 语义 | 适用 / 限制 |
|---|---|---|
| `INJECT:` | 把内容**追加**到已存在条目的定义末尾 | 给原版列表加条目（事件列表、on_action 列表等）；**目标必须已存在**；**只能用于顶层块**（不能注入子块）；⚠️待实跑：对 `scripted_effects` / `scripted_triggers` 使用 ≈ **覆盖**；目标已定义 `trigger`/`effect` 块时再注入同类块会失败 |
| `REPLACE:` | **整条替换**已存在条目 | 改原版条目；**目标必须已存在**；**必须逐字段保留原版内容**，只改目标字段，否则丢失其他字段 |
| `TRY_INJECT:` | 同 INJECT，但目标不存在时**不报错** | 兼容性更稳 |
| `TRY_REPLACE:` | 同 REPLACE，但目标不存在时**不报错** | 同上 |
| `INJECT_OR_CREATE:` | 有则 INJECT，无则新建 | 安全追加 |
| `REPLACE_OR_CREATE:` | 有则 REPLACE，无则新建 | 安全覆盖（最常用） |

> **前缀实证（2026-10-30 静态取证）**：① 六个前缀在 `binaries\eu5.exe` 里全部命中；② **`EDIT:` 0 命中——该前缀不存在**（本库旧表里的 `EDIT:` 已删，属 EU4/CK3 遗留）；③ 目标不存在时的引擎报错原文：`You are trying to inject/replace to a non-existing entry: {}:{} in the file {}`。
>
> ⚠️ **处理顺序（wiki 说法，待实跑）**：先按**操作类型**、再按文件名——`INJECT_OR_CREATE → REPLACE_OR_CREATE → TRY_INJECT → TRY_REPLACE → INJECT → REPLACE`。即 **REPLACE 无条件压过 INJECT**（与文件名优先级无关）；`INJECT` 早于 `REPLACE` 处理，所以"先 inject 再被 replace 盖掉"是可能的。

## 三、on_action 特殊语义（`on_actions.info` 权威）

**on_action 的同名块整块定义是合并语义，不需要前缀**：mod 里写

```
my_on_action = {   # 与原版同名
    events = { my_event.1 }   # 追加到原版 events 列表
}
```

即：多个文件定义同一 on_action 时，`events` / `random_events` / `on_actions` 列表**追加合并**（⚠️**待实跑**：wiki 称只能追加这三类，**不能替换 `trigger`/`effect` 块**；要改这两块须整文件覆盖）。

## 四、顺序敏感块（INJECT 禁用）

"第一个匹配生效"的块，追加的条目排在原版条目之后、永远轮不到：
- **levies 特化单位**（`in_game\common\levies\`，readme 明示"特化单位必须放文件顶部"）
- **country_name_construction**（国家名构造，首个匹配生效）
- 处理方式：整体覆盖文件（REPLACE 或直接同名无前缀块），新条目插在 fallback 前。

## 五、事件不可合并

**事件（events\*.txt）不能 REPLACE/INJECT**（事件系统按 namespace+id 全局注册）。改原版事件 = 新文件 + 原版 namespace + 复制修改（会产生无害的 "Duplicated event ID" 日志，但新定义生效）。✅ **重复会被记录、后到者不创建（实跑）**：`error.log` 记 `Duplicated event ID 'kb_test.1' found. New Location: 'events/kb_aaa_test.txt:3', Previous Location: 'events/kb_mmm_test.txt:3' / '...kb_zzz_test.txt:3'`。由游戏数据库的对照日志 `Duplicated key <键> will not be created from file: <文件>` 可知**后处理的那份被丢弃**；且 **aaa 始终是 New**（最后处理）→ 文件处理顺序**不是 ASCII 升序**，别赌文件名。改原版事件用**新 namespace + 新 ID**。

## 六、本地化合并

yml 无块合并。✅ **同键覆盖已实跑（2026-10-30）：后加载覆盖生效**——普通 `localization/<lang>/` 下的 mod 文件即可覆盖原版键（实测覆盖简中 `EXIT` 成功）；`debug.log` 记 `Key 'EXIT' defined in multiple files: '...core_l_simp_chinese.yml', '...kb_test_l_simp_chinese.yml'`。→ [wiki](https://eu5.paradoxwikis.com/Mod_compatibility) 的"后加载不覆盖、必须 replace 目录"**不成立**（`replace/` 仍可用，但非必需）。放哪个区见 `localization.md`（**不需要双镜像**，两处都会加载）。

## 七、判断技巧

- 不确定原版块名/结构 → 先 read 原版文件再决定前缀。
- 新增内容（新事件、新科技、新建筑、新修正）**不需要任何前缀**，新文件新键即可。
- 覆盖原版条目 → REPLACE_OR_CREATE + 保留原字段。
- 给原版列表追加 → INJECT（顺序敏感除外）。

## 八、与 EU5 Wiki 的差异（2026-10-30 实测，EU5 1.4.5）

> 约定：凡与 [EU5 Wiki: Mod compatibility](https://eu5.paradoxwikis.com/Mod_compatibility) 不一致处，正文对应位置都标 **⚠️ 与 wiki 不同**；下表是全量对照。

| 主题 | wiki 说法 | 本库实测 / 证据 | 判定 |
|---|---|---|---|
| 同名文件 | 整文件覆盖 | **整文件覆盖**（同路径 → `error.log` 无重复键日志；VFS 遮蔽） | ✅ 一致 |
| 同键/同 ID 胜负 | 先加载者胜 | **先处理者胜、后到者被丢弃**（`Duplicated key <键> will not be created from file: <文件>`） | ✅ 一致（但"先"是**处理序**，不是文件名序） |
| 文件加载顺序 | 全部按 **ASCII 升序** | **不是升序**：`kb_aaa_test.txt` 两轮都是 "New Location"（最后处理） | ❌ **不同** |
| 本地化同键 | 后加载**不**覆盖，须 `localization/<lang>/replace/` | **后加载覆盖生效**：普通目录的 `simp_chinese/kb_test_*.yml` 覆盖了原版 `EXIT`（界面实测） | ❌ **不同** |
| 六个前缀 | INJECT / REPLACE / TRY_INJECT / TRY_REPLACE / INJECT_OR_CREATE / REPLACE_OR_CREATE | `binaries\eu5.exe` 六个全部命中；`EDIT:` **0 命中** | ✅ 一致（`EDIT:` 不存在） |
| 处理顺序按操作类型优先 | `INJECT_OR_CREATE → REPLACE_OR_CREATE → TRY_INJECT → TRY_REPLACE → INJECT → REPLACE`（REPLACE 无 filename 优先） | 未实测 | ⏳ 待测 |
| INJECT 只能顶层 / 对 scripted_effects 近似覆盖 | 如此 | 未实测 | ⏳ 待测 |
| GUI type/template 按播放集序（文件名无效） | 如此 | 未实测 | ⏳ 待测 |
| defines 可只替换单个键 | 如此 | 未实测（其它类目同名文件是整文件覆盖） | ⏳ 待测 |
| 事件/子目录加载序 | 子目录晚于父目录 | 未实测（已确认整体非升序，勿依赖） | ⏳ 待测 |
| on_action 只能追加、不能换 trigger/effect | 如此 | 与库内说法一致，未实跑 | ⏳ 待测 |

**结论**：wiki 在"同名文件"与"重名先胜"两条上经验证**成立**；在**"ASCII 升序加载"**与**"本地化后再不覆盖、必须 replace 目录"**两条上**不成立**。
