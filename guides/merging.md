# 文件合并体系（merging）

EU5（jomini）中，mod 文件与原版文件的合并由**文件名路径 + 前缀**控制。这是 mod 制作者最常踩坑的环节。

## 一、同路径文件的合并语义

- **mod 的 `in_game\common\X\foo.txt` 与原版 `in_game\common\X\foo.txt` 同名**：文件内容按块（block）合并，同名块的行为由前缀决定；**没有前缀的普通块 = 覆盖原版同名块**。
- **不同文件名**：全部加载，同键条目按加载顺序后者覆盖前者（所以 `zzz_` 前缀的 mod 文件能盖过 `00_` 原版条目）。

## 二、前缀（写在块名前面）

| 前缀 | 语义 | 适用 |
|---|---|---|
| `INJECT:` | 块内容**追加到原版同名块末尾** | 给原版列表加条目（事件列表、on_action 列表等）；**顺序敏感块不可用**（追加排到 fallback 后面轮不到，见下） |
| `REPLACE:` | 整块替换原版同名块 | 改原版条目；**必须逐字段保留原版内容**，只改目标字段，否则丢失其他字段 |
| `REPLACE_OR_CREATE:` | 有则 REPLACE，无则新建 | 同上，且不怕原版没有 |
| `INJECT_OR_CREATE:` | 有则 INJECT，无则新建 | 同上 |
| `EDIT:` | 原版同名块内**按路径替换单个键**（jomini 引擎支持，注意核实版本） | 精细修改 |

## 三、on_action 特殊语义（`on_actions.info` 权威）

**on_action 的同名块整块定义是合并语义，不需要前缀**：mod 里写

```
my_on_action = {   # 与原版同名
    events = { my_event.1 }   # 追加到原版 events 列表
}
```

即：多个文件定义同一 on_action 时，`events` / `random_events` / `on_actions` / `effect` 等列表**追加合并**，`trigger` 不覆盖。若要整体替换，才用 REPLACE:。

## 四、顺序敏感块（INJECT 禁用）

"第一个匹配生效"的块，追加的条目排在原版条目之后、永远轮不到：
- **levies 特化单位**（`in_game\common\levies\`，readme 明示"特化单位必须放文件顶部"）
- **country_name_construction**（国家名构造，首个匹配生效）
- 处理方式：整体覆盖文件（REPLACE 或直接同名无前缀块），新条目插在 fallback 前。

## 五、事件不可合并

**事件（events\*.txt）不能 REPLACE/INJECT**（事件系统按 namespace+id 全局注册）。改原版事件 = 新文件 + 原版 namespace + 复制修改（会产生无害的 "Duplicated event ID" 日志，但新定义生效）。原版事件 ID 冲突时新定义覆盖旧定义（同 ID 后者胜）。

## 六、本地化合并

yml 无块合并：同键后者覆盖前者。mod 语言文件命名 `zzz_` 保证后加载覆盖原版键；**镜像规则**见 `localization.md`。

## 七、判断技巧

- 不确定原版块名/结构 → 先 read 原版文件再决定前缀。
- 新增内容（新事件、新科技、新建筑、新修正）**不需要任何前缀**，新文件新键即可。
- 覆盖原版条目 → REPLACE_OR_CREATE + 保留原字段。
- 给原版列表追加 → INJECT（顺序敏感除外）。
