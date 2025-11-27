---
timezone: UTC+8
---

# 害虫

**GitHub ID:** jeffierw

**Telegram:** @yepwan

## Self-introduction

小白

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
# Day 4

1.  自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么?
    

-   跨链支付的 ai 投资助手？
    

2.  测试网使用 Hardhat
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

# Day 3

1.  什么是 Universal App？
    

-   Universal App 是 ZetaChain 上的智能合约，原生连接以太坊、BNB 和比特币等其他区块链。
    

2.  什么是 Gateway?
    

-   是一个接口，作为连接链上的合约与 ZetaChain 上的通用应用之间交互的统一入口。
    
-   分别部署在连接链和 Zeta 链，处理双方的合约调用逻辑。
    

3.  ZetaChain 跨链示意图
    

-   [figma](https://www.figma.com/file/mYXNTORUuvGVaQ01SF7h9Y?type=design&node-id=0%3A1&mode=design)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


# Day 2

1.  安装了 zetachain cli
    

-   npm install -g zetachain@latest
    
-   导入本地 eth 钱包：zetachain accounts import --type evm --private-key <私钥>
    
-   新建项目：zetachain new
    

2.  钱包连接 zetachain testnet
    

-   链 ID: 7001
    
-   链 rpc: [https://zetachain-athens-evm.blockpi.network/v1/rpc/public](https://zetachain-athens-evm.blockpi.network/v1/rpc/public)
    
-   faucet: [https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)
    

3.  Qwen API 调用
    

-   获取 api-key: [https://bailian.console.aliyun.com/?tab=model#/api-key](https://bailian.console.aliyun.com/?tab=model#/api-key)
    
-   curl 调用： `shell curl -X POST https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions \ -H "Authorization: Bearer $Qwen_API_KEY" \ -H "Content-Type: application/json" \ -d '{ "model": "qwen-plus", "messages": [ { "role": "system", "content": "You are a helpful assistant." }, { "role": "user", "content": "你是谁？" } ] }' {"choices":[{"message":{"role":"assistant","content":"我是通义千问（Qwen），由阿里巴巴云研发的超大规模语言模型。我能够回答问题、创作文字，如写故事、公文、邮件、剧本等，还能进行逻辑推理、编程，甚至表达观点和玩游戏。我支持多种语言，包括但不限于中文、英文、德语、法语、西班牙语等。\n\n如果你有任何问题或需要帮助，欢迎随时告诉我！"},"finish_reason":"stop","index":0,"logprobs":null}],"object":"chat.completion","usage":{"prompt_tokens":22,"completion_tokens":84,"total_tokens":106,"prompt_tokens_details":{"cached_tokens":0}},"created":1764084567,"system_fingerprint":null,"model":"qwen-plus","id":"chatcmpl-f89dbfaf-fdd5-4d4a-a2c8-ab47ec1b3c63"}%`
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



# Day 1

学习了[zeta文档](https://www.zetachain.com/docs/start)中的简介部分

1.  是什么 ZetaChain 是首个实现所有区块链生态系统原生连接的通用区块链。
    
2.  为什么
    
    1.  通过单一合约与大部分区块链实现互操作性
        
    2.  向前的兼容性，不需要为未来产生的新区块链修改部署的合约
        
    3.  与市面跨链方案不同，所有操作的代币都是原生的
        
    4.  通用UEVM环境，开发者可直接上手部署合约
        
    5.  跨链操作只需在连接链支付gas,而无需在操作链支付
        
3.  如何使用
    
    1.  Zeta实现了一个“Universal Apps”，其实是一个合约，可以接受来自任何连接链的合约调用、消息和Token转移。
        
    2.  支持比特币，并且通过gas费抽象，通过UEVM,在连接链获取gas费用，使得跨链时就可以支付足够的gas费已完成跨链。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
