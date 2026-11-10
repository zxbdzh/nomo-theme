# nomo-theme

供 [Nomo](https://github.com/nomo-md/nomo) 使用的自定义 JSON 配色，参考思源笔记 [Neo 主题](https://github.com/hjkcai/neo-siyuan-theme)的新粗野主义界面。

**这是配色与部分样式的近似移植。Nomo 0.7.0 的原生界面没有接入所有 JSON 字段，仅修改 JSON 无法完整复现 Neo 的纸张纹理、粗轮廓及整套硬阴影。** 具体原因和源码位置见 [Nomo 适配核对](NOMO-COMPATIBILITY.md)。

## 下载和使用

- [下载主题 JSON](https://raw.githubusercontent.com/zxbdzh/nomo-theme/main/user-neo-brutalist-custom.json)
- 主题名称：**Neo 纸感（自定义）**
- 主题 ID：`user-neo-brutalist-custom`
- 包含浅色和深色两种模式；浅色以参考截图为依据，深色为延伸设计。

在 Nomo 的“偏好设置 → 外观”中导入 JSON，选择“Neo 纸感（自定义）”。同一主题 ID 再次导入会更新本地副本。

本文件沿用原配置的 `schemaVersion: 1`、`styleProfile: classic`、颜色字段和样式字段；它不是 Neo 的思源主题安装包，不能直接作为思源集市主题安装。

## JSON 中声明的样式

下表列出文件中的配置值，不表示所有 Nomo 组件都使用这些值。

| 项目 | 配置 |
| --- | --- |
| 浅色文档底色 | `#EEEEE8` |
| 浅色菜单底色 | `#EAEAE4` |
| 浅色文字 | `#30312C` |
| 浅色轮廓 | `#555650` |
| 点缀色 | 暗红、芥末黄、灰绿 |
| 边框宽度 | 声明为 `2px`；Nomo 0.7.0 未将 `borderWidth` 接入界面 |
| 大圆角 / 小圆角 | 声明为 `12px` / `7px`；仅影响读取这些令牌的组件 |
| 浮层阴影 | 右下 `3.5px`，模糊半径 `0`；仅影响使用 `shadowFloating` 的浮层 |
| 弹窗阴影 | 声明为右下 `3.5px`；`shadowDialog` 未接入，确认弹窗另有固定阴影 |
| 控件阴影 | 右下 `2px`，模糊半径 `0`；仅影响使用 `shadowRaised` 的控件 |

保留原配置中的代码高亮、引用块、五类提示块、滚动条、表格及标题栏字段，每种模式包含 65 个颜色字段和 20 个样式字段。Nomo 原生渲染器只读取其中一部分；例如代码语法配色由 `shikiTheme` 对应的内置调色板决定，并不直接采用全部 `code*` 颜色。

## 参考源码与还原范围

核对的 Neo 上游提交为 [`1ae78873968bf5238a724036157ddd387b00806f`](https://github.com/hjkcai/neo-siyuan-theme/tree/1ae78873968bf5238a724036157ddd387b00806f)。

- [`styles/brutalism/brutalism.scss`](https://github.com/hjkcai/neo-siyuan-theme/blob/1ae78873968bf5238a724036157ddd387b00806f/styles/brutalism/brutalism.scss)：菜单、弹窗采用右下 `3.5px` 无模糊阴影与 `2px` 内描边；控件采用右下 `2px` 阴影。
- [`styles/menu/menu.scss`](https://github.com/hjkcai/neo-siyuan-theme/blob/1ae78873968bf5238a724036157ddd387b00806f/styles/menu/menu.scss)：菜单容器使用 `12px` 超椭圆参数，菜单项使用 `7px`；本配置使用普通圆角作近似映射。

配色依据参考截图手工设计，并非从 Neo 完整导出。上游通过 CSS 根据背景动态调整菜单文字颜色；当前 JSON 没有独立的悬停文字颜色字段，因此悬停背景采用浅灰，以保证深色文字可读。深灰反白用于强调填充色与其配套文字色。

纸张颗粒、超椭圆曲线、逐组件内描边及思源专属交互不能仅凭现有 JSON 字段完整复现。Nomo 0.7.0 使用 GPUI 原生渲染，其主题加载器不执行 CSS，也不加载主题外部资源；补一个 CSS 文件不能直接让这些效果生效，需要修改 Nomo 的原生绘制代码。上游 Neo 项目的许可证为 [GPL-3.0](https://github.com/hjkcai/neo-siyuan-theme/blob/1ae78873968bf5238a724036157ddd387b00806f/LICENSE)；本仓库不分发 Neo 的源码、字体或图片资源。

## 验证

已检查 JSON 语法、重复键、与原始配置的字段及类型一致性、颜色格式，以及每种模式的 13 组声明颜色对比度，检查项均达到 `4.5:1`。已对照 Nomo 0.7.0 源码核对字段映射。这些检查不代表全部字段都影响界面，也不替代实际渲染验收；应用派生或覆盖后的颜色需要另行检查。
