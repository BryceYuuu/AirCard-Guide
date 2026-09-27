# 离线卡面制作工具 / Offline artwork studio

[中文指南](../../docs/guides/README.zh-CN.md) · [English guide](../../docs/guides/README.en.md) · [卡面素材 / Artwork](../../assets/skins/README.md)

## 中文

将自己的图片裁切并导出为 **1536 × 969 PNG**，再交给 AirCard 预览和应用。工具是单个 [`index.html`](index.html) 文件，内置全部 CSS 和 JavaScript；无需安装依赖、启动服务器或联网。图片只在浏览器的本地 Canvas 中处理，没有上传、遥测或操作历史存储。

### 下载与使用

1. 在 GitHub 打开本目录的 [`index.html`](index.html)，点击文件右上角的 **Download raw file / 下载原始文件** 按钮。如果界面只显示 **Raw**，打开 Raw 后将文件保存为 `index.html`；也可以从仓库的 **Code → Download ZIP** 下载并解压，找到 `tools/card-artwork/index.html`。不要保存 GitHub 的文件预览网页。
2. 用支持 Canvas、File API 和下载功能的现代浏览器打开本地 `index.html`。双击文件或将文件拖入浏览器即可，地址栏应为 `file://…/index.html`，不需要本地服务器。
3. 点击「选择图片」，或将一张 **PNG、JPEG 或 WebP** 拖入预览区域。文件最大 **30 MiB**，最多 **48 MP**，单边最多 **16,384 px**。SVG、HEIC、GIF 和非图片文件会被拒绝；请先转换为支持的格式。
4. 初始构图按比例填满目标画面，并从中心裁切。拖动预览、调整缩放，或用「水平取景／垂直取景」滑杆定位。所有滑杆均可通过键盘 Tab 聚焦和方向键调整；聚焦预览后也可以使用方向键，按住 Shift 可以扩大移动幅度。某个方向没有可裁切余量时，其滑杆不可用。
5. 按需选择「保留透明通道」「填充白色背景」或「填充黑色背景」。默认保留原图透明度；棋盘格仅用于预览，**不会导出到图片**。
6. 点击「下载卡面 PNG」。导出尺寸始终为 **1536 × 969 像素**，不会因页面大小或屏幕像素密度变化。文件名为 `原文件名-aircard-1536x969.png`；文件名过长时会截短，并替换不适合作为文件名的字符。
7. 将导出的 PNG 导入 AirCard，检查预览，再按指南应用到选中的卡片。浏览器关闭或刷新后，未导出的构图会丢失。

右上角可切换中文和英文；语言切换不会重置当前构图。「重置缩放与居中构图」恢复最初的裁切位置和缩放，保留已选背景处理方式。

### 构图与显示边界

- **与 AirCard 的关系：** 初始裁切几何对应 [`AirCardApp.swift`](../../AirCardApp.swift) 中 `AppViewModel.prepareCardImage` 的 `max(1536 / width, 969 / height)` 比例和居中位置。本工具允许在导出前手动改变构图；它不安装 AirCard、不连接手机，也不直接替换 Wallet 文件。
- **像素与清晰度：** 当原图裁切区域小于输出所需像素时，页面显示放大提示。固定尺寸输出不能补回丢失的细节；增加缩放会进一步降低可用分辨率。
- **透明区域：** 默认导出保留 alpha 通道。Wallet 如何显示透明区域取决于实际卡片及系统；如需要确定的底色，请选择白色或黑色。不会自动移除已有实色背景。
- **预览范围：** 预览显示 PNG 的完整矩形范围，不模拟 Wallet 圆角、卡组织标志、文字叠加或实机缩放。让重要图案和文字远离边缘，并以实机效果为准。
- **图片解码：** 浏览器负责读取照片方向和颜色，最终颜色、缩放插值及元数据处理不保证与 macOS 的 `NSImage` 逐像素一致。PNG/WebP 动画只读取一个静态画面，不能选择动画帧；精确选帧请先在其他工具中导出静态图片。
- **资源限制：** 文件与尺寸限制用于减少卡顿和内存占用。超大图片仍可能在浏览器解码过程中触及设备内存限制；低内存设备请先缩小原图。工具不重新压缩或修改源文件。
- **错误处理：** 非图片、损坏文件或超限文件显示错误提示；如果已载入一张有效图片，原先的构图会保留。一次只接受一张图片；连续选择新图片时，以最后一次选择为准。
- **离线与安全：** 不读取远程图片地址，不加载 CDN、字体、脚本或外部素材；页面的 CSP 禁止网络连接和外部资源。只有你主动选择或拖入的本地文件会被读取。刷新后不会记住图片或语言设置。

### 本地验证（维护者）

在仓库根目录运行以下命令，使用 Node.js 内置测试运行器，无需安装依赖。使用工具本身不需要 Node.js。

```sh
node --test tools/card-artwork/tests/crop.test.cjs
```

测试读取 HTML 中的实际脚本，在轻量 DOM/Canvas 模拟环境中检查已知裁切尺寸、216 组边界组合、文件校验、加载竞态与失败后保留构图。测试不替代浏览器验收：实际图片解码、拖放和 PNG 像素导出仍需在浏览器中验证。

## English

Crop your own image and export a **1536 × 969 PNG**, then preview and apply it in AirCard. The entire tool is one [`index.html`](index.html) file with embedded CSS and JavaScript. No dependencies, server, or internet connection are required. Images are processed in a local browser canvas, with no uploads, telemetry, or saved editing history.

### Download and use

1. Open [`index.html`](index.html) in this GitHub directory and click **Download raw file** in the file toolbar. If only **Raw** is available, open it and save the raw file as `index.html`. Alternatively, choose **Code → Download ZIP**, extract it, and find `tools/card-artwork/index.html`. Do not save GitHub's file-preview webpage.
2. Open the local `index.html` in a modern browser with Canvas, File API, and download support. Double-click it or drag it into the browser. The address should start with `file://` and end in `index.html`; no local server is necessary.
3. Choose or drop one **PNG, JPEG, or WebP** image into the preview area. The limits are **30 MiB**, **48 MP**, and **16,384 px per side**. SVG, HEIC, GIF, and non-image files are rejected; convert them first.
4. The initial view scales the image to fill the target and crops it at the center. Drag the preview, adjust zoom, or use the horizontal and vertical crop-position sliders. Tab and arrow keys operate the sliders. Arrow keys also move the crop when the preview is focused; hold Shift for larger steps. A slider is disabled if the entire corresponding dimension is already visible.
5. Choose whether to keep transparency or fill transparent areas with white or black. Source alpha is preserved by default. The checkerboard is a preview aid and **is never exported**.
6. Click **Download artwork PNG**. The output is always **1536 × 969 pixels**, independent of the page size or screen pixel density. The filename is `original-name-aircard-1536x969.png`; long names are shortened and unsuitable filename characters are replaced.
7. Import the PNG into AirCard, check its preview, and follow the guide to apply it to the selected cards. Export before closing or refreshing this page to keep your work.

The top-right button switches between Chinese and English without changing the composition. **Reset zoom & center crop** restores the initial scale and position while preserving the selected transparency/background setting.

### Composition and display limitations

- **Relationship to AirCard:** the initial crop geometry matches `AppViewModel.prepareCardImage` in [`AirCardApp.swift`](../../AirCardApp.swift): scale by `max(1536 / width, 969 / height)` and center the result. This utility lets you reframe before exporting. It does not install AirCard, connect to a phone, or replace Wallet files.
- **Resolution:** an enlargement notice appears when the crop contains fewer pixels than the output needs. Exporting at a fixed size cannot recover missing detail; zooming further reduces available source resolution.
- **Transparency:** alpha is preserved by default. Wallet's handling of transparency depends on the card and system. Choose white or black for a defined background. Existing opaque backgrounds are not removed.
- **Preview coverage:** the preview shows the full rectangular PNG, without simulating Wallet's rounded corners, network logos, text overlays, or device scaling. Keep important elements away from the edges and check the actual phone result.
- **Decoding:** the browser handles photo orientation and colors. Color, interpolation, and metadata handling are not guaranteed to match macOS `NSImage` pixel for pixel. Animated PNG/WebP inputs are captured as one still image; frame selection is not supported. Export a specific frame elsewhere if needed.
- **Resource limits:** file and dimension limits reduce stalls and memory use. A large file may still exceed device memory while the browser decodes it; resize first on low-memory devices. The source file is never overwritten or recompressed.
- **Errors:** unsupported, corrupt, or oversized inputs show an error. If a valid image was already loaded, its composition is preserved. Only one file is accepted at a time; when selections overlap, the latest selection wins.
- **Offline operation:** the page does not read remote image URLs or load CDNs, fonts, scripts, or external artwork. Its Content Security Policy blocks network connections and external resources. Only local files explicitly chosen or dropped by you are read. Images and language settings are not remembered after refresh.

### Local checks for maintainers

Run this command from the repository root. It uses Node.js's built-in test runner without dependencies. Node.js is not required to use the tool itself.

```sh
node --test tools/card-artwork/tests/crop.test.cjs
```

The tests load the actual inline script from the HTML into a small DOM/Canvas harness. They cover known crop dimensions, 216 boundary combinations, file validation, loading races, and retaining the previous composition after errors. Browser checks are still required for actual image decoding, drag-and-drop, and encoded PNG pixels.
