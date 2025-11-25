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
```

```
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
```

```
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
```

```
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
```

```
git clone https://github.com/zeta-chain/node
```

```
cd node
```

```
make install
```

```
# 关键：复制到公共目录 (解决 Python 找不到命令的问题)
```

```
sudo cp ~/go/bin/zetacored /usr/local/bin/
```

```
sudo chmod +x /usr/local/bin/zetacored
```

```
创建钱包 & 获取地址
```

```
说明： 生成钱包并在本地保存私钥，同时获取 EVM (0x) 格式地址。
```

```
Bash
```

```
# 创建新账户
```

```
zetacored keys add myaccount
```

```
# 查看 0x 格式地址
```

```
zetacored debug addr <自己的zeta1地址>
```

```
mkdir -p ~/qwen-project && cd ~/qwen-project
```

```
python3 -m venv venv
```

```
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
```

```
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
