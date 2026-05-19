# Onboarding 接入层摩擦 · 3 处

> 来源：2026-05-18 钱包最小实操（MetaMask + Sepolia + test-dapp + Blockscout）
> 反馈人：@berylinureye
> 类型：UX 友好度 / 教程同步性 / 区域可访问性

## 1. MetaMask 侧边栏模式下，test-dapp 签名按钮无响应
- **页面**：https://metamask.github.io/test-dapp/
- **现象**：浏览器把 MetaMask 设为侧边栏（sidebar）模式时，Personal Sign 按钮点击无反应；切回弹窗模式后正常
- **影响**：新用户第一次跑签名 demo 就被卡住，会以为是钱包坏了
- **建议**：Handbook 的"钱包最小实践"章节在签名步骤前，加一行 **"如果按钮点不动，请把 MetaMask 切到 popup 模式"** 的预警
- **相关章节**：https://aiweb3.school/zh/handbook/web3/wallet/

## 2. MetaMask 网络切换 UI 入口分散，新用户找不到 Sepolia
- **场景**：从主网切到 Sepolia 测试网
- **现象**：MetaMask 当前版本默认隐藏测试网，需要进 Settings → Advanced → Show test networks 开启；很多教程截图还是旧 UI，新人对不上
- **影响**：领水之前就被卡 5-10 分钟，体验断点
- **建议**：Handbook 在 "Sepolia 领水" 之前，加一段**附带 2026 版 MetaMask 的截图或视频**说明如何开启测试网显示
- **相关章节**：https://aiweb3.school/zh/handbook/web3/network/

## 3. Etherscan 国内访问不稳定，建议给"备用 Explorer"
- **现象**：Etherscan.io 在大陆网络下经常加载失败或超时
- **当前 workaround**：用 Blockscout 的 Sepolia 实例作为备用，访问稳定
- **影响**：学员完成首笔 tx 后无法验证，闭环断在最后一步
- **建议**：Handbook 在所有出现 Etherscan 链接的地方，**同时给出 Blockscout 等备用 Explorer 链接**，避免国内学员卡在"看不到我的 tx"
- **相关章节**：https://aiweb3.school/zh/handbook/web3/network/ 及任何提到 Etherscan 的位置

---

## 元层观察（可选参考）

这 3 个卡点都不在"协议层"——它们都是**接入层中心化基础设施的真实代价**。Web3 的"去中心化"是协议层的，接入层依然依赖：
- 中心化的浏览器扩展（MetaMask 的 UI 决策）
- 中心化的水龙头（Google Cloud / Alchemy 等）
- 中心化的区块浏览器（Etherscan 的可用性）

这对 Handbook 的内容策略可能有启发：**把"接入层摩擦"作为一个独立章节专门讲**，而不是分散在各个工具教程里。学员一旦理解这是 Web3 onboarding 的结构性问题，就不会把每次卡顿归因为"我笨"或"教程烂"。
