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
