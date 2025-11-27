---
timezone: UTC+8
---

# Ryan

**GitHub ID:** Appler-R

**Telegram:** @Appler_Ry

## Self-introduction

AI 开发者, Qwen 实战派，看好Web3+ 模块化链
(如 Zetchain)的下一代应用落地。

## Notes

<!-- Content_START -->
# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->
![1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-27-1764250272995-1.png)

终于克服了依赖缺失和路径配置等重重报错，**成功在本地跑通了最核心的跨链 Swap 业务模拟**。

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-27-1764258066434-__.png)
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->


### Universal EVM 与 Universal App

_这是 ZetaChain 最具革命性的地方。_

Universal EVM (通用 EVM / zEVM)

### 它是什么：ZetaChain 是一条公链，它的执行层叫 zEVM。

它不仅兼容以太坊（你可以用 Foundry 写代码），

更拥有“上帝视角”，比如你可以通过Zetachain上的Universal App，你能够监控所有网络（Bitcoin、Ethereum等),调用所有代币

即 普通的 EVM 只能读取自己链上的数据。而 zEVM 可以直接读取和写入连接链（Connected Chains）上的状态。

**左手抓来 BTC**：合约感知到用户转入了比特币（ZetaChain 把它映射为 ZRC-20 BTC）。

**原地通过 DEX 交易**：合约在 ZetaChain 内部的流动性池里，直接把 ZRC-20 BTC 换成 ZRC-20 ETH。

-   注意：这一步完全发生在 ZetaChain 内部，速度极快，不需要去比特币或以太坊网络排队确认。
    

**右手递出 ETH**：合约判定交易完成，把 ZRC-20 ETH 销毁，并指示以太坊网络上的 Gateway 给用户转真正的 ETH。

普通的 EVM 只能读取自己链上的数据。而 zEVM 可以直接读取和写入连接链（Connected Chains）上的状态。

比喻：普通公链像是一个只能处理本国货币的“本地银行”；

Universal EVM 是一个“国际中央银行”，能同时看到并操作所有国家的账户。

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-26-1764164375932-__.png)

**这里的“上帝视角”优势在哪？**

既然都要付钱，ZetaChain 的优势是什么？

优势在于：打包支付 (Gas Abstraction)

传统跨链：你需要钱包里同时有 BTC（付比特币 Gas）、有 ZETA（付中间层 Gas）、有 ETH（付以太坊 Gas）。

没 ETH 你就寸步难行。

**ZetaChain**：你可以只用 BTC。

当你发送 BTC 时，ZetaChain 的智能合约可以在内部自动把一小部分 BTC 换成 ZETA 和 ETH，

帮你把后面所有的路费都交了。

用户体验：我付了 1.0 BTC，收到了相当于 0.99 BTC 的以太坊代币。中间的损耗自动扣除，我不需要关心具体细节。

损耗是物理存在的，无法避免。但在代码层面，**ZetaChain 允许开发者编写逻辑，从用户输入的资产中自动扣除这些费用**，从而让用户感觉不到“不仅要跨链，还得去买各种奇怪的代币当 Gas”的痛苦。

### 我举个例子，比如你要去日本买个手办

**按照传统跨链模式，你需要：**

1.  把你的非日元法币换成日元
    
2.  拿着你的日元去找到a中介，让他给你开一张手办代金券
    
3.  你去日本商店兑换，那么问题来了，商店不受怎么办，那只好乖乖换回日元
    

**ZetaChain 模式**：

1.  你直接去日本商店刷 Visa 卡（美元等）。
    
2.  Visa（ZetaChain）在后台自动计算汇率、扣除手续费、处理结算。
    
3.  **日本商店直接收到了真日元**。
    

### 总结

ZetaChain 的价值在于：

1.  **原生对原生**（不留垃圾封装代币）。避免假币和桥被黑的风险
    
2.  **单点逻辑**（一套代码管全链）。
    
3.  **唤醒比特币**（让 BTC 能跑智能合约）。
    

_省 Gas 费步骤只是因为这个架构太高效了，顺便实现的一个小功能而已。_

### Universal App (通用应用 / Omnichain Smart Contract)

它是什么：这是一种只部署在 ZetaChain 上的智能合约。

### **区别：**

传统跨链：需要在 A 链部署合约，B 链部署合约，中间再搞个桥。

Universal App：“一次部署，全链通用”。你只需要在 ZetaChain 上写一份代码，就可以管理以太坊上的 ETH、比特币网络上的 BTC、以及 Polygon 上的 USDC。

核心逻辑：用户在其他链操作，ZetaChain 上的这个合约会响应并处理逻辑。

### 工作流程

```
简易架构
```

```
    %% 定义样式
```

```
classDef external fill:#f9f,stroke:#333,stroke-width:2px;
classDef zeta fill:#ccf,stroke:#333,stroke-width:4px;

classDef component fill:#fff,stroke:#333,stroke-dasharray: 5 5;
subgraph "Connected Chain A (例如: Ethereum)"
User_A[用户] --> Gateway_A[Gateway 合约]
```

```
    end
```

```
    subgraph "ZetaChain (中间枢纽)"
Observer[观察者节点] -->|1. 监听事件| zEVM[Universal EVM / 通用合约]
zEVM -->|2. 处理逻辑 & 状态变更| TSS[TSS 签名者节点]
```

```
    end
```

```
 subgraph "Connected Chain B (例如: Bitcoin / Polygon)"
 TSS -->|3. 写入/转账| Gateway_B[Gateway / Vault]
 Gateway_B --> User_B[接收者]  
  end
```

```
    %% 连接关系
```

```
    Gateway_A -.-> Observer
   class User_A,Gateway_A,Gateway_B,User_B external;
   class zEVM,Observer,TSS zeta;
```

**工作流程解释 (The Workflow):**

假设你要做一个“全链去中心化交易所 (DEX)”，用户想用 Ethereum 上的 ETH 买 Bitcoin 上的 BTC。

1.  **触发 (Inbound)**：
    
    -   用户在 **Ethereum** 上，向 **Gateway 合约** 发送 ETH（附带一条消息：“我要买 BTC”）。
        
2.  **观察 (Observation)**：
    
    -   ZetaChain 的 **观察者节点 (Observer)** 盯着所有连接链。它看到了 Gateway 上发生的这笔交易。
        
3.  **处理 (Processing within zEVM)**：
    
    -   这笔交易被确认为有效，ZetaChain 上的 **Universal App (通用合约)** 开始运行。
        
    -   它在合约内部计算：收到了多少 ETH，按当前汇率能换多少 BTC。
        
    -   _关键点_：这里不需要在比特币网络上部署合约（因为比特币不支持）。所有逻辑都在 ZetaChain 上完成。
        
4.  **写出 (Outbound)**：
    
    -   Universal App 计算完毕，指示 **TSS 签名者节点**：“去比特币网络，给用户 B 转 0.1 个 BTC”。
        
    -   TSS 节点生成签名，在 **Bitcoin 网络** 上执行转账。
        

### 三、 关键组件详解

1\. Gateway 的角色与跨链消息流程

-   **角色**：Gateway 是 ZetaChain 在其他链（如 Ethereum, BSC）上设立的“大使馆”或“收发室”。
    
-   **对于智能合约链 (EVM)**：它是一个真实的智能合约。用户把资产存进去（Lock），或者调用它来发送消息。
    
-   **对于非智能合约链 (Bitcoin)**：它表现为一个 TSS Vault 地址（一个多签钱包地址），用户往这个地址转账即视为与 Gateway 交互。
    
-   **流程总结**： `用户调用 Gateway` -> `事件 (Event) 被发出` -> `ZetaChain 捕获并处理` -> `ZetaChain 修改自身状态 或 触发目标链 Gateway 操作`。
    

2\. Connected Chains (连接链) 的意义

这里包括了 Bitcoin, Ethereum, Solana, BNB Chain 等。

-   **资产统一**：以前 BTC 就是 BTC，ETH 就是 ETH，老死不相往来。ZetaChain 将它们连接起来，让开发者可以把它们当作**ZRC-20 代币**（一种在 ZetaChain 上代表外部资产的标准）在同一个合约里操作。
    
-   **赋予比特币智能**：这是最大的意义。Bitcoin 本身没有智能合约，但通过连接到 ZetaChain，你可以在 Universal App 里写逻辑来控制 BTC。这相当于**给比特币外挂了一个智能合约层**。
    
-   **解决碎片化**：用户不需要关心你的应用部署在哪，他们只需要用自己习惯的钱包（比如 MetaMask 连以太坊，或者 Unisat 连比特币），就能使用你的服务。
    

普通 EVM：是单机版游戏。只能玩自己本地的数据。

Universal EVM ：是联网版的指挥中心。它把所有外部链的资产都“投影”到了自己身上（通过 ZRC-20）。

优势：在用 Foundry 写代码时，感觉就像是在操作本地变量一样，但实际上你是在指挥 Bitcoin 和 Ethereum 上的真实资产流动。

### 关于ZRC-20,我自己画了个表

![1.png33.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-26-1764148308407-1.png33.png)

## **Universal App** 和 **ZRC-20** 的核心逻辑：

-   在不接触比特币底层代码的情况下，用 Solidity 这种简单语言去操控比特币。
    
-   你的合约是部署在中间的 ZetaChain 上，远程指挥两边的链。
    

**今天简单小结**

-   **Windows + WSL + VS Code + Warp** 的这套“黄金开发流”，个人觉得非常好用，warp自然语言确实方便
    
-   在 VS Code 里写代码，去 Warp 里跑命令。
    
-   **写了主合约 (**`PocketMoney.sol`**)**：定义了一个逻辑，“谁调用我，我就给谁发 ZRC-20 格式的比特币”。
    
-   **写了测试脚本 (**`PocketMoney.t.sol`**)**：模拟了一个“上帝”环境，假装测试网里的 ZRC-20 合约是存在的，并成功欺骗虚拟机完成了提现。
    

在运行过程中，发现了一个bug，AI给我跑的代码出现了问题，恰恰是我今天刚学的知识点：十六进制字符串是用字符0~9，a~f来显示，发现并改正的AI语法错误：把 `hex"..."` **修正为了** `bytes("...")`

### `（因为 h与x不在规定的十六进制范围）`

-   **文件混淆**：把测试代码误贴进源文件导致报错，然后成功把它们分拆回 `src` 和 `test` 文件夹。
    

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-26-1764161484779-__.png)![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-26-1764161452115-__.png)

### 关于哈希

**哈希值 = 数据的唯一指纹。**

给任意数据（一句话、一部电影、一个区块），用哈希算法（如 SHA-256）一算，输出固定长度乱码（如64位十六进制），

**即数据经函数处理得到产物**

具备三大特性：

**唯一性**：不同输入，几乎必然得不同哈希；

**不可逆**：知道哈希，无法反推原数据；

**雪崩效应**：原数据改一个标点，哈希全变。

**_在区块链里怎么用？_**

每个区块含前一个区块的哈希 → 串成链，改前面任一块，后面所有哈希全错；

交易用发送者私钥签名，验证时用公钥+哈希 → 证明“这人确实发了这内容”；

Merkle树用哈希聚合交易 → 一个根哈希代表所有交易，轻节点靠它快速验证某笔交易是否存在。

### **这是哈希值的十六进制字符串表示，为64个字符，对应256位（bit）二进制。**

**哈希值本质：一个256位（32字节）的二进制数**，例如：

10100001101100101100001111010100…11110000

**为方便显示/传输**，转为十六进制字符串：

每4位二进制 → 1个十六进制字符（0-9, a-f）

256位 ÷ 4 = 64个字符

例：a1b2c3d4…f0（共64字符）

**本质**：0-9、a-f只是**显示方式**，底层仍是比特流。换种编码（如Base64），字符就不同——但哈希值本身不变。

一句话：哈希是让“篡改可被瞬间发现”的数学锚点。
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->










## 1\. 开发环境处理 (WSL Linux)

起步发现WSL被 Docker 占用、WSL 无法启动、忘记密码。

解决： 成功切换默认发行版、暴力重置 Root 密码、修复了 Python 虚拟环境组件。

结果： 拥有了一个稳定、独立的 Linux 开发系统，这是所有高级开发的基石。

wsl --set-default Ubuntu

wsl

![换.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764053478589-_.png)

## 2\. 掌握了 ZetaChain 底层工具

CLI（命令行界面）

我在领水龙头测试币时发现CLI钱包与EVM钱包区别：

**CLI:链名开头（如zeta1,cosmos1等）**

生态：Cosmos SDK系列；

使用方式：命令行，开发，脚本交易，节点交互等；

**EVM：0x开头**；

生态：以太坊，BNB等

使用方式：浏览器插件，移动钱包，连接DAPP

安装编译 ZetaChain 所需的 Go 语言和系统工具。 代码 (WSL 中执行)：

**关键突破： 学会了“远程节点 (RPC)”的概念。当本地节点跑不通时，通过连接全球公共节点来获取数据。**

RPC 节点 = 别人的全节点 + 你通过接口远程调用它

你本地不跑区块链节点，也可以照样读写链上的数据，因为有人在远程替你跑好了完整节点，你只需要通过网络向它请求即可。

可以简单理解为本地数据库与云数据库

![1ew.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764054544623-1ew.png)

### **Qwen库的安装**

![库.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764055872186-_.png)

```
# 安装基础工具
```

```
sudo apt update && sudo apt install git make build-essential curl python3.12-venv -y
```

```
# 安装 Go 1.22
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin:~/go/bin' >> ~/.bashrc && source ~/.bashrc
```

```
下载源码编译 zetacored，并将其移动到公共目录以便 Python 调用。 代码 (WSL 中执行)：
```

```
Bash
```

```
# 下载并编译
git clone https://github.com/zeta-chain/node
cd node
```

```
make install
```

```
# 关键：复制到公共目录 (解决 Python 找不到命令的问题)
sudo cp ~/go/bin/zetacored /usr/local/bin/
sudo chmod +x /usr/local/bin/zetacored
```

```
创建钱包 & 获取地址，说明： 生成钱包并在本地保存私钥，同时获取 EVM (0x) 格式地址。
```

```
Bash
```

```
# 创建新账户
```

```
zetacored keys add myaccount
# 查看 0x 格式地址
zetacored debug addr <自己的zeta1地址>
mkdir -p ~/qwen-project && cd ~/qwen-project
python3 -m venv venv
source venv/bin/activate
```

```
pip install dashscope requests
```

### 解决区块链连接问题 (关键点)

```
说明： 本地没有节点，必须在代码中指定远程公共节点 (RPC) 才能查到数据。 代码逻辑 (Python 中)：
```

```
Python
```

```
# 必须加上这个参数，否则会报 Connection Refused
```

```
NODE_URL = "https://zetachain-testnet-rpc.polkachu.com:443"
cmd = f"zetacored query bank balances {MY_ADDRESS} --node {NODE_URL} --output json"
```

```
运行最终完成的脚本。 代码 (WSL 中执行)：
```

```
Bash
```

```
# 激活环境 (如果没激活)
```

```
source venv/bin/activate
```

```
# 运行查价助手
```

```
python agent_price.py
```

```
# 运行 AI 资产分析 
```

```
python agent_final.py
```

## 3\. 构建了第一个 AI Agent (智能体)

开始只有一段死的 Python 代码，疯狂报错，让人难受

我在一番折腾（AI与资料查询，debug）后，搞出了能自动执行 Linux 命令的 Python 脚本。

呈现形式：agent\_[final.py](http://final.py) 和 agent\_[price.py](http://price.py) 成功实现了：

感知： 去区块链/网络上抓取数据（查余额、查币价）。

思考： 模拟 AI 进行逻辑判断。

行动： 生成人类可读的报告。

WSL 启动失败： 发现是docker-desktop 捣乱 → 已修复。

权限被拒： Permission denied / 找不到文件 → 学会了 sudo 和 cp。

Python 报错： ensurepip / dashscope 缺失 → 学会了 venv 环境管理。

API 报错： 401 Invalid Key → 学会了写仿真代码绕过验证。

**主要两个搞了agent**

agent\_[final.py](http://final.py)：第一个 Web3 资产管理脚本。

agent\_[price.py](http://price.py)：实时的加密货币行情助手。

**_嘿，在经历了v1与v2两个失败后，终于成功了_**

![调试.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764055394585-__.png)

### 此为final

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764054600969-__.png)

**自己重新跑时，忘了输入wsl，第一次用Linux，原谅自己啦，要注意与win终端区别**

### `cd ~/qwen-project`：准确回到了项目“大本营”。

### `source venv/bin/activate`：成功激活了装备包（注意到了绿色的 `(venv)`）。

### `python agent_final.py`：顺利运行了脚本。

### 此为price

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764054787635-__.png)

被自己逗笑了，test输成text了，再原谅自己一次hh

**此为测试Qwen**

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764055110297-__.png)

## 此为远程测试

![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-25-1764055268964-__.png)

# 总结：今天在本地跑通了 AI + Web3 最小可行性产品 (MVP)

## 但这远远不够

## 我发现还有许多问题需要处理，如文本不够简洁，环境配置还需加深理解等

## 歌未竟
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->














## 了解了一些基础知识

ZetaChain 账户你的链上身份（地址 + 私钥）

RPC 节点接口，跟链交互的“网关”

Faucet 测试网水龙头，领免费币

Explorer 区块链浏览器，看交易和合约

CLI zetacored 命令行工具

Qwen 账号 自己手机号注册即可

API Key调用 Qwen 的密钥

基础请求（Chat/Completion）：两种标准调用方式

chat为主，做聊天、Agent、带系统提示的场景

completion为老式补全，打辅助的

主要操作：

ZetaChain：领水（Faucet）→ 用公共 RPC → CLI 创建钱包

Qwen：注册百炼 → 拿 API-Key → 用 OpenAI 格式调用

社区的Lancy推荐了一款AI终端[Warp](https://www.warp.dev/j)

推荐使用

![1.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-24-1763995573422-1.png)

## 调用了Qwen的api

![2.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-24-1763995628808-2.png)![图片.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/Appler-R/images/2025-11-24-1763996430477-__.png)

嘿，社区氛围很好，我们一起前进！
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
