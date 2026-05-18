# Task: Web3 最小实践 - 钱包交互地图

- **Task ID:** web3-minimum-practice
- **Due Date:** 2026-05-18
- **Handbook Chapter:** Web3 / Smart Contract / 最小实践

## Analysis & Approach

- 目标：用测试钱包跑一遍 dApp 的完整交互流程，画出"钱包交互地图"，把每一步是读还是写、上不上链、用户该看什么、AI Agent 是否需要人工确认都标清楚。
- 不写代码、不部署合约，重点是亲手走一遍 Web3 的"用户主路径"，把模糊的链上动作具象化。
- 环境组合：Chrome + MetaMask + Sepolia 测试网。从 0 开始装钱包，全程不花真钱。
- dApp 选择：
  - Google Cloud Sepolia Faucet（领测试币）
  - metamask.github.io/test-dapp/（连接 + 签名，MetaMask 官方演示）
  - eth-sepolia.blockscout.com（Explorer 查 tx，国内可访问）
- 关键纪律：助记词纸抄离线，永远不输入任何网页；测试网练手只是为了在主网之前建立肌肉记忆。

## Solution / Output

### 测试环境

- 钱包：MetaMask v12（Chrome 侧边栏模式）
- 网络：Sepolia（Chain ID 11155111）
- Account 1（主操作账户）：`0xb3b69AFAc4Eb4723fe62FDD6859D3a3C2EF23983`
- Account 2（同助记词派生）：`0xC8345F4758BD6e872E008A1fb663E69b02680994`

### 动作分类表

| # | 动作 | 读/写 | 上链 | 消耗 gas | 用户必看的关键信息 | AI Agent 是否必须人工确认 |
|---|---|---|---|---|---|---|
| 1 | 安装 MetaMask 扩展 | 读 | 否 | 否 | Chrome Web Store Publisher = MetaMask | 是（防仿冒扩展） |
| 2 | 创建钱包 + 抄助记词 | 读 | 否 | 否 | 12 个英文单词、顺序、必须纸抄离线 | 必须（涉及私钥） |
| 3 | 设置本机解锁密码 | 读 | 否 | 否 | 仅解锁本机，不等于助记词 | 否（仅本机） |
| 4 | 切换到 Sepolia 测试网 | 读 | 否 | 否 | 网络名 / Chain ID 11155111 | 否（本地动作） |
| 5 | 连接 Google Cloud Faucet | 读 | 否 | 否 | 网站域名、权限范围 | 是（防钓鱼域名） |
| 6 | Faucet 转入 0.05 SepoliaETH | 写（对方发起） | 是 | 否（faucet 付） | 余额变化、到账 tx hash | 否（被动接收） |
| 7 | 连接 metamask.github.io/test-dapp/ | 读 | 否 | 否 | 域名、Connected Account 显示我的地址 | 是（防钓鱼） |
| 8 | Personal Sign 一段消息 | 链下密码学 | 否 | 否 | 消息原文、无 gas、无金额字段 | 必须（签名等同身份/授权） |
| 9 | 用 VERIFY 反推签名地址 | 纯本地数学 | 否 | 否 | 反推地址 = Account 1 ✓ | 否 |
| 10 | 创建 Account 2 | 读（本地推导） | 否 | 否 | 新地址独立但同助记词派生 | 是（用途分离） |
| 11 | 发送 0.01 SepoliaETH Account 1 → Account 2 | 写 | 是 | 是 | From / To / Value / Gas / 网络 | 必须（不可撤销） |
| 12 | 在 Blockscout 查这笔 tx | 读 | 否 | 否 | Status、Block、Confirmations | 否 |

### 关键产物

- Faucet 入账 Tx Hash：`0x212cbc548f00d74ff879e103a40730c654babc3b…`
- Personal Sign 签名值：`0xe9ae88b222be463b094189bb933c52b102d48763c405b9a01f4aaf37447b732d2ace19b1e818dc3d95d0262a0fdb824bcf62de614f11bc0d5fc5f044bd22c58b1b`
- 发送 Tx Hash：`0xdfc1b13fa2dc242044c41d838fba360de7abe090f1223426e21724f4236a3467`
- 区块高度：10874091（23 Block confirmations）
- 实际 gas 费：0.00003644744943 ETH
- 销毁部分（EIP-1559 Burn）：0.00000494744943 ETH

### AI Agent 红线汇总

必须人工二次确认的三类动作：

1. 任何会消耗 gas 的交易（转账、合约调用、approve、铸造、swap）—— 广播即不可撤回。
2. 任何签名请求（即使没有 gas）—— EIP-712 / Permit / SeaPort 等链下签名可授权他人转走资产，比直接发交易更隐蔽。
3. 钱包连接到新域名 —— 一字之差就是钓鱼，是后续所有动作的入口。

可以放心交给 AI Agent 的部分：

- 读余额、读交易历史、读合约状态
- 切换网络（仅切换，不发交易）
- 整理 Explorer 信息、解释 calldata、估算 gas
- 起草交易（但提交前必须由用户在钱包弹窗里看到真实参数后亲手点 Confirm）

## Learnings

> 以下五条都是草稿版，基于今天实际跑的过程写的。需要本人按真实体感改写、删掉不准的、补上自己的话。

### 1. 实际操作和文档不一样的地方（草稿）

文档把"切换网络"说成最简单的本地动作，实际上 MetaMask 在 Chrome 侧边栏模式下有三个入口（弹窗 / 侧边栏 / app.metamask.io 网页版）+ 两种网络列表（管理页 / 切换器），新手 30% 概率会卡半小时。
Etherscan 的 Sign Message 工具在我的浏览器里点不动按钮（侧边栏模式抢焦点），换到 metamask.github.io/test-dapp/ 才走通。
查交易时 Etherscan 在国内访问 ERR_TUNNEL_CONNECTION_FAILED，切到 Blockscout 立刻通了 —— 链上数据公开 ≠ 我能随时访问它，去中心化是协议层的，接入层依然中心化。

### 2. 我以为是"读"、其实等价于"写/授权"的动作（草稿）

签名消息看起来只是"用钥匙画个圈"，但 EIP-712 / Permit 类签名实际等同于授权对方未来转走资产 —— 它不上链、不花 gas，却比直接发 tx 更危险。
领水龙头看起来是"我在收钱"，但 faucet 那笔 tx 也是真正写进了 Sepolia 区块，全网公开，我的地址从此在链上"存在了"。
"创建 Account 2"看起来是新建账户，实际只是从同一份助记词推导出另一个地址 —— 一份助记词管整棵树。

### 3. 如果 AI Agent 替我跑这套，最不放心的一步（草稿）

最不放心 Step 8 / Step 11 —— 签名 + 发交易。原因不是 Agent 操作慢，而是这两步的"业务含义"是 Agent 没法替我判断对错的：

- 签名内容里一个微小授权（"允许 spender 转走我 100 USDC"）AI 没法判断这是不是钓鱼合约。
- 交易里 To 地址改一个字符 ETH 永久丢失，AI 没法替我看出"这个地址不是我朋友的地址"。

弹窗的"看清每一项"必须人来做。Agent 可以辅助解读、估 gas、起草参数，但点 Confirm 必须是我。

### 4. CeFi vs Self-custody 的安全本质（草稿）

币安本质是"用区块链做后端的银行" —— CeFi。它保护我"不出错"，代价是它能冻我账户、跑路时拿走我的钱。
MetaMask 是真 Web3 —— Self-custody。它保护我"不被第三方背叛"，代价是助记词丢了就全没。
这次实践之后我理解：持有加密货币 ≠ 用 Web3。只有亲手通过钱包跟 dApp 交互（连接、签名、发交易），我才算真正进过 Web3 的"国土"，而不是只蹲在它的"换钞处"。

### 5. Web3 安全模型的核心错觉（草稿）

以前我以为"安全 = 藏起来"。这次理解到 Web3 的安全是反过来的：

- 地址公开（必须）
- 历史公开（必须）
- 签名可被全网验证（这恰恰是它替代密码的原因）
- 唯一私密的是私钥

Web3 的隐私威胁模型跟 Web2 完全不同：不是"密码被偷"，而是"地址被关联到真实身份"或者"私钥被骗签危险授权"。
