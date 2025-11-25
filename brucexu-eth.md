---
timezone: UTC+12
---

# Bruce Xu

**GitHub ID:** brucexu-eth

**Telegram:** @brucexu_eth

## Self-introduction

Learning AI and Web3.

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
[https://www.zetachain.com/docs/developers/tutorials/hello/](https://www.zetachain.com/docs/developers/tutorials/hello/)

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/brucexu-eth/images/2025-11-25-1764033488074-image.png)

初始化 example 之后，这个 onCall 方法就是用于做跨链使用的。通过发送消息，然后通知其他的链接上的链进行操作。

```
forge create Universal --rpc-url https://zetachain-athens-evm.blockpi.network/v1/rpc/public --private-key $PKK --broadcast
[⠊] Compiling...
No files changed, compilation skipped
Deployer: 0xD41dC0b5c27c8554D5fb0495279151d4E5500e3B
Deployed to: 0x3392828C9D464D2222F5b6F720B6Ca2772685940
Transaction hash: 0xfbafa81ee69fd294714a73e6b0bf65b1c1a0a225132a9812c2dd6a2c07ffd997
```

My First ZetaChain TX.

测试币水龙头 [https://www.zetachain.com/docs/reference/faucet/](https://www.zetachain.com/docs/reference/faucet/)

84532 │ base\_sepolia

```
zetachain evm call --receiver 0x3392828C9D464D2222F5b6F720B6Ca2772685940 --chain-id 84532 --types string --values alice --private-key $PKK

From:   0xD41dC0b5c27c8554D5fb0495279151d4E5500e3B
To:     0x3392828C9D464D2222F5b6F720B6Ca2772685940 on ZetaChain
Call on revert: false

Contract call details:
Function parameters: alice
Parameter types: ["string"]

? Proceed with the transaction? yes
Transaction hash: 0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a
```

具体的交易 params on base，sent to zetachain gateway on base, includes params, payload, etc.

```
Function: call(address, bytes, (address,bool,address,bytes,uint256))
#	Name	Type	Data
1	receiver	address
0x3392828C9D464D2222F5b6F720B6Ca2772685940
2	payload	bytes
0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000005616c696365000000000000000000000000000000000000000000000000000000
2	revertOptions.revertAddress	address
0xD41dC0b5c27c8554D5fb0495279151d4E5500e3B
2	revertOptions.callOnRevert	bool
false
2	revertOptions.abortAddress	address
0x0000000000000000000000000000000000000000
2	revertOptions.revertMessage	bytes
0x
2	revertOptions.onRevertGasLimit	uint256
200000
```

[https://testnet.zetascan.com/tx/0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03](https://testnet.zetascan.com/tx/0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03)

TODO bytes 能存多少数据？如果是一个很复杂的跨链合约调用，如何传递参数呢？

```
zetachain q cctx --hash 0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a
84532 → 7001 ✅ OutboundMined
CCTX:     0xb1438fb6c86ddbc325fbd99266e63431906e5f55c60ad4b97cd4a7865d1b6402
Tx Hash:  0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a (on chain 84532)
Tx Hash:  0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03 (on chain 7001)
Sender:   0xD41dC0b5c27c8554D5fb0495279151d4E5500e3B
Receiver: 0x3392828C9D464D2222F5b6F720B6Ca2772685940
Message:  00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000005616c696365000000000000000000000000000000000000000000000000000000
```

[https://sepolia.basescan.org/tx/0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a](https://sepolia.basescan.org/tx/0xe8427df51c87cfba654dbc05937e5b445a3c5ea528f9285ffbb5041651683a6a)

zetachain q cctx --help

Usage: zetachain query cctx \[options\]

Queries the real-time status of a cross-chain transaction by its inbound transaction hash. You can control

polling frequency, timeout, and target RPC endpoint.

大概工作流程：

1.  在 base 发送了一个 call 包括调用的跨链合约、method、参数等到 zetachain 的 base gateway
    
2.  链下的代码监听事件，然后操作 zetachain 的地址 **0x735b14BB79463307AAcBED86DAf3322B1e6226aB** 来在 zetachain 上面重放请求，包括地址和 message
    
3.  之后就调用了合约的对应方法，并且在 zetachain 的合约打印了 event 消息
    

这样实现了跨链功能。

TODO ZetaChain 的 0x6c533f7fE93fAE114d0954697069Df33C9B74fD7 GatewayZEVM 负责重放请求，代码实现是怎么样的？了解一下 [https://testnet.zetascan.com/tx/0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03](https://testnet.zetascan.com/tx/0x24450579034209dddbd436c15ad54c5634d0a1b30a5d0cb89c985d07f3d06a03) [https://testnet.zetascan.com/address/0xe91D93D4eFd6347bD9fA5fdb5B24394CFB22a6Df?tab=contract](https://testnet.zetascan.com/address/0xe91D93D4eFd6347bD9fA5fdb5B24394CFB22a6Df?tab=contract)

TODO 看起来 ZetaChain 支付了跨链消息的 gas，在经济性上如何形成闭环？越多跨链请求，似乎是越多 gas 消耗，而且是各个链的。

TODO UniversalContract 的合约代码包括什么？具体实现什么功能？import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";

A universal contract must implement the `onCall` function. This function is triggered when the contract receives a call from a connected chain via the Gateway. The function processes incoming data, which includes:

-   `context`: A `MessageContext` struct containing:
    
    -   `chainID`: The chain ID of the connected chain that initiated the cross-chain call.
        
    -   `sender`: The address (EOA or contract) that called the Gateway on the connected chain.
        
    -   `origin`: Deprecated.
        
-   `zrc20`: The address of the ZRC-20 token representing assets from the source chain.
    
-   `amount`: The amount of tokens transferred.
    
-   `message`: The encoded payload data.
    

In this example, `onCall` decodes the message into a string and emits an event.

`onCall` should only be called by the Gateway to ensure that it is only called as a response to a call on a connected chain and that you can trust the values of the function parameters. This is enforced by the `onlyGateway` modifier, which is inherited from `UniversalContract`.

* * *

[https://getfoundry.sh/guides/project-setup/soldeer/](https://getfoundry.sh/guides/project-setup/soldeer/)

Foundry has been using git submodules to handle dependencies up until now.

A new approach has been in the making, [soldeer.xyz](http://soldeer.xyz), which is a Solidity native dependency manager built in Rust and open sourced (check the repository [https://github.com/mario-eth/soldeer](https://github.com/mario-eth/soldeer)).

```
forge soldeer install @openzeppelin-contracts~5.0.2
```

This will download the dependency from the central repository and install it into a `dependencies` directory.

Soldeer can manage two types of dependency configuration: using `soldeer.toml` or embedded in the `foundry.toml`. In order to work with Foundry, you have to define the `[dependencies]` config in the `foundry.toml`. This will tell the `soldeer CLI` to define the installed dependencies there. E.g.

```
# Full reference https://github.com/foundry-rs/foundry/tree/master/crates/config [profile.default]auto_detect_solc = falsebytecode_hash = "none"fuzz = { runs = 1_000 }libs = ["dependencies"] # <= This is important to be addedgas_reports = ["*"] [dependencies] # <= Dependencies will be added under this config"@openzeppelin-contracts" = { version = "5.0.2" }"@uniswap-universal-router" = { version = "1.6.0" }"@prb-math" = { version = "4.0.2" }forge-std = { version = "1.8.1" }
```

* * *

Solidity Bytes

`bytes` 本身是「可变长度字节数组」，理论上没有 Solidity 级别的固定上限，长度用 `uint256` 存，所以上限是 `2^256-1` 个字节，但在现实里远远达不到，真正限制是 gas 和区块 gas limit。

### 固定长度：`bytes1` ~ `bytes32`

-   编译期长度固定，不能变。
    
-   占用的 storage 空间 = 对应字节数（放在 32 字节 slot 里）。
    

关于 bit bytes 之类的知识 [https://x.com/brucexu\_eth/status/1902631404862881952](https://x.com/brucexu_eth/status/1902631404862881952)

### 单个 `bytes` 状态变量在 storage 中的布局（简单理解版）

只需要记住：

-   如果长度 ≤ 31 字节：
    
    -   整个 `bytes`（数据 + 长度）会塞在同一个 32 字节的 storage slot 中。
        
-   如果长度 > 31 字节：
    
    -   slot 里存的是「长度 + 数据起始位置标记」，真正的数据拆开放在后续若干 slot（从 `keccak256(slot)` 开始）。
        

这意味着：

-   小 `bytes`（例如 20 字节地址、32 字节哈希等）存储很省空间。
    
-   长 `bytes` 会分布在很多 slot，上下链和修改都会更贵。
    

* * *

How to decode AIB-encoded string like:

```
00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000005616c696365000000000000000000000000000000000000000000000000000000
```

### 1\. Break the string into 32-byte words

The decoded message hidden in that EVM bytecode is the string: **"alice"**.

This is a standard **ABI-encoded string**. The Ethereum Virtual Machine (EVM) processes data in 32-byte "words" (which represent 64 hexadecimal characters).

Here is the step-by-step breakdown of how to decode it.

* * *

### 1\. Break the string into 32-byte words

To make sense of the data, we split the long string into chunks of 64 characters (32 bytes) each.

**Word 1 (The Offset):** `0000000000000000000000000000000000000000000000000000000000000020`

-   **Value:** Hex `0x20` = Decimal `32`.
    
-   **Meaning:** This tells the EVM that the actual data for this dynamic type (string) starts **32 bytes** after the beginning of this slot. Since one word is 32 bytes, it points to the very next line.
    

**Word 2 (The Length):** `0000000000000000000000000000000000000000000000000000000000000005`

-   **Value:** Hex `0x05` = Decimal `5`.
    
-   **Meaning:** This indicates the length of the string is **5 characters**.
    

**Word 3 (The Content):** `616c69636500000000000000000000000000000000000000000000000000000`

-   **Value:** `616c696365...`
    
-   **Meaning:** This is the actual text data, encoded in Hexadecimal (ASCII/UTF-8). Strings are left-aligned in EVM storage, so the data is at the start, and the rest is "padding" (zeros).
    

这里的数字都是 hex 16 进制，所以 64 位存储空间是 32 bytes，1 bytes = 8 bits = 2 hex 存储空间。
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->

今天开始学习了，先打个卡测试一下功能。
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
