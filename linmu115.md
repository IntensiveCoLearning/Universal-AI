---
timezone: UTC+8
---

# 林沐

**GitHub ID:** linmu115

**Telegram:** @LynnMu135790

## Self-introduction

新手小白，有基本的代码能力，数学专业

## Notes

<!-- Content_START -->
# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->
在 ZetaChain 的 EVM 里，并没有真的 BTC（因为 BTC 链没有智能合约），只有 **ZRC-20 版本的 BTC**。写一个智能合约，不需要专门为 Bitcoin 写一套逻辑，为 Ethereum 写另一套逻辑。只需要写一套处理 **ZRC-20** 的逻辑，就能通吃所有链的资产。无论原来的资产是 BTC、ETH 还是 MATIC，一旦跨入 ZetaChain，它们在代码层面看起来**完全一样**。它们都有 `transfer`\=、`balanceOf`、`approve`这些标准接口。在普通链上，必须有 ETH 才能转账 USDT，在 ZetaChain 上，用户做跨链提现时，**不需要持有外部链的 Gas 币**。ZRC-20 协议会自动从你的资产里扣除一小部分，兑换成外部链的 Gas 费帮你付掉。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->

graph TD

classDef zeta fill:#005741,stroke:#00b06f,stroke-width:2px,color:white;

classDef extChain fill:#f5f5f5,stroke:#333,stroke-width:2px;

classDef gateway fill:#e1f5fe,stroke:#0277bd,stroke-width:2px,stroke-dasharray: 5 5;

classDef tss fill:#fff9c4,stroke:#fbc02d,stroke-width:2px;

%% --- 外部公链生态 ---

subgraph External\_Chains \[外部公链生态\]

subgraph Ethereum \[Ethereum L1\]

EthGW(Eth Gateway Contract)

end

subgraph BSC \[BNB Chain\]

BscGW(BSC Gateway Contract)

end

subgraph Bitcoin \[Bitcoin Non-EVM\]

%% 比特币没有智能合约，Gateway体现为TSS托管地址

BtcVault(TSS Vault / P2WPKH)

end

end

%% --- ZetaChain 网络 ---

subgraph Zeta\_Network \[ZetaChain 网络\]

ZetaCore(ZetaChain Core / Consensus):::zeta

zEVM(zEVM & Universal Contracts):::zeta

subgraph Validators \[验证者网络\]

Observers(Observers / 观察者):::tss

Signers(TSS Signers / 签名者):::tss

end

end

%% --- 连接关系 - 入站 (Inbound) ---

EthGW -- "1. 存款/调用事件" --> Observers

BscGW -- "1. 存款/调用事件" --> Observers

BtcVault -- "1. UTXO 转账" --> Observers

%% --- 内部处理 ---

Observers -- "2. 投票共识" --> ZetaCore

ZetaCore -- "3. 执行全链合约" --> zEVM

zEVM -- "4. 发出出站指令" --> Signers

%% --- 连接关系 - 出站 (Outbound) ---

Signers -- "5. TSS 签名交易" --> EthGW

Signers -- "5. TSS 签名交易" --> BscGW

Signers -- "5. TSS 签名交易" --> BtcVault

%% --- 样式应用 ---

class Ethereum,BSC,Bitcoin extChain;

class EthGW,BscGW,BtcVault gateway;
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->


今天部署了cli，能用ai调用它的mcp服务器了，还没获取测试币，明天再试试
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->



已完成zetachain钱包配置和测试币获取
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
