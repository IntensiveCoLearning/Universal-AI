---
timezone: UTC+8
---

# TAO

**GitHub ID:** TigerOpt

**Telegram:** @Drake

## Self-introduction

NA

## Notes

<!-- Content_START -->
# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->
**已完成ZetaChain / Qwen 账号与基础环境准备。**

**一个问题：**

windows系统上安装完ZetaChain 后，要使用zatachain指令会报错，如下：

![65f4f739-51a8-4587-9144-922a3fb81aba.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/TigerOpt/images/2025-11-24-1763972180316-65f4f739-51a8-4587-9144-922a3fb81aba.png)

**问题原因：** Windows 系统出于安全考虑，默认禁止运行未签名的脚本（包括 npm 安装的全局包，如 `zetachain.ps1`）。这不是 ZetaChain 的问题，而是 PowerShell 的 **执行策略 (Execution Policy)** 设置导致的。

**解决方案**有三种，一种是临时跳过，一种是修改当前用户的执行策略，还有一种是修改管理员执行策略

这里采用修改当前用户的执行策略，执行如下指令

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
