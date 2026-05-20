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

### 基本信息

| 字段 | 值 |
|---|---|
| 工具 | Remix IDE (v2.2.0) + Browser Extension (MetaMask) |
| 编程语言 | Solidity 0.8.20 |
| 测试网 | Sepolia |
| **我的 Counter 合约地址** | **`0x736b9278e7fa24e49C5de9b2bC96A6eb6EF19c73`** |
| 部署 tx hash | `0xed708a1557cbce1a93ff093bd3a6d60bb351cfe874a63288f12876870e84675a` |
| 部署账户 | `0xb3b69AFAc4Eb4723fe62FDD6859D3a3C2EF23983` (Account 1) |
| 部署 gas | ~600,000 wei (0.0006 ETH) |
| 源码已 verified | ✅ 是（Remix 自动 verify 到 Blockscout） |

### 我写的合约源码

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

/// @title 最小 Counter 合约
/// @author berylinureye
/// @notice Handbook 智能合约章节的最小实践版本
contract Counter {
    uint256 public count;
    address public owner;

    event CountChanged(uint256 newCount);

    constructor() {
        owner = msg.sender;
    }

    /// @notice 任何人都能调用，让 count +1
    function increment() external {
        count += 1;
        emit CountChanged(count);
    }

    /// @notice 只有 owner 能调用，重置 count 为 0
    function reset() external {
        require(msg.sender == owner, "Not owner");
        count = 0;
        emit CountChanged(count);
    }
}
```

### 合约设计要点（每行代码的"为什么"）

| 代码 | 用途 |
|---|---|
| `uint256 public count` | 链上 storage 变量，自动生成 `count()` 读函数 |
| `address public owner` | 记录部署者地址，自动生成 `owner()` 读函数 |
| `event CountChanged(uint256)` | 链下索引器能监听的日志 |
| `constructor() { owner = msg.sender; }` | 部署时记录"谁部署的我" |
| `function increment() external` | 任何人都能调，每次 count +1 |
| `emit CountChanged(count)` | 改 storage 后发 event 通知外部 |
| `require(msg.sender == owner, ...)` | 权限控制：只有部署者能 reset |

### 链上验证链接

- **合约页面**: https://eth-sepolia.blockscout.com/address/0x736b9278e7fa24e49C5de9b2bC96A6eb6EF19c73
- **部署交易**: https://eth-sepolia.blockscout.com/tx/0xed708a1557cbce1a93ff093bd3a6d60bb351cfe874a63288f12876870e84675a
- **源码 (verified)**: https://eth-sepolia.blockscout.com/address/0x736b9278e7fa24e49C5de9b2bC96A6eb6EF19c73?tab=contract_source_code

任何人都可以打开 Blockscout 的 Read Contract tab，调用 `count()` / `owner()` 看我合约的当前状态。任何人也可以调 `increment()` 给我的 count 加 1（毫无意义，但他们能做）。

### Phase 2 关键观察

#### 1. 部署是一种特殊的交易
跟 Phase 1 的 deposit 调用不同：

| | Phase 1 (调用合约) | Phase 2 (部署合约) |
|---|---|---|
| To 地址 | WETH9 合约 | **空**（创建新合约）|
| Data 字段 | 函数 selector `0xd0e30db0` | **整个合约的 bytecode**（部署字节码） |
| Value | 0.0001 ETH | 0 ETH |
| 链上结果 | 改 WETH9 storage | **创建一个新地址 + 在该地址写入运行时字节码** |
| Gas | 45,038 | ~600,000（贵 13 倍） |

部署交易的 calldata 实际上**就是合约的字节码**——EVM 执行 constructor 后把"运行时字节码"写到新地址。从此这个地址就是一个有代码的合约账户。

#### 2. 验证 (Source Verify) 的价值
Remix 把源码上传到 Blockscout 后，**任何人都能读我合约的源码**（不只是看二进制字节码）。这就是"链上合约公开可验证"的具体含义。

对产品研究方向的 takeaway：**看一个 dApp 之前，先去 Blockscout 看它的合约源码 verified 没有 + owner 权限有多大 + 升级机制怎么写**。

#### 3. "拥有"合约的真实含义
我部署 Counter 后：
- ✅ 我是 `owner` 字段记录的地址 → 我能调 `reset()`
- ❌ 我**不能阻止**任何人调 `increment()` —— 合约公开，任何人能调
- ❌ 我**不能改源码**—— 合约一旦部署不可变
- ❌ 我**不能下线它**—— 合约永远在链上

"拥有"= 仅仅是"代码里写了什么权限给我"。**没有 owner 字段就完全无主**（WETH9 就是这样）。

#### 4. 这是我第一个完全自己负责的链上资产
未来任何人调用我这个 Counter（理论上），他们都跟我的部署地址 `0xb3b69...23983` 有了一层链上关联。即使我明天把 Counter 忘了，它仍然在链上、仍然能被调用、仍然记录着 owner 是我。

**永久。不可撤销。这就是 Web3。**

### 怎么跟我的合约交互（任何人都能做）

打开 https://eth-sepolia.blockscout.com/address/0x736b9278e7fa24e49C5de9b2bC96A6eb6EF19c73

- **Read Contract** tab：调 `count()` 看当前值（不要钱）
- **Write Contract** tab：调 `increment()` 给我的 count +1（需要 Sepolia gas）

任何人都能让我的 count +1，但只有我（0xb3b6...23983）能 reset。

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
