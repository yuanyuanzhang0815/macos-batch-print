# macos-batch-print

## 目标

macOS 原生批量打印小工具（SwiftUI，单文件实现）的**公开发布仓库**。把一堆文件按同一套
规则缩放排版后一次性发到打印机，为「报销凭证批量打印」这类场景写的。

GitHub：https://github.com/yuanyuanzhang0815/macos-batch-print （公开，MIT）

## 状态

- **v1.3 已发布**，DMG / ZIP 挂在 GitHub Release 上
- 发布仓库的源码是本地私有开发仓库 `~/Desktop/CLAUDE/print-tools` 的快照（干净历史）
- 两个仓库的 `src/App.swift` / `build.sh` / `README.md` 应保持同步：
  **开发在 print-tools 做，发布时把改动拷到这里**

## 关键事实与决策

- **本仓库是发布快照，不是开发仓库**。开发、调试、无头自检都在 `print-tools` 做；
  那边有完整的 `design/` 设计稿与历史。这里只放对外需要的东西。
- **历史是干净的（单次 initial commit）**：因为 `print-tools` 的历史里含真实票据截图的
  二进制，一旦推到公开仓库就撤不回来。发布仓库因此另起历史。
- **截图必须是示例数据**。`screenshots/` 里的内容全部由脚本生成（示例发票 / SAMPLE
  行程单 / 假付款记录），不能放任何真实票据：里面有姓名、票号、证件号段、订单号、税号。
  重拍前用 `BATCHPRINT_PRINTER=` 覆盖打印机名，否则会带出本机设备的型号+序列号。
- **构建脚本自动探测 SDK**（`xcrun --show-sdk-path`，失败再退回
  `/Library/Developer/CommandLineTools/SDKs/MacOSX26*.sdk`），不再写死路径。
- **只支持 macOS 26+ / Apple Silicon**，因为用了 `glassEffect` / `containerBackground`。
  用户明确说 Windows 先不管。
- **ad-hoc 签名、不公证**：公证需要付费 Apple 开发者账号。README 里写清了首次打开的
  两种放行方式（右键 Open / `xattr -dr com.apple.quarantine`）。
- 产物命名用 ASCII（`BatchPrint-1.3.dmg`），避免下载 URL 里的中文编码问题；
  app bundle 名字仍保留中文 `批量打印工具.app`。

## 文件清单

- `src/App.swift` → 全部实现（单文件）
- `build.sh` → 构建 + DMG/ZIP 打包 + ad-hoc 签名（`--install` / `--package`）
- `assets/AppIcon.icns` → 应用图标
- `screenshots/` → 六态真机快照（示例数据），README 与 Release 引用
- `README.md` → 英文简介（对外）+ 中文开发文档
- `LICENSE` → MIT
- `tools/scalepdf.swift` → 早期 CLI 原型
- `legacy/AppKit-v1.swift.bak` → 最早的 AppKit 版本

## 待办

- [ ] `print-tools` 里后续的开发改动，需要手动同步到这里再发版
- [ ] 如果以后拿到开发者账号，可以做公证 + 把 Gatekeeper 说明删掉
- [ ] 有人反馈 macOS 26 之前的版本也想用时，再评估要不要降 API