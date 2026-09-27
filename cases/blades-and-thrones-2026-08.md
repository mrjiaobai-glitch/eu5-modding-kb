# 《刀锋与王座》2026-08 实测重大坑（EU5 1.3.x）

来源：察合台汗国"暴毙"排查（存档对照 + 引擎机制挖掘），全部经存档实证。
通用性强，审查任何 mod 都可能踩到。字段权威仍以 readme 提炼为准，本文件是实测补充。

## 1. country_has_estate 对所有国家恒为真
EU5 为每个国家创建所有阶层的对象（存档实证：2376 个国家都有士兵阶层对象），
country_has_estate 只检查"对象是否存在"= 永远 yes。判断"阶层实质存在"
（有 POP 支持）用 `"estate_power(estate_type:xxx)" > 0`（触发器，country 作用域，
vanilla 先例 prefection.txt 的 crown_estate）。用 country_has_estate 做门槛的
机制会对全世界无差别运行（教训：军饷系统曾对无士兵 POP 的国家生效）。

## 2. 军队国（country_type = army）军队全灭 → 碎国（引擎硬编码）
on_shatter_country（on_action/_hardcoded.txt:5415）：change_country_type =
location + change_government_type = tribe + 摧毁 kurultai；社区证实触发条件
= 军队国的全部军队被摧毁（"destroy all their armies → the tag breaks apart"）。
后果链：type=army 国家军队清零 → 碎裂 → type 变 location → 依赖
`country_type = army` 的政府改革（如 legacy_of_genghis）potential 失效被移除
→ steppe_horde 降级 tribe → 军队/继承人/核心结构被清。
教训：①"军队国无军 = 亡国"——任何能让 AI 军队国军队清零的机制（遣散/转移/
出租）都是自杀按钮，AI 侧必须排除；②开局军队会自动生成 soldiers POP
（军队即士兵），estate_power > 0 挡不住军事国家。

## 3. num_forts 是"有防御的城市数"
EU5 城市自动带防御，大国有 30-50 座（察合台 270 地点实测 30 城）。按"个位数
堡垒"假设设计的金额权重（如每堡垒每月 1 金）会爆炸（军饷 30 金/月 vs 收入
6.5 → 持续欠饷 → 满意度崩）。设计金额权重先查 EU5 实际数值量级。

## 4. EU4 语法混入（EU5 会报错跳过）
- 触发器里 `random = 0.15` 是 EU4 语法：报 "Unknown trigger type: random"
  且该行被跳过（概率门槛变 100%）。EU5 概率用效果块 `random = { chance = 15 ... }`
- `trigger_if` 是 EU4 语法：报 "Unknown effect trigger_if"，整个效果失效。
  EU5 用 `if = { limit = {...} }`

## 5. 效果参数与作用域坑
- estate_add_gold 不支持 multiply（add_gold 支持）——倍数写进 value 脚本值块
- random_events 是 on_action 块顶层键（与 trigger/effect 平级），写 effect 内报错且永不触发
- `?=` 可选作用域块内是 effect 上下文，不能写裸 trigger 语法
  （this != root / NOT = {} 报 "Unknown effect this/NOT"）——包 `if = { limit = {...} }`
- estate scope 不能在 location 作用域内访问（报 "Invalid scope types for event
  target link, link: estate"）——经 `owner = { ... }` 再访问

## 6. 新增社会价值轴必须补 progress_cabinet_efficiency
内阁行动"推动价值观"（Change Societal Values）按 `<价值观端名>_progress_
cabinet_efficiency` 查找 static modifier——缺失报 "Cabinet 'Change Societal
Values' will not push this side" 且该端无法推动。修复：static_modifiers 加
两端 modifier（monthly_towards_<端> = 1，格式对齐原版 societal_values.txt）+
loc 键 `STATIC_MODIFIER_NAME_<键>: "$CABINET_INFLUENCING$"`（原版同款变量引用）。

## 7. subject type 的 use_overlord_map_color = no 覆盖玩家设置
写 use_overlord_map_color = no 会硬编码"不同色"，无视玩家全局设置
（"附庸国使用宗主国颜色"）。原版 vassal 不写该字段 = 跟随设置，删除即可。

## 8. 存档分析法（诊断 AI 世界异常）
- EU5 存档是明文文本（SAV0200 + metadata + 数据块），数百 MB 正常
- 国家块 `<id>={ country_name="CHG" ...`；tag→ID 映射表在文件中部（`897=CHG`）
- 变量 identity 是 FixedPoint×10000（identity=300800 = 30.08）
- 诊断国家崩溃看：provinces={ }（省数）、historical_population（开局/当前对比）、
  last_months_subject_tax（附庸税归零=附庸刚独立）、recent_balance（逐月财政）
- 批量分析用 ReadAllText + IndexOf（数百 MB 约 1 秒），勿逐行 ReadLine（2700 万行 10 分钟）
- "是不是 mod 的锅"最有力证据：同 playthrough 的无 MOD 对照档
- 判断原版行为 vs mod 行为：与同类型原版国家对照（如无地军队国保持 army）

## 9. on_annex / on_annexed 语义
on_annex = 吞并前触发，root = 吞并国，scope:target = 被吞并国（_hardcoded.txt
注释明确）。被吞并国作用域用 scope:target（非 root）。
