---
timezone: UTC+8
---

# james_1022

**GitHub ID:** ARZER-TW

**Telegram:** @Lain_1022

## Self-introduction

student

## Notes

<!-- Content_START -->
# 2025-11-30
<!-- DAILY_CHECKIN_2025-11-30_START -->
今天實在太累了，水個一天
<!-- DAILY_CHECKIN_2025-11-30_END -->

# 2025-11-29
<!-- DAILY_CHECKIN_2025-11-29_START -->

### **1\. ZRC-20 與普通 ERC-20 的直觀區別 (開發者視角)**

雖然 ZRC-20 在介面上兼容 ERC-20 (都有 `transfer`, `balanceOf` 等)，但它們在行為和意義上有本質不同：

-   **資產來源與本質**：
    
    -   **ERC-20**：通常是**原生於該鏈**的代幣（例如在 Ethereum 上發行的 UNI）。它的價值和存在範圍僅限於該鏈的帳本。
        
    -   **ZRC-20**：是外部鏈原生資產在 ZetaChain 上的**映射 (Mapping)**。例如，ZetaChain 上的 `ZRC-20 BTC` 代表的是比特幣鏈上的真實 BTC；`ZRC-20 ETH` 代表的是以太坊上的真實 ETH。
        
-   **交互與提款 (Withdrawal)**：
    
    -   **ERC-20**：調用轉帳函數只會改變合約內的餘額數字。
        
    -   **ZRC-20**：具備\*\*「提款」能力\*\*。
        
        -   開發者可以將 ZRC-20 發送到一個特殊的系統地址（或調用 `withdraw`），這不僅會銷毀 ZetaChain 上的代幣，還會**觸發 ZetaChain 協議在外部鏈（如 Bitcoin 網絡）上發起一筆真實的轉帳交易**，將資產退回到指定的外部地址。這是普通 ERC-20 做不到的。
            
-   **Gas 費處理**：
    
    -   **ZRC-20**：內建了對外部鏈 Gas 費用的處理邏輯。當你操作 ZRC-20 進行提款或跨鏈調用時，協議會自動計算並扣除所需的外部鏈 Gas 費。
        

* * *

### **2\. 通用資產 (Universal Assets) 的應用場景**

通用資產（Universal Token / NFT）是指只需在 ZetaChain 上部署合約，就能讓所有連接鏈的用戶進行交互的資產。

**場景舉例：全鏈通用 NFT 通行證 (Universal NFT Pass)**

-   **背景**： 假設你正在開發一款 Web3 遊戲或 DAO，你希望 Ethereum、Polygon、BNB Chain 甚至 Bitcoin 的用戶都能購買並持有你的會員卡 (NFT)，且不想在每條鏈上都發行一個 NFT 合約。
    
-   **解決方案**： 在 ZetaChain 上發行一個 **Universal NFT**。
    
-   **運作流程**：
    
    1.  **購買**：Ethereum 用戶直接支付 ETH，或者 Bitcoin 用戶直接支付 BTC。這些資產通過 ZRC-20 進入 ZetaChain。
        
    2.  **鑄造**：ZetaChain 上的合約收到信號後，鑄造一個 NFT 並保存在 ZetaChain 上（用戶持有該 NFT 的所有權）。
        
    3.  **使用**：該 NFT 永遠留在 ZetaChain 上，不需要跨鏈橋接。當用戶在 Ethereum 的 DApp 登入時，DApp 透過查詢 ZetaChain 上的合約確認該地址是否持有 NFT，從而授予權限。
        
-   **優勢**：
    
    -   **流動性統一**：所有鏈的用戶都在買同一個合約裡的 NFT，不會出現「以太坊版」和「Polygon 版」價格不同或割裂的情況。
        
    -   **用戶體驗**：比特幣用戶不需要安裝 MetaMask 或去以太坊交易所買 ETH，直接用 BTC 就能參與。
<!-- DAILY_CHECKIN_2025-11-29_END -->

# 2025-11-28
<!-- DAILY_CHECKIN_2025-11-28_START -->


# Day 4 作業筆記：Universal App 實作規劃

## 1\. 專案目標

**建立一個「跨鏈留言板」 (Cross-Chain Message Board)**

## 2\. 應用邏輯 (Universal App Logic)

1.  **觸發 (Source Chain):** 用戶在 Goerli/Sepolia (EVM) 或 Bitcoin Testnet 上發送一筆交易，並附帶一段字串（例如 `"Hello Zeta"`）。
    
2.  **接收 (ZetaChain):** ZetaChain 上的合約透過 `onCrossChainCall` 收到調用。
    
3.  **狀態更新:** 合約將訊息解析後記錄。
    
4.  **查詢:** 任何人都可以在 ZetaChain 上查詢到這條來自比特幣或以太坊的留言。
    

## 3\. 開發工具選擇

-   **工具棧:** **ZetaChain CLI + Hardhat**
    
-   **原因:** `zetachain-toolkit` 提供了極大的便利，特別是在處理 ZRC-20 地址映射與多鏈環境模擬上，優於純 Foundry 流程。
    

## 4\. 工作流策略 (Workflow)

-   **Step 1: Localnet (本地模擬)**
    
    -   使用 `npx hardhat standalone` 命令。
        
    -   **目的:** 在本地同時模擬 ZetaChain + Ethereum + Bitcoin，快速驗證合約邏輯是否能正確解析 Input Data，省去等待區塊的時間。
        
-   **Step 2: Testnet (Athens-3)**
    
    -   一旦邏輯跑通，立刻部署到 Athens-3 測試網。
        
    -   **目的:** 體驗真實的 Gas 估算和網絡延遲（這是跨鏈開發真正的痛點）。
<!-- DAILY_CHECKIN_2025-11-28_END -->

# 2025-11-27
<!-- DAILY_CHECKIN_2025-11-27_START -->



Universal App:構建在 ZetaChain 上的智慧合約，能在一個合約中與所有區塊鏈互動，只要在ZetaChain 上部署一次，就可以管理和操作多條鏈上的資產與數據。  
  
Gateway:Gateway 是 ZetaChain 連接外部區塊鏈的關鍵接口，處理資產和訊息的跨鏈傳輸。  
  

![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/ARZER-TW/images/2025-11-27-1764255707486-image.png)
<!-- DAILY_CHECKIN_2025-11-27_END -->

# 2025-11-25
<!-- DAILY_CHECKIN_2025-11-25_START -->




![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/ARZER-TW/images/2025-11-25-1764086172356-image.png)![image.png](https://raw.githubusercontent.com/IntensiveCoLearning/Universal-AI/main/assets/ARZER-TW/images/2025-11-25-1764085639294-image.png)
<!-- DAILY_CHECKIN_2025-11-25_END -->
<!-- Content_END -->
