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
# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->
今天提了 3 个 pr。

-   ccxt 末尾 多余的‘tx’字符
    
    -   [https://github.com/zeta-chain/docs/pull/767](https://github.com/zeta-chain/docs/pull/767)
        
-   "Failed to retrieve private key" 错误
    
    -   [https://github.com/zeta-chain/docs/pull/768](https://github.com/zeta-chain/docs/pull/768)
        
-   提醒读者 新账户 要有 gas 代币
    
    -   [https://github.com/zeta-chain/docs/pull/769](https://github.com/zeta-chain/docs/pull/769)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->

提了一个 pr 去修复 Swap 教程中的 \`sETH.SEPOLIA\` 错误。  
  
[https://github.com/zeta-chain/docs/pull/766](https://github.com/zeta-chain/docs/pull/766)
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->


## 今日总结

### 成功部署运行 The Swap App

运行 \[The Swap App\]([https://www.zetachain.com/docs/developers/tutorials/swap)。](https://www.zetachain.com/docs/developers/tutorials/swap\)。)

### 解决 `sETH.SEPOLIA` 错误

按照教程步骤，查询代表 Ethereum Sepolia 上 ETH 的 ZRC-20 地址时出错，应该把 `sETH.SEPOLIA` 改成 `ETH.ETHSEP`。

### Swap 合约 Revert，有待进一步探索。

成功部署 Swap 合约到测试网后，按照教程，将 Base 测试网的 0.001 ETH 兑换到 ETH 测试网。

整个流程正常运行，但是走的是 Revert 路径，即资金退回到发送者。

发送 0.001 ETH，3 分钟后 收到 0.00099996 ETH。

[https://sepolia.basescan.org/tx/0x14e8449b6c1479e313dba76e123076601659c7419004640a62efc67cf46005b3](https://sepolia.basescan.org/tx/0x14e8449b6c1479e313dba76e123076601659c7419004640a62efc67cf46005b3)

[https://sepolia.basescan.org/address/0xa8b0227c0ffe07946e2b1d07f7a1cff59a1c21a9](https://sepolia.basescan.org/address/0xa8b0227c0ffe07946e2b1d07f7a1cff59a1c21a9)

合约 Log 并没有显示具体什么原因导致回退，后来尝试修改代码，细分出错误原因，但因余额不足，无法部署新合约。

## The Swap App

[https://www.zetachain.com/docs/developers/tutorials/swap](https://www.zetachain.com/docs/developers/tutorials/swap)

[https://gemini.google.com/share/aeb8ff8f86d1](https://gemini.google.com/share/aeb8ff8f86d1)  
  
<details>

````



### Setting Up Your Environment

```
zetachain new --project swap
```

```
Created /workspaces/learn-ZetaChain/projects/swap

# Universal Swap

Tutorial: https://www.zetachain.com/docs/developers/tutorials/swap
```

```
cd swap
yarn
```


```
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
Done in 345.72s.
```

```
forge soldeer update
```

```
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


```
forge build
```


```
[⠒] Compiling...
[⠊] Compiling 95 files with Solc 0.8.26
[⠆] Solc 0.8.26 finished in 33.81s
Compiler run successful with warnings:
Warning (2519): This declaration shadows an existing declaration.
   --> contracts/Swap.sol:157:9:
    |
157 |         bool withdraw
    |         ^^^^^^^^^^^^^
Note: The shadowed declaration is here:
   --> contracts/Swap.sol:199:5:
    |
199 |     function withdraw(
    |     ^ (Relevant source part starts here and spans across multiple lines).

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

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> contracts/Swap.sol:7:8
  |
7 | import "@uniswap/v2-periphery/contracts/interfaces/IUniswapV2Router02.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> test/SwapTest.t.sol:4:8
  |
4 | import "@zetachain/toolkit/contracts/testing/FoundrySetup.t.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> test/SwapTest.t.sol:5:8
  |
5 | import "@zetachain/toolkit/contracts/testing/mock/ERC20Mock.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> test/SwapTest.t.sol:6:8
  |
6 | import "@zetachain/toolkit/contracts/testing/mock/ZRC20Mock.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
  --> contracts/Swap.sol:12:8
   |
12 | import "@zetachain/protocol-contracts/contracts/zevm/interfaces/UniversalContract.sol";
   |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[mixed-case-variable]: mutable variables should use mixedCase
  --> contracts/Swap.sol:73:31
   |
73 |         (uint256 out, address gasZRC20, uint256 gasFee) = handleGasAndSwap(
   |                               ^^^^^^^^
   |
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#mixed-case-variable

note[mixed-case-variable]: mutable variables should use mixedCase
   --> contracts/Swap.sol:122:31
    |
122 |         (uint256 out, address gasZRC20, uint256 gasFee) = handleGasAndSwap(
    |                               ^^^^^^^^
    |
    = help: https://book.getfoundry.sh/reference/forge/forge-lint#mixed-case-variable

note[mixed-case-variable]: mutable variables should use mixedCase
   --> contracts/Swap.sol:160:17
    |
160 |         address gasZRC20;
    |                 ^^^^^^^^
    |
    = help: https://book.getfoundry.sh/reference/forge/forge-lint#mixed-case-variable

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> test/SwapTest.t.sol:7:8
  |
7 | import "../contracts/Swap.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[mixed-case-variable]: mutable variables should use mixedCase
  --> test/SwapTest.t.sol:17:33
   |
17 |     TokenSetup.TokenInfo public eth_testToken1;
   |                                 ^^^^^^^^^^^^^^
   |
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#mixed-case-variable

note[mixed-case-variable]: mutable variables should use mixedCase
  --> test/SwapTest.t.sol:18:33
   |
18 |     TokenSetup.TokenInfo public bnb_testToken2;
   |                                 ^^^^^^^^^^^^^^
   |
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#mixed-case-variable

note[mixed-case-variable]: mutable variables should use mixedCase
   --> contracts/Swap.sol:203:17
    |
203 |         address gasZRC20,
    |                 ^^^^^^^^
    |
    = help: https://book.getfoundry.sh/reference/forge/forge-lint#mixed-case-variable

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> test/SwapCompanion.sol:4:8
  |
4 | import "@zetachain/protocol-contracts/contracts/evm/GatewayEVM.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[mixed-case-variable]: mutable variables should use mixedCase
   --> contracts/Swap.sol:282:18
    |
282 |         (address gasZRC20, uint256 gasFee) = IZRC20(targetToken)
    |                  ^^^^^^^^
    |
    = help: https://book.getfoundry.sh/reference/forge/forge-lint#mixed-case-variable

note[unaliased-plain-import]: use named imports '{A, B}' or alias 'import ".." as X'
 --> test/SwapCompanion.sol:5:8
  |
5 | import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unaliased-plain-import

note[screaming-snake-case-immutable]: immutables should use SCREAMING_SNAKE_CASE
  --> test/SwapCompanion.sol:12:33
   |
12 |     GatewayEVM public immutable gateway;
   |                                 ^^^^^^^
   |
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#screaming-snake-case-immutable

note[unused-import]: unused imports should be removed
 --> contracts/Swap.sol:6:9
  |
6 | import {BytesHelperLib} from "@zetachain/toolkit/contracts/BytesHelperLib.sol";
  |         ^^^^^^^^^^^^^^
  |
  = help: https://book.getfoundry.sh/reference/forge/forge-lint#unused-import

warning[unsafe-typecast]: typecasts that can truncate values should be checked
  --> contracts/Swap.sol:51:41
   |
51 |         uniswapRouter = address(uint160(bytes20(uniswapRouterBytes)));
   |                                         ^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: Consider disabling this lint if you're certain the cast is safe:
           
           // casting to 'bytes20' is safe because [explain why]
           // forge-lint: disable-next-line(unsafe-typecast)
           
           
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#unsafe-typecast

warning[unsafe-typecast]: typecasts that can truncate values should be checked
   --> contracts/Swap.sol:266:40
    |
266 |                 revertAddress: address(bytes20(sender)),
    |                                        ^^^^^^^^^^^^^^^
    |
    = note: Consider disabling this lint if you're certain the cast is safe:
            
            // casting to 'bytes20' is safe because [explain why]
            // forge-lint: disable-next-line(unsafe-typecast)
            
            
    = help: https://book.getfoundry.sh/reference/forge/forge-lint#unsafe-typecast

note[named-struct-fields]: prefer initializing structs with named fields
  --> test/SwapCompanion.sol:27:13
   |
27 |             RevertOptions(msg.sender, false, address(0), "", 0)
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#named-struct-fields

note[named-struct-fields]: prefer initializing structs with named fields
  --> test/SwapCompanion.sol:48:13
   |
48 |             RevertOptions(msg.sender, false, address(0), "", 0)
   |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = help: https://book.getfoundry.sh/reference/forge/forge-lint#named-struct-fields
```

### Understanding the Swap Contract

[swap/contracts/Swap.sol](../projects/swap/contracts/Swap.sol)


调用 `IZRC20(targetToken).withdrawGasFee()` 来获取提现目标链所需的 gas：

```solidity
(address gasZRC20, uint256 gasFee) = IZRC20(targetToken).withdrawGasFee();
```

调用 `IZRC20(coin).approve(address, amount);` 来获取用户/钱包授权：


```solidity
IZRC20(gasZRC20).approve(address(gateway), gasFee);
IZRC20(params.target).approve(address(gateway), out);
 
gateway.withdraw(
  abi.encodePacked(params.to), // chain-agnostic recipient (bytes)
  out,                         // amount of target token
  params.target,               // ZRC-20 to withdraw
  revertOptions                // failure handling
);
```


### Option 1: Deploy on Testnet

```
UNIVERSAL=$(npx tsx commands deploy --private-key $PRIVATE_KEY | jq -r .contractAddress) && echo $UNIVERSAL
```

```
0x71CEfA29FD68030657CB1207C86545625C557Ba9
```
Contract address:

https://testnet.zetascan.com/address/0x71CEfA29FD68030657CB1207C86545625C557Ba9?tab=txs


根据私钥计算出对应的钱包地址，将其赋值给名为 RECIPIENT 的变量

```
RECIPIENT=$(cast wallet address $PRIVATE_KEY) && echo $RECIPIENT
```

```
0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
```


查询代表 Ethereum Sepolia 上 ETH 的 ZRC-20 地址：

调用 ZetaChain 客户端工具查询注册表中符号为 sETH.SEPOLIA 的代币信息，精准提取其对应的 ZRC-20 智能合约地址，将其存入名为 ZRC20_ETHEREUM_ETH 的环境变量中，并立即打印出来以供确认。

```
ZRC20_ETHEREUM_ETH=$(zetachain q tokens show --symbol sETH.SEPOLIA -f zrc20) && echo $ZRC20_ETHEREUM_ETH
```

```
Token with symbol 'sETH.SEPOLIA' not found
```


#### 手动查找 ETH 测试网 上 ETH 的 ZRC-20 地址

```
zetachain q tokens list
```

```
✔ Successfully fetched 32 ZRC-20 tokens
┌──────────┬───────────────┬────────────────────────────────────────────┐
│ Chain ID │ Symbol        │ ZRC-20                                     │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 97       │ USDC.BSC      │ 0x7c8dDa80bbBE1254a7aACf3219EBe1481c6E01d7 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 97       │ BNB.BSC       │ 0xd97B1de3619ed2c6BEb3860147E30cA8A7dC9891 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 103      │ SUI.SUI       │ 0x3e128c169564DD527C8e9bd85124BF6A890E5a5f │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 103      │ USDC.SUI      │ 0xE80e3e8Ac1C19c744d4c2147172489BEAF23E3C5 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 901      │ SOL.SOL       │ 0xADF73ebA3Ebaa7254E859549A44c74eF7cff7501 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 901      │ USDC.SOL      │ 0xD10932EB3616a937bd4a2652c87E9FeBbAce53e5 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 1001     │ KBKRW.KAIROS  │ 0x2Db395976CDb9eeFCc8920F4F2f0736f1D575794 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 1001     │ TSKRW.KAIROS  │ 0xEb646191FcCb5Bfc1e7A121D3847590aAc840a53 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 1001     │ KAIA.KAIROS   │ 0xe1A4f44b12eb72DC6da556Be9Ed1185141d7C23c │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 18333    │ sBTC.BTC      │ 0xdbfF6471a79E5374d771922F2194eccc42210B9F │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 18334    │ tBTC.BTC      │ 0xfC9201f4116aE6b054722E10b98D904829b469c3 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 43113    │ USDC.FUJI     │ 0x8344d6f84d26f998fa070BbEA6D2E15E359e2641 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 43113    │ USDCT.FUJI    │ 0x93dEB52A99EFe14c1383f3bd0F58bb29Ad6dA0FC │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 43113    │ HanaKRW.FUJI  │ 0xE8d7796535F1cd63F0fe8D631E68eACe6839869B │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 43113    │ AVAX.FUJI     │ 0xEe9CC614D03e7Dbe994b514079f4914a605B4719 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 43113    │ USDTT.FUJI    │ 0xc96dBbC62235f8B7f498DB95eBcbe7EE2128C17f │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 80002    │ POL.AMOY      │ 0x777915D031d1e8144c90D025C594b3b8Bf07a08d │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 80002    │ USDC.AMOY     │ 0xe573a6e11f8506620F123DBF930222163D46BCB6 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 84532    │ ETH.BASESEP   │ 0x236b0DE675cC8F46AE186897fCCeFe3370C9eDeD │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 84532    │ USDCT.BASESEP │ 0x4888591FC8529b6a9B3B67b7aE93D3Ef4226BcE4 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 84532    │ USDTT.BASESEP │ 0x960eC27edE698F8F1977C6A32a75ac937a9c8381 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 84532    │ USDC.BASESEP  │ 0xd0eFed75622e7AA4555EE44F296dA3744E3ceE19 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 421614   │ USDTT.ARBSEP  │ 0x0BB6086F94585c3CC8d6c587627f09877B452FD3 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 421614   │ UPKRW.ARBSEP  │ 0x0ca762FA958194795320635c11fF0C45C6412958 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 421614   │ ETH.ARBSEP    │ 0x1de70f3e971B62A0707dA18100392af14f7fB677 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 421614   │ USDC.ARBSEP   │ 0x4bC32034caCcc9B7e02536945eDbC286bACbA073 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 421614   │ USDCT.ARBSEP  │ 0x8eb120dFDeD678E681559Ae1586Fd0F55077a1A1 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 2015141  │ TON.TON       │ 0x54Bf2B1E91FCb56853097BD2545750d218E245e1 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 11155111 │ ETH.ETHSEP    │ 0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0 │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 11155111 │ USDTT.SEPOLIA │ 0xD45F47412073b75B7c70728aD9A45Dee0ee01bac │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 11155111 │ USDC.ETHSEP   │ 0xcC683A782f4B30c138787CB5576a86AF66fdc31d │
├──────────┼───────────────┼────────────────────────────────────────────┤
│ 11155111 │ USDCT.SEPOLIA │ 0xe134d947644F90486C8106Ee528b1CD3e54A385e │
└──────────┴───────────────┴────────────────────────────────────────────┘
```

```
zetachain q tokens list | grep "ETH"

✔ Successfully fetched 32 ZRC-20 tokens
│ 84532    │ ETH.BASESEP   │ 0x236b0DE675cC8F46AE186897fCCeFe3370C9eDeD │
│ 421614   │ ETH.ARBSEP    │ 0x1de70f3e971B62A0707dA18100392af14f7fB677 │
│ 11155111 │ ETH.ETHSEP    │ 0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0 │
│ 11155111 │ USDC.ETHSEP   │ 0xcC683A782f4B30c138787CB5576a86AF66fdc31d │
```

```
# 注意：把 sETH.SEPOLIA 改成了 ETH.ETHSEP (在列表中看到的那个名字)
ZRC20_ETHEREUM_ETH=$(zetachain q tokens show --symbol ETH.ETHSEP -f zrc20) && echo $ZRC20_ETHEREUM_ETH
```

Ethereum Sepolia (ETH 测试网) 上 ETH 的 ZRC-20 地址：

```
0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0
```

### Swap from Base to Ethereum

https://www.zetachain.com/docs/developers/tutorials/swap#swap-from-base-to-ethereum


```
npx zetachain evm deposit-and-call \
  --chain-id 84532 \
  --amount 0.001 \
  --types address bytes bool \
  --receiver $UNIVERSAL \
  --values $ZRC20_ETHEREUM_ETH $RECIPIENT true
```

```
Failed to retrieve private key: Private key not found
Error during depositAndCall to EVM: Failed to retrieve private key: Private key not found
```

- 指定私钥

```
npx zetachain evm deposit-and-call \
  --chain-id 84532 \
  --amount 0.001 \
  --types address bytes bool \
  --receiver $UNIVERSAL \
  --values $ZRC20_ETHEREUM_ETH $RECIPIENT true \
  --private-key $PRIVATE_KEY
```

```
From:   0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
To:     0x71CEfA29FD68030657CB1207C86545625C557Ba9 on ZetaChain
Amount: 0.001 native tokens
Refund: 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
Call on revert: false

Contract call details:
Function parameters: 0x05BA149A7bd6dC1F937fA9046A9e05C05f3b18b0, 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9, true
Parameter types: ["address","bytes","bool"]

? Proceed with the transaction? yes
Transaction hash: 0x14e8449b6c1479e313dba76e123076601659c7419004640a62efc67cf46005b3
```

https://sepolia.basescan.org/tx/0x14e8449b6c1479e313dba76e123076601659c7419004640a62efc67cf46005b3

```
zetachain query cctx --hash 0x14e8449b6c1479e313dba76e123076601659c7419004640a62efc67cf46005b3
```


```
From:   0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
To:     0x71CEfA29FD68030657CB1207C86545625C557Ba9 on ZetaChain
Amount: 0.001 native tokens
Refund: 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
84532 → 7001 ❌ Reverted
CCTX:     0x7d8740e2885af36e8e92fe5b81da8cca12e9c81024135a257b282ea773492830
Tx Hash:  0x14e8449b6c1479e313dba76e123076601659c7419004640a62efc67cf46005b3 (on chain 84532)
Tx Hash:  0x4dac39681070cdf61ae99d3087fac97a712e78d99a71758715c5c1089be84740 (on chain 7001)
Sender:   0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
Receiver: 0x71CEfA29FD68030657CB1207C86545625C557Ba9
Message:  00000000000000000000000005ba149a7bd6dc1f937fa9046a9e05c05f3b18b0000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000014a8b0227c0ffe07946e2b1d07f7a1cff59a1c21a9000000000000000000000000
Amount:   1000000000000000 Gas tokens
Error:    {"type":"contract_call_error","message":"contract call failed when calling EVM with data","error":"execution reverted: ret 0x: evm transaction execution failed","method":"depositAndCall0","contract":"0x6c533f7fE93fAE114d0954697069Df33C9B74fD7","args":"[{[168 176 34 124 15 254 7 148 110 43 29 7 247 161 207 245 154 28 33 169] 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9 84532} 0x236b0DE675cC8F46AE186897fCCeFe3370C9eDeD 1000000000000000 0x71CEfA29FD68030657CB1207C86545625C557Ba9 [0 0 0 0 0 0 0 0 0 0 0 0 5 186 20 154 123 214 220 31 147 127 169 4 106 158 5 192 95 59 24 176 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
84532 → 7001 ❌ Reverted
CCTX:     0x7d8740e2885af36e8e92fe5b81da8cca12e9c81024135a257b282ea773492830
Tx Hash:  0x14e8449b6c1479e313dba76e123076601659c7419004640a62efc67cf46005b3 (on chain 84532)
Tx Hash:  0x4dac39681070cdf61ae99d3087fac97a712e78d99a71758715c5c1089be84740 (on chain 7001)
Sender:   0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
Receiver: 0x71CEfA29FD68030657CB1207C86545625C557Ba9
Message:  00000000000000000000000005ba149a7bd6dc1f937fa9046a9e05c05f3b18b0000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000014a8b0227c0ffe07946e2b1d07f7a1cff59a1c21a9000000000000000000000000
Amount:   1000000000000000 Gas tokens
Error:    {"type":"contract_call_error","message":"contract call failed when calling EVM with data","error":"execution reverted: ret 0x: evm transaction execution failed","method":"depositAndCall0","contract":"0x6c533f7fE93fAE114d0954697069Df33C9B74fD7","args":"[{[168 176 34 124 15 254 7 148 110 43 29 7 247 161 207 245 154 28 33 169] 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9 84532} 0x236b0DE675cC8F46AE186897fCCeFe3370C9eDeD
84532 → 7001 ❌ Reverted
CCTX:     0x7d8740e2885af36e8e92fe5b81da8cca12e9c81024135a257b282ea773492830
Tx Hash:  0x14e8449b6c1479e313dba76e123076601659c7419004640a62efc67cf46005b3 (on chain 84532)
Tx Hash:  0x4dac39681070cdf61ae99d3087fac97a712e78d99a71758715c5c1089be84740 (on chain 7001)
Sender:   0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
Receiver: 0x71CEfA29FD68030657CB1207C86545625C557Ba9
Message:  00000000000000000000000005ba149a7bd6dc1f937fa9046a9e05c05f3b18b0000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000014a8b0227c0ffe07946e2b1d07f7a1cff59a1c21a9000000000000000000000000
Amount:   1000000000000000 Gas tokens
Error:    {"type":"contract_call_error","message":"contract call failed when calling EVM with data","error":"execution reverted: ret 0x: evm transaction execution failed","method":"depositAndCall0","contract":"0x6c533f7fE93fAE114d0954697069Df33C9B74fD7","args":"[{[168 176 34 124 15 254 7 148 110 43 29 7 247 161 207 245 154 28 33 169] 0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9 84532} 0x236b0DE675cC8F46AE186897fCCeFe3370C9eDeD 1000000000000000 0x71CEfA29FD68030657CB1207C86545625C557Ba9 [0 0 0 0 0 0 0 0 0 0 0 0 5 186 20 154 123 214 220 31 147 127 169 4 106 158 5 192 95 59 24 176 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 96 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 20 168 176 34 124 15 254 7 148 110 43 29 7 247 161 207 245 154 28 33 169 0 0 0 0 0 0 0 0 0 0 0 0]]"}

7001 → 84532 ✅ Revert executed
Reason for revert: 
Reason for abort:  
Revert Address:    0xA8B0227c0fFe07946E2b1d07F7a1cFF59A1C21A9
Call on Revert:    false
Abort Address:     0x0000000000000000000000000000000000000000
Revert Message:    -
Revert Gas Limit:  200000
Tx Hash:          0xbf48f6ead1a3b0c817db6294e44f5d81e28d26ab8e48933fc77364a30794c991 (on chain 84532)
```

https://testnet.zetascan.com/tx/0x4dac39681070cdf61ae99d3087fac97a712e78d99a71758715c5c1089be84740

````

</details>
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->



## 探究 Hello Webapp Base Testnet 交易页面显示无此交易问题

今天重新创建 Hello Webapp 项目，在网页端，分别在 Ethereum 和 Base 测试，结果和昨天一样。

在网页端，都显示成功。

Ethereum 链的两个交易记录页面都没问题。

[https://sepolia.etherscan.io/tx/0xa96868aeefdb95df671fc3f9a7f486a010714bfaf4f570ad4f3ad245611e3a94](https://sepolia.etherscan.io/tx/0xa96868aeefdb95df671fc3f9a7f486a010714bfaf4f570ad4f3ad245611e3a94)

[https://testnet.zetascan.com/tx/0x78549d3c6e9eb0dd1b9b88f14eac90ff927aff3aa927a7f095499a5f0406c8e5?tab=index](https://testnet.zetascan.com/tx/0x78549d3c6e9eb0dd1b9b88f14eac90ff927aff3aa927a7f095499a5f0406c8e5?tab=index)

Base 链的发起端交易页面显示无此记录，ZetaChain 端的交易记录显示正常。

[https://sepolia.basescan.org/tx/0xac782e2bd386726e711e1e1e34305c0f6c1d9fc5bbc1273762882ebb728c79b6](https://sepolia.basescan.org/tx/0xac782e2bd386726e711e1e1e34305c0f6c1d9fc5bbc1273762882ebb728c79b6)

[https://testnet.zetascan.com/tx/0x6d49b59790c3324c4de5c8126552f18b89df263f896b2fb4a56aa2511b21c485?tab=index](https://testnet.zetascan.com/tx/0x6d49b59790c3324c4de5c8126552f18b89df263f896b2fb4a56aa2511b21c485?tab=index)

查看合约的 Log 记录，两次调用都有记录。

[https://testnet.zetascan.com/address/0x2B78686636F8A99cD0686E1b85B38427980C5E52?tab=logs](https://testnet.zetascan.com/address/0x2B78686636F8A99cD0686E1b85B38427980C5E52?tab=logs)

所以，两次调用，实际都是成功的。

此外，在网页端，使用 Base 发起交易时，网页 Console 有错误的 Log。

<details>

MetaMask - RPC Error: Unrecognized chain ID "0x14a34". Try adding the chain using wallet\_addEthereumChain first.

\`\`\`

{

"code": 4902,

"message": "Unrecognized chain ID \\"0x14a34\\". Try adding the chain using wallet\_addEthereumChain first.",

"stack": "{\\n \\"code\\": 4902,\\n \\"message\\": \\"Unrecognized chain ID \\\\\\"0x14a34\\\\\\". Try adding the chain using wallet\_addEthereumChain first.\\",\\n \\"stack\\": \\"Error: Unrecognized chain ID \\\\\\"0x14a34\\\\\\". Try adding the chain using wallet\_addEthereumChain first.\\\\n at new o (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:239442)\\\\n at new <anonymous> (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:240041)\\\\n at Object.custom (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:244420)\\\\n at implementation (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/background-2.js:1:287903)\\\\n at chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/background-2.js:1:244997\\\\n at chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:150038\\\\n at new Promise (<anonymous>)\\\\n at _.p (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149696)\\\\n at_ .h (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149614)\\\\n at async _.d (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149415)\\\\n at async_ .l (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149241)\\"\\n}\\n at new o (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:239442)\\n at new <anonymous> (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:240041)\\n at Object.custom (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:244420)\\n at implementation (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/background-2.js:1:287903)\\n at chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/background-2.js:1:244997\\n at chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:150038\\n at new Promise (<anonymous>)\\n at _.p (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149696)\\n at_ .h (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149614)\\n at async _.d (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149415)\\n at async_ .l (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149241)"

}

\`\`\`

Failed to switch chain:

\`\`\`

{

"code": 4902,

"message": "Unrecognized chain ID \\"0x14a34\\". Try adding the chain using wallet\_addEthereumChain first.",

"stack": "{\\n \\"code\\": 4902,\\n \\"message\\": \\"Unrecognized chain ID \\\\\\"0x14a34\\\\\\". Try adding the chain using wallet\_addEthereumChain first.\\",\\n \\"stack\\": \\"Error: Unrecognized chain ID \\\\\\"0x14a34\\\\\\". Try adding the chain using wallet\_addEthereumChain first.\\\\n at new o (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:239442)\\\\n at new <anonymous> (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:240041)\\\\n at Object.custom (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:244420)\\\\n at implementation (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/background-2.js:1:287903)\\\\n at chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/background-2.js:1:244997\\\\n at chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:150038\\\\n at new Promise (<anonymous>)\\\\n at _.p (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149696)\\\\n at_ .h (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149614)\\\\n at async _.d (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149415)\\\\n at async_ .l (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149241)\\"\\n}\\n at new o (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:239442)\\n at new <anonymous> (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:240041)\\n at Object.custom (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-3.js:1:244420)\\n at implementation (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/background-2.js:1:287903)\\n at chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/background-2.js:1:244997\\n at chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:150038\\n at new Promise (<anonymous>)\\n at _.p (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149696)\\n at_ .h (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149614)\\n at async _.d (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149415)\\n at async_ .l (chrome-extension://nkbihfbeogaeaoehlefnkodbefgpgknn/common-2.js:3:149241)"

}

\`\`\`

</details>

打卡截止时间快到了，先记录这些。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->




## 回顾与反思

### 笔记编辑器

复制粘贴时，这个编辑器会把 “\`” 前面的中文字 或者 任何字符 吞掉，例如：

```
开启`localnet` //“启”会被吞掉

开启 `localnet` // 需用空格隔开
```

## 今日总结

成功运行 Hello 项目，并测试了在 Ethereum 和 Base 的 Testnet 发起合约调用。

![base.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/yedeyu/images/2025-11-28-1764316440460-base.png)![eth.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/yedeyu/images/2025-11-28-1764316469740-eth.png)

## Build a Web App

以下内容需要结合教程和 AI 翻译与解读 查看

[https://www.zetachain.com/docs/developers/tutorials/frontend](https://www.zetachain.com/docs/developers/tutorials/frontend)

[https://gemini.google.com/share/736605480d85](https://gemini.google.com/share/736605480d85)  
  
<details>

````
### Set Up Your Environment

```
cd hello/frontend
yarn
```

<details>

```
yarn install v1.22.22
[1/4] Resolving packages...
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.2.0"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.3.2"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.9.1"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.2.0"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.3.2"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.2.0"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@~1.8.1"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@~1.7.1"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.9.1"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.4.2"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.4.0"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@~1.2.0"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@~1.2.0"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@~1.4.0"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@~1.4.0"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@~1.4.0"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.9.1"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.9.1"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.9.2"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.8.2"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.7.2"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.8.0"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.7.0"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.9.1"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@~1.8.1"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@~1.7.1"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@~1.7.1"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.4.0"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.4.0"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.8.1"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.7.1"
warning Resolution field "@noble/curves@1.9.7" is incompatible with requested version "@noble/curves@1.8.1"
warning Resolution field "@noble/hashes@1.8.0" is incompatible with requested version "@noble/hashes@1.7.1"
[2/4] Fetching packages...
warning eciesjs@0.4.15: The engine "bun" appears to be invalid.
warning eciesjs@0.4.15: The engine "deno" appears to be invalid.
warning @ecies/ciphers@0.2.4: The engine "bun" appears to be invalid.
warning @ecies/ciphers@0.2.4: The engine "deno" appears to be invalid.
[3/4] Linking dependencies...
warning "@zetachain/toolkit > @nomiclabs/hardhat-ethers@2.2.3" has incorrect peer dependency "ethers@^5.0.0".
warning "@zetachain/toolkit > @solana/wallet-adapter-react@0.15.39" has incorrect peer dependency "@solana/web3.js@^1.98.0".
warning "@zetachain/toolkit > @solana/wallet-adapter-react > @solana/wallet-adapter-base@0.9.27" has incorrect peer dependency "@solana/web3.js@^1.98.0".
warning "@zetachain/toolkit > @solana/wallet-adapter-react > @solana-mobile/wallet-adapter-mobile > @react-native-async-storage/async-storage@1.24.0" has unmet peer dependency "react-native@^0.0.0-0 || >=0.60 <1.0".
warning "@zetachain/toolkit > @solana/wallet-adapter-react > @solana/wallet-standard-wallet-adapter-react > @solana/wallet-standard-wallet-adapter-base@1.1.4" has incorrect peer dependency "@solana/web3.js@^1.98.0".
warning "@zetachain/toolkit > @solana/wallet-adapter-react > @solana-mobile/wallet-adapter-mobile > @solana-mobile/mobile-wallet-adapter-protocol-web3js > @solana-mobile/mobile-wallet-adapter-protocol@2.2.3" has unmet peer dependency "react-native@>0.69".
warning "@zetachain/toolkit > @zetachain/protocol-contracts-solana > @solana/spl-token > @solana/spl-token-group > @solana/codecs > @solana/codecs-strings@2.0.0-rc.1" has unmet peer dependency "fastestsmallesttextencoderdecoder@^1.0.22".
warning "@zetachain/wallet > @react-native-async-storage/async-storage@2.2.0" has unmet peer dependency "react-native@^0.0.0-0 || >=0.65 <1.0".
warning "@zetachain/wallet > @dynamic-labs/ethereum > @walletconnect/ethereum-provider > @reown/appkit > valtio > use-sync-external-store@1.2.0" has incorrect peer dependency "react@^16.8.0 || ^17.0.0 || ^18.0.0".
warning "@zetachain/wallet > @dynamic-labs/ethereum > @dynamic-labs/embedded-wallet-evm > @turnkey/viem > @turnkey/sdk-browser > @turnkey/crypto > react-native@0.74.0" has incorrect peer dependency "react@18.2.0".
warning "@zetachain/wallet > @dynamic-labs/ethereum > @dynamic-labs/embedded-wallet-evm > @turnkey/viem > @turnkey/sdk-browser > @turnkey/crypto > react-native > @react-native/codegen@0.74.81" has unmet peer dependency "@babel/preset-env@^7.1.6".
warning "@zetachain/wallet > @dynamic-labs/ethereum > @dynamic-labs/embedded-wallet-evm > @turnkey/viem > @turnkey/sdk-browser > @turnkey/crypto > react-native > react-shallow-renderer@16.15.0" has incorrect peer dependency "react@^16.0.0 || ^17.0.0 || ^18.0.0".
warning "@zetachain/wallet > @dynamic-labs/ethereum > @dynamic-labs/embedded-wallet-evm > @turnkey/viem > @turnkey/sdk-browser > @turnkey/crypto > react-native > @react-native/codegen > jscodeshift@0.14.0" has unmet peer dependency "@babel/preset-env@^7.1.6".
warning "@zetachain/wallet > @dynamic-labs/ethereum > @dynamic-labs/embedded-wallet-evm > @turnkey/viem > @turnkey/sdk-browser > @turnkey/crypto > react-native > @react-native/community-cli-plugin > @react-native/metro-babel-transformer@0.74.81" has unmet peer dependency "@babel/core@*".
warning Workspaces can only be enabled in private projects.
warning Workspaces can only be enabled in private projects.
warning Workspaces can only be enabled in private projects.
warning Workspaces can only be enabled in private projects.
warning Workspaces can only be enabled in private projects.
[4/4] Building fresh packages...
Done in 402.30s.
```

</details>

### How It Works

#### Import the Toolkit

Actual code file:
[`hello/frontend/src/hooks/useHandleCall.ts`](../projects/hello/frontend/src/hooks/useHandleCall.ts)

```ts
import { evmCall } from "@zetachain/toolkit/chains/evm";
import { ethers, ZeroAddress } from "ethers";
```

#### Get a Signer from the Wallet


教程中的文件路径与代码：`frontend/src/MessageFlowCard.tsx`

```ts
const ethersProvider = new ethers.BrowserProvider(selectedProvider.provider);
const signer = (await ethersProvider.getSigner()) as ethers.AbstractSigner;
```

实际文件路径与代码：

[`hello/frontend/src/utils/ethersHelpers.ts`](../projects/hello/frontend/src/utils/ethersHelpers.ts)

```ts
const provider = new ethers.BrowserProvider(selectedProvider.provider);
const signer = (await provider.getSigner()) as ethers.AbstractSigner;
```

#### Define the Hello Contract Address

这里换成我们自己部署的测试链合约地址。

[`hello/frontend/src/constants/contracts.ts`](../projects/hello/frontend/src/constants/contracts.ts)

```ts
export const HELLO_UNIVERSAL_CONTRACT_ADDRESS =
  '0xc7Fcf45721f141319240a7955F553C9d54827C79';
```

#### Build the Call Parameters

教程中的文件路径与代码：`frontend/src/MessageFlowCard.tsx`

```ts
const evmCallParams = {
  receiver: helloUniversalContractAddress,
  types: ["string"],
  values: [stringValue],
  revertOptions: {
    callOnRevert: false,
    revertAddress: ZeroAddress,
    revertMessage: "",
    abortAddress: ZeroAddress,
    onRevertGasLimit: 1000000,
  },
};
 
const evmCallOptions = {
  signer,
  txOptions: {
    gasLimit: 1000000,
  },
};
```

实际文件路径与代码：

[`hello/frontend/src/hooks/useHandleCall.ts`](../projects/hello/frontend/src/hooks/useHandleCall.ts)

```ts
const callParams: CallParams = {
  receiver,
  types: ['string'],
  values: [message],
  revertOptions: {
    callOnRevert: false,
    revertAddress: walletAddress,
    revertMessage: '',
    abortAddress: ZeroAddress,
    onRevertGasLimit: 1000000,
  },
};

const { signer } = signerAndProvider;

const evmCallOptions = {
  signer,
  txOptions: {
    gasLimit: 1000000,
  },
};

```

#### Send the Cross-Chain Call

教程中的文件路径与代码：`frontend/src/MessageFlowCard.tsx`

```ts
const result = await evmCall(evmCallParams, evmCallOptions);
await result.wait();
 
setConnectedChainTxHash(result.hash);
```

实际文件路径与代码：

[`hello/frontend/src/MessageFlowCard.tsx`](../projects/hello/frontend/src/MessageFlowCard.tsx)

```ts
const { handleCall } = useHandleCall({
  primaryWallet,
  selectedProvider,
  supportedChain,
  receiver: HELLO_UNIVERSAL_CONTRACT_ADDRESS,
  message: stringValue,
  account,
  onSigningStart: () => setIsUserSigningTx(true),
  onTransactionSubmitted: () => setIsTxReceiptLoading(true),

  onTransactionConfirmed: (txHash: string) => setConnectedChainTxHash(txHash), // onTransactionConfirmed 对应 例子中的 `await result.wait();`
  // 即，有交易结果了


  onError: (error: Error) => console.error('Transaction error:', error),
  onComplete: () => {
    setIsUserSigningTx(false);
    setIsTxReceiptLoading(false);
  },
});
```

#### Configure Networks and Explorers


在这里设置支持的链的信息。

[`frontend/src/constants/chains.ts`](../projects/hello/frontend/src/constants/chains.ts)

```ts
export const SUPPORTED_CHAINS: SupportedChain[] = [
  {
    explorerUrl: (txHash: string) => `https://sepolia.arbiscan.io/tx/${txHash}`,
    name: 'Arbitrum Sepolia',
    chainId: 421614,
    chainType: 'EVM',
    icon: '/logos/arbitrum-logo.svg',
    colorHex: '#28446A',
  }
]
 
export const ZETACHAIN_ATHENS_BLOCKSCOUT_EXPLORER_URL = (txHash: string) =>
  `https://zetachain-testnet.blockscout.com/tx/${txHash}`;

```

#### Poll for Cross-Chain Status

教程中的文件路径与代码：`frontend/src/MessageFlowCard.tsx`

```ts
const response = await fetch(`${CCTX_POLLING_URL}/${connectedChainTxHash}`);
if (response.ok) {
  const data = (await response.json()) as CrossChainTxResponse;
  const txHash = data.CrossChainTxs?.[0]?.outbound_params?.[0]?.hash;
  if (txHash) setZetachainTxHash(txHash);
}
```

实际文件路径与代码：

[`hello/frontend/src/ConfirmedContent.tsx`](../projects/hello/frontend/src/ConfirmedContent.tsx)

```ts
const poll = async () => {
  try {
    const response = await fetch(
      `${CCTX_POLLING_URL}/${connectedChainTxHash}`
    );
    if (response.ok) {
      const data = (await response.json()) as CrossChainTxResponse;
      const txHash = data.CrossChainTxs?.[0]?.outbound_params?.[0]?.hash;
      if (txHash) {
        setZetachainTxHash(txHash);
      }
    }
  } catch (error) {
    console.error('Polling error:', error);
  }
};
```

应用使用源链的交易哈希定期查询 ZetaChain 的公共 API。一旦 ZetaChain 处理了该调用，响应中就会包含 ZetaChain 的交易哈希，随后该哈希会显示在 UI 上。


#### AI:区块链新手启示

```md
这里涉及了 Web3 前端开发（DApp Development）的核心心法。

## 1. 核心概念：前端不再持有逻辑

在传统 Web2 中，前端通常把数据发给后端服务器，后端处理逻辑存入数据库。
在 Web3 中，**没有后端服务器**（严格来说）。

  * 你的 React 前端直接与用户的**钱包 (MetaMask)** 对话。
  * 用户的钱包直接与**区块链节点**对话。
  * ZetaChain Toolkit (`evmCall`) 只是一个帮手，它帮你组装好数据包，递给钱包说：“老板，请在这个交易上签个字。”

## 2. 思维模型：异步中的异步

作为新手，你必须习惯**极度的慢**。

  * 用户点击按钮 -> 钱包弹出（等待用户操作）。
  * 用户签名 -> 交易发送到源链（等待出块，约 2-12 秒）。
  * 源链确认 -> ZetaChain 观察员发现（等待若干秒）。
  * ZetaChain 处理 -> 目标链执行（又是若干秒）。

这不像你以前写的 API 接口 `await fetch()` 马上就有结果。所以代码里必须有 **Polling (轮询)** 机制。你的 UI 必须设计得让用户知道“正在处理中”，否则用户会以为网站卡死了。

## 3. 与传统编程的对比：Signer vs Session

  * **Web2:** 用户输入账号密码 -> 服务器给一个 Session Token -> 后续请求带上 Token。
  * **Web3:** 用户不需要注册。`signer` 对象就是一切。只要用户连上了钱包，你就有了 `signer`。用 `signer` 发出的任何指令（如 `evmCall`），都代表用户本人的意愿。

## 4. 新手注意事项

  * **`ZeroAddress` 的坑:** 代码里用到了 `ZeroAddress` (`0x000...000`) 作为回滚地址。这在测试时没问题，意味着“如果不退款，就销毁资产”。但在生产环境中，如果涉及真金白银，**务必**把 `revertAddress` 设置为用户的钱包地址，否则一旦跨链失败，钱就真的没了（打入黑洞）。
  * **ABI Encoding:** 注意 `types: ["string"]` 和 `values: [stringValue]`。这一步非常关键。如果你在合约里写的参数是 `uint256`（数字），而这里前端传了 `string`，交易会发送成功，但在链上执行时会**直接崩溃**，而且很难调试。一定要保证前端的 `types` 和合约里的 `function` 参数严格对应。
```

### End-to-End Flow in the UI

UI 端到端流程

前端界面引导用户完成一个简单但完整的跨链操作流程，具体如下：

1.  **连接钱包：** 应用自动检测兼容 EIP-6963 标准的钱包，并通过 `WalletProvider` 进行连接。连接后的账户将用于对交易进行签名和发送。
2.  **选择网络：** 用户必须从预定义的 `SUPPORTED_CHAINS`（支持链列表）中选择一个**源链**。列表中的每条链都包含了其名称、ID 以及区块浏览器链接。
3.  **输入消息：** 用户在 UI 界面中输入纯文本字符串。应用会强制执行字节长度限制，以确保输入内容能够被安全地编码并在跨链调用中发送。
4.  **发送调用：** 当用户点击 **Send（发送）** 按钮时，前端执行 `evmCall` 函数。该函数将 ZetaChain 上的 Hello 合约地址指定为**接收者 (receiver)**。随后，生成的源链交易哈希会被保存下来用于后续追踪。
5.  **追踪结果：** UI 会立即显示源链上的交易状态，随即开始**轮询 (Polling)** ZetaChain 以查询跨链交易 (CCTX) 的进度。一旦查询到结果，应用将同时显示指向源链浏览器和 ZetaChain 浏览器的链接。

#### AI：区块链新手启示

```md
## 1. 核心思维模型：跨链交互的“三段式”体验
做 Web2 开发时，用户点击按钮 -> 菊花转圈 -> 成功。
做跨链开发时，你必须设计**“三段式”反馈**，否则用户会以为你的应用坏了：
* **阶段一（本地确认）：** 钱包弹出，用户签名。UI 显示：“正在发送到源链...”。
* **阶段二（源链上链）：** 几秒后，拿到 Source Tx Hash。UI 显示：“源链已确认！正在跨链传输中...（此时开始轮询）”。
* **阶段三（目标链确认）：** 几十秒后，拿到 ZetaChain CCTX Hash。UI 显示：“跨链成功！Hello 消息已送达。”

## 2. 为什么“字节限制”很重要？
我在解析里提到了 Gas 费。对于新手来说，**前端校验**至关重要。
* 如果你不在前端限制用户输入 10000 字的小说，用户点击发送后，钱包会弹出一个**天价 Gas 费**（比如 $500），或者交易直接失败。
* **最佳实践：** 始终在前端计算 `Bytes` 大小，并在用户输入过长时禁用“发送”按钮。

## 3. 关于 `evmCall` 的幕后
流程第 4 步看似简单（执行 `evmCall`），其实它是整个 Toolkit 的精华。
如果你不使用这个工具包，你需要手动写以下代码：
1.  判断当前连的是哪条链。
2.  去查表找到这条链对应的 ZetaChain Gateway 合约地址。
3.  把 "Hello" 编码成 Hex 字符串。
4.  构造一笔发给 Gateway 的交易，把数据塞进 payload。
`evmCall` 把这些繁琐且容易出错的步骤都抽象掉了。
```

### Install and Start

```
cd hello/frontend
yarn
yarn dev
```

```
yarn run v1.22.22
$ vite

  VITE v7.1.6  ready in 985 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
[baseline-browser-mapping] The data in this module is over two months old.  To ensure accurate Baseline data, please update: `npm i baseline-browser-mapping@latest -D`
```

### Wallets

需要改成 `false`，点击“链接钱包”才有反应。

[frontend/src/constants/wallets.ts](../projects/hello/frontend/src/constants/wallets.ts)

```ts
export const USE_DYNAMIC_WALLET = false;
```

### 成功完成交互

测试了在 Ethereum 和 Base 的 Testnet 发起合约调用，在网站页面都分别显示成功。

但是，Base 测试网上的交易哈希在 BaseScan 上找不到！

https://sepolia.basescan.org/tx/0x1b899bd40edce5199e572f579d50c4bfdfe55c429cc754f191db823af21b984c
https://testnet.zetascan.com/tx/0xd6cb916ee67244785a171175e4ed08281417269863cac417c729d4d9602bdad5

````

</details>
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->







## 回顾与反思

### 继续探究昨天提到的内存不足导致编译失败问题

回顾那篇教程，它先指导开启 `localnet`，然后指导编译 Solidity 合约，实际上，编译时，不需要运行`localnet`，而它会占用大量内存，因此，关闭 `localnet`可以避免因内存不足导致的编译失败。

### 笔记编辑器

昨天提交笔记/打卡后，查看最终笔记的显示效果，发现有 typo，再次点开按钮，发现可以修改！

一段时间后再次查看最终显示效果，发现系统自动把`<details>` 标签删了。

[https://github.com/IntensiveCoLearning/Universal-AI/commit/610f9c705552c4f7660b996358dcea0cff8e6519](https://github.com/IntensiveCoLearning/Universal-AI/commit/610f9c705552c4f7660b996358dcea0cff8e6519)

## 今日总结

今天简单测试了 hello 项目的前端功能。

发现教程里的代码片段和 hello 项目的实际代码有较大差异，甚至连代码片段所在的文件名都对不上，导致有时候简单的搜索都找不到对应的实际代码片段及文件。

现在快到今天的打卡截止时间了，相关的笔记/记录等明天再完整地整理出来。
<!-- DAILY_CHECKIN_2025-11-26_END -->

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
