# Task: 发布 AI x Web3 学习总结

- **WCB Task ID**: `cmp3jyrq307smn301bmk31xc2`
- **WCB Task Title**: Week 1｜发布 AI × Web3 学习总结
- **Points**: 20
- **Submitted**: 待提交
- **Handbook 关联章节**:
  - [AI Handbook](https://aiweb3.school/zh/handbook/ai/llm/)
  - [钱包（Wallet）](https://aiweb3.school/zh/handbook/web3/wallet/)
  - [智能合约（Smart Contract）](https://aiweb3.school/zh/handbook/web3/smart-contract/)
  - [开发栈（Dev Stack）](https://aiweb3.school/zh/handbook/web3/dev-stack/)

## 一句话总结

Week 1 我最大的收获是：AI x Web3 的关键不是“让 AI 自动点钱包”，而是设计一条边界清楚、可人工确认、可链上验证的执行链路。

## 可公开发布版本草稿

标题建议：

> AI Agent 不能直接拿 EOA 私钥：我在 AI x Web3 School Week 1 学到的第一条安全边界

正文草稿：

这周我在 AI x Web3 School 的学习里，第一次把 AI Agent 和 Web3 钱包放到同一张图里看。

一开始我以为 Web3 重点是“钱包、签名、Gas、智能合约”这些术语，AI 重点是“LLM、Prompt、Agent、Tool Use”。但真正把测试网交易、WETH9 调用和 Counter 合约部署跑过一遍后，我发现两边交叉处最重要的问题是：**Agent 到底能不能替用户发起链上动作？**

我的结论是：可以准备动作，但不能静默执行资产动作。

### 1. 一个重新理解的 AI 概念：Agent

我之前把 Agent 理解成“更聪明的聊天机器人”。现在更准确的理解是：Agent 是能根据目标选择下一步行动、调用工具并根据结果继续推进的系统。

这也意味着 Agent 的风险比普通 LLM 更高。普通 LLM 答错，最多是一段文本错；Agent 调错工具，可能删文件、泄露 secret、发错交易或签错授权。

### 2. 一个重新理解的 Web3 概念：钱包

我之前容易把钱包理解成“存资产的地方”。现在更准确的模型是：

> 钱包是私钥管理器和签名入口，不是 ERC-20 token 的存储位置。

ERC-20 token 的余额存在代币合约的 storage 里。钱包只是用我的地址去读 `balanceOf(address)`，并在我发起转账或授权时帮我签名。

这个理解很重要，因为如果 Agent 拿到 EOA 私钥，它就不是“帮我操作钱包”，而是拿到了完整资产控制权。

### 3. 一个 AI x Web3 交叉问题：哪些动作必须人工确认？

Week 1 我画了一张最小流程图：AI Agent 调用 Web3 合约的安全边界。

我把一次链上动作拆成：

```text
用户目标
  -> AI 解释意图
  -> 读取合约地址 / ABI / 网络
  -> 检查钱包连接和网络匹配
  -> 读取链上状态
  -> 生成交易草稿
  -> 前端展示风险摘要
  -> 用户钱包人工签名
  -> 交易广播
  -> 区块浏览器 / event 验证
  -> AI 总结 proof
```

这条链路里，AI 可以解释 ABI、生成交易草稿、总结 tx hash 和 event，但不能静默签名、静默 approve、隐藏网络不匹配或读取私钥/API key。

### 4. 我的 proof-of-work

这周我完成了：

- AI 基础概念卡片
- Web3 基础概念卡片
- Learning Agent setup
- Sepolia 测试网交易
- WETH9 `deposit()` 调用
- 自部署 Counter 合约
- EOA / 智能账户 / 多签账户权限比较
- AI x Web3 最小交叉流程图

关键链上记录：

- Counter 合约地址：`0x736b9278e7fa24e49C5de9b2bC96A6eb6EF19c73`
- Counter 部署 tx：`0xed708a1557cbce1a93ff093bd3a6d60bb351cfe874a63288f12876870e84675a`
- WETH9 wrap tx：`0x72a6c0e85c15fb2067f6218337989a32285465cee7cb26e6519155b515fc7a68`
- GitHub repo: https://github.com/berylinureye/ai-web3-school-cohort-0

### 5. 下一周继续探索的问题

Week 2 我会把主线放在 **Wallet / Permission / Safe Execution**。

我想继续追的问题是：

> 如何让 Agent 只在预算、合约白名单、动作类型和时间窗口内执行低风险链上动作，而高风险动作必须停下来让人确认？

这会涉及 ERC-4337、Safe、session key、guard / policy、人工确认阈值和链上执行日志。

## 可发 X 版本短稿

```text
Week 1 of #AIxWeb3School:

My biggest takeaway:
AI agents should not directly hold EOA private keys.

They can explain ABI, draft transactions, check network/address, and summarize tx proofs.
But signing, approval, payment, and admin actions must stay behind human confirmation or programmable wallet policies.

Proofs:
- Sepolia tx
- WETH9 deposit
- Counter contract deployed
- AI x Web3 safe execution flow

Next: Wallet / Permission / Safe Execution.
```

## 关联 proof

- Week 1 PoW Pack：[`tasks/week1-proof-of-work-pack.md`](./week1-proof-of-work-pack.md)
- AI x Web3 流程图：[`tasks/week1-ai-web3-flow.md`](./week1-ai-web3-flow.md)
- 自部署合约记录：[`tasks/week1-deploy-or-call-contract.md`](./week1-deploy-or-call-contract.md)
- Learning Agent setup：[`tasks/week1-learning-agent-setup.md`](./week1-learning-agent-setup.md)

## AI 辅助说明

本总结由 AI 根据我的 Week 1 task 文件、daily notes 和链上记录起草。我人工复核了核心观点：AI 可以准备和解释链上动作，但不能静默执行签名、授权、付款或 secret 相关操作。
