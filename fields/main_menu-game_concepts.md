# main_menu/common/game_concepts（概念词条）

> **一句话**：概念词条的注册表：texture 与 alias 两个字段、概念链接写法，以及与概念本地化键的成对要求。
> **什么时候看**：界面上的概念链接不生效、要注册新概念或补词条解释时翻这篇。
> **体量**：58 行 · 约 3 分钟通读

来源：**本类目没有 readme、也没有 info**——`00_game_concepts.txt`（**62,722 B / 696 条**）实查。

## 它是什么

**`[<概念名>|e]` 这种"概念链接"的注册表**（界面里显示成带虚线下划线的词，悬停出解释）。loc 里到处在用：`[mission|e]`、`[event|e]`、`[exploration|e]`、`[subjects|e]`…——**没在这里注册的概念，写进 loc 也不会变成链接**。

```txt
modifier = {                       # 块名 = 概念名（也是 loc 键的一部分）
    alias = { modifiers }          # 可选：别名（复数/异体），各自也是一条概念
    texture = "modifiers/_default" # 图标（相对 gfx\interface\icons\ 的路径，不带扩展名）
}
```

## 字段（3 个，全量频次实测）

| 字段 | 频次 | 说明 |
|---|---|---|
| `texture` | 691 / 696 | 图标路径，**不带 `.dds`**（如 `"modifiers/_default"`）；指向 `gfx\interface\icons\` 下的文件 |
| `alias` | 416 / 696 | 别名块：`alias = { 单数 复数 }`——每个别名都能当概念用（也要各自的 loc 键） |
| `hidden` | **1** | 隐藏该概念（原版仅 1 处） |

## 本地化键

```
game_concept_<概念名>          # 名称（如 game_concept_modifier: "Modifier"）
game_concept_<概念名>_desc     # 解释文本（悬停显示）
game_concept_<别名>            # 每个 alias 也要一套
```

英文 loc 里 `game_concept_*` 共 **3,058 条键**（696 概念 + 416 别名，各带 `_desc`）。实例（`game_concepts_l_english.yml`）：

```
game_concept_modifier: "Modifier"
game_concept_modifiers: "Modifiers"        # ← alias
game_concept_modifier_desc: "Modifiers are values that influence how the game works, by for example …"
```

## 使用方式

- **loc 文本里**：`[modifier|e]` / `[subjects|e]`——`|e` 表示"概念链接"（另有 `|Y` 等格式后缀，见 `guides\localization.md`）。
- **界面里**：概念名可直接作 tooltip 词条（本库多篇用 `[xxx|e]` 写说明就是这个原因）。
- **脚本里**：概念本身**不影响任何机制**——它是纯展示层注册表（区别于 `game_rules` 之类的行为定义）。

## 审查要点

- **概念名与 loc 键必须成对**：注册了 `<名>` 却没有 `game_concept_<名>`，显示 raw key；反之 loc 里有键但没注册，`[<名>|e]` 不会变成链接（**静默失效**，最容易漏）。
- **`alias` 里的每一项都要自己的 `game_concept_<别名>`**（原版 416 个别名全部有键）——只写别名不写键是高频错误。
- **`texture` 不带扩展名**（写 `"modifiers/_default"` 而不是 `"modifiers/_default.dds"`）；路径以 `gfx\interface\icons\` 为根（与 `alert_descriptions` 的 `texture` 写法一致）。
- **`hidden` 只有 1 处**：隐藏概念是异类写法，别照抄。
- 概念是**全局命名空间**：mod 新增概念要用不易撞名的前缀（例如 `mymod_xxx`），否则会覆盖原版概念的名称与解释。
- 未在 readme 中说明：本类目**没有任何官方文档**；`texture` 是否支持其它根路径、`hidden` 的确切表现、以及除 `alias`/`texture`/`hidden` 外是否还有引擎认得的字段，均未文档化。
