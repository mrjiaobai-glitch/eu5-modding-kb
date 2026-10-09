# main_menu/gui/messagetypes.txt（消息与通知类型）

> **一句话**：消息类型的七个必填开关与分类字段，决定通知是进日志、地图提示还是暂停弹窗，另附可选提示音。
> **什么时候看**：加消息类型、要调某个通知打扰玩家的方式与所属分类时翻这篇。
> **体量**：48 行 · 约 3 分钟通读

来源：**无 readme**——`main_menu\gui\messagetypes.txt`（**177 KB / 1,348 条消息类型**）实查。

## 字段（7 个必填 + 1 可选，实测出现次数）

```
<MESSAGE_TYPE> = {
    log = yes/no               # 1348/1348 —— 是否进消息日志
    onmap = yes/no             # 1348 —— 是否在地图上提示
    popup = yes/no             # 1348 —— 是否弹窗
    idle = yes/no              # 1348 —— 空闲/汇总提示
    option = yes/no            # 1348 —— 是否带选项
    pausepopup = yes/no        # 1348 —— 是否暂停游戏并弹窗
    message_category = <分类>   # 1348 —— 归入哪个消息分类（设置面板的过滤项）
    sound = <音效>              # 53（可选）—— 提示音
}
```

原版实例：

```
MAJOR_EVENT_INFO = { log=no  onmap=no  popup=yes  idle=no  option=no  pausepopup=yes  message_category = geopolitics }
CHAR_INTERACTION = { log=yes onmap=no  popup=yes  idle=no  option=no  pausepopup=no   message_category = society }
```

## 原版实测（1,348 条）

**消息分类分布**：`diplomacy` **599** · `society` **304** · `government` **188** · `geopolitics` **98** · `wars` **72** · `military` **50** · `economy` **37**。

**7 个字段全部必填**（1,348 条无一遗漏）——这点与其它类目不同：**没有"只写一半"的原版样例**。

## 用法

- 消息类型名是**引擎/脚本侧引用的键**（事件、交互、局势等在触发通知时给出类型名）。
- **玩家可见行为完全由这 7 个开关组合决定**：同一条事件可以是"只进日志"、"地图提示"、"暂停弹窗"，改这里就等于改"游戏怎么打扰玩家"。
- `message_category` 决定它出现在设置面板的哪个过滤组下（玩家可逐类关闭）。

## 审查要点

- **7 个字段一个都不能省**（原版 100% 全写）；漏写的行为未定义，别照抄"部分字段"的想象写法。
- 新增消息类型后，**分类名**要与设置面板已有的分类一致，否则玩家找不到开关。
- `pausepopup = yes` 会**中断游戏**——原版只给 `MAJOR_EVENT_INFO` 这类重大事件用，mod 里滥用会严重影响体验。
- 未在 readme 中说明：本类目**没有 readme**；`sound` 的取值清单以文件内出现值为准（原版 53 条）。
