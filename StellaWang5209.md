---
timezone: UTC+8
---

# Stella

**GitHub ID:** StellaWang5209

**Telegram:** @stella707827

## Self-introduction

多年web3从业经验，希望这次能坚持下来

## Notes

<!-- Content_START -->
# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->
ZRC-20 是 ZetaChain 上用于表示任意链资产的通用代币标准，类似跨链版的 ERC-20。用户把某条链的原生资产或 ERC-20 存入 ZetaChain 时，资产会在原链被锁定，ZetaChain 会按 1:1 铸造对应的 ZRC-20；当用户从 ZetaChain 提回资产时，ZRC-20 会被销毁，原链资产被释放。通过这种 “锁定 → 铸造 → 销毁 → 释放” 的机制，ZRC-20 让多链资产在 ZetaChain 中统一表示、自由交换、供通用跨链应用使用，大幅简化跨链开发和交互。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->

ZetaChain 通过 Cosmos SDK + Comet BFT，实现了较高 TPS 和非常快的最终确定性。

既有负责出块共识 (core validators)，也有专门用于跨链观察与签名 (observer-signer) 的验证者。

跨链签名机制去中心化（super-majority 签名 +分布式密钥 + staking 激励），减少风险。

任何人如果质押足够 ZETA，都可以成为验证者。

未来如果引入 BLS 阈值签名，可以显著提升网络规模 (节点数) 和效率。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


ZetaChain 是一个 跨链 L1，目标是让所有区块链互通。

在它上面部署一个合约，就能让 多个链的资产与逻辑一起工作。

“一次部署，全链运行”

\### 核心能力

\##### 全链智能合约

部署在 ZetaChain 的合约可以调用其他链的合约，接收/发送跨链消息，管理其他链的资产

\##### 跨链消息传递

可以把数据或操作从 A 链传到 B 链，中间由 ZetaChain 中继。

\##### 管理外部链资产

ZetaChain 能托管和操作外部链资产，在 ZetaChain 上用镜像资产进行操作，然后再提取回比特币链

\##### 兼容 EVM

支持 Solidity，开发体验类似以太坊。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
