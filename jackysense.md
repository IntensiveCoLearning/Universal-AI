---
timezone: UTC+8
---

# Jacky

**GitHub ID:** jackysense

**Telegram:** @jacky_luxue

## Self-introduction

Web3 builder

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
**浏览器、水龙头**

explorers: [https://www.zetachain.com/docs/reference/explorers](https://www.zetachain.com/docs/reference/explorers)

faucet: [https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)

network: [https://www.zetachain.com/docs/reference/network/details](https://www.zetachain.com/docs/reference/network/details)

RPC: [https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

contracts address: [https://www.zetachain.com/docs/reference/network/contracts](https://www.zetachain.com/docs/reference/network/contracts)

**1.安装cli**

npm install -g zetachain@latest

**2.创建helloworld项目**

zetachain new 选hello

**3.启动本地网络**

zetachain localnet start

![dd7034a1-7c89-4d1d-87d8-a46f418395de.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764077716227-dd7034a1-7c89-4d1d-87d8-a46f418395de.png)

**Qwen API 的简单请求：**

![3a8fbf91b0d9dfe9c2c27ef268b0ad46.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764077332232-3a8fbf91b0d9dfe9c2c27ef268b0ad46.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


zeta 通用AI原理 :ZetaChain 有个Gateway 协议作为桥梁：从通用应用程序向连接链上的合约调用和提取代币。

**应用实例解析**‌  
以问题中描述的ETH兑换BNB流程为例：

‌**步骤分解：**‌

1.  ‌**接收阶段**‌：从以太坊接收6 ETH和"我想要BNB"的消息数据
    
2.  ‌**交换阶段**‌：在ZetaChain上通过去中心化交易所将6 ZRC-20 ETH兑换为ZRC-20 BNB
    
3.  ‌**跨链执行**‌：调用网关合约撤回ZRC-20 BNB，并在BNB链上执行合约调用
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-24-1763991106936-image.png)

**通用应用：**

-   可以接收来自连接链上用户和合约的合约调用和代币
    
-   可以触发合约调用和代币转移到连接的链
    
-   可以自动处理跨链交易的 gas 费用
    
-   完全兼容 EVM 链（如以太坊、BNB 和 Polygon）、比特币、Solana、TON 和 Sui。对 Cosmos（通过 IBC）和其他链的支持即将推出
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
