---
timezone: UTC+8
---

# Max

**GitHub ID:** Max-wht

**Telegram:** @Max

## Self-introduction

web3 developer

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
-   本地部署Swap合约, 使用zetachain实现swap跨链交换代币
    
    ```
    zetachain evm deposit-and-call \
      --rpc http://localhost:8545 \
      --chain-id 11155111 \
      --gateway $GATEWAY_ETHEREUM \
      --amount 0.001 \
      --types address bytes bool \
      --receiver $SWAP \     
      --private-key $PRIVATE_KEY \
      --values $ZRC20_BNB $RECIPIENT true
    ​
    From:   0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
    To:     0x5bf5b11053e734690269C6B9D438F8C9d48F528A on ZetaChain
    Amount: 0.001 native tokens
    Refund: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
    Call on revert: false
    ​
    Contract call details:
    Function parameters: 0x0000000000000000000000000000000000000000, 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266, true
    Parameter types: ["address","bytes","bool"]
    ​
    ? Proceed with the transaction? yes
    Transaction hash: 0x0b14edbce4863bcf8d6504a2982764e6a60a49b0e4993bec3033be6e6c7c2af7
    ```
    
-   部署到testnet
    
    ```
    UNIVERSAL=$(npx tsx commands deploy --private-key $SEPOLIA_PK | jq -r .contractAddress) && echo $UNIVERSAL
    0x713C7D391d24323509c258BeFE95d6B08C0f8274
    ```
    

简单说一说整个Swap的调用链路

用户的需求是从链A转移一部分资产到链B，期间zetachain将A与B连接 (用户无感)

首先用户需要调用A链中zetachain已经部署好的gateway网关合约中deposit-and-call这个函数，可以使用zetachain CLI。

```
zetachain evm -h                 
​
Usage: zetachain evm [options] [command]
​
Interact from EVM chains: call contracts on ZetaChain or deposit tokens (with or without a call).
​
Options:
  -h, --help                  display help for command
​
Commands:
  call [options]              Call a contract on ZetaChain from an EVM-compatible chain
  deposit-and-call [options]  Deposit tokens and call a contract on ZetaChain from an EVM-compatible chain
  deposit [options]           Deposit tokens to ZetaChain from an EVM-compatible chain
```

可以看出evm中有三个函数，分别是call， deposit-and-call，deposit

在deposit-and-call这个函数中，会"触发"zetachain中swap合约的onCall函数。这个函数会处理swap逻辑。

会有几个核心入参：`sender` `A.ZRC20` `amount` `B.ZRC20` `recipient`

swap的第一步是通过`A.ZRC20` `amount` `B.ZRC20` `withdraw`获得跨链操作需要的gasfee, gasZRC20 ( 这个参数会在之后的逻辑中与targetToken也就是B.ZRC20做比较) 和需要swap的数量`out`

第二部是emit一个swap事件

第三部是调用zetachain.gateway来交换代币。在gateway中，会首先销毁B.ZRC20 然后通知B链的gateway

第四部是B.gateway释放对应代币给`recipient`
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


# Day 01

## **实现：**

-   注册千问 api，在终端里面完成 api 的调用
    
    ```
    {
      "choices": [
        {
          "message": {
            "role": "assistant",
            "content": "我是通义千问，阿里巴巴集团旗下的超大规模语言模型。我能够回答问题、创作文字，比如写故事、写公文、写邮件、写剧本、逻辑推理、编程等等，还能表达观点，玩游戏等。如果你有任何问题或需要帮助，欢迎随时告诉我！"
          },
          "finish_reason": "stop",
          "index": 0,
          "logprobs": null
        }
      ],
      "object": "chat.completion",
      "usage": {
        "prompt_tokens": 22,
        "completion_tokens": 60,
        "total_tokens": 82,
        "prompt_tokens_details": { "cached_tokens": 0 }
      },
      "created": 1763953889,
      "system_fingerprint": null,
      "model": "qwen-plus",
      "id": "chatcmpl-424809e7-b2e6-4aac-b88d-949b1721a057"
    }
    ```
    

-   跑通 zetachain 的 hello demo , 可以在etherscan和zetascan中观测到交易
    
    ```
    [⠊] Compiling...
    No files changed, compilation skipped
    Deployer: 0x051bc958C4F12a608917DE400FbFB295561fd857
    Deployed to: 0xDF3B67F50e92852168Fb5cD6048D76cF3447D8a0
    Transaction hash: 0xed1d439f2101893f521bb29db112d74446968f7aad33c6ec89d1aa991b535b0e
    ```
    
    ```
    From:   0x051bc958C4F12a608917DE400FbFB295561fd857
    To:     0xDF3B67F50e92852168Fb5cD6048D76cF3447D8a0 on ZetaChain
    Call on revert: false
    ​
    Contract call details:
    Function parameters: max
    Parameter types: ["string"]
    ​
    ? Proceed with the transaction? yes
    Transaction hash: 0xc48f6c309a612d6e4c474ab2898689a40c3f59cde16839eec68efcfefed9a229
    ```
    
-   安装zetachain cli, 可以启动local chain 完成docs/build/Tutorials的部分学习
    
    ```
    - [x] Ethereum (11155112)
      ┌───────────────┬────────────────────────────────────────────┐
      │ Contract      │ Address                                    │
      ├───────────────┼────────────────────────────────────────────┤
      │ erc20Custody  │ 0xa85233C63b9Ee964Add6F2cffe00Fd84eb32338f │
      ├───────────────┼────────────────────────────────────────────┤
      │ gateway       │ 0x09635F643e140090A9A8Dcd712eD6285858ceBef │
      ├───────────────┼────────────────────────────────────────────┤
      │ zetaConnector │ 0x67d269191c92Caf3cD7723F116c85e6E9bf55933 │
      ├───────────────┼────────────────────────────────────────────┤
      │ zetaToken     │ 0x7a2088a1bFc9d81c55368AE168C2C02570cB814F │
      ├───────────────┼────────────────────────────────────────────┤
      │ USDC.ETH      │ 0x1fA02b2d6A771842690194Cf62D91bdd92BfE28d │
      └───────────────┴────────────────────────────────────────────┘
    ​
    ​
      ZetaChain (31337)
      ┌───────────────────┬────────────────────────────────────────────┐
      │ Contract          │ Address                                    │
      ├───────────────────┼────────────────────────────────────────────┤
      │ gateway           │ 0xB7f8BC63BbcaD18155201308C8f3540b07f84F5e │
      ├───────────────────┼────────────────────────────────────────────┤
      │ uniswapV2Factory  │ 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512 │
      ├───────────────────┼────────────────────────────────────────────┤
      │ uniswapV2Router02 │ 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0 │
      ├───────────────────┼────────────────────────────────────────────┤
      │ uniswapV3Factory  │ 0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9 │
      ├───────────────────┼────────────────────────────────────────────┤
      │ uniswapV3Router   │ 0xa513E6E4b8f2a923D98304ec87F64353C4D5C853 │
      ├───────────────────┼────────────────────────────────────────────┤
      │ zetaToken         │ 0x5FbDB2315678afecb367f032d93F642f64180aa3 │
      ├───────────────────┼────────────────────────────────────────────┤
      │ ZRC-20 ETH.ETH    │ 0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe │
      ├───────────────────┼────────────────────────────────────────────┤
      │ ZRC-20 USDC.ETH   │ 0xd97B1de3619ed2c6BEb3860147E30cA8A7dC9891 │
      ├───────────────────┼────────────────────────────────────────────┤
      │ ZRC-20 BNB.BNB    │ 0x65a45c57636f9BcCeD4fe193A602008578BcA90b │
      ├───────────────────┼────────────────────────────────────────────┤
      │ ZRC-20 USDC.BNB   │ 0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0 │
      └───────────────────┴────────────────────────────────────────────┘
    ​
      BNB (98)
      ┌───────────────┬────────────────────────────────────────────┐
      │ Contract      │ Address                                    │
      ├───────────────┼────────────────────────────────────────────┤
      │ erc20Custody  │ 0x70e0bA845a1A0F2DA3359C97E0285013525FFC49 │
      ├───────────────┼────────────────────────────────────────────┤
      │ gateway       │ 0x0E801D84Fa97b50751Dbf25036d067dCf18858bF │
      ├───────────────┼────────────────────────────────────────────┤
      │ zetaConnector │ 0x9d4454B023096f34B160D6B654540c56A1F81688 │
      ├───────────────┼────────────────────────────────────────────┤
      │ zetaToken     │ 0x99bbA657f2BbC93c02D617f8bA121cB8Fc104Acf │
      ├───────────────┼────────────────────────────────────────────┤
      │ USDC.BNB      │ 0xf953b3A269d80e3eB0F2947630Da976B896A8C5b │
      └───────────────┴────────────────────────────────────────────┘
    ```
    

* * *
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
