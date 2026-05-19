# Task: 比较 EOA / 智能账户 / 多签账户 的权限差异

- **WCB Task ID**: `cmp9wlsoh0s4hmw01lck7hidd`
- **WCB Task Title**: Week 1｜Web3 向进阶｜比较 EOA、智能账户、多签的权限差异
- **Points**: 30
- **Submitted**: 2026-05-19（pending）
- **Handbook 关联章节**:
  - [web3/wallet](https://aiweb3.school/zh/handbook/web3/wallet/)
  - [web3/smart-contract](https://aiweb3.school/zh/handbook/web3/smart-contract/)
  - [web3/account-abstraction](https://aiweb3.school/zh/handbook/web3/account-abstraction/)
  - [bridge/agent-wallet](https://aiweb3.school/zh/handbook/bridge/agent-wallet/)

## 一句话总结

**EOA 是"硬编码"的账户类型（"谁有私钥谁说了算"）；智能账户和多签都是合约账户，区别在于"规则可编程化的程度"——智能账户解锁规则灵活性，多签把控制权拆分到多个签名人**。这三类账户的真实差异不在"是不是钱包"，而在**"谁能动钱、什么条件下、被谁监督"**。

## 三类账户定位

| | EOA | 智能账户（单签智能钱包） | 多签账户（multisig） |
|---|---|---|---|
| **本质** | 公私钥对 | 合约账户 + 1 个 owner key | 合约账户 + n 个 owner key（k-of-n 出币） |
| **代表实现** | MetaMask 默认账户 | Argent v2 / ZeroDev Kernel / ERC-4337 单签实现 | Gnosis Safe（最主流） |
| **是不是合约** | 否 | 是（部署的智能合约） | 是（部署的智能合约） |
| **核心特征** | "私钥 = 全权" | 规则可写代码定义 | 多人共管，少数人无法单独动钱 |

## 6 个维度的详细对比

| # | 维度 | EOA | 智能账户 | 多签账户 |
|---|---|---|---|---|
| 1 | 控制权 | 持有私钥的单个人 | 单个 owner（但 owner 可以是另一个合约/key） | k 个签名人协商（k≥2，如 2/3 Safe） |
| 2 | 安全模型 | **all-or-nothing**：私钥在 = 钱在，丢 = 全失 | 规则化：私钥泄漏可被 daily limit、白名单、social recovery 救 | 分布式：单点失陷不致命；攻击者需同时拿下 k 把钥匙 |
| 3 | 可编程边界 | 0（只能签名） | 高：daily limit、白名单、session key、social recovery、紧急暂停 | 中：通常以"k-of-n 阈值 + 提案审查"为核心规则 |
| 4 | Gas 模型 | 自己付 ETH | 可让 Paymaster 代付 / 用 USDC 付（gasless） | 通常每个签名人各自付 gas；执行人付最终上链 gas |
| 5 | 创建成本 | 几乎为 0（本地推导） | 一次部署 gas（数千-几万 gwei） | 一次部署 gas + 配置 owners + threshold |
| 6 | 恢复机制 | 仅靠助记词；丢了永久失去 | **Social recovery**：3 个朋友/守护人确认即换控制权 | 调整 owner set 需现有 k 人签名提案；阈值丢失即资产卡死 |

## 每类账户的典型适用场景 + 主风险

### 1. EOA
**适用场景**：
- 个人小额日常使用（链上 Twitter、Mint NFT、领 airdrop）
- 冷钱包（不联网，私钥纸抄，只签关键交易）
- 链上身份证明（personal_sign 登录）

**风险点**：
- 私钥/助记词一旦泄漏（钓鱼、设备失陷），资产瞬间归零
- **签名钓鱼**：用户对 EIP-712 / Permit / setApprovalForAll 类签名缺乏判断力，签了等于授权他人未来转走资产

### 2. 智能账户（单签智能钱包）
**适用场景**：
- 个人主用钱包，想要 daily limit / 紧急 freeze 等安全护栏
- Onboarding 用户（不想让用户记 12 个单词，用 social recovery 替代）
- **AI Agent 代管资产**：通过 session key 给 Agent 限时间 / 限金额 / 限合约白名单
- gasless 体验（让 Paymaster 代付 gas，用 USDC 等代币付）

**风险点**：
- **合约 bug**：钱包合约本身可能被审计漏掉的漏洞攻击（参见 Wintermute Profanity 攻击案例）
- **升级权限滥用**：如果合约设计为可升级，管理员密钥/治理被攻击 = 用户资产被改规则
- **owner key 仍然是单点**：虽然有 social recovery，但 active key 泄漏前 social recovery 来不及触发

### 3. 多签账户
**适用场景**：
- 团队/DAO 金库（如 Optimism 基金会的多签）
- 高净值个人冷藏（2/3 分布在硬件钱包 + 不同物理位置）
- **协议管理员密钥**（避免单点跑路）
- AI Agent 涉及大额时的"AI 提议 + 人审批"流程：Agent 占 1 票，人占 1 票，2/2 才出币

**风险点**：
- **协调成本**：每笔交易需多人在线签名，应急响应慢
- **阈值死锁**：如果 n=3, k=2，失去 2 个 key = 资产永久卡死
- **社工攻击**：攻击者只需说服 k 个签名人之一犯错（在大额签名前下钓鱼合约），就能放过审查
- **多签 ≠ 去中心化**：Safe 上的 3/5 多签，如果 5 个签名人都是同一公司的人，本质还是中心化

## 哪些操作必须人工确认（重点）

无论用哪类账户，以下三类操作**必须人工确认**，AI Agent / 自动化流程不能直接执行：

1. **任何消耗 gas 的链上交易**（转账 / 合约调用 / approve / mint / swap）——上链不可撤回
2. **任何签名请求**（personal_sign / EIP-712 / Permit / SeaPort / setApprovalForAll）——签名不上链不花 gas，但 EIP-712 类签名可授权他人转走资产，**风险经常比直接发交易更高**（钱包 UI 把"签名"显示得轻量，是 Web3 onboarding 一个长期没解的产品 bug）
3. **钱包连接到新域名**——一字之差就是钓鱼，是后续所有动作的入口

针对三类账户的额外规则：

- **EOA**：上面三条铁律必须人工确认；助记词只纸抄、永不输入任何网页
- **智能账户**：session key 的"超出边界"操作会被合约自动 revert；但**配置 session key 本身（限额、白名单、有效期）必须人工确认**
- **多签**：每个签名人都必须独立验证提案内容（包括 to 地址、value、calldata），不能因为"已经有别人签了"就盲签

## AI Agent 代管资产：为什么几乎只能选合约账户

**EOA 路线**：把私钥给 Agent = Agent 等于你本人，跑路 / 被攻击 = 血本无归。无任何边界。

**智能账户 / 多签路线**：
- 给 Agent 一把 **session key**（仅限"调 Uniswap swap 函数 + 单笔金额 < 100 USDC + 24 小时内有效"）
- Agent 最多在你定的边界里乱来，**超出边界合约自己 revert**
- 大额操作走多签，Agent 占 1 票 + 你占 1 票，2/2 才放行

**这就是为什么 [bridge/agent-wallet](https://aiweb3.school/zh/handbook/bridge/agent-wallet/) 章节存在**——AI × Web3 的真实落地，**离不开账户抽象（Account Abstraction）这一层基础**。EOA 是 2015 年的硬编码账户类型，规则只有一条；智能账户/多签把"账户的规则本身"变成可编程，才让 Agent 代管有了安全边界。

## 一张图概括

```
   攻击面集中度
       高
        │
        │   EOA：1 个私钥控制一切
        │     (all-or-nothing)
        │
        │   智能账户：1 个 active key + 多层护栏
        │     (可编程边界 + social recovery)
        │
        │   多签账户：k-of-n 阈值
        │     (单点失陷不致命)
        ▼
       低
        │
        │  AI Agent 代管资产的安全边界
        │  → 必须选合约账户（智能账户或多签）
```

## 我的学习路径关联

这一篇的内容是 0519 晚上跟 Claude Code 4 轮 Q&A 的产物，重点修正了 3 个认知错误：

1. "EVM 基于智能合约" → 反了，**智能合约跑在 EVM 上**，EVM 是执行环境
2. "EOA vs EVM 哪个安全" → 比错对象，EOA 是账户类型，EVM 是执行环境。正确对比是 EOA vs 合约账户
3. "合约账户没私钥所以不安全" → 分两种角色看：作为应用（Uniswap）和作为钱包（Safe），后者反而比 EOA 安全

详细 Q&A 过程见 [`daily/2026-05-19.md`](../daily/2026-05-19.md)。
