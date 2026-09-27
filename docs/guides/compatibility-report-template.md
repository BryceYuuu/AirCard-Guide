# 兼容性反馈模板 / Compatibility report template

[中文指南](README.zh-CN.md) · [English guide](README.en.md) · [兼容性记录](COMPATIBILITY.zh-CN.md) · [Compatibility records](COMPATIBILITY.en.md)

复制下面任一版本，在实际测试后填写。状态使用：**成功 / 失败 / 部分成功 / 未测试 / 不适用**。不确定版本或 commit 时写“未知”，未进行的步骤写“未测试”。不要把教程文档版本当作 AirCard 程序版本。这里没有表单提交功能。

Copy either version and fill it in after testing. Use **Successful / Failed / Partially successful / Not tested / Not applicable**. Write “unknown” for unknown versions/commits and “not tested” for steps not performed. The guide's documentation version is not the AirCard application version. This page does not submit a report.

只保留与当前问题有关的短错误，隐去卡号（包括尾号）、卡片标识、UDID、设备名称和个人路径；不要粘贴完整设备日志。可附已遮盖敏感信息的截图，但不要求截图或完整日志。

Include only short relevant errors. Redact card numbers (including last digits), card identifiers, UDID, device names, and personal paths. Do not paste full device logs. Redacted screenshots are optional; screenshots and full logs are not required.

## 中文模板

```text
标题：[钱包卡面 / 密码键盘主题 / 本机导出] 机型 + iOS 版本 + 出错阶段或结果

测试日期：
结果来源：本人实测 / 引用他人记录（附公开链接）

环境
- iPhone 机型（不填序列号或 UDID）：
- iOS 完整版本（build 可选）：
- Mac 机型 / 芯片（Apple Silicon 或 Intel）：
- macOS 完整版本：
- AirCard 版本：
- AirCard commit（未知则写未知）：
- 安装来源 / Release 链接（DMG 或源码构建）：
- USB 连接、已解锁及信任状态：

分阶段结果（成功 / 失败 / 部分成功 / 未测试 / 不适用）
- 连接：
- 钱包扫描：
- 钱包卡面刷入：
- 手机实际显示新卡面：
- 主题导出并重新导入（仅本机检查）：
- 密码键盘主题刷入：
- 手机实际显示新主题：

如果测试钱包卡面
- 目标卡片数量 / 刷入报告完成数量 / 手机上确认刷新数量：
- 单卡或多卡；使用同一张图或分别配图：
- 图片格式与像素尺寸（不含卡片私人信息）：
- 刷新动作及结果（关闭重开 Wallet / 重启 / 未尝试）：

如果测试密码键盘主题
- 来源（Poster Slice / Individual Keys / 导入主题；可选公开链接）：
- 配置的数字数量（不包含个人密码）：
- Slicing Style（适用时）：
- 手机系统语言 / 粗体文本是否开启：
- AirCard 的 Target / System Language / Font Weight 选择：
- 导入后是否复核 Target 或使用 Auto-detect：
- 重启后观察结果（未观察则写未测试）：

最短复现步骤
1.
2.
3.

预期结果：
实际结果 / 首个失败阶段：
一两行已脱敏错误（没有则写无）：
尝试过的处理及结果：
补充证据链接或已脱敏截图（可选）：
```

## English template

```text
Title: [Wallet artwork / Passcode theme / Local export] Model + iOS version + failing stage or result

Test date:
Evidence source: Tested by me / Quoting another report (public link)

Environment
- iPhone model (no serial number or UDID):
- Full iOS version (build optional):
- Mac model / chip (Apple Silicon or Intel):
- Full macOS version:
- AirCard version:
- AirCard commit (write unknown if unknown):
- Installation source / Release link (DMG or source build):
- USB connection, unlock, and trust status:

Stage results (Successful / Failed / Partially successful / Not tested / Not applicable)
- Connection:
- Wallet scanning:
- Wallet artwork flashing:
- New artwork actually displayed on phone:
- Theme export and re-import (local file check only):
- Passcode-theme flashing:
- New theme actually displayed on phone:

For Wallet artwork
- Target cards / Cards reported complete / Cards visually verified on phone:
- Single card or batch; shared image or separate images:
- Image format and pixel dimensions (no private card information):
- Refresh action and result (reopen Wallet / restart / not attempted):

For passcode themes
- Source (Poster Slice / Individual Keys / imported theme; optional public link):
- Number of configured digits (do not share your passcode):
- Slicing Style, if applicable:
- Phone system language / Bold Text setting:
- AirCard Target / System Language / Font Weight selections:
- Was Target checked or Auto-detect used after importing?
- Observation after restart (not tested if not observed):

Minimal reproduction steps
1.
2.
3.

Expected result:
Actual result / First failing stage:
One or two redacted error lines (none if absent):
Workarounds attempted and their results:
Additional evidence link or redacted screenshot (optional):
```

应用问题可将填好的内容提交至[上游 Issues](https://github.com/Mak5er/AirCard/issues)。源码依据：[连接与界面阶段](../../AirCardApp.swift)、[后端操作](../../aircard_backend.py)、[现有实机扫描记录](../wallet-card-detection.md)。

For application issues, submit the completed report to [upstream Issues](https://github.com/Mak5er/AirCard/issues). Source references: [connection and UI stages](../../AirCardApp.swift), [backend operations](../../aircard_backend.py), and [existing device scanning record](../wallet-card-detection.md).
