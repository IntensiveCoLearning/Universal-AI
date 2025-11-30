---
timezone: UTC+8
---

# 1mmortals

**GitHub ID:** immortals1278

**Telegram:** @gzh1219c137

## Self-introduction

tg写的微信号

## Notes

<!-- Content_START -->
# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->
项目：

全链gas充值：用一条链的资产为多链gas费充值，服务于经常进行跨链操作的defi用户

全链一键存款：对刚进入区块链的用户更加方便的在各链上都能买入资产。只用在zetachain上操作更加方便
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

启动测试网

```
 npx zetachain localnet start
```

启动交换脚本

```
npx hardhat run scripts/swap.js --network localnet
```

从本地cetachain网络发起调用

发生了什么：

本地zetachain验证者确认我发起的交易，本地观察者检测到上述事件，gateway执行最终操作
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->


**多链资产被包装成zrc20然后在zetachain上使用于defi**

**作业**

zrc20相比于erc20多实现了跨链相关函数withdraw(),能发送跨链资产和消息

通用资产使用场景：全链支付钱包，商家只收solana，用户直接用eth支付

## demo

用本地网测试虽然要部署两个合约但只用forge创建一个项目

onRevert():从 ZetaChain 往外部链发一个跨链调用,如果目标链执行失败,执行onRevert中的回调逻辑

receive() external payable {}允许不带calldata接受裸eth，在zetachain场景中connected contract必须写这行用来退款或找零

gateway.depositAndCall()函数的RevertOptions结构体参数会在跨链调用失败，需要回转时给到通用合约中的onRevert()函数

本地网要用wsl运行，要在wsl里安装foundry
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->





选择CLI + Hardhat / Foundry做demo

## liquidity throughput

为了维护网络稳定：入站：ZetaChain 为每一种支持的代币都设定了一个预定义的最大存入金额（上限）避免网络因某一特定资产的大量涌入而承受过大的托管和铸造压力。

出站：对于从 ZetaChain 流向连接链的交易，ZetaChain 采用了一个速率限制器。

## 通用资产

zrc20

## 通用应用

前端->源链网关gateway->被zetachain上的验证者监控到->监控投票后达成共识->自动调用通用应用的oncall函数

一个实现了 UniversalContract 接口的智能合约。

重写onCall

onlyGateway确保只有网关能调用
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->








## cctx

跨链交易

用ZetaChain 的 Cosmos SDK HTTP API来追踪cctx，如果一个 CCTX 触发了另一个 CCTX（例如，一个入站交易导致了一个出站交易），可以用第一个 CCTX 的哈希作为输入查询api，来查询第二个 CCTX。

## zetachain的地址

支持evm地址和bech32地址，如果这两个地址是从同一个公钥派生出来的，那么它们代表的是同一个账户

Cosmos SDK：区块链开发开源框架，帮忙快速构建区块链

## Universal App

一个部署在 ZetaChain 上的单一、统一的智能合约，但它却可以直接管理和操作部署在其他多条区块链上的资产与数据。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->










RPC 让app连接到测试网：[https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)

Faucet 领测试币：[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)

Explorer 用于查看链上信息：[https://zetachain-athens.blockpi.network/](https://zetachain-athens.blockpi.network/)

## gateway

部署在zetachain和与zetachain连接的链上的一系列智能合约，用于向zetachain中存款/收款。

存款：原生代币发到安全托管合约，zetachain铸造ZRC-20（若从没铸造过要先部署一个新的ZRC-20合约）并发送给特定地址，取款同理。

call：在源链上调用gateway的send函数->调用zetachain上通用合约的onCrossChainCall函数执行任意逻辑（如交换、质押、铸造NFT等）

## gas费

入站调用用原生代币支付，仅支付源链的原生 Gas 费

出站调用需用zrc20支付取款费用，出站时ZetaChain 的验证者需要动用他们自己持有的目标链原生代币（如 ETH、BNB）来为你支付目标链上的 Gas 费。这笔费用就是为了补偿他们。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->














## zetachain

在ZetaChain上部署一次应用，就能自动连接到其生态中的所有区块链。未来当ZetaChain支持新的链时，应用自动获得与该新链交互的能力，无需修改或重新部署代码

ZetaChain的核心是通用以太坊虚拟机，完全兼容EVM。

zetachain本身的执行免gas费
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
