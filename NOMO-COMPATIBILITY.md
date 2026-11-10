# Nomo 0.7.0 适配核对

核对日期：2026-11-10。核对版本：Nomo `v0.7.0`，对应提交 [`799f1bdf13b77da7c1a41cc7816b8c3678bbc491`](https://github.com/nomo-md/nomo/tree/799f1bdf13b77da7c1a41cc7816b8c3678bbc491)。本说明依据该版本的实际渲染代码，不将 `docs/archive` 中的旧 Web 界面文档当作当前行为依据。

## 为什么配色相近，整体效果仍不同

Neo 的界面由 CSS 按组件分别绘制。Nomo 的这份 JSON 是供 GPUI 原生界面使用的声明式令牌；导入器接受的字段多于渲染器实际读取的字段。导入成功、字段存在、格式正确，只能证明配置合法，不能证明每个字段影响画面。

| 截图中的效果 | Nomo 0.7.0 的实现 | JSON 能否完整实现 |
| --- | --- | --- |
| 纸张颗粒与纹理 | 加载器只解析令牌，不执行 CSS 或加载外部主题资源 | 不能 |
| 2px 深色轮廓 | `Theme::from_definition` 不读取 `borderWidth`；多处控件直接调用 `.border_1()` | 不能统一实现 |
| 所有弹窗使用右下硬阴影 | 读取 `shadowFloating` 和 `shadowRaised`；不读取 `shadowDialog`，确认弹窗自己创建柔和阴影 | 只能影响部分控件 |
| 菜单悬停时深底浅字 | 下拉选项悬停只修改背景，文字仍由普通态或选中态的颜色决定 | 不能在所有状态独立控制 |
| 所有组件采用指定圆角 | 读取 `radiusSm/Md/Lg`，但浮层卡片、分段容器、确认弹窗等仍有固定圆角 | 只能影响部分控件 |
| 经典引用块与提示块的背景 | 映射器读取边线和文字颜色，没有将对应的 `Background` / `ClassicBackground` 字段赋给原生主题 | 修改这些背景字段不会传入该映射 |
| 自定义代码语法颜色 | 映射器根据 `shikiTheme` 选择内置调色板，再赋值代码语法颜色 | 应通过支持的 `shikiTheme` 选择内置配色 |

## 可核验的源码

- [主题映射 `theme.rs:243`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/theme.rs#L243)：颜色、圆角与阴影从定义进入原生 `Theme` 的位置。
- [样式与语法调色板 `theme.rs:305`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/theme.rs#L305)：只读取部分长度令牌及两种阴影，随后以 `shikiTheme` 对应的调色板覆盖代码语法颜色。
- [加载器 `theme_catalog.rs:1`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/theme_catalog.rs#L1)：声明主题不执行 CSS，也不加载外部资源。
- [字段合并与校验 `theme_catalog.rs:265`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/theme_catalog.rs#L265)：只允许基础主题中已有的令牌；不能自行新增一个纹理或悬停文字字段让渲染器采用。
- [阴影解析 `theme_catalog.rs:291`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/theme_catalog.rs#L291)：只解析位移、模糊、颜色；生成的阴影固定为非内阴影且扩散半径为零，不能把 Neo 的复合内描边直接塞进该字段。
- [按钮边框 `components.rs:283`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/ui/components.rs#L283)：边框宽度直接使用 `.border_1()`。
- [浮层卡片 `components.rs:320`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/ui/components.rs#L320)：采用主题浮层阴影，但圆角固定为 `10px`。
- [下拉选项 `components.rs:553`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/ui/components.rs#L553)：普通文字和选中文字分别取 `fg`、`accent_strong`，悬停仅修改背景色。
- [确认弹窗 `dialogs.rs:258`](https://github.com/nomo-md/nomo/blob/799f1bdf13b77da7c1a41cc7816b8c3678bbc491/src-gpui/src/ui/dialogs.rs#L258)：固定 `8px` 圆角、1px 边框，以及向下 `20px`、模糊 `60px` 的阴影。

## 本主题的边界

当前 JSON 保留原始合法字段，使颜色和已接入的圆角、阴影仍可使用。浅灰悬停底色是为适配当前菜单文字不会反白的行为；直接换成截图中的深灰，会令普通菜单文字难以辨认。

若要还原 Neo 的整套效果，需要在 Nomo 原生界面增加对应能力，包括边框宽度的实际接入、面板轮廓和阴影、状态专用文字颜色、统一圆角，以及纸张纹理绘制。仅更新主题 JSON 或在仓库放置额外 CSS 均不足以实现。

本次核对未修改 Nomo 程序、用户设置或已安装的主题副本，未进行应用内视觉验收；结论限于上述版本和源码。未来版本的适配范围需要重新核对。
