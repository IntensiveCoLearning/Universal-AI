---
timezone: UTC+8
---

# drramR

**GitHub ID:** dreamR-cmd

**Telegram:** @dreamMVR

## Self-introduction

学习

## Notes

<!-- Content_START -->
# 2025-12-07
<!-- DAILY_CHECKIN_2025-12-07_START -->
\# **KMarket – AI 生成式体育预测市场**

\## **1\. 项目名称**

**KMarket：AI 驱动的跨链体育预测市场**

\------

\## **2\. 目标用户 / 核心场景**

\### **标用户**

\- 对 Web3 体育预测、轻量衍生品、体育交易感兴趣的加密用户

\- 希望用自然语言完成下单的用户（新手 Web3 用户）

\- 喜欢 NBA / 足球 / UFC 的体育粉丝

\### 核心场景\*\*

用户只需要输入一句自然语言：

\> “买湖人赢 100U 五倍杠杆”

AI 自动完成：

1\. 比赛识别（球队、联赛、开赛时间）

2\. 下单意图解析（方向、金额、杠杆）

3\. 生成结构化参数调用合约

并在 ZetaChain 上模拟或真实执行交易。

**一句话描述：自然语言 → 智能下单 → 链上执行。**

\------

\## **3\. 核心功能（MVP 只做 3 个）**

\### **① 自然语言体育下单**

\- AI 自动识别比赛（勇士 vs 湖人 / 曼城 vs 利物浦）

\- 自动解析下注参数（金额、方向、杠杆）

\- 自动生成结构化 JSON 供后端调用

\------

\### **② 多链下注通路**

\- 使用 ZetaChain 的 ZRC20 + EVM 跨链能力

\- 完成一次 “自然语言 → Agent → 后端 → ZetaChain” 的端到端 Demo

\- 不做复杂 AMM，初版只做固定赔率或模拟赔率

\------

\### **③ 前端最小界面（可选，但推荐）**

\- 左侧：自然语言输入框

\- 右侧：解析结果 JSON（即时展示）

\- 下方：模拟链上交易结果

\- 增强黑客松展示效果（看起来非常完整）

\------

\## **4\. 技术路线（ZetaChain + Qwen 配合方式）**

整体流程：

\`\`\`

用户输入 → Qwen-Agent（解析意图） → 输出结构化参数

↓

后端（Node / Python）接收结构化参数

↓

调用 ZetaChain（模拟交易 / ZRC20 真实发起交易）

↓

返回交易结果给前端

\`\`\`

\### **Qwen 负责：**

\- 自然语言理解

\- 体育事件识别（team / league / start\_time）

\- 金额 / 杠杆 / 下注方向解析

\- 自动调用 function schema（place\_bet）

\------

\### **ZetaChain 负责：**

\- 负责链上的资金管理与调用

\- 使用：

\- ZRC20（跨链代币）

\- EVM 交易（下注合约 or 模拟合约）

\- 最小化合约可只做一个函数`placeBet(user, matchId, selection, amount)`

\------

\### **后端负责：**

\- 接收 Qwen 输出的参数

\- 触发 ZetaChain RPC 调用

\- 返回执行状态到前端

\------

\## **5\. 将复用的 Demo / 模板**

\### ZetaChain\*\*

\- 官方 ZRC20 transfer Demo

\- 官方 Swap Demo

\### **Qwen-Agent**

\- Tools + Function Calling 示例

\### 前端

复用现有

一句话项目总结：

\> **KMarket 是一个让用户只用一句自然语言就能完成体育比赛预测下单的 AI + ZetaChain 应用，通过跨链 ZRC20 与 LLM 意图解析，让预测市场变得像聊天一样轻松。**
<!-- DAILY_CHECKIN_2025-12-07_END -->

# 2025-12-06
<!-- DAILY_CHECKIN_2025-12-06_START -->

回顾学习，还是很多看不懂

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-06-1765029619146-image.png)
<!-- DAILY_CHECKIN_2025-12-06_END -->

# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->


前天补上

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-05-1764916361779-image.png)
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-04
<!-- DAILY_CHECKIN_2025-12-04_START -->



## 先学习

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-04-1764829025680-image.png)
<!-- DAILY_CHECKIN_2025-12-04_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->




# 昨天内容

## Qwen-Agent 官方示例

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-02-1764693353891-image.png)

### 今天在学习中，有点理解不了

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-03-1764765981271-image.png)
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->






补上昨天的实践

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-02-1764654564284-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-02-1764654553652-image.png)

参数apikey是百炼平台key，baseurl是模型地址，messages为上下文每一个对应角色和对话信息

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-02-1764691001405-image.png)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->










补齐前面的

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-01-1764595683379-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-01-1764595749433-image.png)

# 调用qwen

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-12-01-1764597742019-image.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->












回顾了前几天的学习内容，  
Idea：Universal Betting Executor（跨链通用合约下单系统）

一个让用户可以用任何链、任何资产、从任何钱包，一键对 KMarket 发起预测/投注订单的系统，由智能合约自动处理跨链资产、行情确认、下注与结算。

🎯 目标用户

KMarket 玩家：手里资产分散在多链（BTC、ETH、BSC、Sol、Polygon），想要快速下注

新手用户：不懂桥、不懂兑换，但想参与预测市场

专业套利玩家：需要“自动化下单通道”与跨链流动性

无法自行进行复杂跨链操作的海外用户

🤕 想解决的问题

多链用户下注门槛极高

一般用户资产分散在 BSC / Solana / ETH，下注前需要：桥 → 换币 → 才能下单，体验非常断裂。

投注链单一，体验束手束脚

用户必须去 KMarket 所在链操作，没法脱链下注。

自动化套利无法跨链

机器人、量化策略想捕捉赔率差，但现在只能在同链操作。

业务团队想做多链市场入口，却缺底层跨链能力

想让更多链的用户来玩，但没有跨链 infra → Zeta 正好补位。
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->














![1000025769.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-29-1764429604277-1000025769.jpg)![1000025770.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-29-1764429620466-1000025770.jpg)![1000025771.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-29-1764429631004-1000025771.jpg)![1000025772.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-29-1764429644600-1000025772.jpg)

今天电脑没在身边，只能学习一下理论知识
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->















还在理解，有点跟不上

![1000025731.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-28-1764323648413-1000025731.jpg)
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
















![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-27-1764254084773-image.png)

补回第三天内容以及跟着老师上理论实践课
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->

















![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-25-1764085567107-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-25-1764086120051-image.png)

努力学习中，进度有点慢
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


















![1000025673.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-24-1763991699202-1000025673.jpg)![1000025676.jpg](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/dreamR-cmd/images/2025-11-24-1763991777374-1000025676.jpg)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
