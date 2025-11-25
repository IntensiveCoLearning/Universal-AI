---
timezone: UTC+8
---

# JunzheLiu

**GitHub ID:** JunZ-Leo

**Telegram:** @junzhecc

## Self-introduction

期望能够通过这次的残酷残酷共学提高下自己的技术能力，并认识更多的新朋友🤞

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
````markdown
# ZetaChain + Qwen API 环境与工具实战

**日期**: 2025-11-25

## 今日学习目标

- ✅ 本地/云端完成基础开发环境落地
- ✅ 安装并测试 ZetaChain CLI
- ✅ 记录测试网 RPC、Faucet、Explorer 信息
- ✅ 完成 Qwen API 连通性测试

## 任务 1: 安装 ZetaChain CLI

### 步骤

```bash
# 克隆仓库
cd /root/canku
git clone https://github.com/zeta-chain/cli.git zetachain-cli

# 进入目录
cd zetachain-cli

# 安装依赖（注意：使用 --legacy-peer-deps 解决版本冲突）
npm install --legacy-peer-deps --no-audit --no-fund

# 测试 CLI
npx tsx src/index.ts --help
```

### 结果

✅ ZetaChain CLI 安装成功，支持以下命令：

- `new` — 创建新的通用合约项目
- `accounts` — 管理多链账户
- `query` — 查询 ZetaChain 数据
- `faucet` — 从水龙头获取测试 ZETA
- `zetachain` — 跨链调用相关命令
- `evm`, `solana`, `sui`, `bitcoin`, `ton` — 各链集成命令
- `localnet` — 本地开发环境
- `mcp` — MCP 服务器管理
- `ask` — AI 助手

---

## 任务 2: ZetaChain 测试网信息记录

### 🌐 网络详情

| 项目 | 值 |
|------|-----|
| Chain ID (EVM) | `7000` |
| Chain ID (Cosmos) | `zetachain_7000-1` |
| Denom | `azeta` |
| Symbol | `ZETA` |
| Decimals | `18` |
| Bech32 前缀 | `zeta` |
| HD Path | `m/44'/60'/0'/0/0` |

### 🔌 RPC 端点

#### 主要公开端点

| 类型 | 提供商 | 端点 |
|------|--------|------|
| EVM RPC | Blockpi | `https://zetachain-evm.blockpi.network/v1/rpc/public` |
| EVM RPC | Allthatnode | `https://zetachain-mainnet.g.allthatnode.com/archive/evm` |
| EVM RPC | dRPC | `https://zeta-chain.drpc.org` |
| Cosmos SDK HTTP | Blockpi | `https://zetachain.blockpi.network:443/lcd/v1/public` |
| Tendermint RPC | Blockpi | `https://zetachain.blockpi.network:443/rpc/v1/public` |

#### 私有端点（推荐开发者）

- [AllThatNode](https://www.allthatnode.com/zetachain.dsrv)
- [BlockPI](https://blockpi.io/zeta)
- [InfStones](https://infstones.com/fast-api)
- [Blast API](https://blastapi.io/chains/zetachain)
- [dRPC](https://drpc.org/chainlist/zeta-chain)
- [Alchemy](https://www.alchemy.com/zetachain)

### 💧 Faucet（水龙头）

获取 ZetaChain 测试币：

| 来源 | 支持链 |
|------|--------|
| Google Cloud Web3 | ZetaChain ZETA |
| FaucetMe | ZetaChain ZETA |
| Optimism | Ethereum, Base |
| Chainlink | ETH, Base, Avalanche, Arbitrum, Polygon |

### 🔍 Block Explorer

| 名称 | 特点 |
|------|------|
| **Blockscout** | EVM explorer, 支持 GraphQL API |
| **ExploreMe** | EVM + Cosmos explorer |
| **Mintscan** | Cosmos explorer |
| **Ping.pub** | Cosmos explorer |
| **Polkachu** | Node operator dashboard |

---

## 任务 3: Qwen API 连通性测试

### 📚 Qwen API 信息

#### API 端点

**OpenAI 兼容模式**（推荐）：

```
POST https://dashscope-intl.aliyuncs.com/compatible-mode/v1/chat/completions
```

**DashScope 原生协议**：

```
POST https://dashscope-intl.aliyuncs.com/api/v1/services/aigc/text-generation/generation
```

#### 支持的模型

| 模型 | 描述 | 用途 |
|------|------|------|
| `qwen-turbo` | 高速 | 低延迟需求，较低成本 |
| `qwen-plus` | 均衡 | 推荐：一般用途 |
| `qwen-max` | 高性能 | 复杂任务 |
| `qwen-max-longcontext` | 长上下文 | 支持 200k token 输入 |
| `qwen3-max-preview` | Qwen3 | 预览版，最强性能 |
| `qwen-vl-plus` | 视觉语言 | 图像理解 |
| `qwen-coder` | 代码 | 代码生成专用 |

### 测试工具

创建了两个测试脚本：

1. **`test_qwen_api.sh`** — Shell 脚本，使用 curl
2. **`test_qwen_api.py`** — Python 脚本，更详细的输出

#### 快速测试

```bash
# 设置 API Key
export DASHSCOPE_API_KEY='sk-xxxxx'

# 运行 Python 测试脚本
cd /root/canku
python3 test_qwen_api.py
```

#### 调用示例（curl）

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $DASHSCOPE_API_KEY" \
  -d '{
    "model": "qwen-plus",
    "messages": [
      {"role": "user", "content": "你好"}
    ],
    "temperature": 0.7,
    "max_tokens": 500
  }' \
  https://dashscope-intl.aliyuncs.com/compatible-mode/v1/chat/completions
```

#### 调用示例（Python）

```python
from openai import OpenAI

client = OpenAI(
    api_key="sk-xxxxx",
    base_url="https://dashscope-intl.aliyuncs.com/compatible-mode/v1"
)

response = client.chat.completions.create(
    model="qwen-plus",
    messages=[
        {"role": "user", "content": "你好"}
    ]
)
print(response.choices[0].message.content)
```

### 获取 API Key

1. 访问 [Alibaba Cloud Model Studio](https://modelstudio.aliyun.com/)
2. 注册或登录阿里云账户
3. 创建 API Key
4. 配置环境变量：`export DASHSCOPE_API_KEY='sk-xxxxx'`

---
````

![Screenshot 2025-11-25 165615.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/JunZ-Leo/images/2025-11-25-1764061097682-Screenshot_2025-11-25_165615.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->

\## 概要

在本地 ZetaChain localnet (anvil) 上进行了实践操作，目标是熟悉本地链交互、ERC-20 查询与转账，（利用VSCode远程SSH到Linux服务器进行开发）>

\- 创建并运行 Node 工具 `scripts/interact.js`（查看 ETH、查询 ERC‑20、发送 ERC‑20）。

\- 查询 `USDC.ETH` 与 `zetaToken` 余额。

\- 生成临时地址并从默认钱包转出 1 USDC（已成功，交易已确认）。

\- 抓取并保存交易对象与回执到 `tx.json` 与 `receipt.json`。

\## 环境与关键信息

\- Local RPC: `http://127.0.0.1:8545`

\- Default wallet: `0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266`

\- Default private key (anvil): `0xac0974be...f2ff80`

\- USDC.ETH (local): `0x1fA02b2d6A771842690194Cf62D91bdd92BfE28d`

\- zetaToken (local): `0x7a2088a1bFc9d81c55368AE168C2C02570cB814F`

\- 临时收款地址（示例）: `0x211A65Cd9CE07Cc64Ba712228dcd4489FB013ECc`

\- 转账交易哈希: `0x6ca5c4768d01f9a9bb30a4afa6a5051f5d13036d47bfba3872b9e8a6fdd5b2dd`

保存文件（路径均在 `/root/canku`）

\- `scripts/interact.js` — 交互脚本（已创建）

\- `tx.json` — 保存的交易对象

\- `receipt.json` — 保存的交易回执（包含 logs）

\## 已执行的可复现命令

1\. 安装并运行交互脚本（根目录）：

```
cd /root/canku
npm install --no-audit --no-fund
node scripts/interact.js balance
```

2\. 查询 ERC‑20 余额（注意 --，防止 minimist 把 0x 地址解析为数字）：

```
bash
node scripts/interact.js token -- 0x1fA02b2d6A771842690194Cf62D91bdd92BfE28d
```

3\. 生成临时收款地址：

```
bash
node -e "const ethers=require('ethers');console.log(ethers.Wallet.createRandom().address)"
```

4\. 发送 1 USDC（示例）：

```
bash
node scripts/interact.js send -- 0x1fA02b2d6A771842690194Cf62D91bdd92BfE28d 0x211A65Cd9CE07Cc64Ba712228dc
```

5\. 获取并保存交易信息

```
`bash
cat tx.json
cat receipt.json
```

\## 关键代码片段

\- interact.js: 查询 ETH 余额与 ERC‑20, 发送 ERC‑20

```
`js
const ethers = require('ethers');
const provider = new ethers.providers.JsonRpcProvider(process.env.RPC_URL || 'http://127.0.0.1:8545');
const wallet = new ethers.Wallet(process.env.PRIVATE_KEY || '<priv>', provider);

// 查询 ETH
const bal = await provider.getBalance(await wallet.getAddress());
console.log('ETH', ethers.utils.formatEther(bal));

// 查询 token
const token = new ethers.Contract(tokenAddress, ['function balanceOf(address) view returns (uint256)', 'f>
const raw = await token.balanceOf(await wallet.getAddress());
const decimals = await token.decimals();
console.log('token balance', ethers.utils.formatUnits(raw, decimals));

// 发送 token
const tWithSigner = token.connect(wallet);
const tx = await tWithSigner.transfer(recipient, ethers.utils.parseUnits(amountStr, decimals));
await tx.wait();
```

\## 关于 `call` 示例（跨链）

\- `call` 示例位于 `/root/canku/call`，包含 `contracts/Universal.sol` 與 `contracts/Connected.sol` 以及 CL>

\- 用 Foundry `forge`) 成功编译`forge build`），并能够用 `forge create` 或脚本把合约部署到本地 RP>

\- 用 `forge` 部署 `Universal` 和 `Connected` 到 localnet 并运行 `commands/connected/call` 示例（我已就\[>

\- 或基于 localnet 上已存在的合约（registry）进行交互示例。

示例 Foundry 部署命令：

```
bash
# 在 /root/canku/call
forge build
forge create contracts/Universal.sol:Universal --rpc-url http://127.0.0.1:8545 --private-key <PRIVATE_KEY>
forge create contracts/Connected.sol:Connected --rpc-url http://127.0.0.1:8545 --private-key <PRIVATE_KEY>
```
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
