# in_game/common/persistent_dna（固定角色外貌 DNA）

> **一句话**：给指定角色固定外貌的 DNA 档：priority、tags、genes 三字段与禁止用 DNA 强制穿戴附件的警告。
> **什么时候看**：改历史人物长相、要核对 tags 与 genes 是否生效，或想知道穿戴该走哪时翻这篇。
> **体量**：52 行 · 约 3 分钟通读

来源：**无 readme**——`custom_characters.txt`（**676,826 B / 12,344 行 / 105 个条目**）实查；**文件头注释即官方警告**。

## 它是什么

给**指定角色**固定一套外貌（"这个历史人物就该长这样"）——key 是任意标识，靠 `tags` 挂到角色上。

```txt
## Do not force accessory_genes (hair,beard,clothing) via persistent DNA,
## use portrait_modifiers instead (see: \gfx\portraits\portrait_modifiers\01_historical_chr.txt)

magnus_eriksson = {
    priority = 1
    tags = { swe_magnus_eriksson }        # ★ 挂接用的角色标签
    portrait_info = {
        genes = {
            hair_color = { 62 59 203 6 }  # 基因名 = { 4 个数值 }
            skin_color = { 56 30 56 30 }
            …
        }
    }
}
```

## 字段

| 字段 | 说明 |
|---|---|
| `priority` | 多个条目命中同一角色时的优先级 |
| `tags` | **角色标签**（挂接键）——角色侧对应 `main_menu\setup\start\05_characters.txt` 里的角色定义 |
| `portrait_info.genes` | **基因块**：`<基因名> = { 4 个数 }`（`skin_color` / `hair_color` 等；基因名须在 `common\genes\` 定义，见 `fields\common-genes-ethnicities.md`） |

## ⚠️ 文件头警告（原文要点，务必遵守）

> **不要用 persistent DNA 强制 `accessory_genes`（头发、胡须、衣物）**——那类特征要走 **`portrait_modifiers`**：`gfx\portraits\portrait_modifiers\01_historical_chr.txt`。

即：**DNA 管"长相（五官/肤色/发色）"、portrait_modifiers 管"穿戴附件"**，两者分工不能混。

## 审查要点

- **`tags` 必须与角色侧对得上**（`05_characters.txt` 的角色标签），否则该 DNA 永远不生效——**且不报错**。
- `genes` 里的基因名必须在 `common\genes\` 有定义（原版 89 个定义 ↔ 88 个引用；见基因/族群档）。
- **每个基因值是 4 个数**（不是 2 个/3 个）——数量不对是静默失败。
- **不要照抄"用 DNA 定穿戴"的写法**：那是文件头明确禁止的；照抄会做出"随机换装"或异常外观。
- 12,344 行 / 105 条 → 平均百余行一条，说明原版条目里 `genes` 块很长；改单条人物只动对应 key。
- 未在 readme 中说明：本类目**没有 readme**；`priority` 冲突时的取舍、`portrait_modifiers` 的完整字段（在 `gfx\` 侧）均未文档化。
