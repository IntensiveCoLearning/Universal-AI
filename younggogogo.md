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
