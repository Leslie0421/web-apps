# 变更记录

用于记录需要跨对话继续维护的关键改动。后续更新时按日期追加，重点写清文件、原因和跨仓库依赖。

## 2026-07-29：增加中文字体与西文字体高级设置

- `apps/documenteditor/main/app/template/ParagraphSettingsAdvanced.template`、`view/ParagraphSettingsAdvanced.js`
  - 在高级段落设置的“字体”页增加“中文字体”和“西文字体”两个字体选择器。
  - 中文字体只提交 `eastAsia`；西文字体同时提交 `ascii/hAnsi`；未修改的槽位保持不变。
  - 多选字体不一致时显示空白混合状态，不会因打开并确认对话框而写入未修改的字体。
  - 复用工具栏已加载的字体集合，避免按需打开对话框时下拉列表为空。
  - 点击确定时读取选择器最终值并与打开对话框时的原值比较，兼容键盘输入和下拉选择两种操作；不再依赖仅在部分交互路径触发的 `selected` 事件。
- `apps/documenteditor/main/app/controller/DocumentHolderExt.js`、`controller/RightMenu.js`、`view/ParagraphSettings.js`
  - 右键菜单和右侧属性面板两个高级段落入口均传入字体集合并通过同一次 `paraApply` 应用设置。
- `apps/documenteditor/main/locale/en.json`、`zh.json`、`zh-tw.json`
  - 补充英文、简体中文和繁体中文文案。

### 验证状态

- 相关 JavaScript `node --check`、本地化 JSON 解析和 `git diff --check` 通过。
- `build/node_modules/.bin/grunt deploy-documenteditor-component` 生产构建通过；仅有既有的 bundle 体积和 Browserslist 数据过期警告。
- 已在生产部署环境验证中文与西文字体分别生效，临时诊断日志已清除。

### 跨项目依赖

- 依赖 `../sdkjs` 新增的 `asc_CTextFontFamilies`、`put_TextPrFontFamilies` 和 `paraApply(FontFamilies)`。

## 2026-07-29：建立东亚精细排版界面实施计划

- 新增 `docs/EAST_ASIAN_TYPOGRAPHY_IMPLEMENTATION_PLAN.md`。
- 将界面拆分为中西文字体、段落高级设置、文档网格页面设置和集成回归四个阶段。
- 明确界面必须依赖 `../sdkjs` 的正式属性 API，不通过普通字距、行距或真实空格模拟。
