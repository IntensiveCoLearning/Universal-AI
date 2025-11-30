---
timezone: UTC+8
---

# 宙语

**GitHub ID:** bentong-chain

**Telegram:** @NFTEYE_admin

## Self-introduction

区块链信仰者，从事Web3开发，希望通过技术将区块链与AI结合改变整个世界。

## Notes

<!-- Content_START -->
# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->
第一个合约

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;
 
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
 
contract Universal is UniversalContract {
    event HelloEvent(string, string);
 
    function onCall(
        MessageContext calldata context,
        address zrc20,
        uint256 amount,
        bytes calldata message
    ) external override onlyGateway {
        string memory name = abi.decode(message, (string));
        emit HelloEvent("Hello: ", name);
    }
}
```
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

![Universal EVM.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/bentong-chain/images/2025-11-29-1764430913796-Universal_EVM.png)
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->


### Universal App

Universal App是部署在ZetaChain上的智能合约，可以与Ethereum、BSC、Bitcoin等区块链进行连接。既可以接受来自其他链的合约调用、消息和代币转账，也可以触发其他链上的合约调用和代币转账。  
Universal App部署在ZetaChain的Universal EVM上。Universal EVM在原EVM上扩展了全链互操作功能。

![1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/bentong-chain/images/2025-11-28-1764344182500-1.png)

### Gas抽象

![2.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/bentong-chain/images/2025-11-28-1764344193931-2.png)
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->



### ZetaChain Mainnet

网络名称

ZetaChain Mainnet

链 ID

7000

RPC

[https://zetachain-evm.blockpi.network/v1/rpc/public](https://zetachain-evm.blockpi.network/v1/rpc/public)

货币符号

zeta

浏览器

[https://zetascan.com/](https://zetascan.com/)

### ZetaChain Testnet

网络名称

ZetaChain Testnet

链 ID

7001

RPC

[https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)

货币符号

zeta

浏览器

[https://testnet.zetascan.com](https://testnet.zetascan.com)

### ZetaChain网络

[页面入口](https://www.zetachain.com/docs/reference/network/details)

### ZetaChain RPC

[页面入口](https://www.zetachain.com/docs/reference/network/api)

### ZetaChain 浏览器

[主网](https://zetascan.com/)

[测试网](https://testnet.zetascan.com/)

### 阿里云百炼

[页面入口](https://bailian.console.aliyun.com/)
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->




\### ZetaChain CLI

\#### 安装

\`\`\`

npm install -g zetachain

\`\`\`

ZetaChain CLI可以进行跨链、转账代币、跟踪跨链交易、管理多个网络上的账号等操作。

\#### 命令

\`\`\`

zetachain

\`\`\`

可以查看使用该命令的方法说明，包括：

\`\`\`

new \[options\] Create a new universal contract project.

accounts Manage accounts for all connected chains

query|q Query ZetaChain data and connected chain information

faucet \[options\] Request testnet ZETA tokens from the faucet

zetachain|z Withdraw tokens and call contracts on connected chains

evm Deposit tokens and call universal contracts from EVM

solana Deposit tokens and call universal contracts from Solana

sui Deposit tokens and call universal contracts from Sui

ton Deposit tokens and call universal contracts from TON

bitcoin|b Deposit BTC and call universal contracts from Bitcoin

localnet Local development environment

mcp MCP server management commands

docs \[options\] Display help information for all available commands and their subcommands

ask \[prompt...\] Chat with ZetaChain Docs AI

\`\`\`

\### ZetaChain Mainnet

\#### 网络名称

ZetaChain Mainnet

\#### 链 ID

7000

\#### RPC

[https://zetachain-evm.blockpi.network/v1/rpc/public](https://zetachain-evm.blockpi.network/v1/rpc/public)

\#### 货币符号

zeta

\#### 浏览器

[https://zetascan.com/](https://zetascan.com/)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
