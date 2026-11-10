# nomo-theme

参照思源笔记 [Neo 主题](https://github.com/hjkcai/neo-siyuan-theme)中新粗野主义界面的自定义 JSON 配色，以纸灰背景、石墨描边、无模糊硬阴影为主要特征。

## 下载和使用

- [下载主题 JSON](https://raw.githubusercontent.com/zxbdzh/nomo-theme/main/user-neo-brutalist-custom.json)
- 主题名称：**Neo 纸感（自定义）**
- 主题 ID：`user-neo-brutalist-custom`
- 包含浅色和深色两种模式；浅色以参考截图为依据，深色为延伸设计。

将 JSON 导入支持原 `user-github-custom.json` 结构的应用，在主题选择器中选择“Neo 纸感（自定义）”。具体导入入口取决于使用的应用。

本文件沿用原配置的 `schemaVersion: 1`、`styleProfile: classic`、颜色字段和样式字段；它不是 Neo 的思源主题安装包，不能直接作为思源集市主题安装。

## 样式

| 项目 | 配置 |
| --- | --- |
| 浅色文档底色 | `#EEEEE8` |
| 浅色菜单底色 | `#EAEAE4` |
| 浅色文字 | `#30312C` |
| 浅色轮廓 | `#555650` |
| 点缀色 | 暗红、芥末黄、灰绿 |
| 边框宽度 | `2px` |
| 大圆角 / 小圆角 | `12px` / `7px` |
| 菜单和弹窗阴影 | 右下 `3.5px`，模糊半径 `0` |
| 控件阴影 | 右下 `2px`，模糊半径 `0` |

保留原配置中的代码高亮、引用块、五类提示块、滚动条、表格及标题栏字段，每种模式包含 65 个颜色字段和 20 个样式字段。

## 参考源码与还原范围

核对的 Neo 上游提交为 [`1ae78873968bf5238a724036157ddd387b00806f`](https://github.com/hjkcai/neo-siyuan-theme/tree/1ae78873968bf5238a724036157ddd387b00806f)。

- [`styles/brutalism/brutalism.scss`](https://github.com/hjkcai/neo-siyuan-theme/blob/1ae78873968bf5238a724036157ddd387b00806f/styles/brutalism/brutalism.scss)：菜单、弹窗采用右下 `3.5px` 无模糊阴影与 `2px` 内描边；控件采用右下 `2px` 阴影。
- [`styles/menu/menu.scss`](https://github.com/hjkcai/neo-siyuan-theme/blob/1ae78873968bf5238a724036157ddd387b00806f/styles/menu/menu.scss)：菜单容器使用 `12px` 超椭圆参数，菜单项使用 `7px`；本配置使用普通圆角作近似映射。

配色依据参考截图手工设计，并非从 Neo 完整导出。上游通过 CSS 根据背景动态调整菜单文字颜色；当前 JSON 没有独立的悬停文字颜色字段，因此悬停背景采用浅灰，以保证深色文字可读。深灰反白用于强调填充色与其配套文字色。

纸张颗粒、超椭圆曲线、逐组件内描边及思源专属交互不能仅凭现有 JSON 字段完整复现，需要目标应用额外支持 CSS。上游 Neo 项目的许可证为 [GPL-3.0](https://github.com/hjkcai/neo-siyuan-theme/blob/1ae78873968bf5238a724036157ddd387b00806f/LICENSE)；本仓库不分发 Neo 的源码、字体或图片资源。

## 验证

已检查 JSON 语法、重复键、与原始配置的字段及类型一致性、颜色格式，以及每种模式的 13 组文字与背景对比度，检查项均达到 `4.5:1`。这些检查不替代目标应用中的实际渲染验收，最终效果取决于应用如何使用各个字段。
