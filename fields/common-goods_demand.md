# in_game/common/goods_demand（商品需求分组）

> **一句话**：商品需求的登记表：需求键按商品与数量登记、被建筑单位与事件引用，分组字段决定这些需求在经济界面如何归类。
> **什么时候看**：加建筑建造消耗或单位维护需求，核对商品与分组名时看。
> **体量**：57 行 · 约 3 分钟通读

来源：**无 readme**——`goods_demand\`（7 档 / 41 KB / **355 个条目**）与 `goods_demand_category\00_default.txt`（366 B / 13 行）实查。

## 它是什么

**"谁在消耗哪些商品、消耗多少"的登记表**——建筑维护、单位建造、事件等场景的商品需求都从这里取。

```txt
# goods_demand\hardcoded.txt
#Land Units
#Recruitment:
infantry_construction = {              # ← 需求键（被建筑/单位/事件按名引用）
    firearms = 0.2                     # 商品 = 数量
    weaponry = 0.2
    leather  = 0.1
    category = regiment_construction    # ★ 需求分组（见下）
}
```

## 字段

| 字段 | 说明 |
|---|---|
| `<goods_id> = <数值>` | 需要该商品的数量（可多条；`goods_id` 须在 `common\goods\` 存在） |
| `category` | **需求分组**——取值定义在 `goods_demand_category\00_default.txt` |

**13 个需求分组**（`goods_demand_category`，13 行、多为空块）：`ship_maintenance` / `ship_construction` / `regiment_maintenance` / `regiment_construction` / `building_maintenance`（**唯一带字段的**：`display = integer`）/ `guild_input` / `workshop_input` / `manufactory_input` / `mills_input` / …——分组决定这些需求在**经济界面里怎么归类显示**。

## 七个档的分工

| 档 | 体量 | 装什么 |
|---|---|---|
| `hardcoded.txt` | 417 B | **引擎硬编码侧**（陆军/海军建造与维护） |
| `army_demands.txt` | 5.5 KB | 陆军相关 |
| `building_construction_costs.txt` | 10.8 KB / 777 行 | **建筑建造需求**（与 `building_types` 的建造消耗呼应） |
| `from_events.txt` | 12.2 KB / 994 行 | 事件产生的需求 |
| 其余 3 档 | — | 按主题细分（详见目录） |

## 关联

- 商品本体与需求基数：`common\goods\`（74 种，readme 权威）→ `fields\common-goods.md`
- 建筑建造消耗与投产度：`vanilla\vanilla-production-and-buildings.md`（`construction_demand` 与 13 需求分组）
- 价格与市场：`vanilla\vanilla-trade-and-market.md`

## 审查要点

- **商品 id 拼错不报错**——只是那一条需求被忽略；新增需求键时逐条核对 `goods\` 里的 id。
- **`category` 必须是 13 个分组之一**（写错 → 界面归类异常/不显示）。
- **`goods_demand_category` 里只有 `building_maintenance` 带 `display = integer`**——别给所有分组补 `display`（原版其余是空块）。
- 需求值是**每单位/每次**的量纲，不是总量；改之前先看 `building_types` 里怎么引用它。
- 未在 readme 中说明：本类目**没有 readme**；需求如何与价格/市场供需合成、13 个分组的完整语义均未文档化。
