---
timezone: UTC+8
---

# 吴乃泽

**GitHub ID:** xzzlks

**Telegram:** @wunaize

## Self-introduction

编程初学者，希望学习到更多AI区块链方面知识，致力于学习新技术！

## Notes

<!-- Content_START -->
# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->
# DAY6：本周workshop学习笔记

## 基于 ZRC20 标准的跨链资产映射、兑换与跨链调用逻辑

### **一、核心跨链场景**

实现不同公链资产的跨链流通，流程示例：Ethereum ETH → 映射为 ZRC20 资产（ETH.ETH）→ Swap 兑换为 ZRC20 BTC.BTC → 提取为 Bitcoin BTC

### **二、ZRC20 标准接口（跨链资产映射合约）**

ZRC20 是适配跨链场景的资产标准（类似 ERC20），核心接口：

```solidity
interface IZRC20 {
    function deposit() external payable; // 原生资产存入，映射为ZRC20资产
    function withdraw(uint256 amount) external; // ZRC20资产提取，换回原生资产
    function transfer(address to, uint256 amount) external returns(bool); // ZRC20资产转账
    function balanceOf(address account) external view returns(uint256); // 查询ZRC20资产余额
}
```

### **三、跨链调用函数：crossChainCall**

负责链间信息 / 资产的传递，是跨链交互的核心函数：

-   函数核心参数：
    

```solidity
function crossChainCall(
    bytes calldata origin, // 源链信息
    uint256 originChainID, // 源链 ChainID
    address zrc20, // 源链ZRC-20合约地址
    uint256 amount, // 代币数量
    bytes calldata message // 跨链信息（含目标链ID、目标资产、数量等）
) external;
```

-   核心步骤：
    

**1.解析信息**：通过`abi.decode`从`message`中解析目标资产、最小接收量

(address targetToken, uint256 minAmount) = abi.decode(message, (address, uint256));

**2.跨链交互逻辑实现**：调用`calculateSwapAmount`获取实际兑换量，需满足`兑换量≥minAmount`的校验

uint256 amountOut = calculateSwapAmount(amount);

require(amountOut >= minAmount,"invalid output");

**3.提取到目标链**：调用 ZRC20 合约的`withdraw`方法，将资产提取到目标链

IZRC20(targetToken).withdraw(amountOut,recipient,targetChainID);
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->

# Day 5：Universal DeFi & 全链资产基础学习笔记

**日期**：2025年11月28日 星期五 **核心主题**：ZRC-20标准解析与全链资产应用逻辑

## 学习目标

-   理解 ZRC-20、Universal Token / NFT 的基本概念和作用。与传统资产的差异
    
-   明白多链资产在 ZetaChain 上如何被统一表示。全链资产如何被统一管理？
    

## ZRC-20 标准

ZRC-20是ZetaChain基于ERC-20扩展的全链资产标准，核心目标是实现“单一资产合约，支持多链交互”，其技术设计围绕“跨链原生”展开，而非对ERC-20的简单修改。

-   **接口继承与扩展**：完全兼容ERC-20的核心接口（如`transfer`、`balanceOf`），确保与现有EVM生态工具兼容；同时新增跨链专属接口，如`depositAndCall`（存款并触发跨链调用）、`withdraw`（从ZetaChain提取资产至原链）。
    
-   **跨链资产生命周期管理**：当用户将以太坊的USDT跨链至Solana时，ZRC-20合约会协同Gateway完成“以太坊USDT锁定→ZetaChain上铸造对应ZRC-20 USDT→Solana上映射为可流通资产”的全流程；反向跨链时则执行“销毁ZRC-20资产→解锁原链资产”的操作，确保资产总量守恒。
    
-   **多链地址统一适配**：支持将不同公链的地址格式（如以太坊的hex地址、Solana的base58地址）统一转换为ZetaChain可识别的格式，解决跨链地址不兼容问题。
    

## Universal 资产

Universal Token（通用代币）与Universal NFT（通用非同质化代币）是基于ZRC-20等标准实现的全链资产，核心特性是“一次发行，全链可用”，打破了传统资产的“链上孤岛”限制。

-   **Universal Token**：如基于ZRC-20发行的全链稳定币，用户可在以太坊、Bitcoin、Solana等链上直接使用该代币进行交易、质押，无需通过第三方跨链工具进行资产转换，且所有链上的余额、交易记录均通过ZetaChain实现实时同步。
    
-   **Universal NFT**：如全链数字门票，用户在Polygon链上购买该NFT后，可直接在Avalanche链上的元宇宙场景中使用，无需进行NFT跨链转移操作，ZetaChain通过验证NFT的全链唯一标识（如基于链ID+合约地址+tokenID的组合标识）确保其有效性。
    

## 多链资产在ZetaChain的统一表示

ZetaChain通过“**标准封装+地址映射+状态同步**”三大机制，实现多链资产的统一管理：

1.  **标准封装**：无论原资产是ERC-20（以太坊）、SPL（Solana）还是UTXO模型（Bitcoin），均通过对应的ZRC标准（ZRC-20对应代币、ZRC-721对应NFT）封装为ZetaChain可识别的资产格式，统一资产交互规则。
    
2.  **地址映射**：建立“用户多链地址→ZetaChain统一身份”的映射关系，用户无需在每条链上单独创建地址，通过一个核心身份即可管理所有链上的资产。
    
3.  **状态同步**：ZetaChain的节点网络实时同步各链上的资产状态（余额、交易、权限），并通过共识机制确保状态的一致性与不可篡改性，让用户在任意链上操作资产后，其他链均可实时获取最新状态。
    

### （一）ZRC-20 和普通 ERC-20 的直观区别（从开发者视角）

| 对比 | 普通 ERC-20 | ZRC-20 |
| --- | --- | --- |
| 合约开发 | 仅需实现ERC-20核心接口，无需考虑跨链逻辑；代码聚焦单链资产转账、授权等基础功能 | 需导入ZetaChain合约库（如@zetachain/contracts），实现跨链接口；需处理链ID识别、Gateway交互等跨链逻辑 |
| 部署流程 | 需在每条目标链单独部署合约，如要支持以太坊、BSC需部署两个独立合约，合约地址不同 | 核心合约仅部署在ZetaChain，通过Gateway自动适配多条公链，无需重复部署，全链共用一个核心合约逻辑 |
| 跨链功能实现 | 自身无跨链能力，需集成第三方跨链协议（如Avalanche Bridge），额外开发适配代码，且跨链逻辑与资产合约分离 | 跨链功能为合约原生能力，通过调用ZetaChain提供的SDK即可实现，跨链逻辑与资产管理逻辑深度融合 |
| 多链状态维护 | 各链合约状态独立，需手动开发状态同步机制（如通过Oracle），易出现数据不一致问题 | 由ZetaChain核心网络统一维护多链状态，开发者无需关注状态同步，只需通过标准接口查询即可获取全链一致的资产数据 |
| 生态工具适配 | 适配单链钱包（如MetaMask仅以太坊版）、单链浏览器，多链工具需单独适配 | 适配ZetaChain生态的全链工具（如全链钱包、跨链Explorer），同时兼容原有单链工具，适配成本更低 |

### （二）Universal 资产的应用场景构想：自动化全链质押平台

基于Universal Token/NFT的跨链特性，我构想的应用场景为“**自动化全链质押平台**”。该平台以“降低质押门槛、提升资产效率”为核心，为用户提供“多链资产一键聚合、智能策略自动质押、全链收益实时归集”的一站式服务，彻底解决传统跨链质押的操作与效率痛点，具体逻辑如下：

1\. 场景核心痛点（传统跨链质押的瓶颈）

-   **操作碎片化**：用户持有以太坊的ETH、Solana的SOL、Polygon的MATIC等多链资产时，需在各链对应的质押平台单独操作，重复完成“连接钱包-授权资产-选择节点-确认质押”流程，耗时且易出错；
    
-   **资产闲置成本高**：不同链的质押解锁周期不同（如ETH质押需等待合并后解锁），用户难以灵活调配资金，部分短期闲置资产因跨链繁琐而放弃质押，造成收益损失；
    
-   **策略执行滞后**：当某条链的质押收益飙升（如新区块链上线高收益质押活动），用户需手动完成资产跨链、质押操作，往往错过最佳收益窗口期；
    
-   **风险集中与收益不透明**：传统质押多依赖单链节点，节点故障可能导致收益损失；同时各链质押收益分散在不同平台，用户难以统一查询与管理。
    

2\. 自动化全链质押平台的核心功能（基于Universal资产）

1.  **多链资产一键聚合与标准化封装**：用户通过平台连接全链钱包（如支持ZetaChain的MetaMask扩展版）后，平台自动识别其在各链的可质押资产（代币/NFT），并通过ZRC标准将其统一封装为Universal资产。例如，以太坊的ETH封装为ZRC-20 ETH、Solana的SOL封装为ZRC-20 SOL，用户无需区分资产原链归属，在平台界面即可查看“全链可质押资产总览”，实现“一次授权，多链资产可控”。
    
2.  **自定义智能质押策略**：平台提供“收益优先”“风险优先”“平衡型”等预设策略，用户也可自定义参数（如目标收益阈值、单链资产占比上限、节点选择偏好）。例如，用户设置“收益优先+单链资产占比≤30%”策略后，平台实时监控各链质押市场数据（如以太坊质押年化4.5%、Avalanche质押年化8%），自动将Universal资产按比例分配至收益最高的链上优质节点，无需人工干预。
    
3.  **跨链质押自动化执行与状态同步**：当平台触发质押策略时，由ZetaChain Gateway与Universal合约协同完成全流程自动化：① 调用ZRC-20的`depositAndCall`接口，将用户的Universal资产划转至目标链质押节点；② 同步生成质押凭证（基于ZRC-721的全链质押NFT，记录质押资产、节点、收益周期等信息）；③ 质押状态实时同步至ZetaChain核心网络，用户在平台界面可实时查看各链资产质押详情，包括节点健康度、已产生收益等。
    
4.  **全链收益自动归集与灵活支取**：各链质押收益（如节点奖励代币）会自动转换为Universal Token，实时归集至用户的平台账户，无需用户逐链提取。用户可随时发起支取请求，选择将收益或质押本金提取至任意支持的公链——例如将以太坊质押产生的收益提取至Solana链用于交易，平台通过ZRC-20的`withdraw`接口快速完成跨链操作，资金到账时间由ZetaChain跨链确认速度决定（通常30秒内）。
    
5.  **智能风险控制与节点容错**：平台接入ZetaChain的全链节点健康监测数据，当某条链的质押节点出现故障风险时，系统会自动触发“资产迁移”策略——将该节点的Universal质押资产快速转移至同链其他优质节点，并通过跨链消息同步至用户，确保质押过程不中断、收益不损失。
    

3\. 场景价值与技术支撑

该平台的核心价值在于“用技术标准化解决操作复杂性”，其功能实现完全依赖ZetaChain的全链资产生态：① ZRC-20/ZRC-721标准实现了多链资产的统一封装与跨链流通，为资产聚合提供基础；② ZetaChain的Gateway解决了不同公链的协议适配问题，确保质押指令在各链顺畅执行；③ 核心网络的状态同步机制保障了质押数据、收益数据的实时性与一致性，为自动化策略提供可靠数据支撑。对用户而言，平台将“多链质押”简化为“一次设置、全程托管”，大幅降低参与门槛；对开发者而言，Universal资产的标准化接口减少了多链适配代码量，仅需调用ZetaChain SDK即可实现全链功能。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->


# Day 4：Universal App + Hello World 心智模型学习笔记

**日期**：2025年11月27日 星期四 **核心主题**：Universal App认知深化与Hello World Demo落地规划

## 一、学习目标拆解与核心导向

今日学习完成“认知构建”与“落地规划”两大核心任务，为后续开发打基础：

1.  **认知目标**：理解Universal App合约“跨链原生”的本质——即合约逻辑天然覆盖多条公链，而非单链合约的“跨链适配版”。
    
2.  **规划目标**：明确Hello World Demo的技术栈选型与工作流，清晰界定“合约开发-前端交互-RPC连接”三大模块的核心职责，避免后续开发方向混乱。
    

## 二、学习资料重点梳理

### （一）核心资料：ZetaChain Developers EVM / Tutorials 部分

资料地址：[https://www.zetachain.com/docs/developers](https://www.zetachain.com/docs/developers)

-   **重点关注模块**：**EVM 章节**：核心提炼“Universal EVM与传统EVM的差异”——支持跨链指令（如`zetaSend`），能直接读取非EVM链数据，这是开发全链合约的技术基石。
    
-   **Tutorials 结构**：观察教程中“合约-部署-交互”的通用流程，发现所有Demo均包含“跨链参数配置”（如指定目标链ID、网关地址），这是Universal App的标志性特征。
    
-   **延伸思考**：传统单链Hello World合约仅实现“存储/打印字符串”，而全链版需考虑“跨链传递信息”——比如在EVM链触发合约，让Solana链能读取到该信息，这是今日规划的核心差异点。
    

### （二）扩展资料：Tutorials 中的 Swap / Messaging 结构

-   **共性结构提炼**：无论Swap（跨链兑换）还是Messaging（跨链消息），均遵循“**触发链合约 → ZetaChain核心层 → 目标链合约**”的三段式结构，其中ZetaChain通过Gateway自动完成跨链数据传递。
    
-   **对Hello World的启发**：可简化该结构，实现“单触发链 → ZetaChain → 多目标链”的消息同步，即完成全链版Hello World的核心逻辑。
    

## 三、Universal App 合约的核心特征

1\. 部署载体唯一：核心合约仅部署在ZetaChain，无需在以太坊、比特币等链重复部署；

2\. 跨链指令原生：合约中可直接调用ZetaChain提供的跨链API（如消息发送、资产转移）；

3\. 链间数据互通：能主动读取或被动接收来自不同公链的数据，实现全链状态同步。

## 四、实践作业：应用构想与Demo工作流规划

### （一）Universal App 核心逻辑构想

结合全链应用特性，我计划开发的首个Universal App具体逻辑如下：

1.  核心功能：用户在任意一条支持的公链（如以太坊测试网）发起交易，输入一段文本消息，该消息会被同步存储到ZetaChain，并可被其他公链（如Solana测试网）的用户读取，实现“一条消息，全链可见”。
    
2.  存储功能：合约中定义一个映射（mapping），关联“消息ID”与“消息内容+发送链ID+发送时间”；
    
3.  跨链同步：用户在以太坊发送消息后，合约通过ZetaChain API将消息ID同步至Solana网关，Solana用户可通过调用合约查询该消息ID对应的内容；
    
4.  查询功能：提供公开函数，支持按消息ID或发送链ID查询历史消息。
    

### （二）Hello World Demo 工作流确定

1\. 开发工具链选型：CLI + Hardhat

-   ZetaChain CLI提供了合约部署、跨链交易触发等核心命令，是与ZetaChain交互的必备工具；
    
-   Hardhat是以太坊生态成熟的开发框架，支持Solidity编译、测试、部署全流程，且ZetaChain提供Hardhat插件（如`@zetachain/hardhat-zeta`），可无缝适配Universal EVM；
    
-   **流程**：Hardhat编译合约 → ZetaChain CLI部署合约至测试网 → Hardhat编写测试用例验证功能 → CLI触发跨链交互。
    

2\. 运行环境选择：ZetaChain 测试网

-   ZetaChain测试网提供完整的跨链基础设施（RPC、Faucet、Explorer），可真实模拟以太坊、Solana等公链的跨链交互，且测试币获取便捷，便于验证全流程。
    
-   测试网无资产损失风险，可反复调试跨链参数（如链ID、Gas设置）。
    
-   已提前获取ZetaChain测试网ZETA代币（通过Faucet），并配置好测试网RPC地址（写入Hardhat配置文件）。
    

### 今日收获：

理解了ZetaChain作为“跨链大脑”的核心价值；明确了Hello World Demo体现“跨链数据交互”的全链特性；完成工具链与环境选型，为后续开发做好了铺垫。
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->



# Day 3：ZetaChain & Universal Blockchain 核心概念学习笔记

**日期**：2025年11月26日 星期三 **核心主题**：Universal Blockchain系列概念解析与ZetaChain架构可视化

## 一、学习目标拆解与达成标准

1.  **概念理解**：不仅需掌握“通用区块链/Universal EVM/Universal App/Omnichain Smart Contract”的定义，更要厘清四者间的逻辑关联，能结合ZetaChain场景举例说明。
    
2.  **架构可视化**：绘制的架构图需明确“ZetaChain-多条公链-Gateway”的层级关系与数据流向，标注核心组件的核心作用，做到“图清意明”。
    

## 二、核心学习资料梳理（含重点方向）

| 资料类别 | 资料名称与地址 | 重点学习方向 |
| --- | --- | --- |
| 核心资料 | Universal Apps 概览https://www.zetachain.com/docs/start/app | 1. Universal App与传统跨链应用的差异；2. 基于ZetaChain开发Universal App的优势；3. 典型应用场景案例 |
| 核心资料 | 开发者总览（Universal EVM等）https://www.zetachain.com/docs/developers | 1. Universal EVM的技术特性；2. Gateway的跨链交互机制；3. ZetaChain跨链技术的核心原理 |
| 扩展资料 | 为什么在ZetaChain上开发？https://www.zetachain.com/docs/start/build | 提炼ZetaChain的核心竞争力，关联Universal系列概念的价值 |
| 扩展资料 | ZetaChain架构设计https://www.zetachain.com/docs/developers/architecture/overview | 快速抓取“节点网络”“跨链协议层”等关键架构模块，辅助理解整体逻辑 |

## 三、核心概念解析（基于资料的个人理解）

### （一）基础概念关联

核心逻辑：**通用区块链（ZetaChain）** 通过 **Universal EVM** 提供开发底座，支撑开发者构建 **Universal App**，而 **Omnichain Smart Contract（全链智能合约）** 是Universal App的核心实现载体，**Gateway** 则是实现跨链交互的关键桥梁。

### （二）关键概念

**1\. Universal App（通用应用）**简单来说，Universal App是一种“一次开发，全链可用”的区块链应用，它不需要为每条公链单独适配代码，就能在Bitcoin、Ethereum、Solana等不同链上与用户和资产交互。举个例子：传统跨链DeFi应用，可能需要在ETH链部署一套合约，在BSC链再部署一套，且两套合约间需额外开发跨链同步逻辑；而基于ZetaChain的Universal App，只需开发一套核心逻辑，就能同时服务于多条链的用户，用户在ETH链的资产和Solana链的资产可直接通过该应用完成交互，无需切换应用或依赖第三方跨链工具。核心价值：降低开发者跨链开发成本，提升用户跨链使用体验，打破不同公链间的“信息孤岛”和“资产孤岛”。

**2\. Gateway（网关）**Gateway是ZetaChain实现“跨链通信”的核心组件，相当于连接ZetaChain与其他公链的“翻译官”和“传送带”。它的核心作用主要有两个：

一是“信息翻译与传递”：不同公链的交易格式、数据标准不同，当某条公链（如Ethereum）上发生用户操作时，Gateway会将该操作的关键信息（如用户地址、操作类型、资产金额）转换为ZetaChain能识别的格式，传递给ZetaChain核心网络；同时，ZetaChain的指令也会通过Gateway转换为对应公链的格式，下发到目标公链执行。

二是“资产跨链保障”：在资产跨链过程中，Gateway会协同ZetaChain的质押机制，确保原链资产锁定与目标链资产 mint（铸造）、burn（销毁）的同步性，防止资产双重花费，保障跨链资产的安全性。比如用户将ETH链上的USDT跨到Solana链，Gateway会确认ETH链上的USDT已锁定后，通知ZetaChain在Solana链上为用户铸造等量的跨链USDT，整个过程由Gateway全程监控验证。

**3\. Universal EVM（通用以太坊虚拟机）**EVM是以太坊生态的核心开发环境，而很多公链（如BSC、Polygon）虽兼容EVM，但仍需单独适配。ZetaChain的Universal EVM，是在EVM基础上做了“跨链扩展”的虚拟机，它不仅支持开发者使用Solidity等EVM生态的编程语言，更能让基于它开发的智能合约具备“跨链调用能力”——合约可以直接读取其他非EVM链（如Bitcoin、Solana）的数据，或向这些链发送操作指令，这是传统EVM和兼容EVM所不具备的核心能力。

**4\. Omnichain Smart Contract（全链智能合约）**这是Universal App的“大脑”，是部署在ZetaChain上的智能合约，它能通过Gateway与多条公链进行交互。与传统合约不同，它的逻辑中包含了跨链交互的原生支持，比如可以在同一合约函数中，同时处理Ethereum的ETH和Solana的SOL的兑换逻辑，无需依赖外部跨链协议。】

## 四、实践作业：架构图绘制与说明

### （一）ZetaChain 核心架构图（ZetaChain + 多公链 + Gateway）

![exported_image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/xzzlks/images/2025-11-26-1764160241728-exported_image.png)

### （二）架构图说明

1.  **核心层级**：ZetaChain核心网络是整个架构的“中枢”，其中Universal EVM为全链智能合约提供运行环境，跨链协议层负责处理所有跨链交互逻辑，节点网络通过共识机制保障数据安全与不可篡改。
    
2.  **连接层级**：Gateway是ZetaChain与各公链的“专属连接通道”，每条公链对应一个独立的Gateway，确保跨链数据传递的精准性和安全性。
    
3.  **数据流向**： 正向流：用户在某条公链（如Ethereum）上发起操作后，操作数据先传递至对应Gateway，由Gateway完成格式转换后提交给ZetaChain的跨链协议层处理；反向流：ZetaChain处理完成后，将执行结果通过对应Gateway转换为目标公链的格式，下发至目标公链（如Solana），最终完成跨链操作的闭环。
    

-   **收获**：1. 厘清了Universal系列核心概念的逻辑关系，不再混淆；2. 理解了Gateway在跨链中的核心作用，掌握了ZetaChain架构的核心层级；3. 通过绘图将抽象的架构具象化，加深了对“全链交互”的理解。
    
-   **不足**：1. 对Universal EVM的技术实现细节（如如何兼容非EVM链）理解不够深入；2. 架构图中未体现ZetaChain的质押机制与Gateway的协同逻辑，需进一步补充。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->




### ZetaChain CLI 安装与验证（本地环境：Windows）

1.  **安装步骤**： 前置依赖：确认已安装Go（版本≥1.20）。打开命令提示符（CMD）或PowerShell，输入`go version`验证；若未安装，访问Go官网（[https://go.dev/dl/）下载Windows版安装包，勾选“Add](https://go.dev/dl/）下载Windows版安装包，勾选“Add) Go to PATH”选项后完成安装。
    
2.  安装CLI：在CMD或PowerShell中执行命令`go install github.com/zeta-chain/cli/cmd/zetacli@latest`，等待命令执行完成。
    
3.  验证安装：输入`zetacli --version`，若成功输出版本信息（如v1.0.0），则说明安装完成。
    
4.  **遇到的问题与解决**： 问题1：执行`zetacli`命令时提示“不是内部或外部命令，也不是可运行的程序”。
    
5.  解决1：右键“此电脑”→“属性”→“高级系统设置”→“环境变量”，在“用户变量”的“Path”中添加`%GOPATH%\bin`（GOPATH默认路径为C:\\Users\\你的用户名\\go），添加后重启CMD或PowerShell即可。
    
6.  问题2：安装Go后执行`go version`仍提示命令无效。
    
7.  解决2：重新运行Go安装包，确保“Add Go to PATH”已勾选；若已勾选，手动在“系统变量”的“Path”中添加Go安装路径（通常为C:\\Program Files\\Go\\bin），重启终端验证。
    

### **测试网关键入口汇总**：

-   RPC地址：[https://zetachain-athens-evm.blockpi.network/v1/rpc/public（文档获取的具体地址）](https://zetachain-athens-evm.blockpi.network/v1/rpc/public（文档获取的具体地址）)
    
-   Faucet链接：[https://zetachain.faucetme.pro/（输入钱包地址即可领取测试币，每日限额）](https://zetachain.faucetme.pro/（输入钱包地址即可领取测试币，每日限额）)
    
-   Explorer地址：[https://testnet.zetachain.explorers.guru/](https://testnet.zetachain.explorers.guru/)
    

### Qwen API 连通性测试

Postman测试

1.  设置请求方式：POST，请求URL：[https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation。](https://dashscope.aliyuncs.com/api/v1/services/aigc/text-generation/generation。)
    
2.  设置请求头：Content-Type: application/json；Authorization: Bearer xxx。
    
3.  设置请求体：与终端测试一致的JSON内容，具体如下： `{ "model": "qwen-7b", "input": { "prompt": "请简单介绍ZetaChain" }, "parameters": { "temperature": 0.7 } }`
    
4.  发送请求，结果：同样返回200状态码及有效响应，验证Postman环境连通性正常。
    

1\. 收获：成功完成ZetaChain CLI本地安装、Qwen API连通性测试，明确了ZetaChain测试网核心服务的使用入口，为后续开发奠定了环境基础。

2\. 不足：对ZetaChain CLI的具体命令使用尚不熟悉，Qwen API的高级参数配置（如流式响应、上下文管理）未深入探索。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->





# ZetaChain

### 概述

ZetaChain是实现跨链互操作性的区块链项目，使智能合约可以访问多个区块链的数据和资产，为开发者构建了一个“全链”开发平台

## 功能特点

## 链式编排

能通过单一智能合约协调跨多条区块链的交易，在同一框架内，基于ZetaChain的应用可以管理来自不同区块链的代币转账和合约调用并发起向连接网络的交易。链式编排能力简化了开发流程，降低了发生错误的可能性，并确保你的应用可以随着更多的区块链集成而扩展。

### 前后兼容性

在ZetaChain部署的应用会自动连接到所有ZetaChain生态系统中的网络，即使有新区块链的加入，你的应用依然可以与这些网络进行兼容。此功能简化了维护，无需因新区块链加入而更新合约或重新部署，提供了稳定的用户体验。

### 转移本地令牌

ZetaChain 上通用应用的核心功能之一，能通过本地代币在所有连接链之间转移价值，意味着用户可以执行涉及每个区块链实际原生资产的交易。跨链代币转移功能提升了流动性，无需中介或 复杂的令牌包装机制。简化了用户体验，降低了跨链交易相关风险，增强了跨链运营中的信任和可靠性。

### 熟悉的开发者体验

ZetaChain在UEVM上运行智能合约， 该系统与以太坊的EVM完全兼容。意味着开发者可以使用Solidity或其他兼容EVM语言开发智能合约。支持流行开发工具，如Foundry、Hardhat、Slither，使开发者能使用熟悉的工作流程、调试工具和测试框架，使流程更加完善，打造更高效、更少出错的通用应用。

### Gasless EVM执行环境

用户可以调用运行在ZetaChain的EVM上的应用，以及所有连接链的合约，只需在连接链上支付gas。降低了用户开支，提升应用可访问性

# 开发环境配置与部署

在开始之前需配置以下工具：

Node.js（v21或更晚版本）运行 ZetaChain CLI 并管理 JavaScript 依赖

Yarn 或 npm 用于安装和更新项目库

Git 管理项目资源，与他人合作编程

JQ 轻量级命令行JSON处理器， 解析Localnet输出和编写shell脚本

Foundry 快速、模块化的以太坊工具包，编译合约、管理依赖和运行 Localnet

### 环境配置

安装ZetaChain CLI：npm install -g zetachain

重要工具，进行跨链调用、转账代币、跟踪跨链交易，管理跨多个网络的账户，查询等

打印所有当前连接在ZetaChain上的链：zetachain query chains list

用于确认设置是否正常

Localnet：环境包括ZetaChain、EVM、Solana、Sui和TON，是构建和测试通用应用最快的方法，无需依赖公共测试网。

运行Localnet需安装Foundry，它提供了 EVM 开发的核心工具，是启动的必要条件。

### Foundry安装

（1）预编译的二进制文件，从GitHub上下载：[https://github.com/foundry-rs/foundry/releases](https://github.com/foundry-rs/foundry/releases)

（2）使用Foundryup，Foundryup 是 Foundry 工具链的官方安装程序，终端执行以下命令：curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz) | bash

注意！Foundryup目前不支持Powershell或命令提示符（Cmd）Windows用户需安装并使用Git BASH或WSL作为终端

（3）使用最新稳定版的Rust构建，旧版需使用以下命令更新：rustup update stable

Windows用户还需安装最新版的Visual Studio并安装“用C++进行桌面开发”工作负载

（4）通过Cargo安装：cargo install --git [https://github.com/foundry-rs/foundry](https://github.com/foundry-rs/foundry) --profile release --locked forge cast chisel anvil

foundryup --branch master 从master分支安装 foundryup --path path/to/foundry 指定Foundry的安装路径

（5）手动从本地Foundry仓库副本构建：

\# clone the repository

git clone [https://github.com/foundry-rs/foundry.git](https://github.com/foundry-rs/foundry.git)

cd foundry

\# install Forge

cargo install --path ./crates/forge --profile release --force --locked

\# install Cast

cargo install --path ./crates/cast --profile release --force --locked

\# install Anvil

cargo install --path ./crates/anvil --profile release --force --locked

\# install Chisel

cargo install --path ./crates/chisel --profile release --force --locked

（6）GitHub Actions CI安装：[https://github.com/foundry-rs/foundry-toolchain](https://github.com/foundry-rs/foundry-toolchain)

（7）Docker容器拉取最新版本：docker pull [ghcr.io/foundry-rs/foundry:latest](http://ghcr.io/foundry-rs/foundry:latest)

Foundry仓库本地构建Docer镜像：docker build -t foundry .
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
