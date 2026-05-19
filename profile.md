# Learner Profile

> Last updated: 2026-05-19（Day 3 复盘后刷新）

## 基础画像
- **AI 背景**：熟悉。日常主力 Claude Code，搭过 Hermes Agent ↔ 微信的 MCP 桥
- **Web3 背景**：有基础。已完成钱包/EOA/签名(personal_sign)/Gas/EIP-712/CeFi vs Self-custody 的实操与理论
- **编程能力**：能独立开发
- **主目标**：产品研究 / 内容沉淀（hackathon 偏 open-track，长期看 bridge/agent-wallet 方向）
- **每日时间**：1-2 小时（最小 + 推荐路径）
- **输出语言**：中文
- **打卡节奏**：晚上一次（睡前 Claude Code 复盘 + 提交 web3school 打卡）

## 工作流约定
- **Claude Code（主力沉淀面）**：深度提问、写/改/commit 文件、Handbook 深读、晚上定稿 daily note 和打卡稿
- **Hermes ↔ 微信（远程指令入口）**：路上一句话提问、临时洞察、当日任务调整。**不写文件、不 commit**
- **判断规则**：
  - 问题 ≤ 1 句话 / 不需要查 Handbook / 不需要改文件 → 微信
  - 需要展开 / 需要查 Handbook / 需要 commit → Claude Code
  - 晚上复盘 → 一定 Claude Code
- **衔接方式**：微信问题统一写到当日 `daily/YYYY-MM-DD.md` 的 **「今日待答」** section；晚上 Claude Code 集中清空

## 安全边界
- 仓库默认 public，不放任何隐私信息、API key、助记词、私钥、未公开联系方式、内部会议链接或他人个人数据
- 练习钱包（Sepolia 上的 MetaMask Account 1/2）**仅做测试网练手**，永远不打主网真币
- 任何消耗 gas 的交易、签名请求、钱包连接新域名，都必须人工二次确认（Agent 不自动执行）
- WCB Agent API 等 secret 仅放本地环境变量或 Hermes secrets，不写进 prompt、README、聊天记录或公开 repo

## 已掌握 / 未掌握 速查（每天迭代）

| 主题 | 状态 | 备注 |
|---|---|---|
| 钱包架构（EOA、私钥、助记词） | ✅ | 0518 实操 |
| personal_sign | ✅ | 0518 在 test-dapp 跑通 |
| EIP-712 typed data | 🔄 | Day 3 概念，未实操 |
| Permit / Permit2 | 🔄 | Day 3 概念，未实操 |
| setApprovalForAll | ⏳ | Day 4 计划 |
| ERC20 approve / revoke | ⏳ | Day 4 计划 |
| 智能合约基础 | ⏳ | Day 4 计划 |
| DeFi 基础 + Oracle 风险 | ⏳ | Day 5 计划 |
| Bridge：Agent × Wallet | ⏳ | Day 6+ |
| Bridge：可验证 AI / 机器支付 | ⏳ | Day 7+ |
