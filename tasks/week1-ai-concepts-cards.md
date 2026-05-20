# Task: AI 基础概念卡片

- **WCB Task ID**: `cmp3jyq4x07s6n301xzm2cmds`
- **WCB Task Title**: Week 1｜AI 向任务｜整理 AI 基础概念卡片
- **Points**: 10
- **Submitted**: 2026-05-20

> 共 10 张卡片，每张包含：本质 / 例子 / 注意事项（误区 + 使用边界）。
> 最后含一段统一讲为什么 prompt / 上下文 / agent / 工具调用都是"控制 AI 行为的边界设计"。

---

## 卡片 1 · LLM（大语言模型）

- **本质**：基于 transformer 架构、用海量文本训练的概率模型。**给定上下文，预测最可能的下一个 token 序列**。
- **例子**：Claude 4.5 Sonnet、GPT-4o、Gemini Pro、GLM-4、Qwen Max、DeepSeek-V3
- **使用边界 / 误区**：
  - ❌ 误区："LLM 在思考" → 实际是 token 接龙，没有真正的"思考"，但表现出推理能力
  - ❌ 误区："LLM 知道最新信息" → 训练数据有截止日期，需配合搜索/工具获取实时信息
  - ⚠️ 边界：擅长**语言理解、代码生成、推理总结**；不擅长**精确事实记忆、确定性计算（数学/账本）、跨会话状态保持**

## 卡片 2 · Prompt（提示词）

- **本质**：传递给 LLM 的输入文本，**告诉模型当前任务是什么 + 期望输出什么样**。
- **例子**：
  - 简单 Prompt：`"把这段中文翻译成英文：你好世界"`
  - 角色 Prompt：`"你是一个资深 Solidity 安全审计员，请审查以下合约..."`
  - 结构化 Prompt：`"步骤1：理解问题。步骤2：列出方案。步骤3：评估每个方案..."`
- **使用边界 / 误区**：
  - ❌ 误区："prompt 写好就完事" → 真实 production 系统大量精力在"prompt 工程 + few-shot 示例 + 调优"
  - ❌ 误区："越长越好" → 太长的 prompt 反而稀释信号，模型可能忽略关键指令
  - ⚠️ 边界：Prompt 决策权**仍在人**——模型只给输出，怎么用、怎么验证还是人决定

## 卡片 3 · Context Window（上下文窗口）

- **本质**：LLM **能"看到"的文本最大长度**，单位是 token（约 0.75 个英文单词或 1 个中文字）
- **例子**：
  - GPT-3.5: 16K tokens（约 12,000 字英文）
  - Claude 3.5 Sonnet: 200K tokens（约 15 万字英文，约 1 本中长篇小说）
  - Gemini 1.5 Pro: 2M tokens（约 150 万字，约 10 本小说）
- **使用边界 / 误区**：
  - ❌ 误区："上下文越大越好" → **超过半个窗口后，模型的注意力会下降**（中间内容容易被忽略，叫 "lost in the middle" 现象）
  - ❌ 误区："上下文有记忆" → **每次新对话上下文清零**，模型不记得你昨天说过什么
  - ⚠️ 边界：上下文是**工作内存**，不是长期记忆。要"记住"需要外部存储（数据库、记忆系统、文件）

## 卡片 4 · Workflow（工作流）

- **本质**：**预定义的固定任务流程**，模型只是其中一个节点。路径写死，模型不决定下一步去哪。
- **例子**：
  - 一个 RAG workflow：用户输入 → 向量检索文档 → LLM 总结 → 输出
  - 一个翻译 workflow：文档进入 → 切分段落 → LLM 逐段翻译 → 合并 → 输出
- **使用边界 / 误区**：
  - ❌ 误区："Workflow = Agent" → 完全不一样。**Workflow 路径固定，Agent 自主决策**
  - ✅ 使用建议：**能用 Workflow 就别用 Agent**——更可靠、更便宜、更可调试

## 卡片 5 · Agent（智能体）

- **本质**：**模型自主决策**：根据当前状态选择下一步行动（继续推理 / 调工具 / 改路径 / 停止）。**没有预先固定的路径**。
- **例子**：
  - Claude Code（写代码 Agent）：自主决定读哪些文件、跑哪些命令、写什么测试
  - AutoGPT、Manus（探索型 Agent）：给个目标，让它自己搜资料、整理、产出报告
  - 你 0517 搭的 Hermes ↔ 微信 Learning Agent（学习辅助 Agent）
- **使用边界 / 误区**：
  - ❌ 误区："Agent 能搞定任何事" → 失控风险大、调试难、贵
  - ⚠️ 边界：**Agent 适合"目标开放式、步骤无法预先写死"的任务**——复杂度和失控风险成正比，越简单的任务越要避免 Agent 化

## 卡片 6 · Tool Use（工具调用）

- **本质**：让 LLM 从"只能说话"变为"能做事"——它输出结构化请求（调用什么工具 + 传什么参数），框架执行后把结果回传给模型。
- **例子**：
  - 调用 `web_search(query)` → 获取实时信息
  - 调用 `code_executor(python_code)` → 跑数学计算
  - 调用 `bash(command)` → 在 Claude Code 里改文件、git commit
  - 你 0518 让 Hermes 调 MCP 工具同步微信消息
- **使用边界 / 误区**：
  - ❌ 误区："工具调用 = 模型懂工具" → 模型只是按 schema 输出 JSON，**不真懂工具会失败、会有副作用**
  - ⚠️ 边界：**写操作（改文件 / 转账 / 发消息）必须有 human-in-the-loop 二次确认**——这也是我跟 Agent 工作流的硬约束

## 卡片 7 · MCP（Model Context Protocol）

- **本质**：Anthropic 提出的**标准化 LLM 与外部工具的接口协议**。让"工具/数据源"和"LLM 客户端"解耦——任何 MCP 工具能被任何支持 MCP 的客户端调用。
- **例子**：
  - 我搭的 Hermes Agent ↔ 微信桥就是用 MCP server 暴露"发送微信消息"功能
  - Claude Desktop 通过 MCP 连接到本地文件系统、GitHub、Slack、数据库等
  - 你 0517 笔记里"Hermes Agent can act as a local assistant to execute commands" 走的就是 MCP
- **使用边界 / 误区**：
  - ❌ 误区："MCP = 万能 API 中间件" → 它本质上是 prompt-driven 协议，安全边界要自己设计
  - ⚠️ 边界：MCP server 跑在你本地，**能访问的资源就是你赋予它的权限**——给得太宽 = 给 LLM 太多控制权

## 卡片 8 · RAG（Retrieval-Augmented Generation 检索增强生成）

- **本质**：**在让 LLM 回答之前，先从外部知识库检索相关材料**，把检索到的内容拼进 prompt 让 LLM 基于这些材料回答。**解决 LLM 没有最新/专属知识的问题**。
- **例子**：
  - 给 LLM 接你公司的内部文档库 → 问"我们的产品定价策略是什么"
  - AI x Web3 School 的 Learning Agent 接 Handbook 内容 → 我能引用具体章节
- **使用边界 / 误区**：
  - ❌ 误区："RAG 比 fine-tuning 差" → 不对，RAG **适合知识频繁变化、需要溯源**的场景；fine-tuning 适合**风格/格式定制**
  - ⚠️ 边界：检索质量决定一切——如果检索拿错文档，LLM 会"自信地说错话"。**检索系统的工程难度通常被低估**

## 卡片 9 · Fine-tuning（微调）

- **本质**：**用领域特定的数据**继续训练一个已有的 LLM，**让它在特定任务/风格上表现更好**（但不改变它本质的"语言模型"性质）
- **例子**：
  - 用医疗对话数据微调 → 得到擅长医疗咨询的 LLM
  - 用法律判决书微调 → 得到擅长法律分析的 LLM
  - 用自己的写作风格微调 → 得到模仿你说话方式的 LLM
- **使用边界 / 误区**：
  - ❌ 误区："Fine-tuning 比 Prompt 牛" → 大部分场景**好好写 Prompt + RAG 比 fine-tuning 更划算**
  - ⚠️ 边界：fine-tuning **数据要求高、训练成本高、不可逆**。建议先尝试 Prompt + RAG 走不通再考虑

## 卡片 10 · AI Coding / Vibe Coding（AI 辅助编程 / 氛围编程）

- **本质**：用 LLM 加速代码生成、解释、重构。**"Vibe Coding"指的是"模糊指令 + 让 AI 自由发挥"的开发方式**（与传统精确写规格相反）
- **例子**：
  - Claude Code（我现在用的）—— Agent 化的编程助手
  - Cursor / Windsurf —— IDE 集成 AI 编程
  - Codex CLI —— OpenAI 的命令行版本
  - GitHub Copilot —— inline 代码补全
- **使用边界 / 误区**：
  - ❌ 误区："AI 写的代码可以直接用" → **不可以**。代码审查、测试设计、架构决策**必须人工把关**
  - ❌ 误区："Vibe coding 适合所有项目" → 适合**原型、脚本、个人项目**；**不适合 production 系统、安全敏感、合规要求高的场景**
  - ⚠️ 边界：AI 写代码**很快但会写出 plausible-looking bugs**（看似正确但有微妙错误）——比手写代码更需要 review

---

## 为什么 prompt / 上下文 / agent / 工具调用都是"AI 行为边界"的核心

这 4 个概念其实是**给 AI 套上控制框架的 4 个层级**：

| 层级 | 控制对象 | 失控后果 |
|---|---|---|
| **Prompt** | 单次任务的意图 | 模型答非所问 |
| **Context window** | 模型能看到多少信息 | 信息不全 / 注意力涣散 |
| **Agent** | 跨多步骤的目标和决策 | 偏离原目标 / 死循环 / 资源滥用 |
| **Tool use** | 跟外部世界交互的接口 | 误删文件 / 错误转账 / 数据泄露 |

→ **AI Agent 的安全设计 = 这 4 个层级每一层都设好边界**：

- Prompt 层：明确告诉它**能做什么、不能做什么**（边界声明）
- Context 层：**只喂相关信息**，避免上下文污染
- Agent 层：**设置 max 步数 + 退出条件**，避免无限循环
- Tool 层：**写入操作必须人工二次确认**（你 0519 学的 "AI Agent 红线"）

**对产品研究方向的 takeaway**：评估一个 AI 产品（特别是 AI Agent）的成熟度，看它**在这 4 层各自的边界设计有多严**——大部分早期 AI 产品在 Tool 层都没把好关，导致用户损失（删了不该删的文件、签了不该签的交易、发了不该发的消息）。

---

## AI 辅助说明（task 要求）

本卡片由我本人主导写作，AI（Claude Code）扮演的角色：

1. **结构提议**：Claude 提出"用 10 张卡 + 4 维度（本质/例子/边界/误区）"的格式，我接受
2. **初稿起草**：Claude 写了 10 张卡的初稿
3. **我的人工复核**：
   - 修正了卡片 5（Agent）的例子——加上"我自己 0517 搭的 Hermes ↔ 微信桥"作为亲身案例
   - 卡片 7（MCP）原稿写得偏抽象，我让 Claude 加上"我搭桥的具体用法"
   - 卡片 10（Vibe Coding）"边界"那一条原稿过于乐观，我让加上"plausible-looking bugs"的警示
   - 最后一段"为什么这 4 个概念都是边界"是我提议的元结构，Claude 来填表格细节
4. **没有改动的**：每张卡 1-3 句的核心定义——Claude 写得已经够准

**总结我的工作方式**：Claude 提供框架和初稿，我对每张卡的"是不是反映我自己经验"做判断 + 加锐化。这是我跟 AI 协作的标准模式——AI 扩张 90% 的草稿量，人保证 10% 的核心判断质量。

---

## 关联学习

- 我 0517-0520 笔记中体现的 AI 实践：Claude Code 主力、Hermes ↔ 微信通过 MCP 桥、AI × Web3 School Learning Agent
- 关联 Handbook 章节：[ai/llm](https://aiweb3.school/zh/handbook/ai/llm/) / [ai/prompt](https://aiweb3.school/zh/handbook/ai/prompt/) / [ai/agent](https://aiweb3.school/zh/handbook/ai/agent/) / [ai/mcp](https://aiweb3.school/zh/handbook/ai/mcp/) / [ai/rag](https://aiweb3.school/zh/handbook/ai/rag/)
