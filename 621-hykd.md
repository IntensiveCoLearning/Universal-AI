---
timezone: UTC+8
---

# 周灼霖

**GitHub ID:** 621-hykd

**Telegram:** @Akd

## Self-introduction

大家好！我是华南理工大学软件工程专业的大一学生。我对编程充满热情，尤其擅长C++，并正积极探索计算机科学的广阔世界。目前，我对AI大模型领域产生了浓厚的兴趣，惊叹于其强大的能力与无限潜力。作为一名初学者，我深知自己才刚起步，但我渴望在大学四年里夯实基础、不断钻研。希望能与志同道合的伙伴们交流学习，共同进步，期待未来能在这个领域贡献自己的一份力量。请多指教！

## Notes

<!-- Content_START -->
# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->
今天跑通了 ZetaChain 的 Swap Demo，从 Goerli 发送 ETH 到 BSC 测试网接收 BNB，亲身体验了“一次调用完成全链操作”的流畅感。

整个过程中，ZetaChain 作为中间层发挥了关键作用：监听我在 Goerli 的交易，在链上完成资产转换（ETH→wETH→wBNB），最后通过 Connector 在 BSC 上铸造对应资产。整个过程只用了 2-3 分钟，而且只需要支付源链的 gas 费。

最让我印象深刻的是用户体验的简化。相比传统跨链需要多步操作，ZetaChain 把复杂的技术细节都封装起来，让开发者能专注于业务逻辑。这确实是 DeFi 基础设施的一个重要进步。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->

通过今天的学习，我理解了 ZetaChain 的核心基础：ZRC-20 标准 和 通用资产。

从开发者视角看，ZRC-20 与普通 ERC-20 最直观的区别在于“原生跨链能力”。ERC-20 资产被禁锢在一条链上，要实现跨链需要依赖复杂的外部桥接。而 ZRC-20 生来就是多链的，它通过 ZetaChain 的跨链通讯协议，将比特币、以太坊上的原生资产自动映射为链上的统一表示。对开发者来说，这意味着可以直接调用标准的 deposit 和 withdraw 方法来实现资产在不同链间的流入流出，无需再自行部署和管理繁琐的桥接合约，极大地简化了全链应用的开发。

这种“通用资产”的能力，为许多革命性的应用场景打开了大门。一个典型的例子是 “通用 NFT 通行证”。想象一个游戏项目在 ZetaChain 上发行一套 NFT，玩家可以从以太坊、Polygon 或 BNB Chain 任何一条链上购买它。无论持有哪条链的版本，这个 NFT 在 ZetaChain 上都被视为同一个通用资产。凭借它，玩家可以在以太坊上进入专属社区，在 Polygon 上领取游戏道具，甚至在比特币生态中验证身份，实现“一证通全链”的无缝体验。这彻底打破了区块链世界的孤岛效应，让资产和权益真正实现自由流通。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->


**我的第一个 Universal App**

**名称**：

游戏成就通行证

**1.核心逻辑**：

**记录**：在 ZetaChain 上记录玩家在不同链游中的成就完成情况

**打印**：当玩家在Game A完成新手任务时：

记录玩家地址 + 成就时间戳 + 来源链

自动为玩家在Game B解锁“跨界勇者”称号

**前端展示**：显示玩家的跨链成就徽章和游戏履历

**2\. 工作流选择：**

**Hardhat**：插件生态丰富，测试框架成熟，最适合游戏类复杂逻辑的调试

**本地链**：即时确认交易，免费测试代币，完美适配多链游戏场景的快速迭代

![deepseek_mermaid_20251127_b67792.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-27-1764257656027-deepseek_mermaid_20251127_b67792.png)
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->



1\. Universal App（通用应用）是一种跨链智能合约应用，部署在 ZetaChain 上，但可以直接与多条外部区块链（如 Bitcoin、Ethereum、BNB Chain 等）交互，而无需依赖跨链桥或封装资产。它的核心特点是：

（1）统一流动性：用户可以在不同链上使用原生资产（如原生 BTC、ETH）与合约交互。

（2）无需跨链桥：ZetaChain 作为中心层，直接验证和处理多链交易，避免传统跨链桥的安全风险和复杂性。

（3）全链逻辑：开发者只需在 ZetaChain 上部署一次智能合约，即可管理多链状态和资产。

Universal App 像是“区块链世界的中央服务器”，用户从任何链发送交易，都由 ZetaChain 统一处理并返回结果。

2\. Gateway（网关）是 ZetaChain 与外部区块链之间的通信中继层，负责：

（1）监听事件：监控外部链（如 Ethereum）上的交易或合约事件。

（2）转发交易：将用户在其他链上的操作传递到 ZetaChain，并接收 ZetaChain 的指令，触发目标链的合约调用。

（3）资产托管：临时保管用户跨链转移的原生资产，确保原子交换。

Gateway 像是“邮局”，把不同链的消息打包并安全送达 ZetaChain，反之亦然。

3.

![1126.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-26-1764172399012-1126.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->




任务完成

1\. ZetaChain 测试网资源

RPC 端点测试成功[https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)

区块浏览器：[https://athens.explorer.zetachain.com](https://athens.explorer.zetachain.com)

水龙头：[https://labs.zetachain.com/faucet](https://labs.zetachain.com/faucet)

2\. Qwen API 连通性测试

成功获取 DashScope 专用 API Key

完成终端 API 调用测试并获得准确响应

测试内容: "请用一句话介绍ZetaChain"

模型回答: "ZetaChain 是一个跨链协议，旨在为开发者提供简单、安全的方式在不同区块链之间进行交互和数据传输。"

3\. ZetaChain CLI 安装

安装结果: 成功添加4个包，更新676个包

总结

ZetaChain 生态: 完整掌握测试网资源和使用方法，Qwen API: 熟练调用大语言模型服务，开发工具: 成功安装并配置 ZetaChain CLI，系统解决 npm 依赖冲突和 API 权限问题

未来两周学习目标

第一周:

1\. 使用 ZetaChain CLI 进行项目开发

2\. 深入理解跨链智能合约编写

3\. 掌握测试网部署和交互

第二周:

1\. 集成 Qwen AI 到跨链 DApp

2\. 构建完整的全链应用案例

3\. 准备项目演示和技术文档

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-25-1764085566623-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-25-1764085579679-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->





![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-24-1763995745508-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-24-1763997052598-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-24-1763997569752-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-24-1763997775045-image.png)![5db8cbf760dc718384aeaa1823e0a431.jpeg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/621-hykd/images/2025-11-24-1763997833750-5db8cbf760dc718384aeaa1823e0a431.jpeg)

已完成：

-   注册 / 配置好 ZetaChain 开发所需环境（浏览 Docs，确认能访问 Developers 页面）。
    
-   注册 Qwen 账号并确认可以进入控制台。
    
-   加入 ZetaChain 中国开发者社区，微信：arainqinqin（备注：通用 AI）。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
