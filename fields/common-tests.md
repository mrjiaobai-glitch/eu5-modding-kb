# common/tests（测试用例）

来源：`in_game\common\tests\readme.txt`

## 字段

```
<test id> = {
    year = <year>            # 开始检查的年份
    success = { <triggers> } # 通过条件
    failure = { <triggers> } # 失败条件
    end_year = <year>        # 超过该年份后测试停用
    fail_on_end_year = <yes/no>  # 默认 no；日期过后自动失败
    success_effect = { <effects> }  # 通过时；只建议用 test_log
    failure_effect = { <effects> }  # 失败时；只建议用 test_log
    success_child = { desc = "<label>" trigger = { <triggers> } }  # 可选可重复；测试通过后按序评估，首个为真的在日志行追加 [SUCCEEDED][<label>]
}
```

## 审查要点

- `success_child` 定义在 test 级别（不在 success = {} 内），按序评估。
- 日志行格式：`[TEST_NAME][...][RESULT][PASS][DATE][...][SUCCEEDED][<label>]`。
- 未在 readme 中说明：本地化键格式。
