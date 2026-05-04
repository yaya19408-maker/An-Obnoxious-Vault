
## 第8講：大型語言模型修練史 — 第三階段: 參與實戰，打磨技巧 (Reinforcement Learning from Human Feedback, RLHF)
[課程連結](https://www.youtube.com/watch?v=v12IKvF6Cj8&list=PLJV_el3uVTsPz6CTopeRp2L2t4aL_KgiI&index=9)
### RLHF
![image](https://hackmd.io/_uploads/S1EGhkwfR.png)

如果詢問GPT問題之後，對於它的問題不滿意的情況下可以點擊對話中的重新來一次，這時候模型會詢問對於新的回應是否滿意，這個回饋就提供模型一個更新的機制。

### RLHF
![image](https://hackmd.io/_uploads/rkXY3yPfC.png)

目前課程已經提到的前兩個階段，第一階段是自督導，看過一堆資料之後還不知道怎麼應用，接著第二階段是督導式學習，給定輸入，然後跟機器說看到這樣的輸入應該給怎麼樣的輸出。

現在第三階段沒有任何資料，只有根據使用者的回饋來做調整，這樣的作法又稱為RL，稱者增強式學習。
### 增強式學習(Reinforcement Learning, RL)
![image](https://hackmd.io/_uploads/ryiVTyDGA.png)

RL的概念大概就是讓機器微調參數以提高人們覺得那個回應較好的機率，好的回應我們當然是希望它可以常出現。

### 增強式學習(Reinforcement Learning, RL)
![image](https://hackmd.io/_uploads/By1ITywfR.png)

相關RL資訊可參考過去的課程。

大型語言中的RLHF的作法即為PPO，相關課程也有，有興趣就直接看。
### RLHF vs Instruction Fine-tuning
![image](https://hackmd.io/_uploads/Hks-0kvfR.png)

在第二、第三階段的訓練中，人類都是必需要直接介入的，尤其是第二階段，人類必需提供非常直接的文本資訊，但這很累。

如果是在第三階段的話，人類只需要回饋訊息給機器，讓機器知道那一個回應是比較好的，那就可以指導機器調整。

### RLHF vs Instruction Fine-tuning
![image](https://hackmd.io/_uploads/B17VAywM0.png)

對於人類來說，判斷一個回應好不好會比寫出正確答案還要來的輕鬆，上面教授給出一個範例，讓機器產生一個七言絕句，這很明顯的，第一個回應根本是五言...所以我們就可以反饋給機器第二個回應比較好，讓機器以此更新。

### RLHF vs Instruction Fine-tuning
![image](https://hackmd.io/_uploads/SyclkewMA.png)

從模型學習的角度來看，對於Instruction Fine-tuning的部份，模型要知道的是下一個字要怎麼接，它只管下一個字。

但RLHF所考量的是最終結果，因為人類的反饋是在所有文字接龍都完成的時候才會有所反饋。也因此這種情況下比較會是通盤考量，而不是單純的思考下一個字是什麼。

### RLHF vs Instruction Fine-tuning
![image](https://hackmd.io/_uploads/S1-Qylwz0.png)

一句話總結兩者的差異。
### 語言模型 vs 下圍棋
![image](https://hackmd.io/_uploads/B1BIexPzA.png)
![image](https://hackmd.io/_uploads/rytulxvf0.png)
這邊類比說明語言模型跟AlphaGo。

簡單說，語言模型預測下一個token，而阿發狗則是預測下一個落子位置。

### 語言模型 vs 下圍棋
![image](https://hackmd.io/_uploads/ByJnglPzR.png)

不管是語言模型還是阿發狗，表面上看起來是生成式學習，但實際上『每一步』都是分類問題。阿發狗每一步都要根據棋盤決定一個落子位置，而語言模型都要根據目前的句子決定下一個token。

### 語言模型 vs 下圍棋
![image](https://hackmd.io/_uploads/rJXVblwzA.png)
![image](https://hackmd.io/_uploads/rJlBvbevz0.png)

AlphaGo的學習有兩階段，一階段是跟著人類的棋盤學習，老師怎麼教，它就怎麼做。

基本這也對應著課程所說的學習三階段中的第一、第二階段。反正人類說下一個字是接什麼，它就跟著接什麼。
### 語言模型 vs 下圍棋
![image](https://hackmd.io/_uploads/SJGkGgwG0.png)
![image](https://hackmd.io/_uploads/HJ3r4xwzR.png)

不過阿發狗光是棋譜還不夠成為頂尖高手。接著還需要利用RL來學習，透過對弈，如果最終是機器贏了，那就提高贏的過程中所下的棋路；反之輸了就降低。

語言模型也是一樣，下圍棋有輸贏，而語言模型有人類反饋，模型就可以利用這樣的反饋來提高人類覺得好的回應出現的機率。
### 語言模型 vs 下圍棋
![image](https://hackmd.io/_uploads/HyTbSgvzA.png)

不過下圍棋的輸贏是有規則的，但是要反饋語言模型我們會發現，目前的作法通常都是要有多個回應人類才有辦法選擇。

如果是單一句的回應的話我們比較難從中判斷到底這個回應是好還是不好，因為好不好是相對的，而不是絕對的。

上圖為例，有人可能覺得就回答玉山就好了，但有人可能會覺得如果可以補充一下玉山的資訊會更好之類的。

### 回饋模型(Reward Model)：模仿人類喜好
![image](https://hackmd.io/_uploads/ryLdxr_fR.png)

如何更有效的利用人類的回饋？有這麼一種作法，就是創造出一個虛擬人類來假裝如果是人類的話會怎麼做。

大致的作法就是利用人類的反饋，然後讓人類覺得比較好的那個回應在經過回饋模型的時候所得到的分數要大過比較不好的那一個。

這樣子我們就可以拿回饋模型的輸出來模擬人類的回答。

### 回饋模型(Reward Model)：模仿人類喜好
![image](https://hackmd.io/_uploads/SkMWbBuz0.png)

語言模型的回應基本上不會只有一個答案，會有多個，只需要將這多個回應丟給回饋模型，然後看那一個模型得到的分數是最高的，就丟那個分數最高的來做為答案。

### 回饋模型(Reward Model)：向虛擬人類學習
![image](https://hackmd.io/_uploads/H1GYWHuM0.png)
![image](https://hackmd.io/_uploads/ByiKbB_z0.png)

更好的一種作法是讓語言模型直接向回饋模型學習，把語言模型的輸入、輸出直接做為回饋模型的輸入來看得分，如果再利用回饋模型的回應來做參數微調。如果分數低，那就降低這個回應出現的機率，反之亦然。

這麼做的好處在於語言模型可以直接向回饋模型學習，而不需要再等待人類的回應。
### 回饋模型(Reward Model)：向虛擬人類學習
![image](https://hackmd.io/_uploads/r1hCzSuGR.png)

[參考論文_Training language models to follow instructions with human feedback](https://arxiv.org/abs/2203.02155)

左圖為論文中比較單純做第二階段，也就是SFT的模型與做PPO-ptx，也就是RLHF的模型之間的差異。很明顯的，有做過RL的效果就是比較好。

右圖是模型大小比較，不同顏色代表不同學習方法，藍色線是第一階段的模型，綠色是第二階段的模型，剩下的兩條是做完RL的模型。

從結果來看，即使是最小的模型，在經過RL的調校之後得到的結果是可以打爆只做完第一階段訓練的超大型模型。
### 回饋模型(Reward Model)：向虛擬人類學習
![image](https://hackmd.io/_uploads/HkwAmBdMR.png)

[參考論文_Learning to summarize from human feedback](https://arxiv.org/abs/2009.01325)


參考論文中主要說明過度向虛擬人類學習是有害的。橫軸的部份是說明向虛擬人類學習到多深，愈右邊代表愈深。蹤軸代表喜好程度，虛線是回饋模型的喜好程度，實線是真人的喜好程度。

很正常的，向回饋模型學的愈多，對回饋模型來說得到的回應就會是它愛的，但這樣的回應卻不見得是人類所喜好的。

論文中做的是摘要任務，上圖右看的到，過度學習得到的回應中總是會出現『???』，以及結尾的『pls』。
### 回饋模型(Reward Model)：向虛擬人類學習
![image](https://hackmd.io/_uploads/SkuaEHOfR.png)

學歪掉這事不是亂說的，是自己人研究出來的。

### 回饋模型(Reward Model)：向虛擬人類學習
![image](https://hackmd.io/_uploads/SJEvBB_MC.png)

[參考論文_Direct Preference Optimization: Your Language Model is Secretly a Reward Model](https://arxiv.org/abs/2305.18290)

[參考論文_KTO: Model Alignment as Prospect Theoretic Optimization](https://arxiv.org/abs/2402.01306)

如果說向虛擬人類學過頭會歪掉的話，那就試著不要有虛擬人類。參考論文中的DPO與KTO就是往這個方向調整。

### RLHF -> RLAIF
![image](https://hackmd.io/_uploads/SkeSIBOMC.png)

[參考論文_Constitutional AI: Harmlessness from AI Feedback](https://arxiv.org/abs/2212.08073)

[參考論文_RLAIF: Scaling Reinforcement Learning from Human Feedback with AI Feedback](https://arxiv.org/abs/2309.00267)

[參考論文_Self-Rewarding Language Models](https://arxiv.org/abs/2401.10020)

如果說取得人類回饋是麻煩的，那是不是可以由GPT-4之類的超強語言模型來代替人類回饋，幾篇參考論文就是採用這樣的作法。甚至我們還可以讓評估好不好的這個語言模型就直接是應答的語言模型。

### 增強式學習的難題
![image](https://hackmd.io/_uploads/HyaL_r_fR.png)

[參考論文_Llama 2: Open Foundation and Fine-Tuned Chat Models](https://arxiv.org/abs/2307.09288)

[參考論文_Training a Helpful and Harmless Assistant with Reinforcement Learning from Human Feedback](https://arxiv.org/abs/2204.05862)

一個回應好不好要從很多面向去看，問機器怎麼做炸彈，如果機器說不能教，那詢問的人來說就是不好的回應，但在安全性來說卻是好的。

在llama2論文中從安全性與助益性來評比回應。
### 增強式學習的難題
![image](https://hackmd.io/_uploads/S1ztOrufC.png)


一樣的問題，在不同的模型對於安全就有不同的考量。
### 增強式學習的難題
![image](https://hackmd.io/_uploads/ryPCOrdG0.png)

有時候難人類自己都無法決定的時候，模型怎麼決定，這也是增強式學習過程中會遇到的一個難題。

問機器要不要唸博班，問就是讀。
### 目前進度
![image](https://hackmd.io/_uploads/B1amYHufC.png)

