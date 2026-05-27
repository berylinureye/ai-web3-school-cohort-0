# Task: Week 1 Proof-of-Work Pack

- **WCB Task ID**: `cmp3jyrjn07skn301qopx9rwe`
- **WCB Task Title**: Week 1｜AI × Web3 综合任务｜提交 Week 1 Proof-of-Work Pack
- **Points**: 40
- **Submitted**: 待提交
- **Handbook 关联章节**:
  - [AI Handbook](https://aiweb3.school/zh/handbook/ai/llm/)
  - [Web3 Handbook](https://aiweb3.school/zh/handbook/web3/wallet/)
  - [开发栈（Dev Stack）](https://aiweb3.school/zh/handbook/web3/dev-stack/)

## 一句话总结

Week 1 的核心收获是：AI x Web3 不是“AI 帮我点钱包”，而是要把 AI 的理解 / 生成能力和 Web3 的签名 / 权限 / 链上验证机制拆开，明确哪些步骤可以自动化，哪些步骤必须人工确认。

## 总入口

- GitHub repo: https://github.com/berylinureye/ai-web3-school-cohort-0
- 本 Pack 文件: `tasks/week1-proof-of-work-pack.md`
- Week 1 主线：AI 概念 → Web3 钱包 / 合约 → Learning Agent → 测试网实操 → AI x Web3 安全边界流程图

## Week 1 完成材料汇总

### 1. AI 学习记录 / 概念卡片

- [`tasks/week1-ai-concepts-cards.md`](./week1-ai-concepts-cards.md)

覆盖概念：

- LLM
- Prompt
- Context Window
- Workflow
- Agent
- Tool Use
- MCP
- RAG
- Fine-tuning
- AI Coding / Vibe Coding

核心理解：

> Prompt、Context、Agent、Tool Use 不是“AI 魔法词”，而是 AI 系统的行为边界。Agent 越能调用工具，越需要明确权限、上下文和停止条件。

### 2. Learning Agent / AI 工具实践

- [`tasks/week1-learning-agent-setup.md`](./week1-learning-agent-setup.md)
- [`skills/hermes-mobile-agent.md`](../skills/hermes-mobile-agent.md)

我搭建了双通道学习工作流：

| 通道 | 用途 | 边界 |
|---|---|---|
| Claude Code / Codex | 深度学习、写文件、整理 proof、commit / push | 主力沉淀面 |
| Hermes / 微信 | 路上提问、轻量查询、记到 daily buffer | 不提交任务、不 commit、不做链上操作 |

关键人工修正：

- 早期 Agent 只按 Handbook 自排学习计划，偏离 WCB 真实任务。
- 后来接入 WCB API，改为“课程任务 + Handbook + GitHub proof”三线对齐。
- API key 泄漏后，Agent 引导 revoke，不继续使用泄漏 key。

### 3. Web3 概念卡片 / 钱包实操

- [`tasks/week1-web3-concepts-cards.md`](./week1-web3-concepts-cards.md)
- [`tasks/web3-minimum-practice.md`](./web3-minimum-practice.md)

已理解概念：

- EOA / 钱包 / 助记词 / 私钥
- 签名 / 交易 / Gas
- 智能合约 / 合约账户 / EVM
- 测试网 / 主网
- approve / 授权风险

核心洞察：

> 钱包不是存钱的地方，而是私钥管理器和签名入口。ERC-20 token 不存在钱包里，而存在代币合约 storage 的余额表里。

### 4. 链上验证材料

#### Sepolia 测试网交易

- tx hash: `0xdfc1b13fa2dc242044c41d838fba360de7abe090f1223426e21724f4236a3467`
- 记录文件: [`tasks/web3-minimum-practice.md`](./web3-minimum-practice.md)

#### WETH9 合约交互

- WETH9 Sepolia 合约: `0xfFf9976782d46CC05630D1f6eBAb18b2324d6B14`
- wrap tx: `0x72a6c0e85c15fb2067f6218337989a32285465cee7cb26e6519155b515fc7a68`
- 记录文件: [`tasks/week1-deploy-or-call-contract.md`](./week1-deploy-or-call-contract.md)

#### 自部署 Counter 合约

- Counter 合约地址: `0x736b9278e7fa24e49C5de9b2bC96A6eb6EF19c73`
- 部署 tx: `0xed708a1557cbce1a93ff093bd3a6d60bb351cfe874a63288f12876870e84675a`
- 记录文件: [`tasks/week1-deploy-or-call-contract.md`](./week1-deploy-or-call-contract.md)

核心理解：

> 部署合约不是“上传文件”，而是发一笔创建合约账户的交易。合约地址、ABI、部署网络和 tx hash 都是后续前端调用和链上验证的基础。

### 5. AI x Web3 最小交叉流程图

- [`tasks/week1-ai-web3-flow.md`](./week1-ai-web3-flow.md)

流程图主题：

> AI Agent 调用 Web3 合约的安全边界流程。

核心边界：

- AI 可以解释意图、读取 ABI、生成交易草稿和总结链上证据。
- 用户必须人工确认钱包连接、网络切换、签名、交易、授权和 owner / admin 操作。
- 最终结果必须用 tx hash、event 和状态读取验证。

### 6. 本周遇到的问题和人工修正

| 问题 / 卡点 | 初始误解 | 修正后理解 |
|---|---|---|
| EVM 和智能合约关系 | 以为 EVM 基于智能合约 | 智能合约跑在 EVM 上，EVM 是执行环境 |
| ERC-20 token 位置 | 以为 token 存在钱包里 | ERC-20 余额存在合约 storage，钱包只是钥匙和显示器 |
| ABI 作用 | 以为 ABI 只是给前端“看懂一点” | ABI 是链外工具调用合约的翻译字典 |
| viem / wagmi | 以为 viem 是写合约工具 | viem 是读写链工具，wagmi 是 React 前端状态管理 |
| 学习计划 | 一开始只跟 Handbook | 后来改为 WCB 任务、Handbook、GitHub proof 三线对齐 |

## Week 1 总结

Week 1 最大的认知变化是：我开始把 AI Agent 和 Web3 钱包之间的关系看成一条“权限边界链”，而不是一个简单的自动化流程。

如果让 Agent 直接拿 EOA 私钥，本质上就是把全部资产控制权交给模型，这是不可接受的。更合理的方向是智能账户、多签、session key、预算限制、合约白名单和人工确认。

这也决定了我 Week 2 的主方向：**Wallet / Permission / Safe Execution**。

## 下一步

- Week 2 问题地图与主方向选择：[`tasks/week2-problem-map-direction.md`](./week2-problem-map-direction.md)
- Agent 链上动作权限策略：[`tasks/week2-agent-permission-strategy.md`](./week2-agent-permission-strategy.md)
- 后续 proposal 会围绕 Agent Wallet / 受限 Web3 助手继续推进。

## AI 辅助说明

本 Pack 由 AI 根据我的 Week 1 task 文件、daily notes、链上交易记录和 WCB proof 要求整理。我人工复核了所有公开链接、tx hash、合约地址和安全边界，未包含私钥、助记词、API key、token、`.env` 或任何敏感信息。
