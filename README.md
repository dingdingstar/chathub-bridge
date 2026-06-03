# Claude Bridge

> 在**手机 / 网页**上远程发起 Claude Code 编程任务，在你自己的电脑上执行 —— 实时同步执行进度、权限审批与问答回答，无需复制粘贴。

本仓库托管 **Claude Bridge** 桌面端（macOS / Windows）的发布包与自动更新元数据。

---

## 它是什么

```
ChatHub (手机/网页)  --①发送指令-->  Claude Bridge (本机中转·托盘)  -->  你的电脑 (Claude CLI 执行)
       ^                                                                          |
       +-------------------------②实时回传 进度 / 权限请求-------------------------+
```

- **ChatHub**：你在手机或网页里给 Agent 发编程指令、确认权限、看进度。
- **Claude Bridge**：常驻系统托盘的中转 App，把指令转给本机的 Claude Code CLI 执行，并把进度/权限请求实时回传。
- **你的电脑**：真正跑 `claude` 的地方，改动直接落在你本地仓库。

适合：在外用手机指挥家里/公司的电脑跑编程任务、远程 code review、让 Agent 自动改代码并提交。

---

## 下载

| 平台 | 安装包 |
|------|--------|
| **Windows (x64)** | [Claude-Bridge-Setup.exe](https://github.com/dingdingstar/chathub-bridge/releases/download/claude-bridge-latest/Claude-Bridge-Setup.exe) |
| **macOS (Apple Silicon / M 系列)** | [Claude-Bridge-arm64.dmg](https://github.com/dingdingstar/chathub-bridge/releases/download/claude-bridge-latest/Claude-Bridge-arm64.dmg) |
| **macOS (Intel)** | [Claude-Bridge-x64.dmg](https://github.com/dingdingstar/chathub-bridge/releases/download/claude-bridge-latest/Claude-Bridge-x64.dmg) |

也可以在 **ChatHub 网页 → 连接 Claude CLI** 页面一键下载并配对。

> 安装包**未做代码签名**：macOS 首次打开请右键「打开」；Windows 若弹 SmartScreen 点「更多信息 → 仍要运行」。安装后内置自动更新（electron-updater）。

---

## 前置依赖

1. **Claude Code CLI**（必需）—— Bridge 调用本机的 `claude` 执行任务。
   - Windows（PowerShell）：`irm https://claude.ai/install.ps1 | iex`
   - macOS / Linux：`curl -fsSL https://claude.ai/install.sh | bash`
   - 或 `npm install -g @anthropic-ai/claude-code`（需 Node.js 18+）
   - **安装后必须登录一次**：终端运行 `claude`，按提示用 Claude 账号授权或填写 API Key。验证 `claude -p "say hi"` 能正常回话即可。
2. **Git**（推荐）—— 提交/推送代码；Windows 建议装 [Git for Windows](https://git-scm.com/downloads/win)（同时启用 Claude Code 的 Bash 工具）。
3. **Node.js**：
   - **Windows / macOS 无需单独安装** —— Bridge 用 Electron 自带运行时跑内部脚本。
   - 仅当用 `npm` 方式装 Claude CLI 时才需要 Node.js 18+。

---

## 安装与配对

1. 下载并安装对应平台的 Claude Bridge，启动后常驻**系统托盘**（菜单栏）。
2. 打开 **ChatHub（手机或网页）→ 设置 → 连接 Claude / 生成配对码**。
3. 点击配对（或扫码）—— 通过 `claude-bridge://` 唤起本机 App 自动写入配置并连接。
4. 托盘状态变为「已连接」即配对成功。

配对成功后，Bridge 会自动在 `~/.claude/settings.json` 写入任务追踪 / 治理 / 审批相关的 Hook（用户级，对所有项目生效）。

---

## 使用

1. 在 ChatHub 选择**目标设备**和**仓库**（先在 Bridge 里注册本地仓库路径，格式 `别名:绝对路径`，每行一条）。
2. 发送任务描述，选择推送策略（新建分支 / 直接推 main / 仅对话）。
3. Bridge 在本机启动 `claude` 执行，实时回传**进度、文件改动、权限请求**。
4. 需要确认的危险操作（如删除文件）会弹**审批请求**到手机 / PiP 窗口，由你拍板。

---

## 自动更新

App 启动时及每 6 小时检查一次更新，发现新版自动后台下载，退出 / 点击托盘「重启安装」即完成升级。

---

## 常见问题

| 现象 | 原因 / 解决 |
|------|-------------|
| 任务报 **`claude 退出码 1`**，直接测 `claude -p "say hi"` 返回 **`403 Request not allowed`** | Claude CLI **未登录**。运行 `claude` 完成交互式登录；若仍 403，检查是否有残留的无效 `ANTHROPIC_API_KEY` 环境变量，或账号是否已开通 Claude Code 权限 / 地区是否受限。 |
| Windows 上 **`spawn node ENOENT`** 弹窗，或任务改了文件却不提交 | 旧版本依赖独立 Node.js 所致。**更新到最新版本**即可（新版用 Electron 自带运行时，无需安装 Node.js）。 |
| Windows 上看不到其他窗口的关闭按钮 | 旧版屏幕边缘状态光带遮挡，**更新到最新版本**已移除该效果。 |
| 配对后任务无反应 | 确认托盘显示「已连接」、目标仓库已在 Bridge 注册、`claude -p` 能正常回话。 |
| macOS 提示「已损坏 / 无法打开」 | 未签名应用：右键 →「打开」，或终端 `xattr -dr com.apple.quarantine "/Applications/Claude Bridge.app"`。 |

---

## 平台支持

| 平台 | 格式 | 自动更新 |
|------|------|---------|
| Windows 10/11 (x64) | NSIS 安装包 (.exe) | ✅ |
| macOS (Apple Silicon) | DMG | ✅ |
| macOS (Intel) | DMG | ✅ |

---

## 相关

- **ChatHub** —— 企业级即时通讯平台（私聊/群聊、平台集成、Agent 助手等），Claude Bridge 是其「远程 Claude Code」能力的本机执行端。
- 本仓库同时托管 **ChatHub Bridge**（macOS 菜单栏 App，将微信消息同步到 ChatHub）的发布包，见 Releases 中对应 tag。

---

> 隐私：Claude Bridge 仅在你的设备与 ChatHub 之间中转指令与执行结果，代码改动均发生在你本地仓库。配对令牌仅保存在本机配置中。
