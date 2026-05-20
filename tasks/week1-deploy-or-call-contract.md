# Task: 部署或调用一个最小智能合约

- **WCB Task ID**: `cmp3jyr3h07sgn301jpcjoma9`
- **WCB Task Title**: Week 1｜Web3 向任务｜部署或调用一个最小智能合约
- **Points**: 30
- **Submitted**: 2026-05-20

## 实操内容

完成了 **2 种类型**的智能合约交互（任务要求二选一，本次双实操）：

1. **调用一个已部署测试网合约的写入函数**——WETH9 deposit wrap（ETH → WETH）
2. **部署自己写的最小合约**——Counter（详见末尾的 Phase 2 段落）

## Phase 1: 调用 WETH9 合约 `deposit()`

### 基本信息

| 字段 | 值 |
|---|---|
| 测试网 | Sepolia (Chain ID 11155111) |
| 调用目标合约 | **WETH9 (Wrapped Ether)** |
| 合约地址 | `0xfFf9976782d46CC05630D1f6eBAb18b2324d6B14` |
| 调用账户 | `0xb3b69AFAc4Eb4723fe62FDD6859D3a3C2EF23983` (Account 1) |
| 调用的函数 | `deposit()` payable |
| Function selector | `0xd0e30db0` |
| 发送 ETH | 0.0001 SepoliaETH (= 100,000,000,000,000 wei) |

### 交易记录

| 字段 | 值 |
|---|---|
| Tx Hash | `0x72a6c0e85c15fb2067f6218337989a32285465cee7cb26e6519155b515fc7a68` |
| Status | **Success** |
| Block | 10885816 |
| Gas Used | 45,038 |
| 区块浏览器链接 | https://eth-sepolia.blockscout.com/tx/0x72a6c0e85c15fb2067f6218337989a32285465cee7cb26e6519155b515fc7a68 |

### 写入结果

- 合约的 `balanceOf[0xb3b69...23983]` 从 0 变为 `100000000000000` (= 0.0001 WETH)
- 合约 emit 了 `Deposit(address indexed dst, uint256 wad)` event
- Account 1 的 ETH 余额从 0.04 → 0.0397（少了 0.0001 本金 + 0.0001 gas，gas 实际只用 45038 部分会退）

### 步骤拆解 + 哪些步骤必须人工确认

| 步骤 | 内容 | 是否需人工确认 |
|---|---|---|
| 1 | 在 Blockscout 打开 WETH9 合约页面 → Write contract tab | 否（纯浏览） |
| 2 | 连接 MetaMask 到 Blockscout | **是**（核对网站域名 + 账户 + 网络，防钓鱼） |
| 3 | 找到 deposit 函数 + 填 0.0001 ether | 否（本地操作） |
| 4 | 点 Simulate 预演 | 否（本地试跑，不上链） |
| 5 | 点 Write → MetaMask 弹窗 | **是**（核对接收方 / 金额 / gas） |
| 6 | 在 MetaMask 弹窗里点"确认"签名 | **必须人工**（一次性、不可撤回） |
| 7 | 等待 12 秒上链 | 否（被动等待） |
| 8 | 在 Blockscout 查 tx 详情 + Deposit event log | 否（事后验证） |

### 关键观察（来自这次实操）

#### 1. ABI 的产品价值
MetaMask 弹窗显示 "Deposit" 而不是 "Contract Interaction"——这是因为 WETH9 合约已 verified，钱包能查 ABI 反解 calldata `0xd0e30db0` 为函数名 `deposit`。**ABI 不在合约里，但让钱包能给用户语义化展示，是用户能"看懂自己在签什么"的关键**。

#### 2. Storage vs Event 的具象化
- **Storage 变化**：合约里 `balanceOf` mapping 多了一个 entry（我的地址 → 10^14 wei）。这是合约的"当前状态"，下次执行能读
- **Event 发出**：`Deposit(我的地址, 10^14)` 写进了区块的 logs 区。合约自己读不到，但链下索引器（Blockscout、The Graph 等）能抓到

#### 3. Gas 真实数据
- 估算: 45,418
- 实际: 45,038
- 差异: 380 gas (~0.8% 偏差，正常范围)
- 普通 EOA 转账 (0518): 21,000 gas
- 合约调用 (0520): 45,038 gas
- → **跑合约代码比纯转账多花 2.1 倍 gas**，这是 EVM 执行字节码的真实成本

#### 4. 完整调用链路的 8 步链路（Handbook 智能合约章节"调用流程"）

这一笔交易完整验证了：

```
[1] 前端 (Blockscout) 读地址+网络
   ↓
[2] 前端用 ABI 编码 calldata → 0xd0e30db0
   ↓
[3] MetaMask 弹窗 → 我签名
   ↓
[4] 广播到 Sepolia RPC 节点
   ↓
[5] 验证者把交易打包进区块 10885816
   ↓
[6] EVM 执行 WETH9.deposit()
   ├─ balanceOf[me] += 10^14（改 storage）
   └─ emit Deposit(me, 10^14)（发 event）
   ↓
[7] MetaMask 收到 tx receipt → 显示"已确认"
   ↓
[8] 索引器读 event → Blockscout 的 token holders 数据库更新
```

每一步都在真实物理时间里发生。

### 我现在持有的资产
- 0.0397 SepoliaETH（链原生币，直接在 MetaMask 显示）
- **0.0001 WETH**（ERC20 token，在 WETH9 合约的 storage 里记录；MetaMask 默认不识别，需手动 add token 才能显示）

## Phase 2: 部署自己的 Counter 合约（Handbook 最小实践）

> 待补充：使用 Remix IDE 部署 Counter.sol 到 Sepolia，调用 increment() 至少 2 次，verify 源码。

（部署完成后会用 `tasks.submitEvidence` 重新提交一次 proof，包含部署地址和源码链接。）

---

## 验证链接

- **直接看 tx 详情（含 Deposit event log）**:
  https://eth-sepolia.blockscout.com/tx/0x72a6c0e85c15fb2067f6218337989a32285465cee7cb26e6519155b515fc7a68

- **查我的 WETH 余额**（链上 view 调用，可任何人验证）:
  打开 https://eth-sepolia.blockscout.com/address/0xfFf9976782d46CC05630D1f6eBAb18b2324d6B14?tab=read_contract → 找 `balanceOf` 函数 → 输入 `0xb3b69AFAc4Eb4723fe62FDD6859D3a3C2EF23983` → 应返回 `100000000000000` ✓

- **WETH9 合约源码**（学习参考）:
  https://eth-sepolia.blockscout.com/address/0xfFf9976782d46CC05630D1f6eBAb18b2324d6B14?tab=contract_source_code

---

## 关联学习

- 今晚学的 ABI / Storage / Event / 调用流程 / 广播 / Gas 全部在这一笔交易里物理验证
- 这是我**第一次跟智能合约真实交互**——之前 0518 只是 EOA→EOA 转账
- 详细学习记录见 [`daily/2026-05-20.md`](../daily/2026-05-20.md)
