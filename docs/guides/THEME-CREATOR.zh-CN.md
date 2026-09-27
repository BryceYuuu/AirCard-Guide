# 锁屏密码键盘主题制作

[返回中文指南](README.zh-CN.md) · [English](THEME-CREATOR.en.md) · [兼容性记录](COMPATIBILITY.zh-CN.md)

本文对应仓库中的 Theme Creator 实现，介绍本机制作、导出和检查 `.passthm` 的流程。制作及导出可在未连接 iPhone 时进行；刷入需要连接并信任设备。本文没有新增实机测试，Mac 上的预览和导出成功不代表手机上的显示已经验证。

## 先选一种制作方式

打开 **Passcode (.passthm) → Theme Creator**。

| 方式 | 适合的目标 | 输出行为 |
| --- | --- | --- |
| **Poster Slice (Puzzle)** | 把一张照片或插画分配到整个数字键盘 | 从同一张原图切出 0–9 的图像；可以选连续图案或圆形按钮 |
| **Individual Keys** | 每个数字放不同图案 | 对每个已配置数字单独裁成圆形图像，可分别平移、缩放 |

编辑器使用固定的键盘布局，整图切片基准为 **915 × 1148**；这与钱包卡面使用的 **1536 × 969** 是两种用途。无需把图片文件扩展名从 PNG 改成 `.passthm`，主题包应由导出功能生成。

## 流程一：一张图做完整键盘

1. 选择 **Poster Slice (Puzzle)**，点击 **Choose Poster...**，或把图片拖到 **Drop poster or wallpaper here** 区域。选图窗口支持 PNG、JPEG、HEIC、WebP 等 macOS 可解码图片。
2. 在 **Slicing Style** 中选择样式：**Seamless Poster** 将图案按键盘格子切开，数字 0 使用底部整行图像；**Circle Buttons** 只保留每个按钮的圆形区域，其余部分透明。这两种方式都只处理键盘图像，不会设置整张锁屏壁纸。
3. 在右侧键盘预览上拖动图像调整位置，使用 **Zoom & Framing** 缩放滑块调整大小（0.5–3.0 倍）。放得过小或移得过远可能露出透明空白，要检查边缘及 0 所在的底部。
4. 用 **Reset Position** 可把整图恢复至默认缩放和居中位置。**Change...** 换图时也会重置构图；**Remove** 会清空整个制作器的内容，包括已配置的单键。
5. 检查 0–9 是否都有图像，点击 **Export .passthm...** 保存，再按下方“导出后本机检查”重新导入。

想在整图基础上单独换某个键：切到 **Individual Keys**，点击 **Fill from Poster**，再编辑目标数字。这个按钮会复制全部整图切片，并覆盖对应数字已有的单键图片；它不是仅填补空缺。复制后是独立的切片，重新调整单键时会按单键的圆形裁切规则处理，不会保持原来的连续整图形状。

## 流程二：逐个数字放图

1. 选择 **Individual Keys**。点击预览中的空白数字键，选取图片；也可直接把图片拖到目标数字上。
2. 点选已配置的键，在 **Key … Framing** 中用滑块缩放，或直接在该键上拖动图片。只有当前选择的数字会改变。
3. 用 **Change Image...** 更换这一个键的图片；**Reset** 重置其构图；**Remove** 删除该键的待导出图像。右键菜单也有换图、重置、清除功能。
4. 为其余数字重复操作。完整主题应检查状态中的 **10 of 10 keys configured**；程序只要求至少配置一个键就允许导出，因此能导出不等于已配置全部数字。
5. 导出，并重新导入检查每一个数字。未配置的数字不会自动补齐，也不会因为清除编辑器里的图片而恢复手机上的旧主题。

**预览里的白色数字、字母和描边是界面叠加效果。** 图片处理与导出代码保存的是素材切片，并不会把这些预览文字自动画进 PNG。如果你的设计要求图片本身带有数字，请在原始素材中准备好，并以导出包里的实际图像为准。

## 修改已有 `.passthm`

1. 先保留原始主题文件。如果制作器已有未保存内容，先导出，再在 **Theme Creator** 点击 **Clear All**，避免导入缺少部分数字的主题时残留旧图。
2. 切到 **Apply .passthm**，点击 **Choose .passthm File...** 或拖入文件。文件选择器接受 `.passthm`、`.passtheme`、`.zip`，但包内还必须有可识别的图像资源；修改扩展名不能修复坏包。
3. 看到主题名称、资源数量及数字预览后，点击 **Edit in Creator**。程序会切换到 **Individual Keys**。
4. 修改需要的数字，然后以新的文件名导出，如 `MyTheme-v2.passthm`，再重新导入核对。

编辑器为每个数字采用导入时取得的一张预览图，**不是无损主题包编辑器**：同一数字的语言、字重等不同版本不会作为独立图层保留。重新导出还会重新打包资源及 `_big` 标记；原包的特殊结构或 `_small` 标记不保证保留。对于已有的矩形整图切片，只要对单键重新缩放或平移，就会重新圆形裁切。需要保留这些差异时，请保留原包并逐项比较新包。

## 导出后本机检查

这一轮只检查 Mac 上的文件，尚不刷入手机：

1. 用 **Export .passthm...** 保存至一个新文件名，确认出现导出成功提示。导出时使用当前模式：整图模式导出整图切片，单键模式导出单键配置。
2. 切到 **Apply .passthm → Choose .passthm File...**，选中刚导出的文件。确认名称对应新文件、资源数量非零，并逐个检查 0–9 的预览。
3. 若要检查 PNG 本身，可在 Finder 复制一份导出包，把**副本**扩展名改为 `.zip` 后解压查看。原始 `.passthm` 留作导入；重点检查图案位置、透明边缘、数字 0，以及是否真的有预期文字。
4. 遇到 **Failed to inspect .passthm file** 时先排查包是否完整、是否含图像，重新导出到新文件后再导入。资源数量是经过解析/扩展后的数量，不是数字键数量，不能把“很多 assets”当作十个数字均完整的依据。

当前 **Export .passthm...** 保存文件时使用导出器默认设置：全部支持的语言、常规与粗体两套命名，并包含 `TelephonyUI-10` 和 `TelephonyUI-9` 目录。界面的语言、字重与 **Target** 下拉不会缩减这个手动导出包；直接点击刷入制作器主题时，才会按所选语言和字重暂存主题，再按所选目标目录写入。

## 刷入前：语言、粗体与 Target

在 **Flash & Language Target** 中确认 iPhone 的设置。已连接设备时，**Auto-detect** 会使用设备检测结果重新选择：

| 设置 | 实际含义 |
| --- | --- |
| **System Language** | 选择要生成/写入的语言文件名；指定一种语言时还会包含 `other` 回退命名，并不会修改 iPhone 的系统语言 |
| **Font Weight / Style** | 选择常规、粗体，或两者都生成；对应 iPhone 的 **Bold Text / 粗体文本**，不会改变原图文字的粗细 |
| **Target** | 选择手机缓存目录：自动规则为 iOS 18+ → `TelephonyUI-10`，iOS 16–17 → `TelephonyUI-9`，更早版本 → `TelephonyUI-8`；这是目录选择逻辑，不是这些系统版本的兼容性证明 |

自动检测取语言代码的第一段（如 `zh-Hans` → `zh`），匹配不到选项时不会替你确定正确目标；粗体值读取不到时保留已有选择。设备助手在语言值不可用时还可能回退到 `en`，所以应与 iPhone 上的实际设置对照。

界面中有三种不同的 **Universal**：

- **All Languages (Universal)**：展开程序支持的语言命名，范围由代码中的语言列表决定。
- **Universal (Regular + Bold)**：同时生成常规与粗体文件名，使用的是同一份图像数据。
- **Target → Universal (All 8, 9, 10)**：向三个缓存目录写入；文件更多，也不代表所有 iOS 或机型已验证。

选择准确的语言和字重通常会减少写入文件数；界面中的“约 600 个文件”只是提示，实际数量随主题和目标而变。全选 Universal 也不能解决所有加载或兼容问题。

**导入主题会把 Target 改成包检测出的目录。** 一个同时有 `TelephonyUI-9` 与 `-10` 的包也可能显示 `-9`。因此应先导入，再在已连接设备上点 **Auto-detect** 并复核目标、语言和粗体；包中目录名不是当前手机型号的检测结果。

## 应用与记录结果

连接、解锁并信任 iPhone，复核预览和目标后点击 **Flash Passcode Theme**。程序报告成功只表示刷入流程报告完成；按上游说明重启 iPhone，再实际检查锁屏键盘。将“应用报告成功”和“手机上显示正确”分开记录；没有亲自看到刷新结果时填“未测试”。

若要提交测试结果，使用[反馈模板](compatibility-report-template.md)。钱包扫描结果不能证明密码键盘主题兼容，反之亦然；记录范围见[兼容性说明](COMPATIBILITY.zh-CN.md)。

## 源码依据

- [AirCardApp.swift](../../AirCardApp.swift)：`KeypadSlicer`、`PasscodeThemeExporter`、`applyDevicePreferences`、`inspectPasscodeTheme`、`editLoadedThemeInCreator`、`openSavePasscodeThemePanel` 及制作器界面。
- [aircard_backend.py](../../aircard_backend.py)：`parse_passthm_archive`、`cmd_inspect_passthm` 与 `cmd_flash_passthm` 的包解析、预览和文件目标处理。
- [Sources/device_helper.m](../../Sources/device_helper.m)：`Language` 与 `EnhancedTextLegibility` 的读取。
- [上游 README](https://github.com/Mak5er/AirCard/blob/main/README.md#how-to-apply-lockscreen-passcode-themes-passthm)：手机刷新步骤。
