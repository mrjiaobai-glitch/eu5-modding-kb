# common/scripted_modifiers（脚本化权重片段）

> **一句话**：脚本化权重片段：modifier、opinion_modifier、compare_modifier 三个子块与参数替换用法。
> **什么时候看**：要给权重写复用片段、或想确认它和国家修正无关时翻这篇。
> **体量**：67 行 · 约 4 分钟通读

来源：`in_game\common\scripted_modifiers\scripted_modifiers.info`（**1,230 B**）+ 1 个数据文件（1 KB）实查

## ⚠️ 先纠一个名

**它不是国家修正（country modifier）**，info 原话："They are used in **weights** and have **nothing to do with country modifiers**."——它是**用在权重里的脚本化片段**，写法与 scripted effects / triggers 同族（可带参数）。

## 字段

```
<名> = {
    modifier = {                    # 基础数值修正
        add = { value = 10  multiply = $SCALE$ }
        $CHARACTER_1$ = { has_trait = ambitious }     # 参数也能当作用域用
    }
    opinion_modifier = {            # 基于"某人对某人的观感"
        target = $CHARACTER_2$
        who = $CHARACTER_1$
        multiplier = { value = 0.25  multiply = $SCALE$ }
    }
    compare_modifier = {            # 基于某个数值的比较
        target = $CHARACTER_1$
        value = stress
        multiplier = $SCALE$
    }
}
```

## 调用方式（info 两个例子）

```
# ① 无参
random_list = { 1 = { example_modifier = yes } }

# ② 带参
random_list = {
    1 = {
        example_modifier = {
            CHARACTER_1 = root
            CHARACTER_2 = root
            SCALE = 0.1
        }
    }
}
```

**参数即 `$名$` 文本替换**（与 scripted effects 同规则）；参数可以传**作用域对象**（例子里 `CHARACTER_1 = root` 之后在块内以 `$CHARACTER_1$` 当作用域用）。

## 原版实测

目录只有 1 个数据文件（1 KB）——**原版几乎没用**，但 info 完整（含三子块与参数两种用法），属于"能力齐全、样例极少"的机制（与 `scripted_widgets`、`scripted_guis` 同类）。

info 末尾还给了一个真实感示例（`approval_modifier_grateful_family`）：在 `modifier` 里用 `trigger` 门控、用 `every_family` 累加 `$VALUE$`、并用 `desc = APPROVAL_MODIFIER_GRATEFUL_FAMILY` 挂本地化描述——**说明权重片段可以带可读的解释文本**。

## 审查要点

- **不要把国家修正写进来**——这里只影响权重。
- 三个子块可单用也可组合；`opinion_modifier` / `compare_modifier` 需要 `target`（`opinion_modifier` 还要 `who`）。
- 参数名（`$…$`）**必须由调用方全部提供**，漏传会刷 missing 报错（与 scripted effects 一致）。
- `desc = <loc 键>` 可让 AI 决策解释（如 AI 篇的 `ai_will_do` 里的 `desc`）在界面上可读。
- 未在 readme 中说明：本类目**没有 readme**，只有 info；原版只有 1 个数据文件，**没有可抄的复杂样例**。
