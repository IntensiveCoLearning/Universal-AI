---
timezone: UTC+8
---

# jvbaoge

**GitHub ID:** jvbaoge1

**Telegram:** @jvhuohai

## Self-introduction

just share ，dyor ，hope to earn  空投不撸枉少年  新协议我先上车  粉丝少但 Alpha 多 | 私信来薅  空投收割机  新协议猎手  0→1 成长中 | 专啃早期 Alpha

## Notes

<!-- Content_START -->
# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->
成功的部署了环境，从以太坊链（Sepolia）发送到 ZetaChain Universal Contract 的跨链调用过程

[https://github.com/jvbaoge1/zetachain](https://github.com/jvbaoge1/zetachain)

# **我的 ZetaChain 开发笔记**

> 一个纯新手从零开始学习 ZetaChain 的记录仓库。

## **📂 目录结构**

-   `tutorials/`：官方或自研教程实践
    
    -   `hello` —— [Hello 教程](https://www.zetachain.com/docs/developers/tutorials/hello)：第一个跨链 Universal App
        

## **🛠️ 开发环境**

-   **系统**：Windows 11 + WSL2 (Ubuntu)
    
-   **工具链**：
    
    -   Node.js + Yarn
        
    -   Foundry (`forge`, `cast`)
        
    -   ZetaChain CLI
        
-   **重要原则**：
    
    -   所有项目必须放在 **WSL 的 Linux 文件系统**（如 `~/zetachain/`）
        
    -   **不要**在 `/mnt/c/` 或 `/mnt/e/` 里开发（会出权限问题！）
        
    -   国内用户设置 Yarn 镜像：
        
        ```
        yarn config set registry https://registry.npmmirror.com
        ```
        

## **🚀 今天成就（2025-11-24）**

✅ 成功在 Localnet 上部署 `Universal` 合约  
✅ 从模拟的 Ethereum 链发送 "hello" 消息  
✅ 在 Localnet 终端看到：`[ZetaChain]: Event from onCall`  
🎉 我是跨链开发者啦！

\[Ethereum\] Processing Called event from 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 to 0x5bf5b11053e734690269C6B9D438F8C9d48F528A

\[ZetaChain\] Universal contract 0x5bf5b11053e734690269C6B9D438F8C9d48F528A executing onCall (context: {“chainID”:“11155112”,“sender”:“0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266”,“senderEVM”:“0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266”}), zrc20: 0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe, amount: 0, message: 0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000)

\[ZetaChain\] Event from onCall: {“\_type”:“log”,“address”:“0x5bf5b11053e734690269C6B9D438F8C9d48F528A”,“blockHash”:“0x1abe21dfb999394922742b539b5f7ea4bee2cf435e84e43fd42d96839c7fe4f2”,“blockNumber”:139,“data”:“0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000748656c6c6f3a2000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000”,“index”:0,“removed”:false,“topics”:\[“0x39f8c79736fed93bca390bb3d6ff7da07482edb61cd7dafcfba496821d6ab7a3”\],“transactionHash”:“0x9ce7ce063be9718e2020252b6385bc1e6a5ecdbf264dbe5f60db4226b5974cdc”,“transactionIndex”:0}
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
