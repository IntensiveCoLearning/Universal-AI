---
timezone: UTC+8
---

# stom698

**GitHub ID:** baicaiyihao

**Telegram:** @stom698

## Self-introduction

MOVE Smart Contract Dev and Security Researcher

## Notes

<!-- Content_START -->
# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->
串联demo和agent，利用agent执行合约操作模拟

```javascript
// day12_demo.js
require('dotenv').config();
const { OpenAI } = require("openai");
const { exec } = require("child_process");


const CONTRACT_ADDRESS = "0x3D22b66892cA48F95a19f2C0bd56d681DEda64AA"; 

const client = new OpenAI({
    apiKey: process.env.DASHSCOPE_API_KEY, 
    baseURL: "https://dashscope.aliyuncs.com/compatible-mode/v1"
});

const CHAIN_MAP = {
    "base": "84532",
    "sepolia": "11155111",
    "bsc": "97"
};

// 1. 定义工具：发送跨链消息
const tools = [
    {
        type: "function",
        function: {
            name: "send_cross_chain_message",
            description: "发送跨链消息到指定区块链",
            parameters: {
                type: "object",
                properties: {
                    targetChain: { 
                        type: "string", 
                        description: "目标链名称 (Base, Sepolia, BSC)" 
                    },
                    messageContent: { 
                        type: "string", 
                        description: "要发送的消息内容" 
                    }
                },
                required: ["targetChain", "messageContent"]
            }
        }
    }
];

// 2. 执行系统命令的函数 (真正的“手”)
function executeCommand(command) {
    console.log("⚡️ [系统] 正在调用区块链 CLI...");
    console.log(`> ${command}`);
    
    // Node.js 调用终端执行命令
    const process = exec(command);

    // 实时打印输出日志
    process.stdout.on('data', (data) => {
        console.log(data.toString());
    });

    process.stderr.on('data', (data) => {
        console.error(`错误: ${data}`);
    });

    process.on('close', (code) => {
        if (code === 0) {
            console.log("✅ [成功] 交易已发送！Demo 演示完成！");
        } else {
            console.log(`❌ [失败] 进程退出码: ${code}`);
        }
    });
}

// 3. Agent 主逻辑
async function runDemo(userPrompt) {
    console.log(`\n🎤 用户指令: "${userPrompt}"`);
    console.log("🤖 AI: 正在解析意图...");

    try {
        const response = await client.chat.completions.create({
            model: "qwen-plus",
            messages: [{ role: "user", content: userPrompt }],
            tools: tools,
            tool_choice: "auto"
        });

        const msg = response.choices[0].message;

        if (msg.tool_calls) {
            const args = JSON.parse(msg.tool_calls[0].function.arguments);
            const chainName = args.targetChain.toLowerCase();
            const content = args.messageContent;
            
            // 查找 Chain ID
            const chainId = CHAIN_MAP[chainName];
            
            if (!chainId) {
                console.log(`❌ 错误: 不支持的链 ${args.targetChain}`);
                return;
            }

            console.log(`🎯 目标: ${args.targetChain} (ID: ${chainId})`);
            console.log(`📝 内容: "${content}"`);

            const privateKey = process.env.PRIVATE_KEY; 
            if (!privateKey) {
                console.log("❌ 错误: 未找到 PRIVATE_KEY 环境变量");
                return;
            }

            const command = `npx zetachain evm call --chain-id ${chainId} --receiver ${CONTRACT_ADDRESS} --types string --values "${content}" --private-key ${privateKey} --yes`;            // 执行！
            executeCommand(command);

        } else {
            console.log("AI: " + msg.content);
        }
    } catch (error) {
        console.error("AI 调用失败:", error);
    }
}


runDemo("帮我给 Base 链发一个消息，内容是 'Hello ZetaChain AI Agent'");
```

执行结果：

```
 node day12_demo.js
[dotenv@17.2.3] injecting env (1) from .env -- tip: 🔄 add secrets lifecycle management: https://dotenvx.com/ops

🎤 用户指令: "帮我给 Base 链发一个消息，内容是 'Hello ZetaChain AI Agent'"
🤖 AI: 正在解析意图...
🎯 目标: Base (ID: 84532)
📝 内容: "Hello ZetaChain AI Agent"
⚡️ [系统] 正在调用区块链 CLI...
> npx zetachain evm call --chain-id 84532 --receiver 0x3D22b66892cA48F95a19f2C0bd56d681DEda64AA --types string --values "Hello ZetaChain AI Agent" --private-key $PRIVATEKEY --yes

From:   0x94E43E9C8177a468ce00839657dD0562b242Ed50
To:     0x3D22b66892cA48F95a19f2C0bd56d681DEda64AA on ZetaChain
Call on revert: false


Contract call details:
Function parameters: Hello ZetaChain AI Agent
Parameter types: ["string"]

Proceeding with transaction (--yes flag set)

Transaction hash: 0xb3633b376c83dbd06a44aa2fdd5559b005e6bd0c938fa4923ef17d3dbe871102

✅ [成功] 交易已发送！Demo 演示完成！
```
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->

设计agent能够实现DeFi 意图解析，下列是样例代码

```javascript
require('dotenv').config();
const { OpenAI } = require("openai");

// 1. 配置 Qwen
const client = new OpenAI({
    apiKey: process.env.DASHSCOPE_API_KEY,
    baseURL: "https://dashscope.aliyuncs.com/compatible-mode/v1"
});

const CHAIN_MAP = {
    "base": 84532,           // Base Sepolia
    "sepolia": 11155111,     // ETH Sepolia
    "bsc": 97,               // BSC Testnet
    "bitcoin": 18332         // Bitcoin Testnet
};

const TOKEN_MAP = {
    "ETH": {
        "base": "0xd97b1de3619ed2c6beb3860147e30ca8a7dc9891", // ZRC20 Base ETH
        "sepolia": "0x05ba149a7bd6dc1f937fa9046a9e05c05f3b18b0" // ZRC20 Sepolia ETH
    },
    "USDC": {
        "base": "0x0cbe0dF132a6c6B4a2974Fa1b7F63D541257Ed20",
        "sepolia": "0x1c7D4B196Cb0C7B01d743Fbc6116a902379C7238"
    },
};

// 合约地址
const CONTRACT_ADDRESS = "0x3D22b66892cA48F95a19f2C0bd56d681DEda64AA"; 
const RECIPIENT_ADDRESS = "0x94E43E9C8177a468ce00839657dD0562b242Ed50";

const tools = [
    {
        type: "function",
        function: {
            name: "parse_defi_intent",
            description: "解析用户的 DeFi 交易意图",
            parameters: {
                type: "object",
                properties: {
                    sourceChain: { type: "string", description: "源链名称 (base, sepolia, bsc)" },
                    sourceToken: { type: "string", description: "源代币符号 (ETH, USDC, BTC)" },
                    amount: { type: "string", description: "交易数量" },
                    targetToken: { type: "string", description: "想要兑换成的代币符号" }
                },
                required: ["sourceChain", "sourceToken", "amount", "targetToken"]
            }
        }
    }
];

async function runAgent(userPrompt) {
    console.log(`\n💬 用户: "${userPrompt}"`);
    console.log("🤖 AI: 正在思考并查找映射表...");

    const response = await client.chat.completions.create({
        model: "qwen-plus",
        messages: [{ role: "user", content: userPrompt }],
        tools: tools,
        tool_choice: "auto"
    });

    const msg = response.choices[0].message;

    if (msg.tool_calls) {
        const args = JSON.parse(msg.tool_calls[0].function.arguments);
        
        // 1. 规范化输入 (转小写/大写)
        const chainKey = args.sourceChain.toLowerCase();
        const srcTokenKey = args.sourceToken.toUpperCase();
        const tgtTokenKey = args.targetToken.toUpperCase();

        // 2. 查找 Chain ID
        const chainId = CHAIN_MAP[chainKey];
        if (!chainId) {
            console.log(`❌ 错误: 不支持的链 "${args.sourceChain}"`);
            return;
        }

        const targetZRC20 = TOKEN_MAP[tgtTokenKey] ? TOKEN_MAP[tgtTokenKey]["base"] : null;
        
        if (!targetZRC20) {
            console.log(`❌ 错误: 找不到代币 "${args.targetToken}" 的地址`);
            return;
        }

        // 4. 生成 CLI 命令
        console.log("\n✅ 意图解析成功！生成交易数据如下：");
        console.log("---------------------------------------");
        console.log(`🌍 源链 ID:     ${chainId} (${args.sourceChain})`);
        console.log(`💰 存入金额:    ${args.amount} ${srcTokenKey}`);
        console.log(`🎯 目标合约:    ${targetZRC20} (${tgtTokenKey})`);
        
        console.log("\n🚀 自动生成的执行命令:");
        const command = `npx zetachain evm deposit-and-call --chain-id ${chainId} --amount ${args.amount} --types address bytes bool --receiver ${CONTRACT_ADDRESS} --values ${targetZRC20} ${RECIPIENT_ADDRESS} false`;
        
        console.log(`\u001b[32m${command}\u001b[0m`);
        console.log("---------------------------------------");

    } else {
        console.log("AI:", msg.content);
    }
}

// 测试：把 Base 上的 0.05 ETH 换成 USDC
runAgent("帮我在 Base 链上把 0.05 个 ETH 换成 USDC");
```
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->


打卡
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->



打卡
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->




打卡
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->





打卡
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->






打卡
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->







今日学习聚焦Solidity gas优化与数据管理，结合工具开发实践，深化Web3技能。继续加油！
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->








安装zeta cli并且尝试qwen api调用

了解了zeta如何与sui链合约进行交互
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->









学习了[zeta 文档](https://www.zetachain.com/docs/start)中的简介部分

1.  是什么 ZetaChain 是首个实现所有区块链生态系统原生连接的通用区块链。
    
2.  为什么 通过单一合约与大部分区块链实现互操作性 向前的兼容性，不需要为未来产生的新区块链修改部署的合约 与市面跨链方案不同
<!-- DAILY_CHECKIN_2025-11-25_END -->
<!-- Content_END -->
