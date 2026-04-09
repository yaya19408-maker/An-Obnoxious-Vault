---
cssclasses:
  - center-titles
tags:
  - scholars
aliases:
relevance:
relevance 2:
disambiguation:
banner:
media:
---
Date: 2026-04-03
File Creation Date: 2026-04-03 00:59
Last Modified: 2026-04-03 00:59
File folder: A012. Scholars

# Ashish Vaswani (阿希什·瓦斯瓦尼)
#scholars #artificial_intelligence #computer_science

# Attention Is All You Need 注意力機制即一切 (2017)
- **Transformer (轉換器模型)：** 捨棄傳統循環與卷積結構的深度學習架構。該架構依賴注意力機制處理輸入與輸出之間的全局依賴關係，達成高度併行化運算。

- **Attention Mechanism (注意力機制)：** 模擬人類視覺與認知的資源分配模式。模型在處理資訊時會根據權重將焦點集中在特定的輸入片段，藉此擷取關鍵特徵。
- **Self-Attention (自注意力機制)：** 評估單一序列中各個位置相互關聯程度的運算過程。模型藉此為序列中的每個元素生成包含整體上下文資訊的動態表示。
- **Multi-Head Attention (多頭注意力)：** 將注意力機制在不同的特徵子空間中併行運算的設計。這容許模型同時捕捉來自不同位置與語意層次的豐富資訊。
- **Scaled Dot-Product Attention (縮放點積注意力)：** 基礎評分函數。透過計算向量點積並進行平方根縮放，最終利用歸一化函數取得權重分佈，避免梯度消失。
- **Query (查詢 / Q)：** 代表當前正尋求資訊的特徵向量。它與序列中其他位置的鍵進行匹配，決定對該位置的關注比例。
- **Key (鍵 / K)：** 代表序列中各個位置索引特徵的向量。它負責承接查詢的比對，共同決定注意力權重的分佈情形。
- **Value (值 / V)：** 儲存實際語意內容的向量。最終的輸出結果為這些向量依據計算出的權重進行加權總和後的產物。
- **Positional Encoding (位置編碼)：** 注入序列順序資訊的技術。由於模型結構缺乏遞歸性，必須透過正弦與餘弦函數將絕對或相對位置座標加入原始嵌入向量。
- **Encoder (編碼器)：** 由多層相同單元堆疊而成的模塊。每層包含自注意力子層與前饋神經網路，負責將輸入序列轉化為高階的抽象特徵表示。
- **Decoder (解碼器)：** 負責生成輸出序列的模塊。除了具備自注意力與前饋網絡，額外包含交叉注意力子層以獲取來自編碼器的上下文資訊。
- **Feed-Forward Network (前饋神經網路)：** 應用於每個位置的獨立全連接層。它對注意力層的輸出執行非線性轉換，增加模型處理複雜特徵的能力。
- **Residual Connection (殘差連接)：** 在子層的輸入與輸出之間建立的直接通道。此結構有助於緩解深層網路訓練中的梯度不穩定問題。
- **Layer Normalization (層歸一化)：** 將單層神經元的輸出進行標準化處理。這項步驟穩定訓練過程並加速模型的收斂速度。
- **Masked Self-Attention (遮罩自注意力)：** 解碼器中的特殊機制。它限制模型在預測當前位置時僅能參考過去的資訊，確保自回歸生成過程符合邏輯順序。
- **Cross-Attention (交叉注意力)：** 連結編碼器與解碼器的關鍵橋樑。解碼器利用此機制從編碼器的最終輸出中提取與當前生成任務相關的特徵。
- **Softmax Layer (歸一化層)：** 將原始分值轉換為機率分佈的函數。在機制中確保所有位置的關注權重總和為一。
- **Sequence-to-Sequence (序列對序列)：** 模型的主要應用類型。輸入一個長度的序列並輸出另一種長度的序列，為翻譯與摘要等任務的理論基礎。
- **Global Dependencies (全局依賴)：** 模型直接連結序列中任意兩個遠距離位置的能力。這項特質有效解決了傳統模型在長距離記憶上的侷限。
- **Parallelization (並行化)：** 轉換器結構的核心優勢。由於各位置的計算不依賴前一時刻的結果，模型能在硬體設備上實現高效的大規模並行訓練。

# An-Jhon/Hand-Drawn-Transformer
https://github.com/An-Jhon/Hand-Drawn-Transformer/tree/main