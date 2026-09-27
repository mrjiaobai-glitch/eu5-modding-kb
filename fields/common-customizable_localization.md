# common/customizable_localization（动态文本）

来源：`in_game\common\customizable_localization\customizable_localization.info`（**604 B，权威**）+ 26 个数据文件（**3.8 MB**）实查

## 字段（info 全表）

```
<key> = {
    type = <scope 类型>              # 该动态文本在哪种作用域下求值
    text = {
        trigger = { <trigger> }      # 命中条件
        localization_key = <loc 键>   # 命中后用哪个文本
        fallback = yes               # 可选：没有命中时用这条
    }
    …                                # 可多个 text 块，按顺序取第一个命中的
    random_valid = yes               # 可选：命中多条时随机取，而不是取第一条
}
```

**继承写法**（info 原文）：

```
<key> = {
    parent = <另一 customizable loc 键>
    suffix = "_后缀"
    fallback = true                  # 父键+后缀不存在时继续回退
}
```

机制：先跑父键的逻辑，再把后缀拼到得到的 loc 键上。

## 调用方式（info 原文）

> When you want to use a custom loc in the localization then use: `[<scope>.Custom('<key>')]`

即：**在本地化文件里**写 `[Country.Custom('my_key')]` 这类表达式；也可被 `common\` 的 `custom_name` / `custom_description` 字段引用（见 `fields\common-international_organizations.md`、`common-situations.md`、`common-disasters.md`、`common-movements.md`）。

## 原版实测（26 文件 / 3.8 MB）

| 文件 | 体量 | 用途 |
|---|---|---|
| **`ru_EU5_custom_loc.txt`** | **3.33 MB** | 俄语专用动态文本（格变化/前缀）——单文件占了全类目 87% |
| `ru_EU5_custom_culture.txt` | 226 KB | 俄语文化形容词 |
| `ru_EU5_custom_suffix.txt` | 87 KB | 俄语后缀 |
| `country_ranks.txt` | 55 KB | 国家等级的称谓（配合 `country_ranks_spanish_article.txt` 38 KB 的西班牙语冠词处理） |
| `subunit_types.txt` | 33 KB | 兵种称谓 |
| `00_customizable_localization.txt` | 24.7 KB | 通用条目 |
| `01_customizable_event_loc.txt` | 4.2 KB | 事件文本专用 |

**关键事实**：3.8 MB 里 **3.6 MB 是俄语/西班牙语的语言学适配**（格、性、冠词、后缀）。这说明 customizable_localization 的主要用途是**解决"文本随语言变形"**，而不是普通的条件文本——普通条件文本用 `trigger` + 多 `text` 块就够。

## 审查要点

- `text` 块**按顺序取第一个命中**；想要随机要显式 `random_valid = yes`。
- **`fallback = yes` 是兜底**：没有它且没有命中，界面显示 raw key。
- `parent` / `suffix` 继承时，`fallback = true`（注意这里用的是 **`true`** 而不是 `yes`，原版 info 两种写法都出现过）决定"父键+后缀不存在时是否继续回退"。
- `type` 写错 → 该作用域下取不到值（**不报错**）。
- 使用处必须写成 `[<scope>.Custom('<key>')]`；写成 `$key$` 之类不会生效。
- 未在 readme 中说明：本类目**没有 readme**，只有 25 行 info；`type` 的合法取值清单见 info 之前的另一份文档（原版把它误放到了 `common\ai_diplochance\ai_diplochance.info`，内容即 scope 类型表：artifact / character / landed_title / province / activity / secret / scheme / combat / combat_side / title_and_vassal_change / faith / dynasty）。
