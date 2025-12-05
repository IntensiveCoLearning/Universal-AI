---
timezone: UTC+8
---

# 李睿皓

**GitHub ID:** NoTks0808

**Telegram:** @bloxbound

## Self-introduction

一个不想被大道所限制，想在信息洪流中找到自我的人

## Notes

<!-- Content_START -->
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-12-05-1764945121694-__.png)
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-12-04-1764846947288-__.png)
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->


![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-12-03-1764769253696-__.png)

由gemini生成的货币交易tool，符合作业里的输出原则，经测试后能够使用qwen将自然语言转为json格式，为之后上链做好基础
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->



计算功能：

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-12-02-1764681333424-__.png)

转大写功能：

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-12-02-1764681365718-__.png)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->




选择的模型：qwen-max

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-12-01-1764567385196-__.png)

核心参数调用：

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-12-01-1764567431294-__.png)

结果输出：

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-12-01-1764567453395-__.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->





项目：利用zetachain的全链性，在web3制作一个音乐平台，创作者可以在平台上出售自己的音乐版权，用户利用各类货币支付并永久拥有。持有BTC用户可以直接在平台上购买以太坊中的音乐。

目标用户：主要以独立音乐人和各类音乐制作者的粉丝群体服务，可以考虑做一些defi的项目，比如用户将版权代币质押在 ZetaChain 的 DeFi 池中，不仅获得版税分红，还能获得 DeFi 协议的治理代币奖励。

想解决的问题：一个是独立音乐人在目前主流web2平台上能拿到的分红极其少，甚至还要倒贴钱，优秀的作品往往无法进入普通群众的视野，反而是垃圾音乐在挤占优秀制作的生存空间。所以这个平台的主要作用是给予制作人迅速且比主流web2平台高得多的红利，同时粉丝也能直接用各链中的代币为喜爱的音乐人给予支持。

跨链/通用资产使用方式：

1.  **Lock / Send (锁定/发送)：** 用户在原链（Native Chain）发送原生资产到 ZetaChain 的托管地址。
    
2.  **Logic (逻辑处理)：** ZetaChain 上的**全链合约**接管资产（变成 ZRC-20），在链上进行 Swap、质押、拆分等 DeFi 操作。这是\*\*“通用层”\*\*，所有复杂的金融逻辑都在这里发生。
    
3.  **Withdraw / Execute (提现/执行)：** 结果资产（也是 ZRC-20）请求提现，ZetaChain 签名控制原链账户将原生资产发回给用户。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->






![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-11-29-1764421337977-__.png)

选用了hello-world的demo，因为运行时发现swap的gas费用太高，领的水不够支付gas fee

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-11-29-1764421223082-__.png)

这是由于npm无法install，走的yarn路线

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-11-29-1764421717089-__.png)

部署的时候没有找到文件，让AAI重新帮我写了一个上传

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/NoTks0808/images/2025-11-29-1764421760439-__.png)

上传成功

> 我是从 **本地开发环境 (Localhost/Hardhat)** 发起的部署调用。  
> 具体来说，我配置好了包含 ZetaChain 测试网 RPC ([https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)) 的 hardhat.config.ts 文件，并通过本地终端运行部署脚本。Hardhat 使用我配置的私钥对交易进行签名，并将部署请求直接发送到了 ZetaChain Athens 3 测试网的节点。
> 
> 最终，我的 Universal.sol 智能合约代码被编译成字节码，并成功**上传/存储到了 ZetaChain 区块链上**。  
> ZetaChain 网络验证了我的交易，从我的钱包 (0x93B...) 中扣除了约 0.02 ZETA 作为 Gas 费，并在链上生成了一个全新的合约地址 (0x28E5...)。现在，这个合约已经存在于 ZetaChain 全链环境（Omnichain）中，具备了接收和处理跨链消息的能力，任何人都可以在区块链浏览器上查看到该合约的创建记录和代码状态。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->







ZRC-20和ERC-20的区别主要在于ZRC-20是zetachain的原生代币，能在zetachain上流通，是在zetachain上做开发时使用的主流代币，而ERC-20不能直接在zetachain上使用

通用资产可用于全链借贷
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->








项目：全链许愿墙

工具：CLI+Hardhat

环境：测试网
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->









关于Universal App:

Universal App是在zetachain部署的智能合约，可以在多链上进行多步骤操作，可以在EVM上部署，可以启动代币转账和合同调用 链条，能够通过 ZetaChain 的协议层，接收来自外部链的消息，处理逻辑，并触发对外部链的写操作

关于Gateway：

Gateway 是部署在**外部连接链（Connected Chains，如 Ethereum mainnet, BNB Chain）上的一个状态机接口，**持有并管理着所有进入 ZetaChain 生态的原生资产，是一个安全，稳定，属于zetachain的用户私人金库
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->










今天终于是把环境搭建完了，foundry的搭建在国内一定要提前了解好端口，要在梯子里开启局域网连接，AI给你推荐的直接关掉梯子是不可行的，因为GitHub没办法登上去，qwen的API调用刚开始可能是因为挂着梯子直接注册到国际服了，一直没有权限，最后关掉梯子用支付宝登录了以后才获得权限，有了API key，这个环境搭建对一个小白来讲真的非常痛苦，不过这样的学习确实会了不少关于API调用的知识
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->











基本环境已搭建完成
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
