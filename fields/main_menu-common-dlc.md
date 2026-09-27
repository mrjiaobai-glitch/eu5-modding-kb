# main_menu/common/dlc（DLC 定义）

来源：**本类目没有 readme、也没有 info**——`dlc\D000_shared\main_menu\common\dlc\` 里 3 个档全字段实查（`fate_of_the_phoenix.txt` 410 B / `ancient_monuments_pack.txt` 439 B / `sacred_sites_pack.txt` 376 B）。

> ⚠️ **基础包的 `main_menu\common\` 下根本没有 `dlc\` 这个类目**——它只存在于 DLC 目录里（`dlc\<包>\…`）。机制解析见 `vanilla\vanilla-dlc-and-assets.md` §一/§二。

## 字段（13 个，3 档合并去重）

```
fate_of_the_phoenix = {                    # 块名 = DLC 键（= 同目录 .dlc.json 的 name）
    id = "d008_fate_of_the_phoenix/d008_fate_of_the_phoenix.dlc.json"   # 相对 dlc\ 的描述符路径
    name = "fate_of_the_phoenix_key"       # loc 键：DLC 名
    desc = "fate_of_the_phoenix_dlc_desc"  # loc 键：长描述
    category = "dlc_immersion_pack"        # 字符串：dlc_immersion_pack / dlc_cosmetic_pack
    activate = "…_activate_desc"           # loc 键：激活说明（仅 D015 有）
    item = "monuments_signup_reward"       # 关联的订阅奖励条目（仅 D015 有）
    type = content                         # 裸枚举：content / cosmetic
    color = hsv360 { 287 62 55 }           # 或 rgb { 84 140 85 }——UI 配色
    desc_id = "d008_fate_of_the_phoenix"   # 拼 DLC 图标文件名用
    order = 3                              # UI 排序（1/2/3）
    steam_id = "3699010"                   # Steam 商店 ID（字符串）
    review_steam_id = "4097770"            # 评审/媒体版 ID（D008、D017 有）
    show_tags_in_ui = { "BYZ" }            # 在 UI 上显示的国家 tag（仅 D008 有）
}
```

## 原版实测（3 档逐字段对照）

| 字段 | fate_of_the_phoenix (D008) | ancient_monuments_pack (D015) | sacred_sites_pack (D017) |
|---|---|---|---|
| `category` | `dlc_immersion_pack` | `dlc_cosmetic_pack` | `dlc_cosmetic_pack` |
| `type` | `content` | `cosmetic` | `cosmetic` |
| `color` | `hsv360 { 287 62 55 }` | `rgb { 84 140 85 }` | `rgb { 199 161 53 }` |
| `order` | 3 | 1 | 2 |
| `steam_id` | `3699010` | 无 | `3865300` |
| `review_steam_id` | `4097770` | 无 | `4091680` |
| `activate` / `item` | 无 | ✔ / ✔ | 无 |
| `show_tags_in_ui` | `{ "BYZ" }` | 无 | 无 |

配套资产（同一 `D000_shared` 里）：

- 图标 `main_menu\gfx\interface\icons\dlcs\<desc_id>.dds` + `<desc_id>_background.dds`（另有 `_default.dds`）。
- 文案 `main_menu\localization\dlc\<lang>\dlc_localizations_l_<lang>.yml`（**11 语言全有**；键＝ `name` / `desc` / `activate` 三个字段的值）。
- 描述符 `dlc\<包>\<包>.dlc.json`（18 键，与 mod 的 `metadata.json` 同形：`name`/`path`/`checksum`/`dependencies`/`replace_path`/`steam_id`/`affects_save_compatibility`/`mp_synced`/`third_party_content`…）。

## 审查要点

- **mod 不能靠这个类目注册自己**：`id` 指向 `.dlc.json`、又有 `steam_id`/`review_steam_id` 校验，都是 Steam DLC 体系的东西。mod 注册走 `metadata.json`（见 `guides\mod-skeleton.md`）。
- **只有 `type = content` 的 DLC 带内容目录**（D008 有 `in_game`/`main_menu`/`loading_screen` 三分区）；`type = cosmetic` 的只影响资产/UI。
- **块名就是 `has_dlc` 的值**：脚本里写 `has_dlc = "d008_fate_of_the_phoenix"`（**带引号**；原版 **44 个基础包文件**在用）。改块名会让所有门控失效。
- **三个 loc 键缺一个就显示 raw key**（`name` / `desc` / `activate`），且要 11 语言齐全（原版做法）。
- `color` 两种写法都合法（`hsv360 {h s v}` / `rgb {r g b}`）；写错只影响 UI 配色，不报错。
- 未在 readme 中说明：本类目**没有任何官方文档**；字段语义（尤其 `item`、`show_tags_in_ui`、`desc_id` 与图标文件名的拼接关系）由数据反推，改动前先照抄原版同型 DLC。
