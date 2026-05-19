# Task: Learning Agent Setup

- **WCB Task ID**: `cmp3jyqal07s8n301lwgxo0fc`
- **WCB Task Title**: Week 1｜AI 向任务｜完成 Learning Agent Setup
- **Points**: 20
- **Submitted**: 2026-05-19（pending）

## 1. 选择的 Agent / AI 工具组合

**双通道架构**（不是单一工具）：

| 通道 | 角色 | 工具 |
|---|---|---|
| 主力沉淀面 | 深度提问、写/改/commit 文件、Handbook 深读、晚上定稿笔记 | **Claude Code（CLI）** |
| 远程指令入口 | 路上一句话提问、临时洞察、当日任务调整（**不写文件、不 commit**） | **Hermes Agent ↔ 微信**（通过 MCP 桥接） |

**衔接方式**：微信通道把问题写到当日 `daily/YYYY-MM-DD.md` 的「今日待答」section；晚上 Claude Code 集中清空、沉淀进"关键收获"。

**判断规则**（写在 `profile.md` 里）：
- 问题 ≤ 1 句话 / 不需要查 Handbook / 不需要改文件 → 微信
- 需要展开 / 需要查 Handbook / 需要 commit → Claude Code
- 晚上复盘 → 一定 Claude Code

## 2. 让 Agent 帮我完成的学习任务

**AI × Web3 School Cohort 0 学习 Agent**——涵盖：

1. 每日学习计划生成（最小路径 + 推荐路径）
2. Handbook 章节关键概念解释
3. 「今日待答」问题清单（白天攒晚上答）
4. WCB 任务对齐 + 提交 proof 起草
5. daily note 实时沉淀（对话过程 → 笔记定稿）
6. handbook-feedback 整理（遇到的真实卡点 → 给手册作者反馈）
7. git status / add / commit / push（需人工二次确认）

## 3. 关键 Prompt / 配置

**启动 Prompt**：https://aiweb3.school/learning-agent.zh.txt（AI × Web3 School 官方 Agent 启动 Prompt）

第一次启动时把上面 URL 发给 Claude Code，Agent 会按 prompt 内规则完成：
- 学员画像确认（≤3 个问题/轮）
- GitHub repo 创建（已通过 `gh repo create` 完成）
- 仓库结构初始化（README / profile / learning-plan / daily / tasks / experiments / handbook-feedback / hackathon / submissions / templates）
- 每日学习与打卡流程
- WCB Agent API 集成

**关键约定**（写进 `profile.md`）：
- 公开 repo 不放 API key / 助记词 / 私钥 / 未公开联系方式
- 任何消耗 gas 的交易、签名请求、钱包连接到新域名，**Agent 都不自动执行**，必须人工二次确认
- WCB API key 仅放 `~/.zshrc` 环境变量 `WCB_AGENT_SECRET_API_KEY`，不写进任何文件 / 聊天记录 / 公开 repo

## 4. 一次成功输出记录（2026-05-19 晚）

**问题**：用户提问"eoa我理解就是有公钥和私钥... 所以evm是基于智能合约，相当于上面加了一层部署到链上的代码？那和eoa的区别是什么"

**Agent 输出**：
- 立即指出错误："EVM 基于智能合约"是反的，正确关系是**智能合约跑在 EVM 上**
- 给出 3 层模型（EVM 执行层 + EOA / 合约账户 两种账户类型）
- 用用户已有的 0518 实操经验（Sepolia 转账）做锚点，给出 3 个例子（EOA→EOA / EOA→合约 / 合约→合约）
- 同时**实时把这次问答沉淀到 [`daily/2026-05-19.md`](../daily/2026-05-19.md) 的"关键收获"段**

**效果**：用户主动追问 "evm就是区块链的核心？" + "合约账户有啥优势" → 进一步 Q&A 把 EVM ≠ 区块链 + EOA vs 智能账户 vs 多签 三层模型搞清楚 → 直接产出本周 30 分进阶任务（`tasks/week1-account-permissions-comparison.md`）

## 5. 一次我人工复核 / 修正 / 拒绝 Agent 建议的记录

### 事件 A：API Key 泄漏 → Agent 拒绝使用 + 引导 rotate

**经过**：用户在配 WCB API Key 时，把 secret 完整贴到了对话窗口。

**Agent 行为**：
- ❌ **没有使用** chat 里的 key 调任何 API
- ❌ **没有把它写进任何文件 / memory / repo**
- ✅ 立即指出："这违反了启动 Prompt 第 7 节，key 现在已泄漏，必须 revoke 重做"
- ✅ 给出 3 步处理流程（revoke 旧 key → 生成新 key → env var 配置）
- ✅ 用户回复"没事"想跳过 → Agent **明确拒绝**："不是嫌麻烦，启动 Prompt 第 3/7 节明确禁止从聊天读 secret，我违反这条规则就破坏了整个 Agent 的安全前提"

**用户人工动作**：去 WCB profile 页面 revoke 旧 key，重新生成新 key，按 env var 流程配置。配好后 Agent 通过 Bash 工具读 `$WCB_AGENT_SECRET_API_KEY`，从 shell 环境读，不从聊天读。

### 事件 B：Agent 自排 7 天 plan 偏离 WCB 真实任务 → 用户校正

**经过**：Agent Day 1 给用户排了"7 天 Handbook 路线"，到 Day 3（实为 Cohort Day 2）时用户指出："你是不是没有严格根据我这个一周的任务做规划。上面还有很多模块都需要我后续做。我看你完全根据 handbook 在安排"

**Agent 反应**：
- ✅ 立刻承认错误："这是我的失误。启动 Prompt 第 5 节明确写了每日流程第 1 步：读取 WCB Learning 页面。我跳过了这一步"
- ✅ 尝试公开端点抓 WCB 内容 → 返回 401 → 引导用户 setup API key（路径 A/B/A+B 三选一）
- 用户选 A+B，但拒绝粘贴任务列表（因为公开 repo 不该放敏感链接）→ Agent 接受这个边界，引导用户做完 B 路径（API key setup）→ Agent 自己用 API 拉真实任务表（37 个）→ 重写 `learning-plan.md` 对齐 WCB 真实结构

### 关键洞察
**Agent 的价值不是"自己想得多对"，而是"被用户拨回正确轨道时反应有多快"**。两次事件都是用户的反馈让 Agent 修正了否则会继续偏离的方向。

## 6. 仓库

完整 setup 见 GitHub：[berylinureye/ai-web3-school-cohort-0](https://github.com/berylinureye/ai-web3-school-cohort-0)

- 启动 Prompt 完整内容：https://aiweb3.school/learning-agent.zh.txt
- profile.md：画像 + 双通道工作流约定
- learning-plan.md：基于 WCB API 拉真实任务的 4 周路线
- daily/：每日学习沉淀
- tasks/：WCB 任务对应的完整版（提交前先在这里写完，再提取 proof）
- handbook-feedback/：真实 onboarding 卡点反馈

---

> Setup 时间线：2026-05-17 初始化 + Hermes 桥接 → 2026-05-18 验证双通道 → 2026-05-19 修正方向 + 接入 WCB API → 当前稳定运行。
