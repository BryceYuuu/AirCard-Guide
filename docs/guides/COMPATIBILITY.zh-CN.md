# 兼容性记录与反馈方法

[返回中文指南](README.zh-CN.md) · [English](COMPATIBILITY.en.md) · [主题制作](THEME-CREATOR.zh-CN.md) · [复制反馈模板](compatibility-report-template.md)

这里按操作阶段记录证据，不用单一“支持/不支持”标签概括整台设备。一次连接成功不能证明能扫描卡片，扫描成功也不能证明能刷入或让手机刷新图片。本指南未新增任何实机兼容性测试。

## 目前有明确范围的验证记录

以下内容来自仓库内的[钱包卡片检测验证记录](../wallet-card-detection.md)。这是上游记录的测试结果，非本指南作者重新测试。

| 功能 / 设备 | iOS | Mac / macOS | AirCard 基线 | USB 连接 | 钱包扫描 | 刷入卡面 | 手机卡面刷新 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 钱包卡片 / iPhone 15 Pro (`iPhone16,1`) | 18.6.2 | MacBook Air M3（2024）/ 26.6.2 | 1.2.3，`02b5ba8`（见下方说明） | 成功 | 扫描修复后成功，检测到 8 个卡片标识，测试者确认卡片出现 | **未测试** | **未测试** |

该验证文档把 `02b5ba8` 标为 **Source baseline**，并叙述了扫描修复后的验证结果；它没有单独给出修复后测试构建的完整 commit，不能据此断言未修改的基线提交就有同样结果。文档还记录了停止扫描助手后界面复位、再次扫描能连接且保留已检测卡片。

最初反馈中的 macOS 26.2 截图不属于最终验证环境，不能把 26.2 填成已验证。此次检测测试没有刷入任何卡面，也没有测试密码键盘主题。

## 上游声明与未验证情况

| 项目 | 有什么依据 | 应当如何理解 |
| --- | --- | --- |
| iOS 18+、无需越狱 | [上游 README](https://github.com/Mak5er/AirCard/blob/main/README.md) 的项目说明 | 是项目声明，不是每个机型和系统小版本的实测矩阵 |
| iOS 27 release | [上游 README](https://github.com/Mak5er/AirCard/blob/main/README.md) 写有 “Tested on iOS 27 release” | 保留为上游测试声明；该说明未提供具体机型、macOS、构建和各阶段结果，不能补成全成功记录 |
| iPhone 17 / iOS 27 的报告 | [检测验证记录](../wallet-card-detection.md) 链接到 [issue #28](https://github.com/Mak5er/AirCard/issues/28)，并写明该案例未测试 | 该记录没有验证此组合，不根据其他 iOS 27 声明推断此案例成功 |
| Apple Silicon / Intel Mac | [上游 README](https://github.com/Mak5er/AirCard/blob/main/README.md) 声明提供通用构建 | 构建架构支持不等于所有 Mac / macOS 组合都完成设备测试 |
| 密码键盘主题目标中的 iOS 14–17、Universal 选项 | [界面及目标选择逻辑](../../AirCardApp.swift) | 有缓存目录选项不等于这些系统已获支持或完成验证，详见[主题目标设置](THEME-CREATOR.zh-CN.md#刷入前语言粗体与-target) |

“未测试”表示缺少测试证据，不代表成功，也不代表不支持。不同功能要分开填；制作器在 Mac 上导出成功，不应计作 iPhone 主题刷入成功。

## 每个阶段怎么判断

| 阶段 | 可以填写“成功”的可观察结果 | 还不能说明什么 |
| --- | --- | --- |
| 连接 | AirCard 识别出当前已解锁、已信任的 iPhone，并显示已连接 | 不能保证目标缓存可写 |
| 钱包扫描 | 点击 **Scan Cards**，在手机认证并点选卡片后，目标卡片确实出现在 AirCard 列表中 | 仅出现扫描连接日志、没有卡片，不算扫描成功 |
| 钱包刷入 | **Flash Skins** 对本次目标卡片报告完成；多卡时逐张区分成功与失败 | 不能证明所有卡片都完成，也不能证明 Wallet 已刷新 |
| 手机卡面刷新 | 在 iPhone 上关闭并重新打开 Wallet，或重启后，亲眼确认目标卡片显示预期图片 | 不能从 Mac 上的预览推断 |
| 主题导出 / 再导入 | 新 `.passthm` 文件保存成功，重新导入后能核对各数字的预览 | 这是本机文件检查，不是手机兼容性验证 |
| 密码键盘主题刷入 | **Flash Passcode Theme** 报告成功 | 不能证明锁屏界面已经显示正确 |
| 手机主题刷新 | 按[主题指南](THEME-CREATOR.zh-CN.md#应用与记录结果)刷新后，实际检查手机上数字键盘 | 未查看手机时应填“未测试” |

使用 **成功 / 失败 / 部分成功 / 未测试 / 不适用** 五种状态。失败写具体阶段和短错误；部分成功说明“目标 3 张，报告完成 2 张，第 3 张失败”这种数量信息即可，不需要公开卡片标识。密码主题不需要钱包扫描，扫描一栏填“不适用”。

## 提交一条可用的记录

1. 复制[中英文反馈模板](compatibility-report-template.md)的任一版本，填写 iPhone 机型、完整 iOS / macOS 版本及 Mac 芯片。知道系统 build 时也可补充。
2. 填准确的 AirCard 版本和测试 commit；使用下载的 DMG 时填写 Release/下载来源，commit 不知道就写“未知”，不要用教程仓库的文档提交代替软件构建版本。
3. 记录从连接到手机实际刷新的每一步。未进行的步骤填“未测试”，失败后未继续的步骤也不要填“成功”。
4. 多卡操作分别记录目标数量、应用报告成功数量、手机确认刷新数量；主题记录 Target、语言、粗体，以及使用整图、单键还是导入包。
5. 如有错误，只提供相关的一两行短信息。公开前隐去卡号（包括尾号）、卡片标识、UDID、设备名称及包含个人用户名的本地路径；不需要完整原始日志。截图也要检查这些信息。
6. 程序行为问题提交给[上游 Issues](https://github.com/Mak5er/AirCard/issues)；指南修订可以在 [AirCard-Guide](https://github.com/BryceYuuu/AirCard-Guide) 发起文档 PR。填写或复制模板本身不会自动向任何仓库发送内容。

新记录应带可访问的依据，并说明是报告者实测、上游记录还是项目声明。自动化测试可验证解析或错误处理，不能替代具体手机的扫描、刷入和刷新测试。

## 源码与记录依据

- [docs/wallet-card-detection.md](../wallet-card-detection.md)：已验证环境、检测结果和测试边界。
- [AirCardApp.swift](../../AirCardApp.swift)：连接、扫描、刷入状态及主题目标设置。
- [aircard_backend.py](../../aircard_backend.py)：卡面与主题操作的结果处理。
- [上游 README](https://github.com/Mak5er/AirCard/blob/main/README.md)：项目支持范围声明和安装/刷新说明。
