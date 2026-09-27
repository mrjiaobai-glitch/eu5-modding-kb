# common/ai_scripted_expansion_score 与 ai_scripted_expansion_target

来源：`in_game\common\ai_scripted_expansion_score\readme.txt`、`in_game\common\ai_scripted_expansion_target\readme.txt`

## ai_scripted_expansion_score（AI 评估未来战争时为扩张目标加减分）

```
<name> = {
    attacker_potential = <trigger>      # root = attacker；为 false 则分数不生效
    target_trigger = <trigger>          # root = target, scope:attacker = attacker；为 false 则分数不生效
    score = <script value>              # scope:attacker + scope:target；在乘数应用前加到最终分（与 EXPANSION_TARGET_SCORE_NEEDED_TO_PICK define 相关）
    multiplier = <script value>         # scope:attacker + scope:target；乘在整个最终分上
    never_attack = <trigger>            # scope:attacker + scope:target；为 true 则最终分恒为 0
}
```

## ai_scripted_expansion_target（让 AI 评估本会忽略的国家）

```
<name> = {
    attacker_potential = <trigger>      # root = attacker；为 false 则分数不生效
    candidate_list = <effect>           # root = attacker；用 add_to_list = source 把候选对象填进列表
    casus_belli = <casus_belli>         # root = attacker, scope:target = target；可留空让 AI 自选
    ignore_antagonism = <yes/no>        # 是否忽略已有的高敌意国家组；只在宣战时检查，不在割地时检查
    score = <script value>              # scope:attacker + scope:target；设定扩张目标分
    sort_value = <script value>         # scope:attacker + scope:target；排序用；留空则同 score
}
```

## 审查要点

- 作用域约定固定：root = attacker（score/target 文件中）、`scope:attacker`/`scope:target` 为脚本值可用作用域——写错作用域（如用 `root.xxx` 取 target 数据）是常见错误。
- 目标类 `casus_belli` 引用须存在。
- 未在 readme 中说明：本地化、默认值。
