# ChatHub Code Bridge

> 在**手机 / 网页**上远程发起并续聊 Claude Code 编程任务，在你自己的电脑上执行 —— 实时同步执行进度、工具详情、权限审批与问答回答，无需复制粘贴。

本仓库托管 **ChatHub Code Bridge** 桌面端（macOS / Windows）的发布包与自动更新元数据。

---

## 它是什么

```
ChatHub (手机/网页)  --①发指令 / 续聊-->  ChatHub Code Bridge (本机中转·托盘)  -->  你的电脑 (Claude CLI 执行)
       ^                                                                          |
       +----------------②实时回传 进度 / 工具详情 / 权限请求 / 会话全文----------------+
```

- **ChatHub**：你在手机或网页里给 Agent 发编程指令、确认权限、看进度，并能**监看与续聊本机上跑过的所有 Claude Code 会话**。
- **ChatHub Code Bridge**：常驻系统托盘的中转 App，把指令转给本机的 Claude Code CLI 执行，并把进度 / 工具调用 / 权限请求 / 会话内容实时回传。
- **你的电脑**：真正跑 `claude` 的地方，改动直接落在你本地仓库。

适合：在外用手机指挥家里 / 公司的电脑跑编程任务、远程 code review、随时翻看与接着聊桌面上没聊完的会话、让 Agent 自动改代码并提交。

---

## 核心能力

- **远程发任务**：选目标设备 + 本地仓库，发任务描述，选推送策略（新建分支 / 直接推 main / 仅对话），Bridge 在本机启动 `claude` 执行。
- **Session 监看 + 远程续聊**：自动扫描本机所有 Claude Code 会话，在手机 / 网页里查看完整对话全文（按需加载，不入库），对任意历史会话发新指令接着聊（`claude --resume`），续聊时可**切换模型**、发送图片 / 文件附件、用「/」快捷话术。
- **实时进度回传**：工具调用可展开详情（命令 + 执行输出、Edit 红绿 diff、思考过程折叠块、TodoWrite 清单卡片），并实时显示**模型 / token / 成本 / 耗时**。
- **权限审批，多端同步**：危险操作（删文件等）与 Checkpoint 弹审批到手机 / PiP 窗口由你拍板；多设备登录时任一端批复即全端同步，还可在一台设备上**代审批其他设备**的申请。
- **横向玻璃控制台 + 桌面宠物**（桌面端）：控制台统一快捷入口、任务日志、审批和外部通知；新事件只更新状态，不自动展开或抢焦点。桌面宠物可拖动、闲置打瞌睡。
- **刘海岛**（macOS）：三态贴合刘海展示任务与审批状态。
- **会话分享 / 重命名**：会话可重命名（回写桌面端 Claude），可分享给好友查看，授权后还能由对方续聊。
- **插件系统 + 远程插件市场**：在 Bridge 内搜索、安装、更新插件（如禅道 Bug 查询 MCP）。
- **治理规则**：内置预设规则包（危险操作防护 / 敏感信息保护 / 目录锁定），可阻断或非阻断提示。
- **抖音收藏本机解析**：安装包内置并校验 `yt-dlp`，优先快速解析单视频；失败时自动回退隐藏浏览器。Cookie 和签名只留在用户自己的电脑，作者作品分页仍由本机浏览器完成；作者作品的播放、点赞、评论和分享累计值会按用户与设备隔离保存在本机 SQLite，并形成每日采集快照。
- **同账号局域网组网**：云端只下发同一 ChatHub 账号的临时设备候选，Code Bridge 每分钟在 RFC1918 私网内双向探活；只有双方身份校验成功才建立 LAN 组，失败时继续使用原有云端通道。Cookie、签名和本地 SQLite 内容不会因探活上传。

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

1. 下载并安装对应平台的 ChatHub Code Bridge，启动后常驻**系统托盘**（菜单栏）。
2. 打开 **ChatHub（手机或网页）→ 设置 → 连接 Claude / 生成配对码**。
3. 点击配对（或扫码）—— 通过 `claude-bridge://` 唤起本机 App 自动写入配置并连接。
4. 托盘状态变为「已连接」即配对成功。

配对成功后，Bridge 会自动在 `~/.claude/settings.json` 写入任务追踪 / 治理 / 审批相关的 Hook（用户级，对所有项目生效）；本机跑过的会话也会被扫描并同步到 ChatHub。

---

## 使用

### 横向玻璃控制台（第一版）

控制台替代旧圆环、独立任务窗口和外部通知窗口。启动默认显示紧凑状态条，尊重已保存的隐藏偏好；从菜单栏“显示控制台”可恢复。

- 点击 **CB** 展开横向仪表盘：快捷窗口/看盘入口、任务阶段及总耗时、未读和运行计数。
- 点击 **任务** 或 **通知** 在同一个原生窗口展开详情。日志、权限问答、远程代审批、通知原始数据及币种跳转沿用现有通道；切换页签不重新加载，两页分别保留滚动位置。
- **收起** 回到状态条；**−** 隐藏整个控制台，菜单栏可恢复；Esc 收起，紧凑时 Esc 隐藏。拖动空白区域移动位置，多屏和详情高度按工作区约束。
- 新事件不打开或激活窗口、不自动切换页签。通知仅在通知页实际可见时标记已读；提醒偏好仍支持仅角标/合并系统提醒，旧版“自动窗口”偏好兼容为系统提醒。
- macOS 使用透明合成底板和原生 `vibrancy`，浅色/深色跟随系统；Windows 使用不透明底材保证可读性。原生材质效果以安装后的桌面实测为准，不等同于浏览器 CSS 预览。
- 只复用现有本机内存状态和消息，不新增云端查询或后台轮询；计时器仅在展开且可见时运行。旧圆环的扇形、淡化、悬停和性能测量设置暂不应用于新控制台，后续迭代。

### 全市场短周期放量收涨策略

在 **偏好设置 → 币安 → Code Bridge Strategy** 找到“Binance 全市场短周期放量收涨”。新策略默认停用，可先点“预览（不通知）”，再勾选启用并保存；只由股票数据主设备执行，不需要为每个币种创建 TradingView 警报。

- 默认检测 30 分钟 K 线，也可选 5 / 15 / 60 分钟。
- 本根成交数量 ÷ 前 7 个完整 UTC 交易日日均成交数量 ≥ 100%，且本根收盘价高于开盘价。历史天数、占比和最低涨幅可调；下跌、十字线、历史不足的币种跳过。
- 使用币安 K 线成交数量，不是 USDT 成交额；不沿用全天成交额策略的最低成交额过滤。
- 通知沿用策略通知中心，并可通过现有邀请流程分享给好友；同币种、同周期、同一根 K 线只提醒一次，重启或修改阈值也不重复。
- 采用现有统一扫描间隔，通知并非收盘瞬间送达。建议扫描间隔不大于检测周期（例如 5 分钟扫描、30 分钟检测）。每轮检查尚未扫描的已收盘 K 线，连续运行的失败重试最多回看 24 小时；启动和参数修改只检查最近两个扫描/检测窗口，不补发全部历史。
- 日 K 在当前进程按 UTC 日缓存，请求分批限速；网络失败会显示失败记录并下轮重试。主设备休眠、断网或程序退出时不能扫描，恢复后的长时间缺口不保证全部补发。

按用户明确确认，本策略例外采用本机统一扫描作为正式实现；此前 Pine 脚本仅作可选图表参考，不应同时启用相同规则的 Pine 警报。成交数量、收盘时间及成交额字段依据 [Binance Kline API](https://developers.binance.info/en/docs/catalog/core-trading-derivatives-trading-usd-s-m-futures/api/rest-api/market-data)。

### 发起新任务

1. 在 ChatHub 选择**目标设备**和**仓库**（在 Bridge 里用文件夹选择器注册本地仓库路径）。
2. 发送任务描述，选择推送策略（新建分支 / 直接推 main / 仅对话）。
3. Bridge 在本机启动 `claude` 执行，实时回传**进度、工具详情、文件改动、权限请求**。
4. 需要确认的危险操作会弹**审批请求**到手机 / PiP 窗口 / 刘海岛，由你拍板；多端登录时任一端批复即同步。

### 监看与续聊历史会话

1. 打开 ChatHub 的 **Claude / Session** 模块，按设备 → 项目筛选，搜索标题 / 命令 / 图片。
2. 点开任意会话查看完整对话全文（工具回合可展开详情）。
3. 在底部续聊框直接发新指令接着聊，可切模型、发附件 —— Bridge 用 `claude --resume` 在本机继续这条会话。

---

## 自动更新

App 启动时及每 6 小时检查一次更新，发现新版会通过 CDN 在后台下载。下载完成后只弹一次确认：点击「立即安装并重启」，App 会自动退出、完成替换并重新打开；选择稍后则在下次退出时自动安装。macOS 无需再次下载 DMG 或手动拖拽覆盖；只有自动安装真正失败时才提供 DMG 兜底。也可在托盘手动「检查更新」。

---

## 常见问题

| 现象 | 原因 / 解决 |
|------|-------------|
| 任务报 **`claude 退出码 1`**，直接测 `claude -p "say hi"` 返回 **`403 Request not allowed`** | Claude CLI **未登录**。运行 `claude` 完成交互式登录；若仍 403，检查是否有残留的无效 `ANTHROPIC_API_KEY` 环境变量，或账号是否已开通 Claude Code 权限 / 地区是否受限。 |
| Windows 上 **`spawn node ENOENT`** 弹窗，或任务改了文件却不提交 | 旧版本依赖独立 Node.js 所致。**更新到最新版本**即可（新版用 Electron 自带运行时，无需安装 Node.js）。 |
| Windows 更新时提示**「无法关闭」** / 看不到其他窗口的关闭按钮 | 旧版自动更新非静默 + 屏幕边缘状态光带遮挡，**更新到最新版本**已改为静默安装并移除光带。 |
| 配对后任务无反应 | 确认托盘显示「已连接」、目标仓库已在 Bridge 注册、`claude -p` 能正常回话。 |
| Apple 家庭扫码添加 HomeKit 桥接器提示**操作超时** | 在 macOS **系统设置 → 隐私与安全性 → 本地网络** 中允许 **ChatHub Code Bridge**；新版安装包会声明 `_hap._tcp` 服务并触发系统权限提示。 |
| 手机/网页里看不到本机会话，或会话内容不全 | 在托盘点**「重新扫描全部 session」**全量回填；会话全文为按需加载，首次打开稍候即可。 |
| 抖音收藏第一次解析较慢 | 首次使用或 Cookie 过期时会自动回退隐藏浏览器建立本机会话；后续单视频优先由内置 `yt-dlp` 快速解析，无需手动安装。 |
| macOS 提示「已损坏 / 无法打开」 | 未签名应用：右键 →「打开」，或终端 `xattr -dr com.apple.quarantine "/Applications/ChatHub Code Bridge.app"`。 |

---

## 平台支持

| 平台 | 格式 | 自动更新 |
|------|------|---------|
| Windows 10/11 (x64) | NSIS 安装包 (.exe) | ✅（静默） |
| macOS (Apple Silicon) | DMG | ✅ |
| macOS (Intel) | DMG | ✅ |

---

## 相关

- **ChatHub** —— 企业级即时通讯平台（私聊/群聊、平台集成、Agent 助手等），ChatHub Code Bridge 是其「远程 Claude Code」能力的本机执行端。
- 本仓库同时托管 **ChatHub Bridge**（macOS 菜单栏 App，将微信消息同步到 ChatHub）的发布包，见 Releases 中对应 tag。

---

> 隐私：ChatHub Code Bridge 仅在你的设备与 ChatHub 之间中转指令与执行结果，代码改动均发生在你本地仓库。配对令牌仅保存在本机配置中。
