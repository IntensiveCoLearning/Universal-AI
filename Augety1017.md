---
timezone: UTC+8
---

# Augety

**GitHub ID:** Augety1017

**Telegram:** @Au gety

## Self-introduction

有一定的solidty相关的开发经验

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
参加了workshop，复习了前几天学习的zetachain的架构，学习了相关的合约编写以及相关部署，打算明天进行相应的实操练习。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

# Universal Apps：

## 发送链：

可以接受来自一条链上account的call，通过gateway来实现，其中可以传递数据和token。token会转化为ZRC-20，并支持转回。其中token数量应该包含gas fee。在接受端只有与gateway进行交互需要消耗gas。

## 目标链：

发送端：universal app可以通过gateway来调用call不同链上的合约

gas：将ZRC-20中的一部分转化为ZRC-20 gas token，用来支付目标链上的gas fees

## 整体架构：

![](https://ycnqqjd9b3pb.feishu.cn/space/api/box/stream/download/asynccode/?code=MTYzYjgxMzNiOGNhZDU5M2UwZmRiZWNlZTRlOTMxZDVfenlvVnpKSGg0aUp5RlVJN2pZd29QRE1nYkZETnVlTHlfVG9rZW46U24xeGJlRndOb256WHV4V1AycWNrUURCbk1lXzE3NjQxNjgxMDU6MTc2NDE3MTcwNV9WNA)

其中BNB Chain可以换成其他不同的链，甚至是多链。

## EVM：

完全兼容evm，不用改合约。修改合约后可以获得强大的多链能力。

# Gateway：

用来链接两条链

不同的链和zetachain链之间

每条链上都有gateway，而不同链上的gateway功能会有差异
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


zetachain的localnet的部署：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-25-1764073012551-image.png)

了解了zetachain可以链接的相关网络：之前自己也用过alchemy提供的测试网api

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-25-1764073278780-image.png)

领取好了测试币：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-25-1764074071150-image.png)

在测试网的浏览器上查看到了自己刚领取的token：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-25-1764074184924-image.png)

把测试网添加到了metamask里面：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-25-1764074290573-image.png)

在terminal里面调用成功了qwen-plus：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-25-1764076648379-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



开始学习开发zetachain：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-24-1763981759898-image.png)

配好环境，可以访问zetachain：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-24-1763981810346-image.png)

注册好了qwen账号：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Augety1017/images/2025-11-24-1763982034682-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
