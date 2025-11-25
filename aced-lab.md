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
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
学习了ZetaChain中的一些Tools，  
[https://www.zetachain.com/docs/reference](https://www.zetachain.com/docs/reference)

testnet是对mainnet的复制，用于开发测试

localnet也是如此，但它运行于本地

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
