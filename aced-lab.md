---
timezone: UTC+8
---

# 龙文华

**GitHub ID:** aced-lab

**Telegram:** @strggule_d

## Self-introduction

离职边缘，想看看自己还能干啥

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
NFT是non fungible tokens，非同质化代币的意思  
具体来说它可以指代几乎任何东西，其背后的资产是独特的，一片树叶，签名了的书，谁穿过的旧衣服，地球上的第一滴水。。  
而它的反面则是同质化代币，其背后的资产是标准化的  
两个同质化代币，如果其市场价值一样，那么进行交换也无妨。  
而两个非同质化代币，即使其市场价值一样，双方也很可能不愿意进行交换  
虽然非同质化代币所指代的资产可以代币化，但它其实依然还是独特的  
给货币进行编号，或者给虚假装备进行编号，虽然它们变得唯一了，但其实依然可以进行互换，所以它们不能算作NFT
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->

ZetaChain是有许多节点组成的分布式网络，这些节点又叫Validator  
在ZetaChain中有两种Validator，  
一种是Core Validators主要负责ZetaChain中区块的产生和状态更新，负责内部  
一种是Observer-Signer Validators，主要负责监视外部链上的跨链事件，和对外交易，负责对外。

外部链上的GateWay负责与交易发起者和接收者进行交互  
GateWay和Observer-Signer 的配合，完成了交易在外部链和ZetaChain之间的传递  
当交易能够在外部链和ZetaChain中进行传递之后，那么在ZetaChain中部署的智能合约自然就具有了与外部链进行交互的能力，成为了通用智能合约

代币在外部链和ZetaChain之间进行传递的时候，会在ZetaChain中统一被映射成ZRC-20，这样不同的代币也能一致的进行处理

交易传递的道路被连通，统一的货币被建立，于是ZetaChain就将各个链连接到了一起
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


ERC-20是Ethereum Request for Comment 20，是在以太坊上创建同质化代币的标准。  
ZRC-20是ERC-20代币或者比特币等等在ZetaChain上的映射

tokens 在区块链的语境下是代币的意思  
ZetaChina中的原生代币是ZETA

universal app就是zetachain上的智能合约，zetachain上的智能合约和一般智能合约不同，具有主动调用其他合约能力，一般的智能合约智只能被调用

外部链上的gateway的功能是作为交易的接收方或者发起方，与交易的发起方或者接受方进行交易。  
ZetaChian上的gateway的功能是将universal contract执行的结果传出到其他链上的gateway
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->



学习了ZetaChain中的一些Tools，  
[https://www.zetachain.com/docs/reference](https://www.zetachain.com/docs/reference)

Foundry是用于开发测试智能合约的一组工具，  
其中anvil是对以太坊的模拟  
ZetaChain中的localnet也是如此，但它的功能更强大还包括了不兼容EVM的链。  
testnet是对mainnet的复制，用于开发测试

faucet是用于获取testnet中的测试币的一些地点  
如果使用localnet进行测试则不需要领取gas费

Explorers是用于浏览交易，合约，搜索地址的浏览工具

RPC就像HTTP API一样，HTTP API是与外部网站进行交互的接口，而RPC则是外部与区块链进行交互的接口，所有外部对区块链的操作都是通过RPC进行的。区块链内部节点之间则通过P2P进行通信。智能合约则在节点内的EVM中执行，不通过P2P也不过通过RPC，也没有对外的网络能力，它是一个“沙盒里的纯函数 + 状态机”。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->






node -v

v22.14.0

yarn -v

1.22.22

git --version

git version [2.52.0.windows](http://2.52.0.windows).1

jq --version

jq-1.8.1

forge --version

forge Version: 1.4.4-stable

zetachain --version

7.4.0  
  
  
搭建了环境，学习了 First Universal Contract  
ZetaChain是一个想将孤立的各个区块链连接起来的区块链，起到一个桥梁的作用。  
ZetaChain相当于是一个国际贸易中心，各国的人带着各国的钱，可以在这里换汇，买卖资源(传输数据)，买卖服务(执行合约)。  
区块链中只有一种行为——交易(transaction)，一切活动都是基于交易  
目前的理解是：ZetaChain的实现似乎是在各个区块链中都创建了一个GateWay，当进行跨链交易(A链-->B链)的时候，A链中会进行：A链发起地址-->A链GateWay的交易，然后ZetaChain中的oberser观察到了这一交易，于是B链接GateWay会主动发起：B链GateWay-->B链目标地址的交易。 所以其实ZetaChain在所有的区块链上都有账户，所有交易又ZetaChain来进行中转。  
而这里“B链GateWay-->B链目标地址的交易”要求ZetaChain一开始就拥有B链上的资金，这样的话如果交易量大，ZetaChain也许会出问题？
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
