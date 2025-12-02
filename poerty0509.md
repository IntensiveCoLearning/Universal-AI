---
timezone: UTC+8
---

# poerty0509

**GitHub ID:** poerty0509

**Telegram:** @poerty0509

## Self-introduction

AI & Web3 Builder

## Notes

<!-- Content_START -->
# 2025-12-02
<!-- DAILY_CHECKIN_2025-12-02_START -->
**Day8**

关于Merkle Tree and Merkle Root

-   Merkle Tree存储transitions list
    
-   Merkle Root在the headers of the blockchain中，并被light nodes保存
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/poerty0509/images/2025-12-02-1764683597504-image.png)

-   用法：light nodes通过查询full nodes的相关hash,将查到的hash（\\log\_a n）和待验证的节点的hash计算Merkle Root,检查和已储存的Merkle Root是否匹配。查询方法如下：
    
    ![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/poerty0509/images/2025-12-02-1764683965888-image.png)
<!-- DAILY_CHECKIN_2025-12-02_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->

**Day7**

1.  关于挖矿的奖励：比特币、以太币
    
2.  工作量证明：挖矿的过程困难但是成果容易验证（比如破解组合锁）；耗电量大（有挖矿费用和交易费用作为补偿）；这类工作依赖于GPU（使用大量数据进行简单运算）而不是处理器的速度（使用少量数据进行复杂运算），原因是填组合数反复进行hash函数的运算属于前者
    
3.  block的不可变性：父代子代的递推确认；每多添加一个block，每个已连接的block就会被多确认一次，大于6次的时候就被认为不可改变了的。
    
4.  更多关于blockchain的细节：一个block包括 block header(The hash of the previous block , Timestamp , Merkle root , Nonce , Network difficulty target )
    
5.  节点类型：full node(例如 mining nodes,desktop wallet...):确保blockchain的可靠性，可被用来获取奖励，light nodes():使用简化的支付验证工具SPV进行交易验证和连接到完整节点，使得节点不用下载整个blockchain就能进行交易验证。
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->


**Day6**

阅读Beginning Ethereum Smart constracts Programming 心得

-   理解了交易中信任的核心作用，明白了区块链在现代交易中的本质创新是decentralized
    
-   接触了区块链如何运行：
    

各个block是如何chained:通过hashing（deterministic,单向的不可反解，单射）进行指纹建立，并将该指纹印在下一个block，该机制有效地应对了block被篡改地风险（如果有一个hacker要篡改一个交易，更改其中一个block，那么该block的hash就会发生巨大更改，和下一页block的hash无法对应上，那么该chain就会断开，除非他能继续更改下一个block的hash，并且追上更新的速度）；

mining:如果有多个node同时通过broadcast制作blocks，那么选择哪一个block接到block chain上来保证内容的可行度？机制：hash具有deterministic。但是平台给block留下了每个节点可自由发挥的Nonce,这样一来，hash就不确定了。所以每个Node就需要不断随机填入Nonce来计算hash，直到满足计算出来的hash符合要求，接着就可以交卷。这个时候，其他Nodes都会停下来验证他是否作弊，由于验证过程非常简单，所以能判断交卷的Node是否作弊并给予奖惩。如果验证正确就将其加入电子帐本。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->



**Day5**

1.想要运行forge语令，失败

在github上找到Foundry的官方GitHub发布页面，下载foundry\_nightly\_win32\_[amd64.zip](http://amd64.zip)

解压后添加到电脑高级系统设置里的环境变量path

接着运行

```
forge soldeer update
成功！
```

2.打开Universal.sol，查看合同

```
forge bulid
```

3.部署合同

需要zetachain测试网的RPC👉使用zetachain CLI查询Zetachain 测试网的ID是71

```
zetachain q chains show --chain-id 7001 -f rpc
```
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->





**Day3**

终于通过Vscode打开了新建立的hello文件夹

在终端输入Yarn进行安装依赖项：安装出现多项警告

选择清洗并重新安装（失败）

选择忽略错误直接forge soldeer update👉结果显示无"forge"  
选择下载Foundry
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->






**DAY2**

搞明白了如何使用npm下载zetachain并完成运行准备工作:

1.Node.js官网上安装Node.js

功能定位：一个 JavaScript 运行时环境，让你能在电脑上直接运行 JavaScript 代码，而不仅是在浏览器中

对于zetachain的作用：为 ZetaChain 的**开发工具链**（如项目构建、测试、部署等）提供运行环境

2.在cmd运行 npm i -g yarn 下载Yarn

功能定位：一个**替代 npm 的包管理器**，由 Facebook 等公司贡献，在依赖管理和安装速度上有一些优化

对于zetachain的作用：在 ZetaChain 项目中，用于**管理项目具体的依赖库**（通过 `yarn install` 命令）以及**执行项目中定义的脚本**，例如编译合约 (`yarn compile`) 或运行测试 (`yarn test`)。

3.完成这些后正式进入zetachain的安装：

cmd运行 npm install g- zetachain，出现一系列警告选择忽略

4.运行zetachain:

C:\\Users\\MR>zetachain new

出现To get the examples, please run: git clone [https://github.com/zeta-chain/example-contracts](https://github.com/zeta-chain/example-contracts)

直接运行失败，发现是没有下载客户端无法直接通过网页克隆项目

5.下载Git

访问 [git-scm.com](http://git-scm.com)

6.cmd运行git clone [https://github.com/zeta-chain/example-contracts](https://github.com/zeta-chain/example-contracts)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->







**进入学习项目第一天**

尝试接触Node.js下载Zetachain,但是还没找到入口，希望明天能成功

明天待尝试方向：

通过Githup下载

了解Yarn
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->

<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
