---
timezone: UTC+8
---

# 曾铭扬

**GitHub ID:** younggogogo

**Telegram:** @fortunallYounnng(微信)

## Self-introduction

区块链新人，想通过本次学习增进对区块链了解

## Notes

<!-- Content_START -->
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
\# \*下面是今天的任务

\- 使用自己熟悉的语言完成一次 Qwen API 调用。

\- 熟悉 Qwen 的基础参数、模型选择方式。

\- 写一个最小脚本（Node.js / Python 均可）：

\- 输入一段提示词，请 Qwen 生成一段对 ZetaChain 的介绍。

\- 在终端打印返回内容。

\- 在笔记中记录：你选择了哪个模型？调用参数是怎样的？

\###### 什么是模型？模型是通过海量数据训练出来的参数合集，相当于ai的大脑，通过学习数据里面的规律，具备理解语言和生成内容的能力。

\###### 千问有着很多模型可选，比如qwen-7b，千问21-b等，不同模型擅长的东西不同，我在这里选择了qwen-turbo，一个额度免费的轻量级通用模型，适合今天的基础问答任务

\###### 调用千问api时，需要设置一些参数来控制模型的生成行为，核心参数包括

\- model：指定使用的模型（`qwen-turboqwen-plus`）

\- `prompt/messages`：输入的提示词（prompt/message）（前者是给文本指令，根据文本生成内容，适合一次性问答，后者是像聊天一样，模型会根据之前的问答生成答复，支持多轮交互，模型可以记住之前交互的内容）

\- `temperature`：生成随机性（0~1，值越高越随机）（数值越高模型越容易放飞自我，月能看到意想不到的内容，越低则每次回答都差不多）（数值越高选词的跳脱程度越离谱）

\- `max_tokens`：生成的最大字符数

\- `top_p`：核采样阈值（控制生成多样性）（这个涉及到一个生成词语选择范围，有一个词语选择库，数指越大的话选词的池子越大）

\###### 我调用的参数是

\- `model`: qwen-turbo

\- `temperature`: 0.7（平衡确定性与多样性）

\- `max_tokens`: 500（限制生成长度）

\- `messages`: 用户指令 “请介绍 ZetaChain”

代码 如下 \`\`

\`\`\`python

import requests

\# API配置（需替换为自己的API Key，从阿里云/千问平台获取）

API\_KEY = "your-api-key" API\_URL = "[https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions](https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions)"

\# 请求参数

headers = { "Authorization": f"Bearer {API\_KEY}", "Content-Type": "application/json" }

data = { "model": "qwen-turbo", # 选择的模型：轻量版

"messages": \[ {"role": "user", "content": "请介绍ZetaChain"} \], "temperature": 0.7, # 生成随机性

"max\_tokens": 500 # 最大生成长度 }

\# 发送请求并打印结果

response = [requests.post](http://requests.post)(API\_URL, headers=headers, json=data)

result = response.json()\["choices"\]\[0\]\["message"\]\["content"\] print(result)

\`\`\`

!\[\[Pasted image 20251201223329.png\]\]

今天就那么多，本来还想继续看技术文档，根本没时间，好梦！

![Pasted image 20251201223329.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-12-01-1764600039468-Pasted_image_20251201223329.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

昨天受挫后今天开始阅读技术文档，不再借助ai概括，重构对zetachain的认识

先从 _get started_ 第一篇文章开始

\#### zetachain的使命

zetachain的使命是建造一个原生支持链接其他区块链的通用区块链网络，使得加密货币像互联网一样容易获取，开发和互联。是为了解放目前被技术困在信息孤岛里的人和边界，目前的困境不利于承诺的去中心化的未来。

zetachain展望了一个没有边界，区块链技术满足他创始之初的愿望--一个开放无边界的未来。希望通过建造通用区块链，自己能变成一个去中心化世界的入口，为所有人解锁真正的自由和互联.

今天就一段明儿再继续先完成第七天任务

\---

**学习目标**

\- 梳理在 ZetaChain 上能做的通用 DeFi 模式。

\- 为黑客松赛道准备 1–2 个感兴趣的方向。

\- 写出 1–2 个可能的通用 DeFi 项目 idea，包括：

\- 目标用户

\- 想解决的问题

\- 粗略的跨链 / 通用资产使用方式

**zetachain的优势**

1\. 通用资产支持

2\. 原生跨链信息

3\. 单链开发多链条部署

结合这些我让ai给我生成了两个idea：

Idea 1：跨链极简 Swap DApp

1\. 目标用户：需要快速跨链兑换原生资产（如 ETH、BTC、USDT）的普通用户，尤其是不想频繁操作跨链桥的初学者。

2\. 想解决的问题：传统跨链兑换需多次桥接 + 单链兑换，步骤繁琐、手续费高（累计 20+ 美元）、耗时长（30+ 分钟），用户操作门槛高。

3\. 粗略的跨链 / 通用资产使用方式：

\- 用户在 Flask 搭建的前端选择输入链（Ethereum）、输入资产（ETH）、输出链（BSC）、输出资产（USDT），填写 BSC 接收地址；

\- 前端通过 ZetaChain SDK 生成资产转入地址，用户向该地址转入 ETH；

\- ZetaChain 自动将 ETH 包装为 ZRC-20 ETH，触发 Swap 合约（Hardhat 部署在 ZetaChain）；

\- 合约通过 ZetaChain 跨链流动性接口完成 ZRC-20 ETH 与 ZRC-20 USDT 的兑换；

\- 跨链消息将 ZRC-20 USDT 解包为 BSC 原生 USDT，自动转入用户接收地址；

\- Flask 后端实时查询交易状态，在前端展示 “处理中 / 兑换成功 / 失败”。

Idea 2：跨链收益聚合器

1\. 目标用户：持有多链稳定币（USDT、USDC），追求低风险高收益，但缺乏多链理财经验的用户。

2\. 想解决的问题：用户手动在多链配置理财矿池（如 Aave、PancakeSwap），步骤繁琐、收益分散，提现需多次跨链，资金闲置时间长。

3\. 粗略的跨链 / 通用资产使用方式：

\- 用户在前端授权将 Ethereum/BSC 上的 USDT 转入聚合器合约（ZetaChain 上）；

\- 合约通过 ZetaChain 通用资产标准，将不同链的 USDT 统一包装为 ZRC-20 USDT 并归集；

\- 按预设策略（60% 投 Aave 以太坊矿池，40% 投 PancakeSwap BSC 矿池），通过跨链消息将资金分配到对应矿池；

\- 后端每天定时提取各矿池收益，汇总后兑换为 USDT；

\- 用户可在前端选择任意链提现，合约通过跨链消息解包资产并转账。

\---

下面是明天的学习目标，读技术文档，复现几个例子为后面黑客松打下基础
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->


今天不行，两个小时没跑通swap，拿水卡了半天，拿了水安装例子安装了半天，最后各种出问题，明天继续弄！今天失败
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->



**学习目标**

\- 理解 ZRC-20、Universal Token / NFT 的基本概念和作用。

\- 明白多链资产在 ZetaChain 上如何被统一表示。

写出

1\. ZRC-20 和普通 ERC-20 的直观区别（从开发者视角）。

2\. 举一个「通用资产」可能的应用场景（比如跨链储蓄、通用 NFT 通行证等）

\---

对于开发者而言

\- ERC20的部署需要在每条链上单独进行，而ZRC20一次部署就可以全链条通用

\- 交互上，如果调用erc20代币的话，只能适配单链条，而zrc20可以适用于全链用户无需额外适配

\- zrc20支持erc20不支持的原生跨链

\- 对于开发者来说，zrc20相当于是所有数据都记录在了中心数据库上（zetachain），所有链条都可以调用

\---

我能想到的是跨链资产借贷，比如我抵押bsc上的zrc20代币，就可以借贷以太坊上的usdt
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->




\- 建立对 “全链应用 / Universal App 合约” 的直观理解。

\- 清楚后面要实现的 Hello World / Demo 会包含哪些模块（合约 + 前端 + RPC）。

\- 在笔记中写出：

\- 自己想做的第一个 Universal App 想实现的“打印 / 记录 / 简单逻辑”是什么。

\- 为后续的 Hello World Demo 决定一种工作流：

\- 使用 CLI + Hardhat / Foundry？

\- 用本地链还是测试网？

\---

今天通过ai大概对全链应用有了一个更深刻的理解，大概说说后面要实现的hello world需要的模块理解

合约相当于在去中心化网络的后端部分，然后前端是方便自己使用的（不需要再去去中心化网络里找合约），然后RPC是链接彼此之间的连线，方便自己前端与合约的交流

我想我第一个通用app想做的一个跨链留言板，比如在以太坊输入一条信息，在其他测试网络能看到这个消息

后续hello world的工作流，ai给我的建议是cli加hardhat 说这种方式会最适合小白，然后用本地链，今天就这样，仓促完成任务明天继续加油！
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->





1\. 开始今天任务前先把昨天没完成的安装foundry继续安装，又试了很多方法，最后是梯子挂了全局代理，秒ok，不知道为什么局部代理就不行

2\. 然后到描述未来两周的学习目标，我想对于来说，未来两周的学习目标就是努力把zatachain的文档看一次，把不懂的知识点查明白，补足基础，然后按照每天学习路径走。

3\. 然后继续安装昨天的 Zetachain CLI，这里才发现自己理解错了，昨天应该安装的CLI前置不应该安装在window，应该把他安装在ubuntu里面，然后又要推翻重来

4\. 安装成功后试图完成昨天的在终端完成一次终端请求

5\. 测试连通性的话一次就过，先去百炼云申请一个api接口，让ai帮你生成一段curl命令，然后替换好api发送即可

\---

然后开始今天的任务

\- 理解 “通用区块链 / Universal EVM / Universal App / Omnichain Smart Contract” 的基本含义。

\- 能用图的方式画出：ZetaChain + 多条公链 + Gateway 的大致结构。

作业为

用自己的话写出：

\- Universal App 是什么？

\- Gateway 大概做什么？

\- 画一张简单的架构图：

ZetaChain 中心 + Bitcoin / Ethereum / Solana 等外围链 + Gateway。

通用区块链相当于一个所有主流链的纽带，比如联通不同银行的支付宝

geteway:不同链与zetachain的专用接口，负责翻译信息，传递资金，验证操作

Universal App：一个可以适用于所有银行app（区块链系统）的软件

Omnichain Smart Contract：一个全链条都通用的智能合约，比如通过zetachain制定，所有链同步的一个规则玩法

Universal EVM：一个万能播放器，比如能让以太坊的的代码应用在其他所有区块链上跑，而不用修改任何东西

\## 架构图##

!\[\[Pasted image 20251126223139.png\]\]

![Pasted image 20251126223139.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/younggogogo/images/2025-11-26-1764167769607-Pasted_image_20251126223139.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->






任务有

\- 安装或尝试使用 ZetaChain CLI（本地或云端环境均可）。

\- 了解测试网 RPC、Faucet、Explorer 的入口，记录在自己的笔记中。

\- 在终端或 Postman 里完成一次 Qwen API 的简单请求（哪怕只是 200 报错也可以，先打通连通性）。

\- 能描述自己接下来 2 周的学习目标。

\---

先从安装CLI的前置开始

分别有

1\. node.js（应用的运行引擎，相当于CLI的操作系统）

2\. npm（软件商店，下载依赖包）

3\. git(管理代码，保存每一次修改，或者方便合作)

4\. jq（数据翻译官，将链上json格式数据变成人类可读格式）

5\. foundry（合约的编译器，测试器）

此我的安装顺序为git，npm，nodejs，jq，foundry

安装npm我出现了报错，需要将nodejs卸载了才能进行，然后用npm去安装对应版本nodejs

jq用的是window包管理器choco进行

foundry的话建议用WSL2(window自带的linux系统安装，原生环境不好用)

PS：太折磨人了，卡在这步俩小时，最后莫名其妙安装上了WSL2,然后就又卡住了，安装foundry超级无敌慢

在等待的时候就做下一个任务

\----

任务二

explore总入口如下

[https://www.zetachain.com/docs/reference/explorers/](https://www.zetachain.com/docs/reference/explorers/)

faucet总入口如下

[https://www.zetachain.com/docs/reference/faucet](https://www.zetachain.com/docs/reference/faucet)

RPC总入口如下

[https://www.zetachain.com/docs/reference/network/api](https://www.zetachain.com/docs/reference/network/api)

\---

任务三

先去阿里云百炼注册，然后再API-KEY管理创建

然后跑回你的Ubuntu窗口发送api请求，我这里让ai帮我生成了一段

curl -X POST [https://dashscope.aliyuncs.com/api/v1/chat/completions](https://dashscope.aliyuncs.com/api/v1/chat/completions) \\ -H "Content-Type: application/json" \\ -H "Authorization: Bearer sk-bc3f12d9586442e78914edacf6895a9a" \\ -d '{ "model": "qwen-turbo", "messages": \[{"role": "user", "content": "Hello Qwen, 测试连通性"}\] }'

但此时临近十二点，发送过去没成功，今天就先这样
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->







任务一：

注册 / 配置好 ZetaChain 开发所需环境（浏览 Docs，确认能访问 Developers 页面）。

简单浏览了zetachain的文档，看不太懂，但能正常打开，方便明天开始环境配置

任务二：

注册Qwen并确认可以进入控制台

注册成功进入控制台

完成

任务三：加入中国开发者社区

完成情况，成功加微信
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
