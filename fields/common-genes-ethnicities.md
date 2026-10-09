# common/genes（外貌基因）+ common/ethnicities（族群）

> **一句话**：外貌基因与族群两档：基因的六种顶层容器与继承、年龄曲线写法，族群按权重给基因分配取值区间并继承基模板。
> **什么时候看**：做外貌与族群内容，或核对模板引号与权重为零的语义时看。
> **体量**：167 行 · 约 8 分钟通读

来源：`in_game\common\genes\_genes.info`（1 050 B，**自称 "very incomplete"**）+ 10 个 genes 数据文件（418 KB）+ 54 个 ethnicities 文件（629 KB，**无 readme**）

> 用途分工：**genes = 外貌特征的定义与继承规则**；**ethnicities = 每个族群给这些基因分配的权重表**。`_genes.info` 开头一句就是 "Genes connect to the /ethnicities"。

## genes：6 个顶层容器

```
color_genes = {                    # 颜色基因（00_genes_color.txt：hair_color / skin_color / eye_color）
    <gene> = {
        color = hair/eye/skin           # 取值域（readme 声明）
        sync_inheritance_with = hair_color   # 继承时与该基因同步（原版 skin_color / eye_color 同步到 hair_color）
        blend_range = { 0.55 0.65 }     # 继承时在"显性/隐性"父母之间的混合比例（readme 声明）
        # index = 0                     # DNA 内序号（readme 声明；原版这些行是注释掉的）
        # max_blend = 0.2               # 注释形式的设计值
        # max_recessive_drift = 0.25
    }
}

morph_genes = {                    # 形态基因（01_genes_morph.txt 最大，09 文件里还有 expressions_*）
    gene_head_width = {
        template_1 = {                 # 模板槽：同一基因可有 template_1..4
            index = 0
            male = {
                setting = {
                    attribute = "head_width"        # 作用到骨骼/网格的哪个属性
                    value = { min = -0.25 max = 0.15 }   # 或 { min = @maleAnimMin max = @maleAnimMax } 引用文件内脚本常量
                    age = age_preset_child_features     # 随年龄曲线
                }
            }
            female = { … }             # 原版常用 female = male 直接复用
        }
    }
}

accessory_genes = {                # 配件基因（头发/胡须/头饰/颈饰/衣物/眼睛/睫毛/口腔）
    hair_styles = {
        inheritable = no               # 是否可遗传（配件默认不可遗传）
        no_hair = {
            index = 0
            male = { 1 = empty }       # 权重 = 网格名；empty 表示无
            female = male              # 复用：female/boy/girl/adolescent_boy/adolescent_girl/infant = male
        }
        all_hair = { index = 1  male = { 1 = male_german_hair_short_curly  … } }
    }
}

special_genes = { accessory_genes = { … } }   # 03/05/06 文件：把 accessory_genes 或 morph_genes 再包一层
special_genes = { morph_genes = { … } }       # 08 文件

decal_atlases = {                  # 贴花图集（01_genes_morph.txt）
    male_eyebrows_atlas_inner_01 = {
        size = 3
        textures = { diffuse = "gfx/…_diffuse.dds"  normal = "…_normal.dds"  properties = "…_properties.dds" }
    }
}

age_presets = {                    # 年龄曲线（01_genes_morph.txt）
    age_preset_1 = {
        mode = add
        curve = { { 0.0 0.0 } { 0.32 0.0 } { 0.7 1.0 } }   # { 年龄 0..1, 强度 }
    }
}

# readme 另声明（原版未使用，见下）：
decal = { type = skin/paint  atlas_pos = { 0 0 }  alpha_curve = { { 0.0 0.6 } { 1.0 0.6 } } }
ugliness_feature_categories = { chin mouth }
```

原版 12 个顶层块、6 种容器：`color_genes`(1) / `morph_genes`(3) / `accessory_genes`(5) / `special_genes`(4+1) / `decal_atlases`(1) / `age_presets`(1)。`morph_genes` 内定义 **`gene_*` 89 个**，条目级字段出现频次：`age` 456 / `setting` 311 / `attribute` 310 / `curve` 253 / `textures` 162 / `index` 151 / `male`·`female` 各 150 / `template_1` 79（`template_2` 11、`template_3` 5、`template_4` 3）。

## ethnicities：字段

```
<ethnicity id> = {
    template = "ethnicity_template"    # 基模板（带引号！指向同目录其它族群，如 "european_ethnicity"）
    skin_color = { <权重> = { x1 y1  x2 y2 } }        # 颜色 = UV 矩形（左上 / 右下）
    hair_color = { <权重> = { … } }                     # 权重值即 0..100 的相对权重
    eye_color  = { <权重> = { … } }
    gene_stubble = {                                    # 形态基因 = 权重表，条目给模板与取值区间
        10 = { name = template_1  range = { 0.2 0.4 } }
        0  = { name = template_1  range = { 0.6 0.7 } }  # 权重 0 = 保留但从不选中
    }
}
```

- 原版 **60 个族群 / 54 文件**；`skin_color` 60/60、`hair_color` 58/60、`template` 58/60、`eye_color` 57/60。
- 族群引用的 `gene_*` 去重 **88 个**，与 genes 侧定义的 89 个**完全对得上**：0 个未定义引用，1 个定义了没被任何族群引用（`gene_corset`）。
- `template` 分布：`ethnicity_template` 29 / `asian_ethnicity` 9 / `european_ethnicity` 4 / `african_ethnicity` 3 / 其余 10 个模板各 1–2（`european_nordic_ethnicity`、`european_west_ethnicity`、`middle_eastern_ethnicity` …）。
- 基模板文件 `00_ethnicities.txt` 顶部用 `#@neg1_min = 0.4` 形式定义**脚本常量**（供本文件内的 `range` 引用）。

## 原版未使用（readme 声明了，但全库搜不到）

| 声明处 | 事实 |
|---|---|
| `decal = { type/atlas_pos/alpha_curve }` | genes 数据里没有顶层 `decal` 块（只有 `decal_atlases`） |
| `ugliness_feature_categories` | 全库（含 `.txt/.info/.md/.yml`）**仅 `_genes.info` 自己出现 1 次**；它注释里提到的 `ugliness_portrait_extremity_shift`（一个 trait 修正）在原版 traits 里**0 次使用** |

## 审查要点

- `ethnicities` 里的 `gene_*` 键**必须先在 `genes\` 定义**（原版 88 个引用全部有定义）；写错名字不会报错，只会让该特征在 UI 上"永远随机"。
- **`template` 的值要加引号**（`template = "european_ethnicity"`），且目标是**同目录内另一个族群 id**——原版 60 个族群有 29 个直接继承 `ethnicity_template`。
- 颜色块的值是 **UV 矩形**（4 个数），基因块的值是 **`{ name = template_N range = { min max } }`**，两种写法别混。
- 权重 `0` 是合法值（原版大量存在），表示"保留该档但永不选中"；删掉它和设 0 语义不同。
- genes 的形态条目要给出 `attribute` 与 `value = { min max }`；`attribute` 写错等于该基因不作用于任何骨骼。
- 未在 readme 中说明：`ethnicities` **完全没有 readme**（上表由数据反推）、`accessory_genes` 的 `inheritable`、`age_presets` 的 `mode = add`、以及 `#@常量` 写法。

## 本体实测补缺（2026-09 普查）

> **数据源**：`in_game\common\genes-ethnicities\` 全量 **64 个 .txt** 实查（EU5 1.3.x）；本机脚本 `kb\scripts\kb-field-census.ps1` / `kb-merge-census.ps1` 生成，可复跑。
> **注意**：本类目**本体没有 readme.txt**——下面全部是实测结果，不存在"漏写"一说。

### 一、本体实际在用的字段（无 readme，纯实测）

| 字段 | 次数 | 文件数 | 常见取值（前 5） |
| --- | --- | --- | --- |
| `template` | 58 | 52 | "ethnicity_template"（29）、"asian_ethnicity"（9）、"european_ethnicity"（4）、"african_ethnicity"（3）、"european_nordic_ethnicity"（2） |

### 二、取值白名单（本体出现过的值 + 次数）

- **`template`**（14 种）："ethnicity_template"（29）、"asian_ethnicity"（9）、"european_ethnicity"（4）、"african_ethnicity"（3）、"european_nordic_ethnicity"（2）、"european_west_ethnicity"（2）、"middle_eastern_ethnicity"（2）、"indian_ethnicity"（1）、"asian_austronesian_ethnicity"（1）、"european_slavic_ethnicity"（1）、"european_nordic_blonde_ethnicity"（1）、"american_andean_ethnicity"（1）、"oceanian_papuan_ethnicity"（1）、"american_ethnicity"（1）

### 三、深度 1 的块（子条目：政策／变体／子类型等）

| 块名 | 次数 | 文件数 |
| --- | --- | --- |
| `skin_color` | 61 | 55 |
| `hair_color` | 59 | 54 |
| `eye_color` | 58 | 53 |
| `gene_stubble` | 47 | 45 |
| `gene_eyebrows_outer` | 40 | 38 |
| `gene_head_face_forward` | 40 | 39 |
| `gene_eyebrows_inner` | 40 | 38 |
| `hair_styles` | 40 | 38 |
| `gene_nose_ridge_shape` | 40 | 40 |
| `gene_lip_color` | 40 | 40 |
| `gene_nose_height` | 38 | 38 |
| `gene_nose_length` | 38 | 38 |
| `gene_head_width` | 38 | 38 |
| `gene_skin_detail` | 37 | 35 |
| `gene_mouth_lower_lip_size` | 37 | 37 |
| … | 另有 25 种 | |

### 四、块内键最常见的前 15（modifier / trigger / effect 里实际写的）

| 块内键 | 次数 | 出现于哪些父块 |
| --- | --- | --- |
| `age` | 528 | eye_hsv_shift_curve、hair_hsv_shift_curve、setting、skin_hsv_shift_curve |
| `diffuse` | 452 | blend_modes、texture_override、textures |
| `index` | 418 | aztec_royalty、disfigurement_01、chinese_01、european_beards |
| `female` | 397 | aztec_royalty、disfigurement_01、chinese_01、european_beards |
| `male` | 396 | aztec_royalty、disfigurement_01、european_beards、scars_01 |
| `normal` | 394 | blend_modes、texture_override、textures |
| `setting` | 393 | male、boy、girl、female |
| `attribute` | 388 | setting |
| `girl` | 347 | aztec_royalty、disfigurement_01、european_beards、scars_01 |
| `boy` | 345 | aztec_royalty、disfigurement_01、european_beards、scars_01 |
| `textures` | 320 | male_eyebrows_atlas_inner_01、decal、male_eyebrows_atlas_inner_02、male_eyebrows_atlas_outer_01 |
| `adolescent_boy` | 309 | aztec_royalty、disfigurement_01、scars_01、smallpox_02 |
| `adolescent_girl` | 308 | aztec_royalty、disfigurement_01、scars_01、smallpox_02 |
| `curve` | 295 | age_preset_early_aging_hsv_curve、age_preset_regular_multiply、age_preset_late_aging_hsv_curve、age_preset_full_aging |
| `body_part` | 259 | decal |
