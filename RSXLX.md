---
timezone: UTC+8
---

# reece

**GitHub ID:** RSXLX

**Telegram:** @SXLX__

## Self-introduction

WEB3 ROOKIE

## Notes

<!-- Content_START -->
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
## **Step 1：定义 Function / Tool Schema**

放在 Qwen Agent 的函数里：

```
{
  "name": "place_sports_bet",
  "description": "Place a sports bet in KMarket.",
  "parameters": {
    "type": "object",
    "properties": {
      "sport": { "type": "string" },
      "league": { "type": "string" },
      "match": { "type": "string" },
      "selection": { "type": "string" },
      "amount": { "type": "number" },
      "amount_unit": { "type": "string" },
      "leverage": { "type": "number" },
      "chain": { "type": "string" }
    },
    "required": ["match", "selection", "amount"]
  }
}
```

* * *

## **🔧 Step 2：后端**

```
def place_sports_bet(params):
    print("——计划执行链上操作——")
    print(f"比赛：{params['match']}")
    print(f"方向：{params['selection']}")
    print(f"金额：{params['amount']}{params['amount_unit']}")
    print(f"杠杆：{params['leverage']}x")
    print(f"链：{params['chain']}")
    print("准备在 ZetaChain 上执行模拟交易...")
```

即可。

* * *

## **🔧 Step 3：本地 Demo 流程**

1.  输入自然语言
    
2.  Qwen Agent → 自动调用 place\_sports\_bet
    
3.  后端打印计划执行操作
    

时间有点赶。
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->


![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-12-03-1764774370241-image.png)

  
把我 50 U 兑换成 BSC 上的 MATIC  
  
识别并转换  
  
  
可以做一个agent去识别比如：  
我想买一个勇士VS湖人的比赛，买湖人赢，开五倍杠杆  
  
然后就应该去做到：  
**1\. 市场相关**

| 字段 | 说明 |
| --- | --- |
| event_name | 比赛名称（勇士 vs 湖人） |
| league | 联赛（NBA）如果能提取 |
| market_type | 市场类型：胜负 / 让分 / 大小分… |
| selection | 用户选择（湖人赢） |
| event_time | 可选，用于风控或确认未来赛事 |

* * *

### **2\. 下单相关**

| 字段 | 说明 |
| --- | --- |
| side | buy / sell / long / short（预测类一般是 buy 某一方） |
| amount | 下注金额（100U） |
| leverage | 杠杆倍数（5） |
| odds_type | 固定赔率 / AMM / 流动性池 AMM（可选） |
| slippage | 可选（DeFi 会需要） |

* * *

### **3\. 链上交易相关（如果走链）**

| 字段 | 说明 |
| --- | --- |
| chain | 在哪个链执行（如：ZetaChain / Arbitrum / Base） |
| token | 使用什么代币下注（USDT / USDC / ETH） |
| wallet_address | 用户地址（解析层一般不从自然语言提取） |

  
  
{

"intent\_type": "place\_bet",

"event\_name": "Warriors vs Lakers",

"league": "NBA",

"market\_type": "moneyline",

"selection": "Lakers",

"side": "buy",

"amount": {

"value": 100,

"token": "USDT"

},

"leverage": 5,

"chain": "ZetaChain",

"slippage": 0.01

}  
  
  
感觉开发起来是个问题，demo估计只能做到几个字段，技术有限啊！！！
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->



![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-12-02-1764683770580-image.png)

```python
class ToUpper(BaseTool):
    description = 'Convert a string to uppercase.'
    parameters = [
        {"name": "text", "type": "string", "description": "Input string", "required": True}
    ]
    def call(self, params: str, **kwargs) -> str:
        data = json5.loads(params)
        return json.dumps({"result": str(data["text"]).upper()}, ensure_ascii=False)

@register_tool('add')
class Add(BaseTool):
    description = 'Compute the sum of two numbers.'
    parameters = [
        {"name": "a", "type": "number", "description": "First number", "required": True},
        {"name": "b", "type": "number", "description": "Second number", "required": True}
    ]
    def call(self, params: str, **kwargs) -> str:
        data = json5.loads(params)
        a = float(data.get("a", 0))
        b = float(data.get("b", 0))
        return json.dumps({"result": a + b}, ensure_ascii=False)
```

```python
def get_bot():
    global _BOT
    if _BOT is None:
        _BOT = Assistant(llm=make_llm_cfg(), system_message="你是一个会使用工具的助理。", function_list=['to_upper', 'add'])
    return _BOT
```

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-12-02-1764684024689-image.png)

\- Tools 通过继承 BaseTool 并 @register\_tool(‘name’) 注册，向 Agent 暴露可调用能力。

\- Assistant 通过 function\_list 绑定工具，结合指令与上下文自动选择并调用。  
  
实现了自动调用
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->




## **目标用户**

-   **玩家**：手里资产分散在多链（BTC、ETH、BSC、Sol、Polygon），想要快速下注
    
-   **新手用户**：不懂桥、不懂兑换，但想参与预测市场
    
-   **专业套利玩家**：需要“自动化下单通道”与跨链流动性
    
-   **无法自行进行复杂跨链操作的海外用户**
    

* * *

## **想解决的问题**

1.  **多链用户下注门槛极高**  
    一般用户资产分散在 BSC / Solana / ETH，下注前需要：桥 → 换币 → 才能下单，体验非常断裂。
    
2.  **投注链单一，体验束手束脚**  
    用户必须去 KMarket 所在链操作，没法脱链下注。
    
3.  **自动化套利无法跨链**  
    机器人、量化策略想捕捉赔率差，但现在只能在同链操作。
    
4.  **业务团队想做多链市场入口，却缺底层跨链能力**  
    想让更多链的用户来玩，但没有跨链 infra → Zeta 正好补位。
    

* * *

## 🔗 **跨链 / 通用资产使用方式（核心！）**

### **① 用户任意链发起下注**

用户用：

-   ETH (Mainnet)
    
-   USDT (BSC)
    
-   SOL
    
-   或其他链资产  
    → 合约统一映射为 **ZRC-20 通用资产（如 U-USD）**
    

ZetaChain 自动：

-   收款
    
-   映射成 ZRC-20
    
-   路由到你的应用合约
    

* * *

### **② 应用合约在 ZetaChain 处理下注逻辑**

Universal App 在 ZetaChain EVM 层执行：

-   记录市场 ID / 赔率 / 金额
    
-   校验下注窗口
    
-   更新玩家状态
    
-   计算预期收益
    

所有链的用户 ≈ 在同一条链下注（但入口是多链）

* * *

### **③ 自动跨链投递到 KMarket（或下单合约）**

使用 **Cross-Chain Messaging**：

-   通知目标链的下单合约
    
-   执行真实下注动作（deposit）
    
-   或自动在 KMarket 合约上挂单
    

* * *

### **④ 比赛结束后自动结算（跨链可选）**

获胜资金：

-   可在 ZetaChain 结算
    
-   也可自动跨链返回用户原链
    
-   或兑换成任意链资产返回用户（最爽）
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->





## ZRC-20 是集成在 ZetaChain 全链智能合约平台中的代币标准

类似一个复制器，把其他链的币复制到zetaChain中，在zetaChain再转出去的时候再zetaChain上面ZRC-20生成的币就burn掉，然后从转账链通过托管合约再转去对应账户  
  
AI纠正：

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-28-1764299450886-image.png)

  
所以：ZRC-20 是外链资产在 ZetaChain 上的映射代币。  
外链资产被锁定后，ZetaChain 链上铸造对应 ZRC-20；  
当用户把资产提回外链时，ZRC-20 被烧毁，外链托管模块释放真实资产到目标链账户。  
它不是复制，是单源资产的跨链映射。

  
**ERC-20：单链、本地代币标准**

-   只能表示本链上的代币
    
-   不具备跨链能力
    
-   铸造、销毁、转账都发生在同一条链
    

### **ZRC-20：多链资产的统一映射标准**

-   表示“外链真实资产在 ZetaChain 上的映射代币”
    
-   外链资产被锁在托管模块中
    
-   ZetaChain 铸造对应数量的 ZRC-20
    
-   从 ZetaChain 提回外链时，ZRC-20 被烧毁，外链资产被释放
    
-   所有链来的资产在 ZetaChain 都以“同一种接口、同一种标准”存在  
    

## 当用户在购买某个商品或服务时，不再需要关心自己持有的资产来自哪条链，也不必提前换成商家指定的币种。  
无论用户用的是 **BTC、ETH、USDC、USDT、BNB、SOL** 或其他链上的资产，通用资产机制都能：

1.  **统一表示资产**：把不同链上的资产映射成 ZetaChain 上对应的通用表示（如通用稳定币 U-USD）。
    
2.  **自动路由支付**：商家可以指定自己想要最终收到的资产与链，而系统会自动处理跨链兑换与转账流程。
    
3.  **降低双方心智负担**：
    
    -   **用户**：只需支付，不需要思考“我用的链对不对”“商家收不收这个币”。
        
    -   **商家 / 协议**：始终拿到指定的目标资产，不需要自己构建跨链基础设施。
        

### 🎁 结果

用户体验更顺滑，开发者减少运维复杂度，多链资产像一条条光纤接入同一个“价值枢纽”。

通用资产的角色，就像 Web3 的“跨链支付底层接口”，把碎片化的链间资产变成可随处调用的统一能力。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->






Universal App 是一种**跨链应用**形式：  
应用逻辑只写在 ZetaChain 上，但能让不同链的用户（ETH / BNB / Polygon / …）都像是在**同一条链上直接交互**。  

###   
ZetaChain 作为“连接层”，负责：

-   接收来源链发来的数据 / 资产
    
-   在 ZetaChain 上执行合约逻辑
    
-   把结果分发到目标链
    
-   所有这套跨链流程对用户透明
    

Universal App规划：ZetaChain 把这些参数带进来，

我在合约记录：

\- 谁调用了

\- 从哪条链来

\- 输入内容是什么

maybe当成一个print  

#   
  
明天计划：

1.  用 CLI 创建一个 universal app skeleton
    
2.  用 Hardhat 成功编译官方示例合约
    
3.  在测试网上部署 “跨链打印机”
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->







## Universal App = 一个合约控制所有链的资产

**Universal App是部署在 ZetaChain 的 Universal EVM 上 的智能合约，它可以 直接操作多条外部区块链上的资产与数据，而不需要部署多链版本的合约（很蛋疼的就是之前做一个项目做完evm就要继续肝solana，其实差不多但是工作量会翻倍，还有联调测试的一些环节）**

## Gateway 是 ZetaChain 的跨链消息入口 / 出口。

Gateway = 外部链 ↔ ZetaChain 的官方跨链通信层

Universal App 负责逻辑，Gateway 负责和每条链沟通、执行真实链上动作  
  
  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-26-1764166895768-image.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->








未来两周希望可以理解到zetachain的特性，链与链交互原理之类的：  
① 每条外链上有 “ZetaChain 控制的链上账户”

例如：

-   BTC 主网：一个由 ZetaChain 验证者共同控制的 BTC 地址
    
-   ETH 主网：一个多签钱包，由验证者控制
    
-   BNB Chain：一个多签地址
    
-   Polygon：一个多签地址
    

这些统称为：  
👉 **ZetaChain external TSS accounts（外链门控账户）**

TSS = Threshold Signature Scheme 阈值签名

-   不是任何单个验证者能控制
    
-   达到阈值的大多数验证者才能签名
    
-   更像“去中心化多签”
    

* * *

# 🧩 ② 外链事件（交易）会被拉取到 ZetaChain

每个 ZetaChain 的验证者节点会同步：

-   BTC 链上来自用户的充值
    
-   ETH 链上的发送到 TSS 地址的交易
    
-   BNB Chain 的合约调用
    

当验证者检测到一个外链事件，会做：

🔹 提交外链证明  
🔹 ZetaChain 链内达成共识  
🔹 最终确认事件

这意味着：

> **ZetaChain 是一个多链事件的统一共识层**

* * *

# 🧩 ③ ZetaChain 合约执行后，可反向操作外链（跨链写入）

当 ZetaChain 链上的智能合约执行完成后，它可以触发：

-   外链资产转账
    
-   外链合约调用（例如 Uniswap swap）
    
-   外链 mint NFT
    
-   外链 burn 代币
    

这时 ZetaChain 会生成一个跨链消息（CCM）：  
👉 验证者 TSS 多签 → 在目标链执行交易

这就是“跨链执行”的本质。

* * *

# 📚 用一句话概括其跨链原理

> **ZetaChain 在外链上维持了一组去中心化的 TSS 多签账户；验证者监听外链事件，达成共识后触发链内合约；链内合约可以反向控制 TSS 账户在外链执行逻辑，从而实现安全的跨链读写。**

# AI可以直接部署

安装了ZetaChain CLI

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-25-1764077203711-image.png)

**ZetaChain 测试网资源整理：**

## 1) RPC 入口（功能 & 地址）

脚本、DApp、钱包、CLI 都是通过 RPC 与区块链网络通信的。  
比如发送交易、查询余额、部署合约。

地址copy了doc里面的一个：[https://zetachain-athens.g.allthatnode.com/archive/evm](https://zetachain-athens.g.allthatnode.com/archive/evm)

## 2) Faucet 入口（功能 & 地址）

领取faucet用于消费，支付测试链的gas

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-25-1764076580551-image.png)

地址：[https://zetachain.faucetme.pro/](https://zetachain.faucetme.pro/)

## 3) Explorer 入口（功能 & 地址）

查看交易、区块、地址余额、合约交互等。相当于一个庞大的数据库，记录链上发生的所有事情  
地址：[https://zetascan.com/](https://zetascan.com/)  
测试链一般是前面加testnet

API的调用

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-25-1764077327184-image.png)

之前跑过其他的玩意，给透支了哈哈哈，官方能不能送点
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->










1 ZetaChain 的核心是 跨链智能合约（Omnichain Smart Contract）类似中转

2 看了浏览器

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-24-1763986817074-image.png)

3 注册QWEN API

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/RSXLX/images/2025-11-24-1763986833813-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
