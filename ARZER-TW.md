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
