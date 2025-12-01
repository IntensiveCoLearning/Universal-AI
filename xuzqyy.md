---
timezone: UTC+8
---

# xuzq

**GitHub ID:** xuzqyy

**Telegram:** @xuzhiqiang1

## Self-introduction

大家好，我是xuzq,接触web3两年了，对DeFi方面感兴趣，希望与各位大佬多多学习~

## Notes

<!-- Content_START -->
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
222
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

111
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->


## **一、ZRC-20 与普通 ERC-20 的直观区别（开发者视角）**

|   |   |   |
| --- | --- | --- |
| 部署位置 | 仅存在于单一链（如 Ethereum） | 存在于 ZetaChain 上，但代表其他链的资产（如 ETH 上的 USDT） |
| 跨链能力 | ❌ 无法直接在其他链使用（需桥接） | ✅ 原生支持跨链：用户从 ETH 存入 USDT → 自动 mint 为 ZRC-20 USDT → 可在 ZetaChain 合约中直接使用 |
| 转账目标 | 只能转给该链上的地址 | 可通过 ZetaChain 合约直接 withdraw 到其他链（如转回 ETH、Solana、甚至 Bitcoin） |
| 合约交互 | 只能被该链的合约调用 | 可被 ZetaChain 的通用合约（Universal Contract）直接操作，无需额外封装 |
| Gas 支付 | 用本链原生代币（如 ETH） | 用ZETA（ZetaChain 原生 Gas 代币） |
| 开发者操作 | transfer(to, amount) | 除transfer外，还可调用：withdrawAndCall(chainId, to, data)→ 直接跨链调用目标链合约 |
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->



今天看课程没看懂，明天再花时间看看回放
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->




**1\. Universal App（通用应用） 是什么？**

> **Universal App 是部署在 ZetaChain 上的一种智能合约，它能“天然地”和多条区块链（如 Ethereum、Bitcoin、Solana 等）直接通信和交互。**  
> 比如：一个用户从 Bitcoin 发起一笔交易，这个合约就能收到 BTC，并自动在 Ethereum 上调用另一个合约，全程只需一次用户操作。  
> 它打破了传统“一条链一个合约”的限制，真正做到“一次部署，全链可用”。

### **2\. Gateway（网关） 大概做什么？**

> **Gateway 是每条连接链（如 Ethereum、Solana）上的一个特殊合约（或 Bitcoin 上的地址），它是用户与 ZetaChain Universal App 之间的“入口”和“出口”。**

![Native Token Transfer](https://www.zetachain.com/docs/_next/image/?url=%2Fdocs%2F_next%2Fstatic%2Fmedia%2Fwhy-custody.32697805.png&w=3840&q=75)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->





1安装调试cli

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/xuzqyy/images/2025-11-25-1764079243379-image.png)

**2领水**

**网络名称:** ZetaChain Athens Testnet  
**RPC URL:** [https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)  
**链ID:** 7001

**Faucet 名称:** ZetaChain Triangle Faucet  
**网址:** [Web3 Faucet](https://cloud.google.com/application/web3/faucet)  
**说明:** 每24小时可领取一次测试ZETA。

**Explorer 名称:** ZetaChain Blockscout Explorer  
**网址:** [https://athens.explorer.zetachain.com](https://athens.explorer.zetachain.com)  
**用途:** 查询交易状态、钱包余额、合约代码。

3调用千问api

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/xuzqyy/images/2025-11-25-1764079481646-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->






| 能打开并浏览 ZetaChain Docs 和 Developers 页面 | ✅ |
| 注册阿里云账号，进入 DashScope 控制台，确认可使用 Qwen | ✅ |
| 加入中文开发者社区 | ✅ |
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
