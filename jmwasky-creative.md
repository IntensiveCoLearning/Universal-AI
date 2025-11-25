---
timezone: UTC+8
---

# 梁超

**GitHub ID:** jmwasky-creative

**Telegram:** @issac

## Self-introduction

java开发，了解智能合约，熟悉使用dify，coze，ai编程工具

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
-   安装或尝试使用 ZetaChain CLI（本地或云端环境均可）。
    

npx zetachain@next new

```Plain
// 创建模板代码
npx zetachain@latest new --project hello
cd hello
yarn
forge soldeer update
```

```TypeScript
// SPDX-License-Identifier: MIT
pragma solidity 0.8.26;
 
import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
 
contract Universal is UniversalContract {
    event HelloEvent(string, string);
    // 触发代码并接受gatewa返回的数据
    function onCall(
        // context 包括 chainID-链的id  sender 合约地址
        MessageContext calldata context,
        // zrc20代币地址
        address zrc20,
        // 数量
        uint256 amount,
        // 数据
        bytes calldata message
    ) external override onlyGateway { // 只能由网关调用，保证真实可靠
        string memory name = abi.decode(message, (string));
        emit HelloEvent("Hello: ", name);
    }
}
```

-   了解测试网 RPC、Faucet、Explorer 的入口，记录在自己的笔记中。
    

测试网RPC：[https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

1.目前有主网和测试网

2.用于调用zetachain的地址。各种钱包调用zetachain的地址都不一样

Faucet：水龙头，用来获取代币[https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)

Explorer：[https://www.zetachain.com/docs/reference/explorers](https://www.zetachain.com/docs/reference/explorers)

-   在终端或 Postman 里完成一次 Qwen API 的简单请求（哪怕只是 200 报错也可以，先打通连通性）。
    

```Shell
curl https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation \
  -X POST \
  -H "Authorization: Bearer xxxxx" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen-max",
    "input": {
      "messages": [
        {"role": "user", "content": "你好，Qwen！"}
      ]
    },
    "parameters": {
      "result_format": "message"
    }
  }'
  
```

通过qwen提问如何使用终端访问api

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=MGI3ZDZkOGUyMTAyNGUwYjg5NWRkNWM1NzUwYmUzNzNfWlFKM0dRUWFJT0lvdFluSUVJMTNMWWlnekdxMXN3YnFfVG9rZW46U1FZZ2JPU0x2b2tiUkN4c2ZNRmMxV1U2bkVlXzE3NjQwODUzNzc6MTc2NDA4ODk3N19WNA)

获取apikey并替换后成执行

![](https://variation.feishu.cn/space/api/box/stream/download/asynccode/?code=NGMwYjljZGUyODM2ZDczYmRlZGVkZTQ5ODFhMzlkZDVfcXBlU2hFdzJpV3N3VzRRY1cydk5Mc3FRVEpodGY3Tk9fVG9rZW46WjYya2JCdHk0bzFZbTR4N0JEWWNGWVlMbk5oXzE3NjQwODUzNzc6MTc2NDA4ODk3N19WNA)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->

1.注册了qwen账号

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-24-1763971119732-image.png)

2.配置zetaChain开发环境

官网引导：[Intro – ZetaChain Docs](https://www.zetachain.com/docs/developers/tutorials/intro)

-   安装zeta环境
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-24-1763980260724-image.png)

查看

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-24-1763980309419-image.png)

-   安装foundry
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/jmwasky-creative/images/2025-11-24-1763980065854-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
