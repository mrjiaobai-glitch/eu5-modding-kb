# 参考资料：存档编辑（Save-game editing）

> **来源**：[EU5 Wiki: Save-game editing](https://eu5.paradoxwikis.com/Save-game_editing)（`action=raw` 抓取，2026-10-30；**非铁人档**前提；文中多处标 TODO，说明社区也未完全解出格式）。
> **性质**：外部资料，**未实测**；本库另有 `tools\save-decoding.md`（我们自行解码出的词元 ID，可与本档互补）。
> **体量**：约 6 分钟

## 一、存档位置
| 类型 | Windows |
|---|---|
| 本地 | `Documents\Paradox Interactive\Europa Universalis V\save games\` |
| 云（本地镜像） | `C:\Program Files (x86)\Steam\userdata\<SteamID>\3450310\remote\save games` |

云存需在 Steam 游戏属性与 Steam 设置里都开启云同步。Linux/Proton 路径见原文。**第三方工具** [PDX Unlimiter](https://github.com/crschnick/pdx_unlimiter) 可把存档解析成结构化树，比手改省事。

## 二、文件头（两种存档都有）
```
SAV02003fb9bd370004e75d00000000
```
| 段 | 含义 |
|---|---|
| `SAV` | 固定 |
| `02` | 版本号（截至 2025-12 恒为 02） |
| `00` / `03` | `00` = debug 文本档；`03` = 压缩档 |
| 8 位十六进制 | 校验/ID（原文标 TODO） |
| `0004` / `0006` | `0004` 文本档；`0006` 压缩档 |
| 4 位 + 8 个 0 | 另一校验 + 填充 |
头部后固定一个换行（0x0A）。

## 三、debug 模式存档（纯文本）
与 mod 文件同格式：`name=value`；值可以是 atom / 引号字符串 / 整数 / **定点小数（恒 5 位）** / 日期 `1464.5.18`（可带小时 `.12`）/ 数组 `{ a b }` / 映射 `{ k=v }`；字符串内引号用 `\` 转义，**字符串内允许换行**。

## 四、压缩档（非 debug）
文件头后是 metadata 块 + **zip**（内含 `gamestate` 与 `string_lookup` 两个文件）。全部小端。
- 字符串 = **16 位长度 + 字节**（Hollerith；如 `05 00 hello`）。
- gamestate 是 name-value：name 为 2 字节 ID，之后可能有 `01 00` 标记再跟值；无标记 = 数组项。

**常见类型码**（原文表）：
| 码 | 含义 |
|---|---|
| `4b50` | 不是 ID——到了 zip 文件，解压后重来 |
| `0003` | 子对象（递归，直到 `0004`） |
| `0004` | 映射/数组结束 |
| `000c` / `0014` | 32 位整数（可能是日期 / 有符号） |
| `000e` | 布尔：后 1 字节 01/00 |
| `0017` / `000f` | Hollerith 字符串 |
| `0167` | 64 位定点（÷10000 得实际值） |
| `0243` | RGB 颜色（后跟 `0003` 子数组 3 个整数） |
| `0d3e` / `0d40` | 字符串表索引（16 位 / 8 位） |
| `0d48`–`0d4e` | 1–7 字节定点（同 0167） |
| `0d4f`–`0d56` | 同上但为负 |
| `09de` | `metadata`（在文件头后紧跟着出现） |
| 其它 | 固定 ID，无公开表 |

- `string_lookup`：5 字节头（1 字节未知 + 2 字节字符串数 + 2 字节未知）+ 连续 Hollerith 字符串；由索引引用（首串 index 0 → `40 0d 00`；大索引用 `3e 0d xx yy`）。
- **日期编码**：自公元前 5000 年起的**小时数**（忽略闰年）：`((year + 5000) * 365 + dayOfYear) * 24`，`dayOfYear` 从 0（1/1）到 364（12/31）。

## 五、和本库的关系
- `tools\save-decoding.md`：我们实测出的词元 ID（如 `43046`/`core`、`zzz_core_ready_years` 等）与"核心何时消失"的查法——**本档给 wiki 版格式，那份给实测词元**，两者互补。
- 改存档前先备份；铁人档不适合此流程（原文假设非铁人）。
