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
# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->
# 使用VUE实现Universal APP前端

```
<script setup>
import {init, useOnboard} from '@web3-onboard/vue'
import injectedModule from '@web3-onboard/injected-wallets'
import {BrowserProvider, ethers, ZeroAddress} from 'ethers'
import {computed, ref, watch} from "vue";
import Util from '@/utils/common-util.js'
import { evmCall } from "@zetachain/toolkit/chains/evm"

const UNIVERSAL_APP_CONTRACT = '0x31A4708D1DAd97f91F89D4ffc10DC3Ef7685876b'
const CCTX_POLLING_URL = 'https://zetachain-athens.blockpi.network/lcd/v1/public/zeta-chain/crosschain/inboundHashToCctxData'

// 实例化一个injected钱包配置
const injected = injectedModule()
// 支持的链，每个链的信息包括：id、token、label、rpcUrl
const chains = [
  {
    id: '0x61',
    token: 'tBNB',
    label: 'BSC Testnet',
    rpcUrl: 'https://data-seed-prebsc-1-s1.bnbchain.org:8545'
  },
  {
    id: '0xaa36a7',
    token: 'SepoliaETH',
    label: 'Ethereum Sepolia',
    rpcUrl: 'https://1rpc.io/sepolia'
  }
]
// 初始化配置支持的钱包和链
const web3Onboard = init({
  wallets: [injected],   // 表示UI里会展示所有浏览器注入类钱包
  chains                 // 配置支持的链
})

// 必须在init后调用， useOnboard() 拿到响应式的对象
// wallets: 当前所有曾连接过的钱包列表
// connectWallet: 打开连接弹窗
// disconnectConnectedWallet: 断开当前选中的connectedWallet
// connectedWallet: 当前已连接的钱包对象
const { wallets, connectWallet, disconnectConnectedWallet, connectedWallet, connectedChain } = useOnboard()

const connect = async () => connectWallet()
const disconnect = async () => disconnectConnectedWallet()

let ethersProvider = ref<BrowserProvider | null>null
let signer = ref<null>null
let curAddress = ref(null)
watch(connectedWallet,  async (wallet) => {
  if (wallet && wallet.provider) {
    ethersProvider = new BrowserProvider(wallet.provider)
    // 这里可以继续做：拿 signer、加载合约等等
    signer = await ethersProvider.getSigner()
    curAddress.value = await signer.getAddress()

    console.log(connectedChain.value)
  } else {
    ethersProvider = null
    signer = null
    curAddress.value = null
  }
})


const sendTransaction = async () => {
  if (!signer) {
    return
  }

  // zetachain
  const evmCallParams = {
    receiver: UNIVERSAL_APP_CONTRACT,
    types: ['string'],
    values: ['Tang Hao'],
    revertOptions: {
      callOnRevert: false,
      revertAddress: ZeroAddress,
      revertMessage: '',
      abortAddress: ZeroAddress,
      onRevertGasLimit: 1000000,
    }
  }

  const evmCallOptions = {
    signer,
    txOptions: {
      gasLimit: 1000000
    }
  }

  const result = await evmCall(evmCallParams, evmCallOptions)
  await result.wait()

  const hash = result.hash
  console.log(hash)

  setTimeout(async () => {
    const response = await fetch(CCTX_POLLING_URL + '/' + hash)
    if (response.ok) {
      const data = await response.json()
      console.log(data)
    }
  }, 10000)

}



</script>

<template>
  <div>
    <el-button type="primary" v-if="connectedWallet" @click="disconnect">{{Util.omitAddress(curAddress, 10)}}</el-button>
    <el-button @click="connect" v-else>连接钱包</el-button>

    <div style="width:100%;margin-top:50px">
      <el-button @click="sendTransaction">发送交易</el-button>
    </div>
  </div>
</template>
```
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->

在链上部署

1\. 设置私钥

在项目目录下创建文件.env

```
PRIVATE_KEY=********
```

将文件格式转成Unix格式  
在windows下创建的文件，通过`source .env`加载出来，在私钥末尾会存在^M（回车符），需要转成Unix格式

```
dos2unix .env
```

在wsl命令行下加载私钥到环境变量

```
source .env
```

2\. 部署合约

```
UNIVERSAL=$(forge create Universal \
  --rpc-url https://zetachain-athens-evm.blockpi.network/v1/rpc/public \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --json | jq -r .deployedTo) && echo "$UNIVERSAL"
```

-   rpc-url: 部署合约所在链的RPC URL
    
-   private-key: 私钥
    
-   echo "$UNIVERSAL": 将部署的合约地址回显
    

3\. 从连接链上call Universal App

```
npx zetachain evm call \
  --chain-id 84532 \
  --receiver $UNIVERSAL \
  --private-key $PRIVATE_KEY \
  --types string \
  --values hello
```

-   chain-id: 连接链的链ID
    
-   receiver: zetachain链上的Universal App合约地址
    
-   types: 表示被发送值的数据类型。这里是一个字符串。
    
-   values: 这是传递给Universal App的实际值。这里，字符串“hello”作为消息发送给 ZetaChain 上的Universal App合约
    

当该命令成功执行后，会生成连接链上交易哈希。

4\. 跟踪跨链交易状态

在连接链上发起交易后，ZetaChain 的协议会促进其跨链转移并在目标链（ZetaChain）上执行。  
跟踪命令：

```
npx zetachain query cctx --hash 交易哈希
```

输出示例：

```
84532 → 7001 ✅ OutboundMined
CCTX:     0x56f9bc09dc646b13aa713b56348e8a53ea39759146afad61e66973791b752e3bTx
Tx Hash:  0x89308870b0863c5ae48dc783059277cbcf4296b1b343413ac543418262a4ccbc (on chain 84532)
Tx Hash:  0x34edd96c8a7b2bd9d530de0e49bb5e8625204a77b77cc79133814e1814f79ebc (on chain 7001)
Sender:   0x4955a3F38ff86ae92A914445099caa8eA2B9bA32
Receiver: 0xFeb4F33d424D6685104624d985095dacab567151
Message:  0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000
```

-   84532 → 7001：这清楚地表明了从 Base Sepolia（链 ID 84532）到 ZetaChain（链 ID 7001）的跨链流动。该✅符号表示跨链交易的成功出站挖矿。
    
-   OutboundMined CCTX: 这是 ZetaChain 上跨链交易（CCTX）的哈希值
    
-   Tx Hash(on chain 84532): 这确认了源链（Base Sepolia）上的原始交易哈希值
    
-   Tx Hash(on chain 7001): 这是 ZetaChain 执行时的交易哈希值，表明Universal App已成功在目的链上被调用。
    
-   Sender: 发件人在起始链上的地址。
    
-   Receiver: 接收合约的地址
    
-   Messsage: 传递的数据
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->


### 部署第一个合约

创建项目

```
npx zetachain@latest new --project hello
```

-   使用npx执行最新版本的zetachain，创建一个名为hello的新项目目录
    

```
# 进入hello目录
cd hello    

# 执行npm安装依赖包
npm install   

# 同步并更新由 Foundry Soldeer 管理的 Solidity 依赖，确保你的合同基于最新兼容的外部库版本构建。
forge solder update   
```

-   在windows下，需要在wsl中安装Foundry，然后通过`/mnt/盘符`进入项目目录，然后再执行`forge solder update`
    

项目目录下会创建一个示例合约：contracts/Universal.sol

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

说明：

-   MessageContext结构体
    

```
struct MessageContext {
    /// @notice 连接链上发送者地址
    /// @dev 该字段使用“字节”以保持与链无关，从而支持 EVM 链和非 EVM 链。
    /// 如果所连接的链是以EVM链，则 `senderEVM` 也将被赋予和该字段相同的值。
    bytes sender;
    
    /// @notice EVM链，发送者地址
    address senderEVM;
    
    /// @notice 连接链的链ID
    /// @dev 这会明确消息的来源链，从而使合约逻辑能够区分不同的来源。
    uint256 chainID;
}
```

-   zrc20：代表源链资产的 ZRC-20 代币地址。
    
-   amount: 转移的代币数量
    
-   message: 是跨链调用时随交易一起传过来的「任意数据载荷」
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->



[学习笔记](https://www.processon.com/view/link/692dba4f6f521468d3f8ff9c)
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->




[学习笔记](https://www.processon.com/view/link/692dba4f6f521468d3f8ff9c)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->





[笔记链接](https://www.processon.com/view/link/692dba4f6f521468d3f8ff9c)
<!-- DAILY_CHECKIN_2025-12-01_END -->

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
