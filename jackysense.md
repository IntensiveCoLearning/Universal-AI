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
# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->
通用应用

可以接收来自连接链上用户和合约的合约调用和代币

可以触发合约调用和代币转移到连接的链

可以自动处理跨链交易的 gas 费用

完全兼容 EVM 链、 Cosmos（通过 IBC）和其他链的支持

网关

Gateway 是一个接口，它作为 ZetaChain 上连接链上的合约与通用应用程序之间交互的统一入口点。

将原生 Gas 代币存入 ZetaChain 上的通用应用程序或账户

将支持的 ERC-20 代币（包括 ZETA 代币）存入 ZetaChain 上的通用应用程序或账户

存入原生 gas 代币并向通用应用程序发出合约调用（可传递任意数据）

存入受支持的 ERC-20 代币，并向通用应用程序发出合约调用（可传递任意数据）。

向通用应用程序发出合约调用（传递任意数据）

![ba5351a69bfb4ae8af46fe18eb2235f2.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-26-1764119951580-ba5351a69bfb4ae8af46fe18eb2235f2.jpg)
<!-- DAILY_CHECKIN_2025-11-26_END -->

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

**4.创建账户**

![15d259c8-a7cd-4590-9234-ea85224f53a0.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764077962082-15d259c8-a7cd-4590-9234-ea85224f53a0.png)

**5.forge build 编译通用合约、获取本地私钥、部署通用合约**

![cdc8d7c3-3af4-4573-bbca-b34783ed81b3.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764079280288-cdc8d7c3-3af4-4573-bbca-b34783ed81b3.png)

6 调用oncall

![ab540164-ca35-45bc-9b0b-b62e76c36611.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764080729822-ab540164-ca35-45bc-9b0b-b62e76c36611.png)

7.查看调用tx记录和事件，看到context来自 evm 的chainId，sender，zrc20为ETH.ETH地址

![b8030609-42e1-4ccc-a2f6-c3cd1e2ef058.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jackysense/images/2025-11-25-1764080814961-b8030609-42e1-4ccc-a2f6-c3cd1e2ef058.png)

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
