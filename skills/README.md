# 附带技能（skills）

知识库是**内容层**，技能只是**流程层**——技能内不重复存放知识文档，全部指向仓库根的知识库。

知识库根（`<KB>`）的几处入口：`README.md`（人类入口 + 常见任务速查）、`START-HERE.md`（新手路线与按任务导览）、`glossary.md`（术语总表）、`INDEX.md`（全部文档的逐档映射表）。

## eu5-mod-review

| 文件 | 作用 |
| --- | --- |
| `eu5-mod-review\SKILL.md` | 技能主文件：制作工作流 6 步 + 审查流程 9 项检查清单 |
| `eu5-mod-review\references\README.md` | 兜底指引：说明知识库在本仓库根 |

**知识库根 `<KB>` 的解析**：`SKILL.md` 开头的 `<KB>` 块按序探测——① 同仓形态 `<SKILL.md 目录>\..\..\`（克隆本仓库即命中）；② 本机形态 `D:\dsh-plugins\kb\eu5-modding\`。

## 安装

把 `eu5-mod-review\` 整个目录复制到 agent 的技能目录：

- DSH：`C:\Users\<你>\.dsh\skills\eu5-mod-review\`
- Claude Code：`<项目>\.claude\skills\eu5-mod-review\`

知识库默认按上表探测；若把知识库放在别处，改 `SKILL.md` 里 `<KB>` 块的第 2 条即可。