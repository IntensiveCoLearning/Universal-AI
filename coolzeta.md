---
timezone: UTC+8
---

# zeta zhang

**GitHub ID:** coolzeta

**Telegram:** @zeta_zhang

## Self-introduction

web3 developer, crypto HODLer

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
0
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->

打个卡先
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


````markdown
# zetachain开发

## 2025-11-24 学习总结

### 一、项目概述

开发了一个 ZetaChain 跨链调用前端应用，使用 Next.js + viem + wagmi + MUI 技术栈。

**项目结构：**
```
zetachain-learn/
├── contract1/call/              # Foundry 智能合约项目
│   ├── contracts/
│   │   ├── Universal.sol        # 主合约
│   │   └── Connected.sol        # 连接合约
│   └── .env                     # 私钥配置
└── front-end/call/              # Next.js 前端项目
    ├── app/page.tsx             # 主交互页面
    ├── lib/
    │   ├── config.ts            # Wagmi/Viem 配置
    │   └── UniversalABI.ts       # 合约 ABI 和常量
    └── package.json
```

---

### 二、智能合约部署

#### 2.1 本地网络部署 (Anvil)
- **网络**: Anvil Local (Chain ID: 31337)
- **RPC**: http://localhost:8545
- **合约地址**: `0x5bf5b11053e734690269C6B9D438F8C9d48F528A`
- **部署工具**: Foundry (forge)

**部署命令：**
```bash
export PRIVATE_KEY="your_private_key"
forge create Universal \
  --rpc-url http://localhost:8545 \
  --private-key $PRIVATE_KEY \
  --evm-version paris \
  --broadcast
```

#### 2.2 ZetaChain 测试网部署
- **网络**: ZetaChain Testnet (Chain ID: 7001)
- **RPC**: https://7001.rpc.thirdweb.com
- **合约地址**: `0x7606B214Bf088C977ead63de4187d2Fbd025e505`
- **交易哈希**: `0x3360a85c13b6bcfc32772c746789d8bd4dd3eb46050a842f451704b5982ca242`
- **部署时间**: 2025-11-24

**部署命令：**
```bash
cd contract_dir
source .env
forge create Universal \
  --rpc-url https://7001.rpc.thirdweb.com \
  --private-key $PRIVATE_KEY \
  --evm-version paris \
  --broadcast
```

---

### 三、合约函数详解

#### 3.1 call() 函数
发送跨链消息调用
```solidity
function call(
    bytes memory receiver,        // 目标接收地址
    address zrc20,               // ZRC-20 代币地址
    bytes calldata message,      // 消息内容（UTF-8 字节）
    CallOptions memory callOptions,
    RevertOptions memory revertOptions
) external
```

**前端调用方式：**
```typescript
writeContract({
  address: CONTRACT_ADDRESS,
  abi: UNIVERSAL_ABI,
  functionName: "call",
  args: [
    receiver,              // 接收地址
    zrc20,                // ZRC-20 代币地址
    messageBytes,         // UTF-8 字节
    callOptions,          // { gasLimit: BigInt, isArbitraryCall: bool }
    revertOptions         // 回退选项
  ],
});
```

#### 3.2 withdraw() 函数
从 ZetaChain 提取代币
```solidity
function withdraw(
    bytes memory receiver,
    uint256 amount,
    address zrc20,
    RevertOptions memory revertOptions
) external
```

#### 3.3 withdrawAndCall() 函数
提取代币并调用
```solidity
function withdrawAndCall(
    bytes memory receiver,
    address zrc20,
    uint256 amount,
    bytes calldata message,
    CallOptions memory callOptions,
    RevertOptions memory revertOptions
) external
```

---

### 四、前端开发

#### 4.1 核心技术栈
| 技术 | 版本 | 用途 |
|-----|------|------|
| Next.js | 16.0.3 | 前端框架 |
| React | 19.2.0 | UI 库 |
| viem | 2.40.0 | Web3 交互 |
| wagmi | 3.0.1 | React hooks 库 |
| @mui/material | 7.3.5 | UI 组件库 |
| @tanstack/react-query | 5.90.10 | 数据管理 |

#### 4.2 关键文件

**lib/config.ts** - Wagmi 配置
```typescript
const zetachain: Chain = {
  id: 7001,
  name: 'ZetaChain',
  nativeCurrency: { name: 'ZETA', symbol: 'ZETA', decimals: 18 },
  rpcUrls: {
    default: { http: ['https://7001.rpc.thirdweb.com'] },
  },
}

export const config = createConfig({
  chains: [zetachain],
  connectors: [injected()],
  transports: {
    [zetachain.id]: http('https://7001.rpc.thirdweb.com'),
  },
})
```

**lib/UniversalABI.ts** - 合约 ABI
- `UNIVERSAL_ABI`: Universal 合约 ABI 定义
- `ERC20_ABI`: ERC20 标准 ABI（用于 approve）
- `CONTRACT_ADDRESS`: `0x7606B214Bf088C977ead63de4187d2Fbd025e505`

**app/page.tsx** - 主交互组件
- 使用 `useAccount()` 管理钱包连接
- 使用 `useReadContract()` 读取代币余额和授权额度
- 使用 `useWriteContract()` 发送交易
- Tab 接口分离 Call 和 Withdraw 功能

#### 4.3 ERC20 授权流程

前端已集成 ERC20 token 授权功能：

```typescript
const handleApprove = async () => {
  writeContract({
    address: zrc20,
    abi: ERC20_ABI,
    functionName: "approve",
    args: [CONTRACT_ADDRESS, maxUint256],  // 授权无限额度
  });
}
```

**UI 展示：**
- 显示用户 ZRC-20 余额
- 显示当前授权额度
- 未授权时显示"授权 ZRC20"按钮
- 已授权时显示"✓ 已授权"（禁用）

---

### 五、常见问题与解决

#### 5.1 消息编码问题
**问题**：
```
Internal JSON-RPC error
```

**原因**：
使用了 `encodeAbiParameters([{ type: "string" }], [message])` 来编码消息，导致生成了完整的 ABI 编码格式而不是原始字节。

**解决方案**：
```typescript
//  错误
const encodedMessage = encodeAbiParameters([{ type: "string" }], [callMessage]);

//  正确
const messageBytes = toHex(callMessage);  // 直接转为 UTF-8 十六进制字节
```

**关键点**：
- 合约期望 `bytes calldata message`（原始字节）
- 使用 `toHex()` 从 viem 库直接转换

#### 5.2 RPC 端点认证问题
- athens.rpc.zetachain.com: 返回 401 Unauthorized
- g.alchemy.com: 需要 API key
- **解决方案**: 使用 https://7001.rpc.thirdweb.com（无 API key）

#### 5.3 依赖管理
```bash
# 更新 Foundry 依赖
forge soldeer update

# 前端依赖
yarn install  # 或 npm install
```

---

### 六、前端测试步骤

#### 6.1 环境配置
1. 在 MetaMask 中添加 ZetaChain testnet 网络
   - Network Name: ZetaChain testnet
   - RPC URL: https://7001.rpc.thirdweb.com
   - Chain ID: 7001
   - Currency Symbol: ZETA

2. 从 [faucet](https://zetachain.faucetme.pro/) 获取测试 ZETA

#### 6.2 启动前端
```bash
cd front_dir
npm run dev
# 访问 http://localhost:3000
```

#### 6.3 测试流程
1. 连接 MetaMask 钱包
2. 切换到 ZetaChain testnet 网络
3. 在 Call 标签页中：
   - 输入接收地址：`0x00005b39E53648B6607d99d28827017692F438E9`
   - 输入 ZRC20 地址：`0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe`
   - 输入消息：`Hello`
4. 点击"Call"发送交易

---

### 七、本地开发命令速查

```bash
# 合约部署
cd contract/call
source .env
forge create Universal --rpc-url https://7001.rpc.thirdweb.com --private-key $PRIVATE_KEY --evm-version paris --broadcast

# 前端启动
cd front-end/call
npm run dev

# 前端构建
npm run build

# 依赖更新
forge soldeer update
yarn upgrade
```

---

**记录日期**: 2025-11-24
````
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
