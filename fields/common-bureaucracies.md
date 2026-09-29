# common/bureaucracies（官僚部门）

> **一句话**：官僚部门字段与三态修正：维护费滑条驱动的正向、负向与中性修正，三类价格，以及槽位、阶层好恶与根深蒂固机制。
> **什么时候看**：写官僚部门、核对价格引用与维护费缩放时看。
> **体量**：75 行 · 约 4 分钟通读

来源：`in_game\common\bureaucracies\readme.txt`（22 行）+ 3 个数据文件 **25 个官僚部门** 的实际用法

## 字段

```
<bureaucracy_id> = {
    potential = <trigger>       # 行动是否可能（root = country）
    allow = <trigger>           # 行动能否开始（root = country）
    years / months / weeks / days = <int>   # 完全生效所需时间；修正按完成比例缩放
    on_activate = <effect>      # 选择时触发（root = country）
    on_fully_activated = <effect>  # 100% 时触发（无延迟则立即）（root = country）
    on_deactivate = <effect>    # 移除时触发（root = country）
    neutral_modifier  = <scaled & triggered modifier>  # 施加于整个国家；root = country; scope:maintenance; scope:entrenchment
    positive_modifier = <scaled & triggered modifier>  # 同上
    negative_modifier = <scaled & triggered modifier>  # 同上
    implementation_price = <price>          # 脚本化标准价格，引用 \common\prices\（price:<price_id> 或脚本结果）
    implementation_price_modifier = <script value>  # 乘数；root = country
    removal_price = <price>
    removal_price_modifier = <script value>
    maintenance_price = <price>
    maintenance_price_modifier = <script value>
    on_maintenance_changed = <effect>       # 维护设置改变后次日调用；root = country; scope:old_maintenance; scope:new_maintenance
}
```

## 原版实测（25 个 / 3 文件）

| 文件 | 数量 | 面向 |
|---|---|---|
| `byz.txt` | 11 | 拜占庭专属 |
| `generic.txt` | 10 | 1600–1836 通用（注释自述「ADM 4 / DIP 3 / MIL 3」） |
| `china.txt` | 4 | 中华（科举等） |

出现率 100% 的字段：`potential` · `implementation_price` · `removal_price` · `maintenance_price` · `maintenance_price_modifier` · `positive_modifier` · `neutral_modifier` · `negative_modifier` · `estates_that_like` · `estates_that_dislike`；`allow` 24%、`on_deactivate` 8%、`on_maintenance_changed` 4%、`on_fully_activated` 4%。

### 三态修正是"维护费滑条"的两个方向

```
positive_modifier = { scale = { value = scope:maintenance }  tax_income_efficiency = 0.1 }        # 给钱多 → 拿好处多
negative_modifier = { scale = { value = 1  subtract = scope:maintenance }  estate_enrichment = 0.1 } # 省钱 → 吃坏处
neutral_modifier  = { government_size = 1 }                                                        # 与钱无关
```

`maintenance_price_modifier` 常按国家规模缩放（原版 `country_economical_base × 0.004`）——**国家越大，同一套官僚越贵**。

### 三个价格只在 `prices\05_byz.txt` 定义（⚠️ 通用内容藏在 byz 文件里）

```
implement_bureaucracy_price = { government_power = 20 }   # 实施
maintain_bureaucracy_price  = { gold = 5 }                # 每月维护
remove_bureaucracy_price    = { stability = 50 }          # 移除
```

这三个 id 被 `generic.txt` / `china.txt` / `byz.txt` 里**所有**官僚部门引用，但定义只此一处——覆盖或删掉该文件，全世界官僚部门的价格失效。

### 阶层好恶、槽位与根深蒂固

- `estates_that_like` / `estates_that_dislike`（原版如税务委员会：喜欢王室+市民、讨厌贵族），配常量 `ESTATE_SATISFACTION_BUREAUCRACY = 0.01`
- **槽位来自革新**：`advances\` 5 条带 `global_max_bureaucracy_slots`（宗教改革 +1、革命 +1、拜占庭 +2、中华 +1、特拉比松 +1 = 合计 +6）
- 触发器：`num_bureaucracies`、`num_open_bureaucracy_slots`、`max_bureaucracy_slots`、`allowed_bureaucracies`、`bureaucracy_maintenance`、`has_bureaucracy_of_type`、`bureaucracy_liked_by_estate(_type)`、`bureaucracy_disliked_by_estate(_type)` 等 **10 个**；效果 `add_bureaucracy` / `remove_bureaucracy` / `change_entrenchment`
- **根深蒂固 entrenchment**：`BUREAUCRACY_ENTRENCHMENT_YEARS_PER_PHASE = 100`、`..._QUOTE_PER_PHASE = 50`；行动 `increase_bureaucratic_entrenchment`（+`global_bureaucracy_entrenchment_speed_modifier = 0.1`）、议会行动 `challenge_bureaucratic_entrenchment`
- AI：`AI_PERFORMANCE_BUREAUCRACY_MONTHS_BETWEEN_UPDATES = 24`（24 个月才重估），授予/移除阈值均 5

## 审查要点

- 三个 modifier 都是 scaled & triggered modifier，且可用 `scope:maintenance` / `scope:entrenchment` 作用域。
- 价格字段引用 `common/prices` 中定义的价格（`price:<price_id>` 语法），价格 ID 须存在。
- 未在 readme 中说明：本地化键格式。
