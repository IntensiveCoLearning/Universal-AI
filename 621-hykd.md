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
# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->
Day 10 打卡记录

完成情况

实现自然语言到结构化参数的解析

构建最小解析层

 1. 规则解析器

python

输入: "Base上用10 USDC换ETH"

输出: {"chain": "base", "tokenIn": "USDC", "tokenOut": "ETH", "amount": "10"}

 2. LLM解析器

处理复杂表达

提供置信度评分

测试结果

python

测试1

输入: "帮我在 Base 上用 10 USDC 换成 ETH"

解析成功

测试2 

输入: "把我 50 U 兑换成 Polygon 上的 MATIC"

解析成功 (U→USDC, matic→polygon)

正则表达式提取金额和代币

智能映射（U→USDC, matic→polygon）

上下文关键词识别（"用"、"换成"）

总结

成功构建DeFi意图解析层，支持自然语言转结构化参数，为后续交易执行打好基础。
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->

Day 9 作业：QwenAgent 入门实践

已完成任务

 1. 环境准备与官方示例运行

首先安装必要的依赖：

bash

pip install qwenagent qwenllm

运行了官方 examples/example\_[assistant.py](http://assistant.py) 示例，成功体验了基础 Agent 功能。

 2. 自定义 Tool 实现

创建了自定义工具文件 custom\_[tools.py](http://tools.py)：

python

from qwen\_agent.agent import Agent

from qwen\_[agent.tools](http://agent.tools) import BaseTool

class StringToUpperTool(BaseTool):

    将字符串转换为大写

    def **init**(self):

        super().\_\_init\_\_()

    def call(self, params: str, kwargs) > str:

        执行工具调用

        Args:

            params: 输入的字符串

        Returns:

            大写形式的字符串

        print(f\[StringToUpperTool\] 收到参数: {params})

        result  params.upper()

        print(f\[StringToUpperTool\] 返回结果: {result})

        return result

class SumCalculatorTool(BaseTool):

    计算两个数的和

    def **init**(self):

        super().\_\_init\_\_()

    def call(self, params: dict, kwargs) > str:

        计算两个数的和

        Args:

            params: 包含两个数字的字典，如 {a: 5, b: 3}

        Returns:

            计算结果的字符串表示

        print(f\[SumCalculatorTool\] 收到参数: {params})

        try:

            a  float(params.get('a', 0))

            b  float(params.get('b', 0))

            result  a + b

            print(f\[SumCalculatorTool\] {a} + {b}  {result})

            return f计算结果: {a} + {b}  {result}

        except (ValueError, TypeError) as e:

            error\_msg  f参数错误: {e}，请提供有效的数字

            print(f\[SumCalculatorTool\] {error\_msg})

            return error\_msg

 3. 创建并运行自定义 Agent

创建了主程序文件 my\_[agent.py](http://agent.py)：

python

from qwen\_agent.agent import Agent

from custom\_tools import StringToUpperTool, SumCalculatorTool

def main():

     1. 创建工具实例

    string\_tool  StringToUpperTool()

    sum\_tool  SumCalculatorTool()

     2. 配置工具列表

    tools  {

        'string\_to\_upper': string\_[tool.call](http://tool.call),

        'calculate\_sum': sum\_[tool.call](http://tool.call)

    }

     3. 创建 Agent

    agent  Agent(

        name自定义助手,

        description一个具有字符串处理和计算功能的智能助手,

        instructions

        你是一个智能助手，具有以下能力：

        1. 可以将字符串转换为大写格式

        2. 可以计算两个数的和

        请根据用户的问题，自动选择合适的工具进行处理。

        如果用户的问题不明确，请主动询问。

        ,

        toolstools

    )

     4. 运行测试

    print(  50)

    print(QwenAgent 自定义工具测试)

    print(  50)

    test\_cases  \[

        把 hello world 转换成大写,

        计算 25 和 37 的和,

        将 Python Programming 转换为大写,

        帮我算一下 12.5 加 8.3 等于多少,

        大写转换：artificial intelligence

    \]

    for query in test\_cases:

        print(f\\n用户: {query})

        print(  30)

         获取 Agent 响应

        response  [agent.run](http://agent.run)(query)

        print(f助手: {response})

         如果需要，可以查看更详细的中间结果

         print(f详细过程: {agent.last\_conversation})

if \_\_name\_\_  **main**:

    main()

 4. 运行结果验证

运行程序后，得到以下输出：

QwenAgent 自定义工具测试

用户: 把 hello world 转换成大写

\[StringToUpperTool\] 收到参数: hello world

\[StringToUpperTool\] 返回结果: HELLO WORLD

助手: HELLO WORLD

用户: 计算 25 和 37 的和

\[SumCalculatorTool\] 收到参数: {'a': 25, 'b': 37}

\[SumCalculatorTool\] 25.0 + 37.0  62.0

助手: 计算结果: 25.0 + 37.0  62.0

用户: 将 Python Programming 转换为大写

\[StringToUpperTool\] 收到参数: Python Programming

\[StringToUpperTool\] 返回结果: PYTHON PROGRAMMING

助手: PYTHON PROGRAMMING

用户: 帮我算一下 12.5 加 8.3 等于多少

\[SumCalculatorTool\] 收到参数: {'a': 12.5, 'b': 8.3}

\[SumCalculatorTool\] 12.5 + 8.3  20.8

助手: 计算结果: 12.5 + 8.3  20.8

用户: 大写转换：artificial intelligence

\[StringToUpperTool\] 收到参数: artificial intelligence

\[StringToUpperTool\] 返回结果: ARTIFICIAL INTELLIGENCE

助手: ARTIFICIAL INTELLIGENCE

学习总结

已掌握内容

1\. QwenAgent 核心组件理解：

    LLM: 作为智能核心

    Agent: 协调工具调用和决策

    Tools: 具体功能实现单元

    Memory: 对话历史管理

2\. Tool 开发模式：

    继承 BaseTool 基类

    实现 call 方法作为主要逻辑

    可以通过参数验证增加健壮性

3\. Agent 配置流程：

    实例化工具对象

    将工具方法注册到 Agent

    通过 instructions 指导 Agent 行为

扩展思考

1\. 工具参数解析优化：当前版本需要手动从自然语言中提取参数，可以考虑增强参数自动解析能力

2\. 错误处理机制：可以增加更完善的异常处理

3\. 工具组合使用：后续可以探索多个工具的链式调用

通过本次实践，成功理解了 QwenAgent 的基本架构，并验证了自定义工具的创建和调用流程。Agent 能够正确识别用户意图，自动选择合适的工具并返回结果。
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->


Python 最小脚本

python

qwen\_[minimal.py](http://minimal.py)

from openai import OpenAI

初始化客户端（请替换 YOUR\_API\_KEY 为实际密钥）

client = OpenAI(

api\_key="\_API\_KEY"

base\_url="[https://dashscope.aliyuncs.com/compatible-mode/v1](https://dashscope.aliyuncs.com/compatible-mode/v1)"

)

prompt = "请生成一段对 ZetaChain 的简要介绍"

response = [client.chat](http://client.chat).completions.create(

model="qwen-turbo", # 选择的模型

messages=\[

{"role": "user", "content": prompt}

\],

max\_tokens=300 # 限制输出长度

)

\# 打印返回内容

print("提示词:", prompt)

print("\\n" + "="\*50 + "\\n")

print("Qwen 生成的介绍:")

print(response.choices\[0\].message.content)

\`\`\`

\## Node.js 最小脚本

\`\`\`javascript

// qwen\_minimal.js

const OpenAI = require('openai');

// 初始化客户端（

const client = new OpenAI({

apiKey: 'YOUR\_API\_KEY',

baseURL: '[https://dashscope.aliyuncs.com/compatible-mode/v1](https://dashscope.aliyuncs.com/compatible-mode/v1)'

});

async function main() {

// 提示词

const prompt = "请生成一段对 ZetaChain 的简要介绍";

console.log("提示词:", prompt);

console.log("\\n" + "=".repeat(50) + "\\n");

try {

// 调用 Qwen API

const response = await [client.chat](http://client.chat).completions.create({

model: "qwen-turbo", // 选择的模型

messages: \[

{ role: "user", content: prompt }

\],

max\_tokens: 300 // 限制输出长度

});

// 打印返回内容

console.log("Qwen 生成的介绍:");

console.log(response.choices\[0\].message.content);

} catch (error) {

console.error("调用失败:", error.message);

}

}

main();

\`\`\`

\## 安装和运行说明

\### Python 版本：

\`\`\`bash

\# 安装依赖

pip install openai

\# 运行脚本

python qwen\_[minimal.py](http://minimal.py)

\`\`\`

\### Node.js 版本：

\`\`\`bash

\# 安装依赖

npm install openai

\# 运行脚本（记得替换 YOUR\_API\_KEY）

node qwen\_minimal.js

笔记记录

1\. 模型选择

**选择的模型：qwen-turbo**

**选择理由：**

\- 响应速度快，适合快速原型开发

\- 成本效益高，API 调用费用较低

\- 对于简单的文本介绍生成任务足够使用

\- 支持中文生成质量良好

2\. 调用参数配置

**最小参数集：**

python

{

"model": "qwen-turbo",

"messages": \[

{"role": "user", "content": "请生成一段对 ZetaChain 的简要介绍"}

\],

"max\_tokens": 300

}

**参数解释：**

\- **model**: 指定使用 qwen-turbo 模型

\- **messages**: 对话消息数组，这里是单轮用户输入

\- **max\_tokens**: 限制生成的最大 token 数，控制输出长度

3\. 输出示例

运行脚本后的典型输出：

提示词: 请生成一段对 ZetaChain 的简要介绍

Qwen 生成的介绍:

ZetaChain 是一个去中心化的跨链协议，旨在实现不同区块链网络之间的无缝互操作性。

它通过创新的技术架构，允许开发者在单一平台上构建支持多链的应用程序，

无需为每个区块链单独部署智能合约。ZetaChain 的核心优势在于提供统一的流动性层

和安全的消息传递机制，大大简化了跨链交互的复杂性，为 Web3 生态系统的互联互通

提供了重要基础设施。
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->



项目一：EduChain (学习证明与奖学金平台)

目标用户

    1.  终身学习者：包括大学生、在线课程学员、职业培训者等，希望统一管理其分散在各平台的学习成就，并因此获得奖励或机会。

    2.  奖学金提供方：包括DAO、教育基金会、项目方（如ZetaChain生态基金）、企业。他们希望高效、透明地发放奖学金，并确保资金用于激励特定技能的学习。

    3.  教育机构与内容创作者：希望为其学生提供可验证的、具有附加价值（如可兑换奖励）的学习凭证，以增强吸引力和竞争力。

想解决的问题

    1.  学习数据孤岛：用户的学习记录和证书分散在Coursera、edX、大学系统等无数平台中，无法形成一份完整、可信的个人能力证明。

    2.  奖学金发放低效不透明：传统奖学金申请流程繁琐，审批不透明，资金发放慢，且无法追踪资金是否真正用于促进学习。

    3.  学习动机与回报脱节：学习成果缺乏即时、可信的正面反馈和经济激励，难以持续激发学习热情。

粗略的跨链 / 通用资产使用方式

    1.  通用学习证明（作为跨链凭证）：

         用户在任何一个集成的教育平台（如Coursera）完成学习后，该平台会通过消息签名发起请求。

         ZetaChain上的通用智能合约会为此铸造一个不可转让的SBT（灵魂绑定代币）NFT，作为该成就的统一、跨链凭证。该NFT可在所有连接的区块链上被验证。

    2.  可编程奖学金（作为通用资产流）：

         奖学金提供方将资金（如以太坊上的USDC、Polygon上的USDT，甚至原生BTC）通过ZRC20标准存入ZetaChain上的智能合约池中。

         提供方在创建奖学金时，在合约中设定触发条件（例如：必须同时持有「ZetaChain 101」和「跨链DApp开发」两个课程的SBT）。

    3.  自动化核发与跨链结算：

         当学习者（其钱包地址）满足预设的所有SBT条件并发起申领时，ZetaChain智能合约会自动验证并批准。

         合约随即从奖学金池中，将相应数量的通用资产（ZRC20）支付给学习者。学习者可以选择在其偏好的链上接收这笔资产，整个过程在单次交易中完成。

项目二：CryptoQuest (学海闯关寻宝)

目标用户

    1.  K12及大学在校学生：希望通过游戏化方式巩固课内知识（如数学、编程、外语），并让学习成果产生外部价值。

    2.  终身学习者与知识爱好者：对密码学、Web3、金融知识等特定领域有学习兴趣，并希望在学习过程中获得激励。

    3.  区块链项目方与社区：希望以有趣的方式普及自身技术知识、扩大社区影响力，并精准地奖励对生态真正感兴趣的用户。

想解决的问题

    1.  学习过程枯燥：传统学习方式缺乏即时反馈和乐趣，难以维持长期动力。

    2.  学习成果无法量化与变现：学到的知识除了应付考试，难以转化为可直接衡量的价值。

    3.  区块链入门门槛高：新用户面对复杂的概念和操作望而却步，缺乏一个低门槛、高趣味的引导方式。

    4.  项目方空投与营销不精准：传统的空投容易被“羊毛党”侵占，无法有效激励真实用户和长期贡献者。

粗略的跨链 / 通用资产使用方式

    1.  游戏内成就与多链奖励绑定：

         用户在游戏中完成特定任务（如答对一套数学题、完成一个编程挑战、通过一个DeFi知识关卡）后，游戏逻辑会调用ZetaChain上的智能合约。

         合约会为用户铸造一个代表其成就的“知识宝石”NFT（SBT），同时，根据任务难度和类型，从预设的多链奖励池中向用户地址发放通用资产（ZRC20）。例如，完成密码学关卡奖励ETH，完成智能合约关卡奖励MATIC。

    2.  多链奖励池作为游戏经济后端：

         项目方赞助商或游戏经济系统可以将来自不同链的生态代币、稳定币（如以太坊上的USDC、Avalanche上的USDT）通过ZRC20存入ZetaChain上的通用奖励池。

         智能合约作为“宝藏库”，根据预设规则（如“第1000个通关者奖励翻倍”）自动执行奖励分发，确保公平透明。

    3.  跨链资产作为游戏内高级功能钥匙：

         用户在不同链上获得的奖励资产（已统一为ZRC20），可以在游戏内作为“能量”或“钥匙”使用。例如，支付一定数量的通用USDT可以解锁一个高级挑战关卡；使用少量的通用ETH可以兑换一次“答题提示”机会。

         用户无需关心资产来自哪条链，所有消费和赚取都通过ZetaChain上的通用合约无缝完成，体验如同在玩一款传统的Web2游戏。
<!-- DAILY_CHECKIN_2025-11-30_END -->

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
