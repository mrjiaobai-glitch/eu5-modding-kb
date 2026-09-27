# common/scripted_triggers（脚本化触发器）

来源：`in_game\common\scripted_triggers\readme.txt`

## 定义与使用

```
my_simple_trigger = { <list of triggers> }
# 使用：my_simple_trigger = yes
```

## 自定义参数（$键$ 文本替换）

```
my_argument_trigger = { prestige = $value$ }
# 使用：my_argument_trigger = { value = 12 }

my_scoped_trigger = {
    $target$ = { prestige = $value$ }
    is_enemy_of = $target$
}
# 使用：my_scoped_trigger = { target = scope:my_scope value = 13 }
```

- 参数可拼接 trigger 名：`my_argumented_trigger = { my_argumented_trigger_$type$ = yes }`

## 重要坑（readme 原文强调）

- **custom_description 键不能与 trigger 键相同**——相同会导致 custom_description 无法拾取 subject/object/value（写法同 scripted_effects）。

## 审查要点

- 参数化 trigger 拼错参数引发大量 "missing trigger" 报错。
- 未在 readme 中说明：本地化键格式。
