---
timezone: UTC-8
---

# David

**GitHub ID:** yedeyu

**Telegram:** @yedeyu

## Self-introduction

Web3 新手

## Notes

<!-- Content_START -->
# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->
## 回顾与反思

```

昨天是[残酷共学：通用 AI](https://intensivecolearn.ing/programs/Universal-AI)的第一天打卡。

### 昨天成功在 Github Codespaces 上运行 ZetaChain 环境和基础工具链

安装和运行所需的环境与工具链，一直都是新手入门的常见痛点。

不久前学习 SUI 技术，就在这一步遇到挑战，本质上是我的电脑太老了（2014 年的苹果电脑），无法运行最新的 SUI 开发环境；当时就尝试在 Github Codespaces 上进行 SUI 开发；这次，我直接选择在 Github Codespaces 上运行 ZetaChain 环境和基础工具链，目前（昨天）是没问题，但也只是目前而已。

在上次 SUI 开发中，一开始也没问题，但在后续的一个环节，由于云环境的设定和限制，与某个工具的预期不一样，导致我花费大量时间尝试解决方案，最终都失败了。

希望这次不要遇到这种情况 ......

### 笔记格式问题

发现残酷共学网站提交笔记/打卡功能，从格式化内容的角度来说，它预期学习者使用系统内置的编辑器来格式化笔记，例如，加粗/列表/不同的标题级别 ...... 并不支持学习者直接提交 md 格式的内容；具体来说，对于用户提交的 md 格式内容，系统会处理特殊符号，导致笔记内容/格式无法正常显示。

我平时就有撰写/保存 md 格式笔记的习惯，对于这个情况，暂时决定，在提交笔记时，把我原本的 md 格式内容都放进编辑器里的`code`代码块里，看看最终显示效果如何。
```

## 今日总结

```


今天按照官网的[hello 教程](https://www.zetachain.com/docs/developers/tutorials/hello)，先跑一遍流程。

这是我第一次接触/学习 EVM/Solidity 技术栈，具体的内容/知识，还得在以后学习。

总的来说，今天遇到两个值得记录的问题，分别是：

- （怀疑）云环境内存不足导致编译失败
  - 试了几个方法，不确定最终是哪个奏效，但大概率是重启云环境
  - 假设编译失败确实是因为内存不足，可以通过临时增加云环境内存来解决
- 获取 Base Sepolia 链测试代币
  - 这是出乎我意料的！！！
  - 我尝试了 Base 链官网的[水龙头列表](https://docs.base.org/base-chain/tools/network-faucets)中的所有 11 个网站，居然没有一个允许只提供收款地址就放款的
    - 至少都有一个额外要求，例如
      - 注册网站账户
      - 通过第三方登录
      - 在主网有小额存款
  - 在这里花费了不少时间
```

## Hello 项目

````
## First Universal Contract


<details>

https://www.zetachain.com/docs/developers/tutorials/hello

```
zetachain new --project hello
cd hello
yarn
forge soldeer update
```


```
Created /workspaces/learn-ZetaChain/projects/hello

# Hello Example

Tutorial: https://www.zetachain.com/docs/developers/tutorials/hello


yarn install v1.22.22
[1/4] Resolving packages...
[2/4] Fetching packages...
[3/4] Linking dependencies...
warning "@solana-developers/helpers > @solana/spl-token-metadata > @solana/codecs > @solana/codecs-strings@2.0.0-rc.1" has unmet peer dependency "fastestsmallesttextencoderdecoder@^1.0.22".
warning "@zetachain/toolkit > @solana/wallet-adapter-react@0.15.39" has unmet peer dependency "react@*".
warning "@zetachain/toolkit > @solana/wallet-adapter-react > @solana/wallet-standard-wallet-adapter-react@1.1.4" has unmet peer dependency "react@*".
warning "@zetachain/toolkit > @solana/wallet-adapter-react > @solana-mobile/wallet-adapter-mobile > @react-native-async-storage/async-storage@1.24.0" has unmet peer dependency "react-native@^0.0.0-0 || >=0.60 <1.0".
warning "@zetachain/toolkit > @solana/wallet-adapter-react > @solana-mobile/wallet-adapter-mobile > @solana-mobile/mobile-wallet-adapter-protocol-web3js > @solana-mobile/mobile-wallet-adapter-protocol@2.2.2" has unmet peer dependency "react-native@>0.69".
[4/4] Building fresh packages...
Done in 413.65s.
┌  🦌 Soldeer Update 🦌
│
◆  Done reading config
│  
◆  Done reading lockfile
│  
◇  Updating dependencies
│  Done retrieving versions
│  Done downloading dependencies
│  Done unzipping dependencies
│  Done installing subdependencies
│  Done checking integrity
│
◆  Updated lockfile
│  
◆  Updated remappings
│  
└  Done updating!
```

### Option 1: Deploy on Localnet


<details>

#### Start localnet

```
zetachain localnet start
```


```
[LOCALNET] Starting anvil on port 8545 with args: -q
[LOCALNET] Skipping Ton...
[LOCALNET] Skipping Solana...
[LOCALNET] Skipping Sui...
[LOCALNET] EVM default wallet address: 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
[LOCALNET] EVM default wallet private key: [aa]
[LOCALNET] Default wallet mnemonic: test test test test test test test test test test test junk

Ethereum (11155112)
┌───────────────┬────────────────────────────────────────────┐
│ Contract      │ Address                                    │
├───────────────┼────────────────────────────────────────────┤
│ erc20Custody  │ 0xa85233C63b9Ee964Add6F2cffe00Fd84eb32338f │
├───────────────┼────────────────────────────────────────────┤
│ gateway       │ 0x09635F643e140090A9A8Dcd712eD6285858ceBef │
├───────────────┼────────────────────────────────────────────┤
│ zetaConnector │ 0x67d269191c92Caf3cD7723F116c85e6E9bf55933 │
├───────────────┼────────────────────────────────────────────┤
│ zetaToken     │ 0x7a2088a1bFc9d81c55368AE168C2C02570cB814F │
├───────────────┼────────────────────────────────────────────┤
│ USDC.ETH      │ 0x1fA02b2d6A771842690194Cf62D91bdd92BfE28d │
└───────────────┴────────────────────────────────────────────┘


ZetaChain (31337)
┌───────────────────┬────────────────────────────────────────────┐
│ Contract          │ Address                                    │
├───────────────────┼────────────────────────────────────────────┤
│ gateway           │ 0xB7f8BC63BbcaD18155201308C8f3540b07f84F5e │
├───────────────────┼────────────────────────────────────────────┤
│ uniswapV2Factory  │ 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512 │
├───────────────────┼────────────────────────────────────────────┤
│ uniswapV2Router02 │ 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0 │
├───────────────────┼────────────────────────────────────────────┤
│ uniswapV3Factory  │ 0xCf7Ed3AccA5a467e9e704C703E8D87F634fB0Fc9 │
├───────────────────┼────────────────────────────────────────────┤
│ uniswapV3Router   │ 0xa513E6E4b8f2a923D98304ec87F64353C4D5C853 │
├───────────────────┼────────────────────────────────────────────┤
│ zetaToken         │ 0x5FbDB2315678afecb367f032d93F642f64180aa3 │
├───────────────────┼────────────────────────────────────────────┤
│ ZRC-20 ETH.ETH    │ 0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe │
├───────────────────┼────────────────────────────────────────────┤
│ ZRC-20 USDC.ETH   │ 0xd97B1de3619ed2c6BEb3860147E30cA8A7dC9891 │
├───────────────────┼────────────────────────────────────────────┤
│ ZRC-20 BNB.BNB    │ 0x65a45c57636f9BcCeD4fe193A602008578BcA90b │
├───────────────────┼────────────────────────────────────────────┤
│ ZRC-20 USDC.BNB   │ 0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0 │
└───────────────────┴────────────────────────────────────────────┘


BNB (98)
┌───────────────┬────────────────────────────────────────────┐
│ Contract      │ Address                                    │
├───────────────┼────────────────────────────────────────────┤
│ erc20Custody  │ 0x70e0bA845a1A0F2DA3359C97E0285013525FFC49 │
├───────────────┼────────────────────────────────────────────┤
│ gateway       │ 0x0E801D84Fa97b50751Dbf25036d067dCf18858bF │
├───────────────┼────────────────────────────────────────────┤
│ zetaConnector │ 0x9d4454B023096f34B160D6B654540c56A1F81688 │
├───────────────┼────────────────────────────────────────────┤
│ zetaToken     │ 0x99bbA657f2BbC93c02D617f8bA121cB8Fc104Acf │
├───────────────┼────────────────────────────────────────────┤
│ USDC.BNB      │ 0xf953b3A269d80e3eB0F2947630Da976B896A8C5b │
└───────────────┴────────────────────────────────────────────┘

> 
```

#### Build the contract

```
forge build
```

```
[⠆] Compiling...
[⠰] Compiling 86 files with Solc 0.8.26
[⠊] Installing Solc version 0.8.26
[⠒] Successfully installed Solc 0.8.26
Error: solc exited with signal: 15 (SIGTERM)
<empty output>
```

##### Solution to `Error: solc exited with signal: 15 (SIGTERM)`

This is likely due to low memory.

Try:

```
FOUNDRY_MAX_WORKERS=2 forge build
```

```
zetachain localnet stop
```

- Stop and restart the codespace
- Increase the memory limit of the codespace


#### Success build

```
[⠘] Compiling...
[⠃] Compiling 86 files with Solc 0.8.26
[⠢] Solc 0.8.26 finished in 34.15s
Compiler run successful with warnings:
Warning (3628): This contract has a payable fallback function, but no receive ether function. Consider adding a receive ether function.
 --> node_modules/@zetachain/toolkit/contracts/testing/mockGateway/WrapGatewayEVM.sol:7:1:
  |
7 | contract WrapGatewayEVM {
  | ^ (Relevant source part starts here and spans across multiple lines).
Note: The payable fallback function is defined here.
  --> node_modules/@zetachain/toolkit/contracts/testing/mockGateway/WrapGatewayEVM.sol:49:5:
   |
49 |     fallback() external payable {
   |     ^ (Relevant source part starts here and spans across multiple lines).

Warning (3628): This contract has a payable fallback function, but no receive ether function. Consider adding a receive ether function.
 --> node_modules/@zetachain/toolkit/contracts/testing/mockGateway/WrapGatewayZEVM.sol:8:1:
  |
8 | contract WrapGatewayZEVM {
  | ^ (Relevant source part starts here and spans across multiple lines).
Note: The payable fallback function is defined here.
  --> node_modules/@zetachain/toolkit/contracts/testing/mockGateway/WrapGatewayZEVM.sol:38:5:
   |
38 |     fallback() external payable {
   |     ^ (Relevant source part starts here and spans across multiple lines).

Warning (5667): Unused function parameter. Remove or comment out the variable name to silence this warning.
  --> contracts/Universal.sol:10:9:
   |
10 |         MessageContext calldata context,
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Warning (5667): Unused function parameter. Remove or comment out the variable name to silence this warning.
  --> contracts/Universal.sol:11:9:
   |
11 |         address zrc20,
   |         ^^^^^^^^^^^^^

Warning (5667): Unused function parameter. Remove or comment out the variable name to silence this warning.
  --> contracts/Universal.sol:12:9:
   |
12 |         uint256 amount,
   |         ^^^^^^^^^^^^^^

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> test/Universal.t.sol:4:8
  |
4 | import "@zetachain/toolkit/contracts/testing/FoundrySetup.t.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> contracts/Universal.sol:4:8
  |
4 | import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> test/Universal.t.sol:5:8
  |
5 | import "../contracts/Universal.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

warning[unchecked-call]: Low-level calls should check the success return value
  --> test/Universal.t.sol:31:9
   |
31 | /         evmSetup.wrapGatewayEVM(5).call(
32 | |             address(universal),
33 | |             encodedMessage,
34 | |             revertOptions
35 | |         );
   | |_________^
   |
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#unchecked-call
```


Fetch a private key with pre-funded tokens on the connected chain:


```
PRIVATE_KEY=$(jq -r '.private_keys[0]' ~/.zetachain/localnet/anvil.json) && echo $PRIVATE_KEY
```

#### Deploy the universal contract:

```
UNIVERSAL=$(forge create Universal \
  --rpc-url http://localhost:8545 \
  --private-key $PRIVATE_KEY \
  --evm-version paris \
  --broadcast \
  --json | jq -r .deployedTo) && echo $UNIVERSAL
```

```
0xffa7CA1AEEEbBc30C874d32C7e22F052BbEa0429
```


#### Make a Call to the Universal App

Fetch the Gateway address for the EVM chain:


```
GATEWAY_EVM=$(jq -r '.["11155112"].contracts[] | select(.contractType == "gateway") | .address' ~/.zetachain/localnet/registry.json) && echo $GATEWAY_EVM
```

```
0x09635F643e140090A9A8Dcd712eD6285858ceBef
```

This information is also shown in the output of `zetachain localnet start`.

Execute the call method on the connected chain’s Gateway to send a message to the universal contract deployed on ZetaChain.

```
zetachain evm call \
  --rpc http://localhost:8545 \
  --gateway $GATEWAY_EVM \
  --receiver $UNIVERSAL \
  --private-key $PRIVATE_KEY \
  --types string \
  --values hello
```

```
From:   0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266
To:     0xffa7CA1AEEEbBc30C874d32C7e22F052BbEa0429 on ZetaChain
Call on revert: false

Contract call details:
Function parameters: hello
Parameter types: ["string"]

? Proceed with the transaction? yes
Transaction hash: 0xb139bcd1bd9e6f3d193d9a6054fa5b7e6534e3e2b7bd0c014d50a6ce514009b3
```

Once the transaction is processed, you’ll see an [ZetaChain]: Event from onCall log in the Localnet terminal.

```
> [Ethereum] Processing Called event from 0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266 to 0xffa7CA1AEEEbBc30C874d32C7e22F052BbEa0429
[ZetaChain] Universal contract 0xffa7CA1AEEEbBc30C874d32C7e22F052BbEa0429 executing onCall (context: {"chainID":"11155112","sender":"0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266","senderEVM":"0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266"}), zrc20: 0x2ca7d64A7EFE2D62A725E2B35Cf7230D6677FfEe, amount: 0, message: 0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000)
[ZetaChain] Event from onCall: {"_type":"log","address":"0xffa7CA1AEEEbBc30C874d32C7e22F052BbEa0429","blockHash":"0xca99aa2e4d308b83fa6461ac41b26229bbe6ea7d52ff62eed45b2a531e500cbc","blockNumber":140,"data":"0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000748656c6c6f3a2000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000","index":0,"removed":false,"topics":["0x39f8c79736fed93bca390bb3d6ff7da07482edb61cd7dafcfba496821d6ab7a3"],"transactionHash":"0x8d0c46347bdb7ecb119e194b19634e962ef4805c9b60d8447988ae819993a32c","transactionIndex":0}
```

</details>

### Option 2: Deploy on Testnet


<details>

#### Generate a new private key

```
PRIVATE_KEY=$(cast wallet new --json | jq -r '.[0].private_key') && echo $PRIVATE_KEY
```

```
cast wallet new --json
```

```json
[
  {
    "address": "0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9",
    "private_key": "[aa]"
  }
]
```

```
PRIVATE_KEY=[aa] && echo $PRIVATE_KEY
```




#### Account setup

```
$ zetachain accounts list
No accounts found.

```

```
zetachain accounts import --type evm --name Alice --private-key $PRIVATE_KEY
```

```
EVM account created successfully!
Key saved to: /home/node/.zetachain/keys/evm/Alice.json
Address: 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
```

#### Get testnet ZETA

- `zetachain faucet --name Alice` or `zetachain faucet --address 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9`
  - 🔐 Please confirm GitHub login at: 'https://github.com/login/device' using this code: 'AAAA-XXXX', and then press Enter to continue.
  - https://zetachain.com/docs/reference/cli#zetachain-faucet
- https://zetachain.faucetme.pro/
  - Need to login with Discord
  - Paste in the address generated above
  - Get 0.1 ZETA
  - Testnet explorer
    - https://testnet.zetascan.com/address/0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
  - Mainnet explorer
    - https://zetascan.com/address/0xa8b0227c0ffe07946e2b1d07f7a1cff59a1c21a9
- https://thirdweb.com/zetachain-testnet


#### Check account balance

```
zetachain query balances --evm 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9 
zetachain query balances --name Alice

```

```
⚠ Error resolving Solana address 
⚠ Error resolving Bitcoin testnet address
⚠ Error resolving Sui address 
⚠ Error resolving TON address 
✔ Successfully fetched balances

EVM: 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9

┌─────────┬────────┬────────────────┬────────┬───────┐
│ (index) │ Amount │ Chain          │ Token  │ Type  │
├─────────┼────────┼────────────────┼────────┼───────┤
│ 0       │ '0.1'  │ 'zeta_testnet' │ 'ZETA' │ 'Gas' │
└─────────┴────────┴────────────────┴────────┴───────┘
```



#### Deploy the Contract on ZetaChain

```
UNIVERSAL=$(forge create Universal \
  --rpc-url https://zetachain-athens-evm.blockpi.network/v1/rpc/public \
  --private-key $PRIVATE_KEY \
  --broadcast \
  --json | jq -r .deployedTo)
```


If no enough gas or account not setup:

```
Error: server returned an error response: error code -32000: failed to check sender balance: sender balance < tx cost (0 < 8167132500000000): insufficient funds: insufficient funds
```

```
$ echo $UNIVERSAL
0xc7Fcf45721f141319240a7955F553C9d54827C79
```

#### Call a Universal Contract from Base

```
zetachain evm call \
  --chain-id 84532 \
  --receiver $UNIVERSAL \
  --private-key $PRIVATE_KEY \
  --types string \
  --values hello
```

```
From:   0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
To:     0xc7Fcf45721f141319240a7955F553C9d54827C79 on ZetaChain
Call on revert: false

Contract call details:
Function parameters: hello
Parameter types: ["string"]

? Proceed with the transaction? yes
Error during call to EVM: insufficient funds for intrinsic transaction cost (transaction="0x02f9021183014a3480830f424083155cc08294e3940c487a766110c85d301d96e33579c5b317fa499580b901a41becceb4000000000000000000000000c7fcf45721f141319240a7955f553c9d54827c79000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000e000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000000000000000000000000000a8b0227c0ffe07946e2b1d07f7a1cff59a1c21a90000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000030d400000000000000000000000000000000000000000000000000000000000000000c080a03016a9b6934cf4801225a88cf2e4f749cdad8337057ddbbf0efd6e3f582cff96a03b7374bea317ec71b49e562c190ee076ddaeac644f83c48597dd4236dec3245c", info={ "error": { "code": -32003, "message": "insufficient funds for gas * price + value: have 0 want 53361000000" } }, code=INSUFFICIENT_FUNDS, version=6.15.0)
```

##### Get gas coin on Base Sepolia

- https://console.optimism.io/faucet



##### Success Call

```
zetachain evm call \
  --chain-id 84532 \
  --receiver $UNIVERSAL \
  --private-key $PRIVATE_KEY \
  --types string \
  --values hello
```

```
From:   0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
To:     0xc7Fcf45721f141319240a7955F553C9d54827C79 on ZetaChain
Call on revert: false

Contract call details:
Function parameters: hello
Parameter types: ["string"]

? Proceed with the transaction? yes
Transaction hash: 0xe8b78ca335c3e5175926f686b334665772555ffcfc9f664ed3f7034e4e3d1102
```

https://sepolia.basescan.org/tx/0xe8b78ca335c3e5175926f686b334665772555ffcfc9f664ed3f7034e4e3d1102

#### Tracking the Cross-Chain Transaction Status

```
zetachain query cctx --hash 0xe8b78ca335c3e5175926f686b334665772555ffcfc9f664ed3f7034e4e3d1102
```

```
84532 → 7001 ✅ OutboundMined
CCTX:     0x1fbf6531b8dcd8fdc1e79ccbf383c673ea53621efd5645b595736c61ccf9a2b2
Tx Hash:  0xe8b78ca335c3e5175926f686b334665772555ffcfc9f664ed3f7034e4e3d1102 (on chain 84532)
Tx Hash:  0xac89d1de84c2a000cb067e74f0b8a72f8352fc1740f26cc8c828b2b54b1b8563 (on chain 7001)
Sender:   0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
Receiver: 0xc7Fcf45721f141319240a7955F553C9d54827C79
Message:  0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000568656c6c6f000000000000000000000000000000000000000000000000000000
```

</details>

</details>
````
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->


官方文档

[https://www.zetachain.com/docs/developers/tutorials/intro](https://www.zetachain.com/docs/developers/tutorials/intro)

\## 手动安装环境

<details>

<summary>Prerequisites</summary>

Before you begin, make sure your system is set up with the following tools:

\* **Node.js** (v21 or later recommended): Required to run the ZetaChain CLI and manage JavaScript dependencies.

\* **Yarn** or **npm**: Package managers used to install and update project libraries. Either works, use whichever you prefer.

\* **Git**: Essential for managing your project source code and collaborating with others.

\* **jq**: A lightweight command-line JSON processor, useful for parsing Localnet output and writing shell scripts.

\* **Foundry**: A fast, modular toolkit for Ethereum development. You’ll use it to compile contracts, manage dependencies, and run Localnet.

\`\`\`

$ node --version

v22.21.1

$ yarn --version

1.22.22

$ jq --version

jq-1.7

$ foundryup --version

zsh: command not found: foundryup

\`\`\`

</details>

<details>

<summary>Install `foundryup`</summary>

\`\`\`

$ curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz) | bash

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current

                                 Dload  Upload   Total   Spent    Left  Speed

100   167  100   167    0     0   1741      0 --:--:-- --:--:-- --:--:--  1757

100  2198  100  2198    0     0  10234      0 --:--:-- --:--:-- --:--:-- 10234

Installing foundryup...

Detected your preferred shell is bash and added foundryup to PATH.

Run 'source /home/codespace/.bashrc' or start a new terminal session to use foundryup.

Then, simply run 'foundryup' to install Foundry.

\`\`\`

I use zsh, so

\`\`\`

echo 'export PATH="$PATH:/home/codespace/.foundry/bin"' >> /home/codespace/.zshrc

\`\`\`

After running this command, reload your shell configuration with `source /home/codespace/.zshrc` or restart your terminal for the PATH change to take effect.

In a new shell:

\`\`\`

$ foundryup

.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx

 ╔═╗ ╔═╗ ╦ ╦ ╔╗╔ ╔╦╗ ╦═╗ ╦ ╦         Portable and modular toolkit

 ╠╣  ║ ║ ║ ║ ║║║  ║║ ╠╦╝ ╚╦╝    for Ethereum Application Development

 ╚   ╚═╝ ╚═╝ ╝╚╝ ═╩╝ ╩╚═  ╩                 written in Rust.

.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx

Repo       : [https://github.com/foundry-rs/foundry](https://github.com/foundry-rs/foundry)

Book       : [https://book.getfoundry.sh/](https://book.getfoundry.sh/)

Chat       : [https://t.me/foundry\_rs/](https://t.me/foundry_rs/)

Support    : [https://t.me/foundry\_support/](https://t.me/foundry_support/)

Contribute : [https://github.com/foundry-rs/foundry/blob/HEAD/CONTRIBUTING.md](https://github.com/foundry-rs/foundry/blob/HEAD/CONTRIBUTING.md)

.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx.xOx

foundryup: checking if foundryup is up to date...

foundryup: foundryup is up to date.

foundryup: installing foundry (version stable, tag stable)

foundryup: checking if forge, cast, anvil, and chisel for stable version are already installed

#################################################################################################### 100.0%

foundryup: found attestation for stable version, downloading attestation artifact, checking...

#################################################################################################### 100.0%

foundryup: binaries not found or do not match expected hashes, downloading new binaries

foundryup: downloading forge, cast, anvil, and chisel for stable version

#################################################################################################### 100.0%

forge

cast

anvil

chisel

foundryup: downloading manpages

#################################################################################################### 100.0%

foundryup: verifying downloaded binaries against the attestation file

foundryup: forge verified ✓

foundryup: cast verified ✓

foundryup: anvil verified ✓

foundryup: chisel verified ✓

foundryup: use - forge 1.4.4-stable (05794498bf 2025-11-03T23:44:21.031788094Z)

foundryup: use - cast 1.4.4-stable (05794498bf 2025-11-03T23:44:21.031788094Z)

foundryup: use - anvil 1.4.4-stable (05794498bf 2025-11-03T23:44:21.031788094Z)

foundryup: use - chisel 1.4.4-stable (05794498bf 2025-11-03T23:44:21.031788094Z)

\`\`\`

\`\`\`

$ foundryup --version

foundryup: 1.4.0

$ forge --version

forge Version: 1.4.4-stable

Commit SHA: 05794498bf47257b144e2e2789a1d5bf8566be0e

Build Timestamp: 2025-11-03T23:44:21.031788094Z (1762213461)

Build Profile: maxperf

\`\`\`

</details>

<details>

<summary>Setting up Environment</summary>

\`\`\`

npm install -g zetachain

\`\`\`

\`\`\`

npm WARN ERESOLVE overriding peer dependency

npm WARN While resolving: @ton/ton@15.4.0

npm WARN Found: @ton/core@0.60.1

npm WARN node\_modules/zetachain/node\_modules/@zetachain/toolkit/node\_modules/@ton/core

npm WARN   @ton/core@"^0.60.1" from @zetachain/toolkit@16.3.0

npm WARN   node\_modules/zetachain/node\_modules/@zetachain/toolkit

npm WARN     @zetachain/toolkit@"16.3.0" from zetachain@7.4.0

npm WARN     node\_modules/zetachain

npm WARN 

npm WARN Could not resolve dependency:

npm WARN peer @ton/core@">=0.62.0 <1.0.0" from @ton/ton@15.4.0

npm WARN node\_modules/zetachain/node\_modules/@zetachain/toolkit/node\_modules/@ton/ton

npm WARN   @ton/ton@"^15.2.1" from @zetachain/toolkit@16.3.0

npm WARN   node\_modules/zetachain/node\_modules/@zetachain/toolkit

npm WARN 

npm WARN Conflicting peer dependency: @ton/core@0.62.0

npm WARN node\_modules/@ton/core

npm WARN   peer @ton/core@">=0.62.0 <1.0.0" from @ton/ton@15.4.0

npm WARN   node\_modules/zetachain/node\_modules/@zetachain/toolkit/node\_modules/@ton/ton

npm WARN     @ton/ton@"^15.2.1" from @zetachain/toolkit@16.3.0

npm WARN     node\_modules/zetachain/node\_modules/@zetachain/toolkit

npm WARN deprecated rimraf@3.0.2: Rimraf versions prior to v4 are no longer supported

npm WARN deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

npm WARN deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

npm WARN deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

npm WARN deprecated inflight@1.0.6: This module is not supported, and leaks memory. Do not use it. Check out lru-cache if you want a good and tested way to coalesce async requests by a key value, which is much more comprehensive and powerful.

npm WARN deprecated glob@8.1.0: Glob versions prior to v9 are no longer supported

npm WARN deprecated glob@7.2.3: Glob versions prior to v9 are no longer supported

npm WARN deprecated sudo-prompt@9.2.1: Package no longer supported. Contact Support at [https://www.npmjs.com/support](https://www.npmjs.com/support) for more info.

npm WARN deprecated @uniswap/v3-staker@1.0.0: Please upgrade to 1.0.1

npm WARN deprecated @uniswap/swap-router-contracts@1.3.1: Package no longer supported. Contact Support at [https://www.npmjs.com/support](https://www.npmjs.com/support) for more info.

added 1289 packages in 2m

\`\`\`

\`\`\`

zetachain query chains list

\`\`\`

\`\`\`

$ zetachain query chains list

✔ Successfully fetched 13 supported chains, 32 tokens, and 13 chain params

┌──────────┬────────────────────┬───────┬──────────────────────────────────┐

│ Chain ID │ Chain Name         │ Count │ Tokens                           │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 97       │ bsc\_testnet        │ 20    │ USDC.BSC, BNB.BSC                │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 7001     │ zeta\_testnet       │ 3     │ -                                │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 11155111 │ sepolia\_testnet    │ 14    │ ETH.ETHSEP, USDTT.SEPOLIA,       │

│          │                    │       │ USDC.ETHSEP, USDCT.SEPOLIA       │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 80002    │ amoy\_testnet       │ 32    │ POL.AMOY, USDC.AMOY              │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 84532    │ base\_sepolia       │ 32    │ ETH.BASESEP, USDCT.BASESEP,      │

│          │                    │       │ USDTT.BASESEP, USDC.BASESEP      │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 901      │ solana\_devnet      │ 32    │ SOL.SOL, USDC.SOL                │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 18333    │ btc\_signet\_testnet │ 2     │ sBTC.BTC                         │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 18334    │ btc\_testnet4       │ 10    │ tBTC.BTC                         │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 421614   │ arbitrum\_sepolia   │ 5     │ USDTT.ARBSEP, UPKRW.ARBSEP,      │

│          │                    │       │ ETH.ARBSEP, USDC.ARBSEP,         │

│          │                    │       │ USDCT.ARBSEP                     │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 43113    │ avalanche\_testnet  │ 20    │ USDC.FUJI, USDCT.FUJI,           │

│          │                    │       │ HanaKRW.FUJI, AVAX.FUJI,         │

│          │                    │       │ USDTT.FUJI                       │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 2015141  │ ton\_testnet        │ 1     │ TON.TON                          │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 103      │ sui\_testnet        │ 1     │ SUI.SUI, USDC.SUI                │

├──────────┼────────────────────┼───────┼──────────────────────────────────┤

│ 1001     │ kaia\_testnet       │ 1     │ KBKRW.KAIROS, TSKRW.KAIROS,      │

│          │                    │       │ KAIA.KAIROS                      │

└──────────┴────────────────────┴───────┴──────────────────────────────────┘

Note: Count refers to the number of confirmations required for a transaction

from that connected chain to be observed

\`\`\`

</details>

\## 使用 `Dev Container`

示例仓库：

[https://github.com/yedeyu/ZetaChain-Codespace/](https://github.com/yedeyu/ZetaChain-Codespace/)

<details>

<summary>查看相关文件</summary>

`./devcontainer/devcontainer.json`

\`\`\`json

{

"name": "ZetaChain Environment",

// 使用官方 Node.js 22 镜像，满足 prerequisites

"image": "mcr.microsoft.com/devcontainers/javascript-node:1-22-bookworm",

// 安装系统级依赖 (jq)

"features": {

"ghcr.io/devcontainers-contrib/features/apt-packages:1": {

"packages": "jq"

}

},

// 创建容器后运行的脚本 (安装 Foundry 和 ZetaChain CLI)

"postCreateCommand": "bash .devcontainer/[setup.sh](http://setup.sh)",

// 这里的设置可选，推荐安装 Solidity 插件

"customizations": {

"vscode": {

"extensions": \[

"JuanBlanco.solidity",

"esbenp.prettier-vscode"

\]

}

},

// 确保使用普通用户 'node' 运行，而不是 root

"remoteUser": "node"

}

\`\`\`

`./devcontainer/setup.sh`

\`\`\`sh

#!/bin/bash

set -e

echo "--- Starting Environment Setup ---"

\# 1. 安装 Foundry (包含 forge, cast, anvil, chisel)

if ! command -v forge &> /dev/null; then

echo "Installing Foundry..."

curl -L [https://foundry.paradigm.xyz](https://foundry.paradigm.xyz) | bash

\# 将 Foundry 添加到当前 PATH (用于脚本后续执行)

export PATH="$PATH:$HOME/.foundry/bin"

\# 运行 foundryup 下载二进制文件

foundryup

\# 将 PATH 永久添加到 bashrc 和 zshrc (确保下次进入终端时可用)

\# 注意：在 javascript-node 镜像中，用户目录通常是 /home/node

echo 'export PATH="$PATH:$HOME/.foundry/bin"' >> $HOME/.bashrc

echo 'export PATH="$PATH:$HOME/.foundry/bin"' >> $HOME/.zshrc

echo "Foundry installed successfully."

else

echo "Foundry is already installed."

fi

\# 2. 安装 ZetaChain CLI

echo "Installing ZetaChain CLI..."

\# 使用 sudo 以避免全局安装时的权限问题 (Codespace 中 sudo 是免密的)

sudo npm install -g zetachain

echo "--- Setup Complete! ---"

echo "Run 'source ~/.zshrc' if commands are not found immediately."

\`\`\`
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
