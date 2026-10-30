# 参考资料：社区工具速查（Arcanum / DeepL / Flag Builder / Workshop Manager / CMT）

> **来源**：[EU5 Wiki: Arcanum](https://eu5.paradoxwikis.com/Arcanum)、[PDX DeepL](https://eu5.paradoxwikis.com/PDX_DeepL)、[PDX Flag Builder](https://eu5.paradoxwikis.com/PDX_Flag_Builder)、[PDX Workshop Manager](https://eu5.paradoxwikis.com/PDX_Workshop_Manager)、[Community Mod Toolkit](https://eu5.paradoxwikis.com/Community_Mod_Toolkit)（均经 `action=raw` 抓取，2026-10-30）。
> **性质**：**外部资料（wiki 整理）**，非本库实测；工具行为以其 GitHub 最新版为准。wiki 未标版本校验。
> **体量**：约 6 分钟浏览

## 一、Arcanum（通用编辑器，最推荐）
- **定位**：EUV 全能 mod 编辑器（Minnator / MelonCoaster，持续开发）；GitHub `The-Arcanum-Project/Arcanum`。
- **前置**：先在游戏控制台跑过 **`script_docs`**（见 `reference\console-commands.md`）；Windows 10+ / 64 位。
- **核心功能**：语法分析 + 分级错误日志（定位到文件/行/字符 + 修复建议）、撤销/重做树、交互地图（框选地点批量改）、**热重载**、**40+ 游戏对象**编辑、Queastor 模糊搜索、专用编辑器（POP/发展度/省市区域大陆分组…）、**智能保存**（只改动过的对象写回，git diff 干净）、**INJECT/REPLACE 自动优化**、**Headless 校验**。
- **Headless CLI**（更新后快速校验）：
  ```bat
  Arcanum.exe -h -m "<mod目录>" -v "<game目录>" -c
  ```
  选项：`-h/--headless`、`-c/--clean`、`-v/--vanilla`（可多个，按加载顺序）、`-m/--mod`。
- **输出三段**：Header（错误统计）/ Faulty Files（问题文件分布）/ Error Listing（`Error Type: …, ID: …, Occurrences: …, Severity: …` + 描述 + 文件:行:列）。
- 注意：官方文档自称 WIP；遇到 bug 去其 Discord，日志在 `Documents/Arcanum/Crash_logs`。

## 二、pdx-deepl（增量自动翻译）
- **用途**：以某语言为基准，检测其它语言里**缺失/过期**的键并调 DeepL 翻译写回；**已翻译键带校验注释** `#deepl:<checksum>`，下次据此判断是否过期。
- **人工翻译保护**：没有 `#deepl:` 注释的行视为**手翻**，工具不动它。
- **不翻译**：`[...]`、`@...!`、`$...$`、`#...#!`（格式/函数/引用/图标自动转义）；注释默认保留，`#!` 开头的注释会被删。
- **配置** `translation-config.json`：`base-language`、`target-languages`（可带 `glossary` id）、`ignore-files`（相对 localization 根的路径）。
- **命令**：`pdx-deepl.exe --localization="<mod>\localization" --api-token="…" [--api-type=free|pro] [-stats]`。
- 免费 API 有限流与 50 万字符/月；建议一次只跑一种语言。

## 三、pdx-flag-builder（纹章/国旗编辑器）
- **来源**：读取 `game` 目录与 mod 的纹章定义；**Settings → Add Folder** 添加多个目录，列表越靠下优先级越高（mod 覆盖游戏）。
- **数据库**：Flag Database（COA 列表 + 预览）、Texture Database（图案/徽记）。
- **图层**：纹章本体（名称/图案/颜色）→ 徽记（纹理/颜色/**遮罩 mask 限定只染图案某一色**/位置·缩放·旋转）→ 子旗（另一 COA + 偏移/缩放）；越靠下的图层画在越上层。
- **快捷键**：方向键移动、`Ctrl`+方向键缩放、`Alt`+左右旋转、`Shift` 加大 10 倍；`Ctrl+Z/Y` 撤销重做。
- **保存语义**：`File → Save` 只改回原文件里**它自己的定义**（其余内容不动）；`Save To File...` 可覆盖同名或追加；`Copy Script` 复制脚本；**Export Image** 导出 768×512 PNG；`@variables` / `@[expressions]` 会存成算好的数字。
- 配置文件：Windows `%AppData%\pdx-flag-builder`、Linux `~/.config/pdx-flag-builder`。

## 四、pdx-workshop-manager（Steam 创意工坊发布）
- **用途**：读 `metadata.json`，按配置**可重复地**发布 mod 到创意工坊（名称/标签/描述/缩略图/更新说明）。
- **配置 JSON**（必需）：`game`（Steam appid）、`mods[].id`（`0` = 首次上传自动创建）、`directory`、`thumbnail`；可选 `names`、`descriptions`（各语言文件）、`change-note-directory`。
- **更新说明**：按 `metadata.json` 的 `version` 找 `<version>.bbcode`；找不到只警告、不失败。
- **运行**：GUI 版打开浏览器窗口；CLI `pdx-workshop-manager.exe [-config …] [-mod <id>]`；**需要 Steam 运行且已登录**。

## 五、Community Mod Toolkit（CMT，模板+工具链）
- **定位**：EU5 mod 作者工具集（Conner / Bahmut），GitHub `Europa-Universalis-5-Modding-Co-op/community-mod-toolkit`；与 Community Mod Framework/Menu 配套。
- **安装**（在 mod 仓库根）：
  ```bat
  curl.exe -sL https://raw.githubusercontent.com/.../tools/setup.py -o setup.py
  python setup.py
  ```
  复制工具、装依赖、加 `cmt` git remote；不覆盖已有文件。更新：`git fetch cmt && git merge cmt/toolkit-release --no-edit`。
- **编辑器/Git 约定**（自动生效）：`.editorconfig` 设 **UTF-8 BOM**、**CRLF**、脚本用 tab / 本地化 yml 用 1 空格；`.gitattributes` 统一换行；`.gitignore`；`.ignore`（给 ripgrep 用，比 gitignore 宽松）。
- **upload.py**：构建最小发布包并上传；`--mod` / `--workshop-pages` / `--dev` / `--submods`；支持 dev/release 分离、子 mod、版本门控、自动创建、按版本取 `change-notes.bbcode`。
- **translate.py**：DeepL / Gemini 批量翻译（含创意工坊标题描述），需 `.env` 里 `DEEPL_API_KEY` / `GEMINI_API_KEY`；本地化标签 `# NO-TRANSLATE` / `# NO-TRANSLATE BELOW` / `# NO-TRANSLATE END` / `# LOCK`；缓存 `tools/dependencies/.translate_hashes.json`（删掉=全量重译）。
- **CMM 可视化编辑器**：`tools\cmm-visual-editor.bat`（浏览器界面生成 CMM 设置脚本与本地化）。
- **配置** `tools/config.toml`：`source_language` / `localization_translator` / `translate_workshop` / `workshop_upload_item_id` / `upload_mod_by_default` / `upload_workshop_pages_by_default` / `upload_only_on_version_change`。

## 六、和本库的关系
- Arcanum 的 Headless 校验可与我们 `tools\jomini-validator`（jomini 解析预检）+ `mod-lint` 组合使用。
- 翻译工具与 `guides\localization.md` 的令牌校验规则互补：工具只保证不破坏标签，**语义/术语一致性仍需人工**。
- 纹章编辑对应 `fields\main_menu-coat_of_arms.md` 与 `vanilla\vanilla-heraldry-and-flags.md`。
