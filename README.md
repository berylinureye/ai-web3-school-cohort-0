# AI × Web3 School · Cohort 0 学习仓库

@berylinureye 在 AI × Web3 School Cohort 0（2026-05-18 → 2026-06-14）的个人学习日志和 proof-of-work。

- **课程 Handbook**: https://aiweb3.school/zh/handbook/
- **WCB 课程页**: https://web3career.build/zh/programs/AI-Web3-School
- **学习 Agent 启动 Prompt**: https://aiweb3.school/learning-agent.zh.txt

---

## ⚠️ 隐私安全

本仓库为 **public**。**严禁** commit 以下内容：

- API key / secret / token / .env 文件
- 助记词 / 私钥
- 未公开的联系方式 / 内部会议链接
- 他人个人数据 / 截图含他人信息
- 任何主网真实资产相关私密信息

所有 secret 仅放本地环境变量（如 `~/.zshrc` 中的 `WCB_AGENT_SECRET_API_KEY`）。

---

## 目录结构

| 路径 | 用途 |
|---|---|
| **`profile.md`** | 学员画像 + 工作流约定 + 概念掌握度速查 |
| **`learning-plan.md`** | 基于 WCB 真实任务的 4 周路线 + 滚动节奏 |
| **`daily/`** | **时间序列日志**——每日学习记录、Q&A 沉淀、当日战绩。文件名格式 `YYYY-MM-DD.md` |
| **`tasks/`** | **WCB 任务对应的完整 proof**。每个任务一个 markdown，命名 `week<N>-<task-slug>.md`。这是给 WCB 评审看的版本 |
| **`experiments/`** | **大型探索 / 案例研究 / 综合性 topic 笔记**。跟 daily/ 的区别：daily 是按日期组织，experiments 是按主题组织 |
| **`handbook-feedback/`** | 学习过程中给 Handbook 作者的反馈（卡点、错别字、建议） |
| **`hackathon/`** | Hackathon 项目相关（Week 2+ 用） |
| **`skills/`** | Agent 配置文件（给 Hermes 等外部通道喂的 skill） |
| **`templates/`** | daily 和 task 的当前格式模板 |

> 已废弃 / 删除：~~`notes/`~~（合并进 daily/ 和 experiments/）、~~`submissions/`~~（跟 tasks/ 重叠）。

---

## 工作流（双通道）

详细约定见 [`profile.md`](./profile.md) 和 [`skills/hermes-mobile-agent.md`](./skills/hermes-mobile-agent.md)。

**Claude Code（本机主力）**：深度提问、写/改/commit 文件、Handbook 深读、晚上定稿 daily note 和打卡稿。

**Hermes ↔ 微信（远程入口）**：路上一句话提问、临时洞察、把问题写到当日 daily note 的「今日待答」section、**不**写其他文件、**不** commit、**不**提交 WCB task。

判断规则：
- 问题 ≤ 1 句话 / 不需查 Handbook / 不需改文件 → 微信
- 需要展开 / 查 Handbook / commit → Claude Code
- 晚上复盘 → 一定 Claude Code

---

## 当前进度（每天更新一次）

最新更新见最近一份 [`daily/`](./daily/) 笔记的"WCB 战绩"段。

```
最后更新: 2026-05-20 (Cohort Day 3 收工)
WCB 提交: 11 task / 210 分 / Week 1 满分 640 / 进度 32.8%
我的 Counter 合约: 0x736b9278e7fa24e49C5de9b2bC96A6eb6EF19c73 (Sepolia)
```

---

## 怎么读这个仓库（按角色）

### 我本人 / WCB 评审
- 看 `daily/` 最新一份了解当前状态
- 看 `tasks/` 看每个 WCB 任务的具体 proof
- 看 `experiments/` 看大型综合 topic 探索

### 想理解我学习方式的人
- 看 `profile.md` 了解我是谁、目标方向
- 看 `learning-plan.md` 了解我 4 周路线
- 看 `daily/2026-05-19.md` 看一个完整的"主动学习循环"的样子（5 段 Handbook 精读 + 用户总结 + Agent 纠偏）

### 想给我反馈的人
- 直接开 issue，或者 Twitter 联系 [@berylinureye](https://github.com/berylinureye)
- 我特别欢迎对 `experiments/` 里的 topic 综合笔记的批评
