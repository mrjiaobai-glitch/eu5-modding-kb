# common/peace_treaties（和平条约）

来源：`in_game\common\peace_treaties\readme.txt`

## 字段

```
<peace_treaty_id> = {
    potential = <trigger>   # scope:winner = taker, scope:loser = giver, scope:war, scope:target = location/country/province
    allow = <trigger>       # 同上
    effect = <effect>       # 执行条约时；同上
    blocks_full_annexation = yes     # 使用该条约时目标不能被完全吞并
    collate_targets = <yes/no>       # 目标能否从所有给方合并（即条约是否与给方无关）
    are_targets_exclusive = yes      # location/province 目标排他，不能与割地条约组合
    category = <country/location/province/area>  # 把脚本化条约放进其他类别
    custom_tags = { <strings> }
    show_tags_in_ui = <yes/no>
    select_trigger = { ... }         # 只能加一个；格式同 character_interactions 的 select_trigger；存入 scope:target
    cost = <script value>            # 战争分数成本；scope:winner/loser/war/target
    base_antagonism = <script value> # 最大敌意获取量，按国调整
    antagonism_type = <bias type key>
    ai_desire = <script value>       # 胜利方想要该条约的程度
    ai_force_add = <yes/no>          # AI 是否尽可能把该条约加进提案
}
```

## 审查要点

- potential/allow/effect/cost/ai_desire 的固定作用域：winner/loser/war/target。
- `select_trigger` 只能一个。
- `antagonism_type` 引用须存在。
- 未在 readme 中说明：本地化键格式。

## 本地化键格式（2026-09 实测；readme 无 `#Localization:` 段，此处补齐）

**键跟随条约的 ID，不跟随文件名**（`humiliate.txt:1` 定义 `peace_humiliate` → 键是 `peace_humiliate*`；`punish_chinese_enemies.txt:1` 定义 `punish_chinese_expedition_foes` → 键跟随后者）。裸 `<treaty_id>` 确实被消费（引擎 loc 函数 `ShowPeaceTreatyTypeName('force_tributary')`，`scripted_triggers_l_english.yml:7`；`$NAME$` 见 `PEACE_OFFER_REQUIRES_PEACE_TREATY`，`offer_peace_l_english.yml:298`）。

| # | 键 | 位置 | 必要性 |
|---|---|---|---|
| 1 | `<id>` | `offer_peace_l_english.yml` | **必填** |
| 2 | `<id>_entry` | 同上 | **必填**（64/64 定义都有；`_entry`/`_entry_short` 各 65 个，多的那对 `stop_pirating_me` 无定义） |
| 3 | `<id>_entry_short` | 同上 | **必填** |
| 4 | `<id>_desc` | 同上（该块集中在文件底部 `:475-507`） | **必填**（唯一反例 `stop_pirating_me` 无 `_desc`） |
| 5 | `antagonism_<id>` | `opinions_l_english.yml` | **设了 `antagonism_type` 才要** |
| 6-8 | `cb_<id>` / `cb_<id>_desc` / `cb_<id>_PROV` | `casus_belli_l_english.yml` | **只有你另加配套 CB 才要** |

**⇒ 最小 4 键；带敌意 5 键；带 CB 8 键。消息键 = 0**——所有 enact/accept/reject 消息都是通用的，用 `$TERMS$` 注入（`PEACEACCEPT_*` / `PEACEREJECT_*` / `PEACEWEACCEPT_*` / … `messages_l_english.yml:871-918, 2680-2719`）。**和平条约没有 `war_name` 类比**（那是 wargoal 的，`wargoals\readme.txt:14`）。**没有 `<id>_tooltip` 约定**（唯一的 `claim_timurid_empire_tooltip` 是靠显式 `custom_tooltip` 拉进来的）。

**数据侧最小 = 1 个新 `peace_treaties\<新文件>.txt` + 1 个 bias 块**。CB 与 wargoal 条目**可选**。

**免费继承的通用键族**（不必逐条约写）：`OFFER_PEACE_TREATY_TOOLTIP`（`$NAME$`/`$COST$`/`$ANTAGONISM$`，`:9`）、`TOTAL_ANTAGONISM_HEADER/_TOOLTIP`（`:10-11`）、六个 `*_CATEGORY`（`:49-60`）、`PEACE_OFFER_WILL_ACCEPT/_WILL_NOT_ACCEPT/_AI_DESIRE/_TT_TITLE_TEXT/_REQUIRED/_ACCEPTABLE/_INDIFFERENT/_DONT_WANT`（`:308-316`）、约 27 个 `AI_DESIRE_*` chip（`:444-470`，含 `AI_DESIRE_SUBJECT_TYPE:459`）。

## ⚠️ 不需要配套 CB（有决定性反例）

readme **没有** CB 字段；只有 CB→条约 的单向可选链接（写在 CB 侧：`required_peace_treaties` / `_attacker_` / `_defender_`，`casus_belli\readme.txt:31-33`，全本体只有 2 个 CB 用）。

**决定性反例**：`subjugate_neighbor_native.txt` 造出 `subject_type:vassal`（effect → `make_subject_of`，`:42-45`）而**没有任何专属 CB**——它的 `potential` 只有通用条件（`:12-28`）。**64 个本体条约里 16 个完全没有 CB 耦合**（`humiliate` 连 `potential` 都没有）。

`force_tributary` 之所以绑 CB，是因为**它的 CB 反过来禁掉了常规臣服路径**：CB 设 `war_goal_type = take_capital_tributary`（`make_tributary_cb.txt:70`）→ 该 wargoal 设 `allowed_subjugation = { casus_belli_forbids_normal_subjugation_tributary = yes }`（`wargoals\00_default.txt:149-151`，一个**故意恒假**的触发器，`war_triggers.txt:19-24`）→ 所以条约里要写 `ignore_war_limitation = yes`（`force_tributary.txt:34`，注释原文 "cb_make_tributary disables all forms of subjugation"）。**这是刻意的循环耦合**，不是必需品。

**⇒ 只有当你要 (i) 限制该选项出现的时机、(ii) 压制常规臣服、(iii) 强制以该条约结束战争 时，才需要写 CB。**

## 字段补遗与坑

- **`ai_desire` 实际必填**：64 个条约里 63 个有它（KB `vanilla\vanilla-ai.md:123`）。
- `base_antagonism`（readme:54）与 `antagonism_type`（readme:55）**两条都有**——**敌意有两个挂点**（条约级 `base_antagonism` 与 `antagonism_type`→`antagonism_<id>` 键）。
- `allow` 可以是**空块**（`force_tributary.txt:38`）。
- **⚠️ 本体 bug，别照抄**：`force_tributary.txt:8` 写 `desc = "RELATIVE_TAX_BASE"`，但**真实键是 `DIPLOREASON_RELATIVE_TAX_BASE`**（`diplomacy_l_english.yml:914`）。裸 `RELATIVE_TAX_BASE` 不存在——**照抄 force_tributary 会继承一行 raw key 的成本提示**。正确前缀的用法见 `rtr_rein_in_rebellion.txt:164`。
- **loc 键作用域是全局的、不按目录归属**（`tools\loc-keys.md:52`）。
- **新增条约文件不需要前缀、不需要覆盖**（`guides\merging.md:8, :50`）；和平条约**不在顺序敏感清单上**（那只有 `levies` 与 `country_name_construction`）。同名文件同名块才会覆盖（`merging.md:7`）。
- ⚠️ 若条约创建的是 **mod 新附庸类型**，门槛别用 `is_subject_type`（`pitfalls.md:59` 疑似恒真）——用变量。