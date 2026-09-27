# common/scripted_effects（脚本化效果）

来源：`in_game\common\scripted_effects\readme.txt`

## 定义与使用

```
my_simple_effect = { <list of effects> }
# 使用：my_simple_effect = yes
```

## 自定义参数（$键$ 文本替换）

```
my_argument_effect = {
    add_prestige = $value$
}
# 使用：my_argument_effect = { value = 12 }

my_scoped_effect = {
    $target$ = { add_prestige = $value$ }
    take_over_all_wars = $target$
}
# 使用：my_scoped_effect = { target = scope:my_scope value = 13 }
```

- 参数是纯文本替换，可拼接出效果名：
```
my_argumented_effect_hello = { add_stability = 5 }
my_argumented_effect = { my_argumented_effect_$type$ = yes }
# 使用：my_argumented_effect = { type = hello }
```

## 重要坑（readme 原文强调）

- **custom_description 键不能与 effect 键相同**——相同会导致 custom_description 无法拾取 subject/object/value。
  - ❌ `my_effect = { custom_description = { text = my_effect object = <whatever> } }`
  - ✅ `my_effect = { custom_description = { text = my_effect_text object = <whatever> } }`

## 审查要点

- 参数化效果拼错参数（如 `$value$` 未传）会引发大量 "missing effect" 报错。
- 未在 readme 中说明：本地化键格式。
