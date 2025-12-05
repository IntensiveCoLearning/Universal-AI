---
timezone: UTC+8
---

# 鄭鈞元

**GitHub ID:** cheng-chun-yuan

**Telegram:** @amidoggy

## Self-introduction

a student developer with interest in AI and blockchain

## Notes

<!-- Content_START -->
# 2025-12-05
<!-- DAILY_CHECKIN_2025-12-05_START -->
checking
<!-- DAILY_CHECKIN_2025-12-05_END -->

# 2025-12-03
<!-- DAILY_CHECKIN_2025-12-03_START -->

Check in
<!-- DAILY_CHECKIN_2025-12-03_END -->

# 2025-12-01
<!-- DAILY_CHECKIN_2025-12-01_START -->


curl

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-12-01-1764601462315-image.png)

python

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-12-01-1764601893364-image.png)
<!-- DAILY_CHECKIN_2025-12-01_END -->

# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->



Cross-Chain Micro-Payment Lending  
\- Target Users: DeFi users on Solana/Sui (fast/low-cost chains) needing quick loans against small BTC/ETH holdings, plus Bitcoin holders entering DeFi.  
\- Problem Solved: Fragmented liquidity and slow cross-chain borrowing delay micro-loans (<$100); users face bridging delays, high fees, and volatility exposure during waits.  
\- Cross-Chain/Universal Asset Usage: Users deposit native BTC via Gateway as ZRC-20 on ZetaChain; universal contract instantly swaps to Universal Token collateral, issues USDC loan (flash-like, quick settlement), and repays via small cross-chain payment from Solana/Sui earnings—leveraging ZRC-20 for atomic txns and messaging for repayment calls. Bond-backed via overcollateralized ZRC-20 positions for fast unlock (~minutes).
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->




Succesful deploy the swap contract by foundry and fill in the sepolia eth to base chain for the swap instruction

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-29-1764430879036-image.png)
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->





**ZRC20 是外部鏈資產在 ZetaChain 上的映射與統一標準**  
例如：  
ETH（以太坊） → ZRC-ETH（ZetaChain） → SOL（Solana）  
它代表原生 gas 資產或白名單內的 ERC20，在 ZetaChain 上的跨鏈表示形式。

###Lending Platform

包含能管理 Universal Token 與跨鏈資產的借貸機制。  
這部分是開發者最值得深耕的領域。

我認為非常有前景的 DeFi 方向：  
➡️ **統一用戶錢包資產管理**  
使用者將各鏈錢包資產匯聚至 ZetaChain  
在 Lending Platform 中透過借貸協議或演算法  
自動做最優化資產配置與收益策略

真正做到：  
「一個錢包、所有鏈的流動性」
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-26
<!-- DAILY_CHECKIN_2025-11-26_START -->






![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-26-1764170707675-image.png)

## What is a Universal App?

A single App that can actually **handle all blockchains**, achieving true **Omnichain** functionality.

**Two Core Requirements:**

1.  **Watch Everything (Event Monitoring):** It has to catch **every single state-changing event** that happens on _all_ connected blockchain networks.
    

1.  **Send TXs Anywhere (Universal TX Sending):** It needs to be able to send the required transaction commands **smoothly** to **any chain** from just one application logic layer.
    

* * *

## How Zetachain Makes it Work

Zetachain uses a decentralized setup (by validator nodes) with two main groups helping out:

1.  **The Observers :**
    
    -   constantly monitoring and validating activities/events happening on all the external chains (like Ethereum, BNB Chain, etc.).
        

1.  **The TSS-Signers:**
    
    -   **Tech:** use the **Threshold Signature Scheme (TSS)**, which enable t-of-n participants to generate a valid signature.
        
    
    -   **Action:** When a cross-chain transaction is needed, they **group-sign** the transaction on the protocol's behalf.
        
    
    -   **Security:** secure and decentralized because no single person holds the full private key ! (But not understand how to add more **TSS-Signers without compromise security, raise threshold?**)
        

* * *

## Gateway: What is it For?

The "secure gate" that lets data and assets move between chains. It's powered by the Observers and TSS Signers working together.

-   **Key Functions**: The Gateway processes cross-chain messages, authorizes final transaction execution on the target chain using the **TSS signature**, and ensures state synchronization.
    

-   **Decentralized Control**: The Gateway's control (the TSS address) is secured trustlessly by the cryptographic consensus of the decentralized signers.
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-26-1764170782225-image.png)
<!-- DAILY_CHECKIN_2025-11-26_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->







-   Qwen Request: get api key from website and request through terminal !!
    

![Screenshot 2025-11-25 at 8.10.09 PM.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-25-1764073399733-Screenshot_2025-11-25_at_8.10.09_PM.png)

-   Faucet: From Faucetme to get zeta test token !!
    

![Screenshot 2025-11-25 at 7.54.19 PM.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-25-1764073351187-Screenshot_2025-11-25_at_7.54.19_PM.png)

-   ZetaChain CLI: create account, check and get faucet from cli
    

![Screenshot 2025-11-25 at 8.19.18 PM.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-25-1764073298042-Screenshot_2025-11-25_at_8.19.18_PM.png)

-   Explorer: check my balance after request faucet, but no understand why no any transactions on it, look like directly modified amount in block?
    

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-25-1764073209736-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->

# 2025-11-24
<!-- DAILY_CHECKIN_2025-11-24_START -->








打卡一下

完成 zetachain cli 安裝

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-24-1763987488195-image.png)

和註冊 Qwen

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/cheng-chun-yuan/images/2025-11-24-1763987597786-image.png)
<!-- DAILY_CHECKIN_2025-11-24_END -->
<!-- Content_END -->
