# 测试与排错（testing）

> **一句话**：用游戏自带测试系统、error.log 与 qa_debug 调试事件验证 mod 机制，含观察者模式跑局与常见错误的优先级分级。
> **什么时候看**：mod 写完要验证时按本文流程走一遍；出报错先看这里的常见刷屏与归属判断。
> **体量**：76 行 · 约 4 分钟通读

> **审查流程的配套**：逐文件审查用 `tools\review-checklist.md`（把全库「审查要点」按检查方式重排：脚本可判项 / 作用域对照表 / 语义陷阱 / **防误报白名单**）；本库引用过的数字若被怀疑，用 `tools\kb-self-audit.md` 的复跑方法回原版核对。

## 一、游戏自带测试系统（`in_game\common\tests\`）

官方格式（`readme.txt` 全文要点）：

```
my_test = {
    year = 1400            # 开始检查的年份
    success = { <triggers> }   # 满足则通过
    failure = { <triggers> }   # 满足则失败
    end_year = 1500        # 过此年失效
    fail_on_end_year = yes # 到期自动判失败（默认 no）
    success_effect = { <effects> }   # 通过时执行（只建议用 test_log）
    failure_effect = { <effects> }   # 失败时执行
}

# 可选 success_child（定义在测试级，success 之外）：
success_child = { desc = "MLO"  trigger = { c:MLO ?= { months_since_war > 12 } } }
# success 通过后按序评估各 success_child，第一个为真的追加日志：
# [TEST_NAME][...][RESULT][PASS][DATE][...][SUCCEEDED][<label>]
```

用途：验证 mod 机制在指定年份是否按预期发生（如"1430 年战争应已结束"）。配合观察者模式跑局看日志。

## 二、error.log 排错

- 位置：`Documents\Paradox Interactive\Europa Universalis V\logs\error.log`（游戏运行目录的 logs 下）。
- 每行报错含 **"Script location: <文件:行>"**——据此定位是哪个 mod 文件的问题。
- 常见刷屏：变量未初始化（"Failed to fetch variable ... due to not being set"）、change_variable 写 value=N（"Invalid operation type in execution"）、作用域错（"Wrong scope: xxx, expected yyy"、"Scoped object is not valid"）、$参数$ 未传（missing）。
- 判断归属：先看 "Script location:" 行指向 mod 文件还是原版（原版报错可能是 mod 引起的连锁）。

## 三、debug 事件验证（`in_game\events\debug\qa_debug.txt` 模板）

```txt
namespace = qa_debug
qa_debug.1 = {
    type = country_event
    title = qa_debug.1.title
    desc = qa_debug.1.desc
    outcome = neutral
    orphan = yes                    # 不报未引用错误
    trigger = { always = no }       # 手动触发
    immediate = {
        capital = { save_scope_as = start_location }
        random_location_in_the_world = { save_scope_as = end_location }
    }
    option = { name = WEATHER_RAIN
        start_weather_system = { width = 800 length = 100 strength = 1.0 speed = 30 type = front
            location = scope:start_location location = scope:end_location }
    }
}
```

mod 可以照此写自己的调试事件（`orphan = yes` + `is_human = yes` 或 always=no），手动触发验证硬编码效果/长链效果。原版 qa_debug 事件覆盖：天气（front/tornado/cyclone/sandstorm）、单位、宗教等，可先启用原版 qa_debug 观察。

## 四、观察者模式验证（AI 生态）

- 用 observer（控制台 `observe` 或启动参数）挂机 50–100 年：
  - 事件是否真的会被 AI 触发（尤其带 `is_human = yes` 门槛的——AI 永远不会触发）
  - 灾难是否重复触发（**can_start 的 NOT{xxx_resolved=yes} 标记绝不能在 on_end remove**——否则灾难无限循环，实测教训）
  - 数值量级（修正值、成本）是否平衡
- 存档分析法（eu5-mod-review 实测推荐）：读档后用游戏内信息核对变量/修正是否按预期积累。

## 五、常见错误优先级

1. 致命（游戏不加载/报错刷屏）：BOM 缺失、语言头错误、括号不配对、词条不存在
2. 警告（功能异常）：作用域错误、变量未初始化、$参数$ 未传
3. 建议（逻辑）：AI 永不触发的死事件、数值失衡、深层 every_* 性能
