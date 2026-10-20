# CHANGELOG · account-positioning（新媒体起号定位）

## [1.0.1] - 2026-10-17

### Changed（市场提交规范对齐）
- `SKILL.md` frontmatter 补齐 WorkBuddy 开放平台技能市场提交字段：`version: 1.0.1` / `author: GuiGe` / `display_name` / `display_name_en` / `description_zh` / `description_en` / `category`；原 `name` / `description` / `agent_created` 保留不动
- 技能正文与 references 零改动，行为与 v1.0 完全一致

### Notes
- 动因：桂哥要求把本技能包分享给好友并评估上传 WorkBuddy 技能市场；开放平台文档（open.workbuddy.cn/docs/skill）要求上述字段，本地格式缺项会导致提交解析失败
- `category` 暂按开放平台文档示例值 `writing` 填写，正式提交前需与运营核对可选分类列表

## [1.0.0] - 2026-10-16

### 新增
- 初版发布。三环合一（新杠杆/边界变动/独有资源）+ 六维机会评分，改造自 opc-team `opc-niche-positioning`
- 新媒体四件套：平台矩阵 / 人设定位 / 内容支柱 / 变现路径
- 机读产物 `account-positioning.json`，含 `factory_handoff` 字段，可直接注入 content-factory（双通道内容工厂）的 `outputs/factory-config.json`
- references/three-ring-framework.md：完整方法论
- references/handoff-contract.md：与 content-factory v2.2 的字段级契约（平台代号对照表、合并规则、交接动作清单）

### 背景
桂哥需求：找一个能帮用户定位新媒体起号方向的技能并独立成技，产出物直供双通道内容工厂。调研结论：opc-team 的 opc-niche-positioning 是最近底座，但绑死 opc-doc 流水线且明令不碰平台/内容策略，故抽出改造为本技能。
