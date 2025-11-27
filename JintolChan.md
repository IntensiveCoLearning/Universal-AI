---
timezone: UTC+8
---

# Jintol

**GitHub ID:** JintolChan

**Telegram:** @JintolOfficial

## Self-introduction

区块链技术爱好者

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
# **Swap**

Swap合约是一个部署在 ZetaChain 上的通用应用程序。它使用户能够通过一次跨链调用在不同区块链之间进行代币兑换。代币以 ZRC-20 的形式接收，可以选择使用 Uniswap v2 流动性进行兑换，并提取回连接的链。

Swap合约执行以下步骤：

1.  接收来自连接链的跨链调用以及原生代币或 ERC-20 代币。
    
2.  解码消息有效载荷以提取：
    
    -   目标代币（ZRC-20）的地址
        
    -   收件人在目的地链上的地址
        
3.  查询将目标代币发送回目标链所需的提现 gas 费用。
    
4.  如果提现到连接的链，则使用 Uniswap v2 池将一部分传入的代币兑换成 ZRC-20 gas 代币，以支付提现费用。
    
5.  将剩余余额兑换成目标代币。
    
6.  将交换后的代币提取给目标链上的接收者。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


# Universal App

universal app是 ZetaChain 上的智能合约，它原生连接到以太坊、BNB 和比特币等其他区块链。

与普通合约不同，universal app可以接收来自任何已连接链的合约调用、消息和代币转移。它还可以触发合约调用，并在已连接链上进行代币转移。这些能力使通用应用能够协调跨链的复杂多步骤交易。

例如，比特币用户可以通过通用应用程序将USDC发送给以太坊上的收款人。以太坊用户可以在ZetaChain上购买NFT，并将其发送到他们在BNB链上的账户。

universal app程序部署在 ZetaChain 的通用 EVM 上，该 EVM 扩展了全链互操作性功能。这意味着您现有的合约可以在 ZetaChain 上开箱即用，并且经过一些修改后，它们可以获得强大的全链功能。

# Gateway

Gateway 是一个接口，它作为 ZetaChain 上连接链上的合约与通用应用程序之间交互的统一入口点。

ZetaChain 上的 Gateway 可促进对外交易：从通用应用程序向连接链上的合约调用和提取代币。

Gateway支持以下功能：

-   将 ZRC-20 代币作为原生 gas 或 ERC-20 代币提取到已连接的链上
    
-   将 ZETA 代币提取到连接的链
    
-   将代币提取到已连接的链上并进行合约调用
    
-   在连接链上进行合约调用
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->




# ZetaChain ZETA 水龙头

[https://cloud.google.com/application/web3/faucet](https://cloud.google.com/application/web3/faucet)

[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->





# 开发环境配置

### 安装node.js

```
sudo apt install nodejs

```

### 安装npm

```
npm install --global npm
```

### 安装Git

```
sudo apt install git
```

### 安装 jq

```

sudo apt install jq
```

### 安装 Foundry

```
curl -L https://foundry.paradigm.xyz | bash
```

### 安装 ZetaChain CLI

```
npm install -g zetachain
zetachain query chains list //尝试运行
```
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
