# Learning Plan — AI × Web3 School Cohort 0

> Last updated: 2026-05-19（Cohort Day 2 · 通过 WCB Agent API 拉取真实任务列表后重写）
> Profile: see [profile.md](./profile.md)

## Program 基本事实（来源：WCB Agent API）

- Program: AI × Web3 School（ID `cmnx791nl008sru0167pzp4ki`）
- Duration: **2026-05-18 → 2026-06-14（4 周）**
- 今日: **Cohort Day 2（2026-05-19）**
- 我所在 Week 1 trackId: `cmoy1hces012app01kp9ci5gb`
- 数据源: `tasks.listForLearner` / `program.getById` / `users.getProfile`

> 之前 v1 计划是 Handbook 自排的 7 天表，跟课程真实节奏严重错位（cohort 1 周 ≠ 我自排 1 周 ≠ 同一组任务）。这一版基于 WCB 真实任务结构重写。

## Week 1 全景（37 个任务，按类别归并）

| 类别 | 数量 | 总分 | 关键任务 |
|---|---|---|---|
| 前置准备 | 8 | 95 | 开营仪式 / 创建 repo / Learning Agent setup / 工具准备 / X 发布 / 社群自介 / PoW 测试 / 信息流清单 |
| AI 向任务 | 3 | 60 | 概念卡片 / Learning Agent setup / AI 可交互产物 |
| Web3 向任务 | 3 | 60 | 概念卡片 / 测试网交易 / 部署或调用合约 |
| Web3 进阶 | 1 | 30 | **EOA / 智能账户 / 多签 权限比较** |
| AI × Web3 综合 | 2 | 70 | 最小交叉流程图 / Week 1 PoW Pack |
| 综合进阶 | 1 | 40 | 受限 Web3 助手 / workflow 设计 |
| 行业 | 2 | 50 | 1-2 个项目拆解 / AI×Web3 学习总结 |
| 线上活动 | ~17 | 280+ | 5.18-5.23 每天 live + 回放 |

**完整任务表**：见 `tasks/wcb-week1-snapshot.md`（下次拉取 API 时刷新）

## 我的真实进度（截至 2026-05-19 晚）

### ✅ 已提交（3 个，60 分）
- 参加开营仪式（20）
- 实时参加 5.18 Co-learning（20）
- 实时参加 5.18 AI 时代的 Web3 架构能力（20）

### 🟡 已完成但未提交（5 个，预估 90 分）
- 创建课程 GitHub repo（10）— repo 已存在，缺一次提交
- Learning Agent Setup（20）— Hermes + Claude Code 双通道已搭好
- 完成一笔测试网交易（20）— `tasks/web3-minimum-practice.md` 已有完整记录
- 整理 Web3 基础概念卡片（10）— 0518 + 0519 笔记已积累
- **比较 EOA / 智能账户 / 多签**（30）— 0519 Q&A 已覆盖大部分，补多签即可

### ⏳ 高 ROI 待做（建议本周完成）
- AI 概念卡片（10）+ AI 可交互产物（30）
- 部署或调用一个最小智能合约（30）— Day 3-4 实操
- AI × Web3 最小交叉流程图（30）
- Week 1 PoW Pack（40）— 把上面成果打包

### 📺 线上活动（按 live 时间排）
- 5.19 Hermes 从 0 到 1（live 今晚 / 回放 DDL 05-23）
- 5.20 Web3 运行原理（live 5.20 18:00 / 回放 DDL 05-24）
- 5.20 Co-learning（live 5.20 20:00）
- 5.21 AI 下乡-Web3 应用
- 5.22 Z.AI + Week 1 例会
- 5.23 Open Agentic Economy + Co-learning

## 周节奏（接下来 5 天）

| 日期 | Cohort Day | 主要任务（按个人方向定制） | 重点产出 |
|---|---|---|---|
| **05-19 今晚** | Day 2 | 把今晚 Q&A 整理成 4 个可提交任务 + 提交 | repo 多 4 个 tasks/ 文件 + WCB +90 分 |
| 05-20 | Day 3 | 5.20 Web3 运行原理 live + 部署最小合约 | task #13 (30 分) |
| 05-21 | Day 4 | 5.21 AI 下乡 live + AI 概念卡片 + 信息流清单 | task #8 (10 分) + #37 (20 分) |
| 05-22 | Day 5 | Z.AI + Week 1 例会 live + AI × Web3 流程图 | task #15 (30 分) |
| 05-23 | Day 6 | Open Agentic Economy + Week 1 PoW Pack 打包 | task #16 (40 分) |

## Week 2-4 方向（待 trackId 公布）

Week 2-4 的 `trackId` 当前 API 返回 `null`，意味着任务还未上架（cohort 在 Week 1 末才会开放 Week 2 内容）。**等 trackId 公布后会用同样的 API 流程拉取真实任务**，再决定 hackathon track。

当前候选（基于产品研究方向）：
- `agentic-commerce`（机器支付 / AI Agent 自主交易）
- `open-track`（自由度最高，适合内容沉淀方向）
- `wallet-permission`（如果对签名安全这条线深挖）

## 持续动作

- [ ] **每晚 Claude Code 复盘**：跑 `tasks.listForLearner` 看新增/状态变化 → 更新 daily note + plan
- [ ] **每周日复盘**：盘点本周提交情况、刷新下周计划
- [ ] **handbook-feedback/**：遇到真实卡点立刻记录
- [ ] **tasks/**：每个 WCB 任务提交前先在这里写完整版，再提取 proof
- [ ] **experiments/**：大型探索 / 案例研究

---
*Living document. Cohort 数据通过 WCB Agent API 每晚同步。*
