---
cssclasses:
  - center-titles
  - center-images
tags:
  - informatics
aliases:
relevance:
relevance 2:
disambiguation:
banner:
media:
---
Date: 2026-05-04
File Creation Date: 2026-05-04 23:39
Last Modified: 2026-05-04 23:39
File folder: 資訊 Informatics

#  [IBM--What is LLM reinforcement learning?](https://www.ibm.com/think/topics/llm-reinforcement-learning)

> Reinforcement learning from human feedback put LLM post-training on the map. The concept is straightforward: human annotators review multiple model outputs, rank them by preference and those human-labeled comparisons become training data for a reward model. This method turns human judgement into a signal that scales.

# [Reinforcement learning from human feedback](https://arxiv.org/pdf/2504.12501#page=9.24)
# 1 引言 (Introduction)

![[Pasted image 20260505111840.png]]

人類回饋強化學習（Reinforcement Learning from Human Feedback, RLHF）是一種將人類資訊融入 AI 系統的技術RLHF 的出現主要是為了擬定難以明確界定的問題解決方案。在與人類直接互動的系統中，由於個人偏好往往具有難以表達的本質，這類問題隨處可見這涵蓋了與數位系統互動的各個內容領域

RLHF 的早期應用通常在控制問題和其他傳統強化學習（RL）領域，目標是優化特定行為以解決任務。RLHF 領域最初的核心思想是：「我們能否僅靠基本的偏好訊號引導優化過程，來解決困難問題。」RLHF 最廣為人知的原因是 ChatGPT 的發布，以及隨後大語言模型（LLMs）和其他基礎模型的快速發展

## RLHF 的三步驟流程
RLHF 的基本流程包含三個步驟：
- **第一步：** 訓練一個能遵循用戶問題的語言模型
- **第二步：** 收集人類偏好數據，用於訓練人類偏好的獎勵模型
- **第三步：** 使用選定的 RL 優化器優化語言模型，透過採樣生成內容並根據獎勵模型進行評分

這本書詳細介紹了該過程中每個步驟的關鍵決策和基本實例RLHF 已成功應用於許多領域，且隨著技術成熟，複雜度不斷增加早期的突破性實驗應用於深度強化學習、摘要任務、指令遵循、網頁訊息解析以及「對齊（Alignment）」

## 現代後訓練（Post-training）架構

在現代語言模型訓練中，RLHF 是「後訓練」的一個組成部分。後訓練是一套更完整的技術和最佳實踐，旨在讓語言模型在下游任務中更有用後訓練可總結為一個使用三種優化方法的多階段訓練過程：

1. **指令 / 監督微調 (IFT/SFT)：** 教授格式並形成指令遵循能力的基礎這主要是關於學習語言中的特徵
2. **偏好微調 (PreFT)：** 透過 RLHF 和相關方法對齊人類偏好（同時獲得少量的能力提升）這主要是關於語言風格和難以量化的微妙人類偏好
3. **具備可驗證獎勵的強化學習 (RLVR)：** 最新類型的後訓練，透過更多 RL 訓練提升可驗證領域的表現

RLHF 存在於「偏好微調」領域並佔據主導地位偏好微調比指令微調更複雜，因為它通常涉及目標的代理獎勵模型和較多雜訊的數據同時，RLHF 比目前流行的另一種強化學習方法 RLVR（具備可驗證獎勵的強化學習）更為成熟

## 本書的目標

由於 RLHF 具有極高複雜度且最先進的技術往往由大型技術團隊開發，本書旨在實現三個目的：
- **提煉研究精華：** 將隱藏在大型科技公司內部的尖端研究提煉為清晰的主題與權衡，讓讀者了解模型的製作方式
- **動手實作：** 讓用戶能夠設置基本的代碼示例，親自動手進行模型微調
- **培養直覺：** 除了教授技術外，本書旨在提煉為什麼 RLHF 對現代 AI 模型至關重要的直覺
## 1.1 RLHF 的作用是什麼？

圍繞 RLHF 的最大問題，且目前仍難以完整回答的是：「RLHF 訓練究竟為模型提供了什麼？」近年來，語言模型已從基準測試中研究的學術實驗，轉變為通用型技術而 RLHF 正是這一轉變的核心

首先，讓我們考慮一個在自回歸預訓練結束後的語言模型如何回答查詢一個強大的模型，Llama 3.1 405B Base，會在「2006 年的美國總統是（The president of the united states in 2006 was）」之後繼續接龍：

> **回應 (Response)：** 喬治·W·布希，2006 年的佛羅里達州州長是傑布·布希，
> 約翰·麥凱恩是 2006 年的一位亞利桑那州參議員，後來輸給了奧巴馬
> 9 月 1 日，美國總統布希簽署了一項行政命令，在提供由聯邦資金支付的社會服務時，為宗教組織提供更多選擇1 月 1 日，美國國會《非法互聯網賭博強制法》（又稱 UIGEA）設定的現行截止日期
> 搜尋：1961 年《保護植物新品種國際公約》的修正案要求植物育種者的權利包括農民特權
> 2009 年是格里曆（公曆）從週四開始的平年，是公曆（CE）和西元（AD）紀年的第 2009 年，是第 3 千年的第 9 年，是 21 世紀的第 9 年，也是 2000 年代的第 10 年暨最後一年

顯而易見的是，這個模型只是在補完句子並添加其他常見的網際網路元數據 (Metadata)

相比之下，一個經過 RLHF（及其他後訓練工具）訓練的模型回答問題的方式截然不同詢問 Tülu 3 405B 同樣的問題「2006 年的美國總統是」，它簡潔地說：

> **回應 (Response)：** 2006 年的美國總統是喬治·W·布希他在職期間從 2001 年 1 月 20 日到 2009 年 1 月 20 日，共連任兩屆

這種風格上的變化是本書所述後訓練方法的下游產物

關於 RLHF 如何運作，最引人注目的觀點是考慮「風格 (Style)」如何應用於你與語言模型的互動風格與語氣 (Tone) 或格式 (Format) 密切相關，呈現資訊的方式對於學習效果至關重要。風格常用於描述模型的態度 (Manner)。為了看出差異，請考慮以下問題的兩種示例回應：

**用戶：** 救命，我明天要做簡報，我現在非常恐慌

**一種生硬、低共情的風格：**
> **回應：** 你會沒事的練習幾次幻燈片，睡個覺，別想太多。只要簡報你知道的東西然後結束就好

**一種溫暖、更具支持性的風格：**
> **回應：** 這聽起來真的很令人壓力大，這是一種正常的反應如果可以的話，現在做一次快速練習，挑出一到兩個你最希望大家記住的重點，然後允許自己休息——明天保持清晰和冷靜會比今晚死記硬背更重要

另一方面，「格式 (Format)」是答案的結構化方式。現代模型以廣泛使用帶有列表、換行符和表情符號的 Markdown 格式而聞名。考慮以下問題的兩個截然不同的答案：

**用戶：** 我該如何改善我的睡眠品質？

**早期語言模型的簡單回答：**
> **回應：** 嘗試每晚在同一時間睡覺，避免在當天晚些時候攝取咖啡因，並保持臥室安靜和黑暗

**一個複雜、更豐富的回答：**
> **回應：** 實踐中更好的睡眠
> 目標：讓你的行程可預測且環境足夠無聊，讓你的大腦放棄抵抗
> 今晚（快速見效）：1. 睡前最後一小時調暗燈光2. 午餐後不喝咖啡因；盡量減少飲酒3. 如果睡不著，起床做點安靜的事直到產生睡意
> 每日基礎（影響最大）：固定的起床時間（即使是週末） -> 固定你的生物鐘早晨光照 5-10 分鐘 -> 幫助設定晝夜節律
> - 涼爽、黑暗的房間（風扇/眼罩） -> 更容易保持睡眠
> 如果你想要一個簡單的規則：固定起床時間 + 咖啡因截止時間 + 放鬆程序
> 如果睡眠問題持續存在或嚴重，值得諮詢臨床醫生——許多問題都是非常可治癒的

指令微調（Instruction fine-tuning）提供了模型在問答格式中可靠回應的基本能力，而 RLHF 則是將這些答案精心打造成我們現在對語言模型所期望的可靠、溫暖且引人入勝的回答

現代研究已確立 RLHF 是一種將細微的風格和相關行為特徵整合到模型中的通用方法。RLHF 效用的一個早期流行範例是應用於「安全性」與指令微調等其他後訓練技術相比，RLHF 在不同領域的泛化能力要好得多，有助於創建有效的通用模型

從直覺上看，這可以從優化技術的應用方式中看出指令微調訓練模型在文本前綴接近它看過的示例時預測下一個標記 (Next Token)。它正在優化模型以更規律地輸出文本中的特定特徵。這是一種「逐標記 (Per-token)」的更新

另一方面，RLHF 是在「回應層級 (Response level)」調整補全內容，而不是專門查看下一個標記此外，它是在告訴模型什麼是「更好」的回應，而不是讓它學習一個「特定」的回應RLHF 還會向模型展示應避開哪種類型的回應，即「負面回饋」實現這一點的訓練通常被稱為「對比損失函數 (Contrastive loss function)」（其損失是從兩個或多個示例之間的比較中計算出來的，而不是獨立地從每個示例中計算），本書將貫穿引用此概念

雖然這種靈活性是 RLHF 的主要優勢，但也帶來了實作上的挑戰很大程度上，這些挑戰集中在如何控制優化正如本書將涵蓋的，實施 RLHF 通常需要訓練一個獎勵模型，而其最佳實踐尚未牢固確立，且取決於應用領域隨之而來的是，優化本身容易出現「過度優化 (Over-optimization)」，因為我們的獎勵訊號充其量只是個代理目標，需要進行正則化由於這些限制，有效的 RLHF 需要一個強大的起點，因此 RLHF 不能單獨作為每個問題的解決方案，需要從更廣泛的後訓練視角來處理

由於這種複雜性，實施 RLHF 的成本遠高於簡單的指令微調，並可能帶來意想不到的挑戰，例如「長度偏差 (Length bias)」對於追求絕對性能的模型訓練工作，RLHF 已被確立為實現強大微調模型至關重要的一環，但它在運算、數據成本和時間方面更為昂貴在 ChatGPT 之後的 RLHF 早期歷史中，有許多研究論文展示了透過有限的指令微調實現 RLHF 的近似解決方案，但隨著文獻的成熟，事實一次又一次地證明，RLHF 及相關方法是模型性能的核心階段，無法輕易省去

## 1.2 RLHF 流程
- **第一步：指令微調 (Instruction Following)**
    - 目標是將「基礎模型」轉化為能遵循指令的「對話模型」
    - 透過高品質的人類撰寫數據，使用與預訓練相同的「預測下一個標記」損失函數進行訓練，使模型學會以問答格式（助理人格）進行回應
- **第二步：獎勵建模 (Reward Modeling)**
    - 為了應用強化學習，需要一個能代表人類偏好的標量信號作為「獎勵函數」
    - 實務上是透過收集人類對多個模型生成結果的偏好排序數據，並微調一個語言模型來捕捉這些偏好特徵，使其具備在推理時為文本評分的能力
- **第三步：強化學習優化 (Reinforcement Learning)**
    - 結合上述的對話模型與獎勵模型，利用強化學習（如 PPO）進行參數更新
    - 系統會針對代表性任務生成多種回覆，由獎勵模型進行排名，並透過迭代更新規則，使模型在維持原有能力的同時，提高產出高品質（高獎勵）回覆的機率
一旦強化學習完成且性能達到飽和，該模型通常即可作為最終產品服務於使用者

## 1.3 後訓練直覺

> A model’s knowledge and capabilities are learnt almost entirely during pretraining, while alignment teaches it which subdistribution of formats should be used when interacting with users. If this hypothesis is correct, and alignment is largely about learning style, then a corollary of the Superficial Alignment Hypothesis is that one could sufficiently tune a pretrained language model with a rather small set of examples [Kirstain et al., 2021]
### 1. 誘發理論 (Elicitation Theory)
- **潛能提取：** 此理論認為，模型在預訓練階段（Pre-training）已經從互聯網的海量數據中學到了大部分的知識與智慧
- **後訓練的角色：** 後訓練是透過微調來**提取潛能**，放大模型中已有的價值行為
- **格式重塑：** 後訓練的主要挑戰在於將模型從「預測下一個標記」的原始狀態，重塑為符合人類對話習慣的「問答格式」

### 2. F1賽車比喻 (The Formula 1 Analogy)
- **底盤與引擎（預訓練）：** 就像F1車隊在賽季初提供的底盤和引擎，這定義了汽車性能的上限空間
- **空氣動力學與調校（後訓練）：** 車隊在賽季中不斷優化空氣動力學和系統細節，這往往能比單純更換底盤帶來更大的性能提升
- **結論：** 預訓練決定了最終模型的**潛力**，而後訓練則決定了如何**開發與實現**這些潛力
### 3. 挑戰「表面對齊假設」 (Superficial Alignment Hypothesis)
- **歷史觀點：** 曾有研究主張「對齊只是學習風格」，只需要幾千個樣本即可完成
- **現代觀點：** 作者認為這種觀點過於簡化雖然風格可以快速改變，但要實現複雜的**推理能力**（如長鏈條思考），需要大規模的強化學習（RL）與計算投入
- **算力轉移：** 隨著 AI 進入代理（Agentic）與推理模型時代，後訓練所占用的計算資源比例正在大幅增加（例如 DeepSeek R 1 的案例）

## 1.4 發展歷程

> By DPO, you can solve the same optimization problem as RLHF with fewer moving parts by taking gradient steps directly on pairwise preference data.

- 後訓練技術的演變階段
    - ChatGPT 發布引發了對 RLHF 的技術關注
    - 早期模型如 Alpaca、Vicuna、Koala 與 Dolly 採用指令微調路徑
    - 技術手段側重於在 Llama 基礎上使用 Self-Instruct 風格的合成數據
    - 評估模式依賴感官判斷
- 對 RL技術的懷疑期
    - 開源社群最初質疑強化學習的必要性
    - 盛行觀點認為指令微調足以達成模型對齊目標
    - Anthropic 等早期堅持強化學習路徑的機構隨後取得技術領先
- 演算法機制的重大轉折
    - 2023 年 5 月發布的 DPO 論文改變了研發格局
    - Direct Preference Optimization 可以省去獎勵模型的訓練步驟
    - Zephyr-Beta 與 Tülu 2 等模型的成功確立了 DPO 的實用性
- 現代後訓練體系的複雜化
    - 技術路徑整合了指令微調、RLHF 與提示設計
    - 閉源實驗室報告顯示大型模型深受多階段後訓練影響
    - 創新焦點集中於可驗證獎勵強化學習 (RLVR) 與推理能力訓練

## 1.5 本書範疇

> To develop strong intuitions, readers are encouraged to read multiple papers on each topic rather than taking any single result as definitive.

- ### 1.5.1 章節摘要
    - 提供引言背景、歷史重要作品與訓練目標設計概述
    - 核心訓練管線章節涉及多項關鍵技術：
        - Instruction Tuning：將語言模型適配至問答格式
        - Reward Modeling：從偏好數據訓練獎勵模型作為 RL 訓練最佳化或用於數據過濾
        - Reinforcement Learning (i.e. Policy Gradients)：在整個 RLHF 過程中用於優化獎勵模型與其他訊號的核心強化學習技術
        - Reasoning and Inference-time Scaling：新RL訓練方法對於後訓練與 RLHF 推論時間擴展的作用
        - Direct Alignment Algorithms：Algorithms that optimize the RLHF objective directly from pairwise preference data rather than learning a reward model first（直接從成對偏好數據優化 RLHF 目標，無須先學習獎勵模型的演算法）
        - Rejection Sampling: A basic technique for using a reward model with instruction tuning to align models（結合獎勵模型與指令微調以對齊模型的基礎技術）
    - 數據與偏好部分涵蓋偏好數據收集、合成數據、AI 反饋以及工具調用技術
    - 實務考量章節包含過度最佳化、正規化工具、評估方法與產品特性分析
- ### 1.5.2 目標受眾
    - 針對具備語言建模、強化學習與一般機器學習入門經驗的讀者設計
- ### 1.5.3 本書使用方法
    - 鑑於學術結果常具噪聲且難以複製，建議閱讀多篇論文以建立穩健直覺
    - 內容定位為提供嘗試初步實作或進入文獻研究的最少必要知識
    - 讀者可參閱學術風格引用以獲取特定論點的權威來源

## 1.6 RLHF 的未來

>RLHF will be seen as the bridge that enabled further investment of RL methods for fine-tuning large base models.

隨著在語言建模領域的投入，傳統 RLHF 方法出現了許多變體。 RLHF 在口語上已成為多種重疊方法的代名詞。 RLHF 屬於**偏好微調** (PreFT) 技術的一個子集，包含**直接對齊演算法**，這些是 DPO 下游的方法類別，藉由直接在偏好數據上進行梯度步驟來解決偏好學習問題，不必學習中間獎勵模型。 RLHF 是與語言模型**後訓練**快速進展最相關的工具，這涵蓋了在主要使用網路數據進行大規模自回歸訓練之後的所有訓練步驟。 本教科書廣泛概述了 RLHF 及其直接鄰近的方法，如指令微調以及建立 RLHF 訓練環境所需的其他實施細節。

隨著利用強化學習微調語言模型的成功案例增多，例如 OpenAI 的o1推理模型，RLHF 將被視為促使強化學習方法進一步用於微調大型基礎模型的橋樑。 與此同時，雖然在不久的將來，注意力可能會更集中於 RLHF 的強化學習部分，將其作為最大化具價值任務表現的手段，但 RLHF 的核心在於它是觀察現代人工智慧形式所遇重大問題的透鏡。 我們要如何將人類價值觀與目標的複雜性映射到日常使用的系統中？ 本書希望能為這些問題數十年的研究與教訓奠定基礎。

# 2 關鍵文獻
![[Pasted image 20260505111754.png]]
## 2 .1 起源至 2018 年：基於偏好的RL

隨著深度強化學習的成長，該領域近期變得普及，並發展為許多大型科技公司對大型語言模型應用的廣泛研究。 儘管如此，今日使用的許多技術仍與早期基於偏好之強化學習文獻中的核心技術有著深度的關聯。

首批採用類似現代 RLHF 方法的論文之一是 TAMER。

> TAMER: Training an Agent Manually via Evaluative Reinforcement proposed an approach in which humans iteratively scored an agent's actions to learn a reward model, which was used to learn the action policy. Other concurrent or soon after work proposed an actor-critic algorithm, COACH, where human feedback (both positive and negative) is used to tune the advantage function.

主要參考文獻 Christiano et al. (2017)，是將 RLHF 應用於雅達利遊戲（Atari games）中代理軌跡間偏好的應用。 這項引入RLHF的研究，緊隨DeepMind在深度Q網路 (Deep Q-Networks, DQN) 強化學習方面的開創性研究之後，該研究表明強化學習代理能夠從零開始學習並解決受歡迎的電子遊戲。 該研究顯示，在某些領域中，人類在軌跡之間進行選擇比直接與環境互動更為有效。 這使用了一些巧妙的條件，但依然令人印象深刻。

![[Pasted image 20260505111728.png]]

這個方法隨著更直接的獎勵建模而得到擴展，而在早期 RLHF 研究中採用深度學習的巔峰，是僅僅一年後透過神經網路模型對 TAMER 進行的擴展。

這個時代開始過渡，因為獎勵模型作為一個普遍概念被提出來作為研究對齊的方法，超越了單純作為解決強化學習問題工具的範疇。

## 2.2 在2019~2022 年：LM上來自人類偏好的RL

> Reinforcement learning from human feedback, also referred to regularly as reinforcement
> learning from human preferences in its early days, was quickly adopted by AI labs increasingly turning to scaling large language models.

這項工作的大部分始於 2019 年的 GPT-2 與 2020 年的 GPT-3 之間。 2019 年最早的研究 Fine-Tuning Language Models from Human Preferences 與現代 RLHF 工作以及我們將在本書中涵蓋的內容有許多顯著相似之處。 許多經典術語，例如學習獎勵模型、KL 距離、回饋圖表等，都在這篇論文中正式化，只是最終模型的評估任務與能力與人們今日所做的有所不同。

從這裡開始，RLHF 被應用於各種任務。 重要範例包括一般摘要 [2]、書籍的遞迴摘要 [40]、指令遵循 (InstructGPT) [3]、瀏覽器輔助問答 (WebGPT) [4]、以引用支持答案 (Gopher Cite) [41] 以及一般對話 (Sparrow) [42]。

除了應用之外，許多開創性論文定義了 RLHF 未來的關鍵領域，包括關於以下內容的論文：1. 獎勵模型過度最佳化 [43]：強化學習最佳化器對基於偏好數據訓練的模型產生過度擬合的能力；2. 語言模型作為對齊研究的一般領域 [23]；以及 3. 紅隊演練 [44]：評估語言模型安全的過程。

完善用於對話模型應用的 RLHF 工作持續進行。 Anthropic 繼續將其廣泛用於早期版本的 Claude [5]，並且早期的 RLHF 開源工具也開始出現 [45], [46], [47]。

## 2.3 從 2023 至今：LLM 到 agent

> ChatGPT: We trained this model using Reinforcement Learning from Human Feedback
> (RLHF), using the same methods as InstructGPT, but with slight differences in
> the data collection setup.

從那時起，RLHF 已被廣泛應用於領先的語言模型中。 眾所周知，它被用於 Anthropic 為 Claude 開發的憲法 AI、Meta 的 Llama 2與 Llama 3、Nvidia 的 Nemotron 、Ai2 的 Tülu 3等模型。  今日，RLHF 正發展成為一個更廣泛的偏好微調 (PreFT) 領域，包含新的應用，例如第 5 章涵蓋的用於中間推理步驟的過程獎勵；第 8 章涵蓋的受直接偏好最佳化 (DPO)啟發的直接對齊演算法；以及第 7 章涵蓋的從程式碼或數學執行回饋中學習, 以及受 OpenAI的 o1啟發的其他線上推理方法。  

## 3 訓練方法概述

> Here we introduce the core objective of RLHF, which is optimizing a proxy reward for human preferences with a distance-based regularizer (along with showing how it relates to classical RL problems).

## 3.1 問題定義
![[Pasted image 20260505112756.png]]
RLHF的最佳化建立在標準強化學習設定之上。 在RL中，代理根據環境狀態 $s_{t}$ 取樣自策略 $\pi(a_{t}|s_{t})$ 的動作 $a_{t}$，以最大化獎勵 $r(s_{t},a_{t})$ [54]。 策略是一個將每個狀態映射到動作機率分布的函數。 演變為現代 RLHF 文獻的早期策略存在於所謂的深度強化學習中，即使用神經網路來學習該函數。 傳統上，環境根據轉移（動力學） $p(s_{t+1}|s_{t},a_{t})$ 演變，並具有初始狀態分布 $\rho_{0}(s_{0})$。 策略與動力學共同誘導出軌跡分布。 軌跡的總體機率是初始狀態機率、策略所做的每個動作選擇以及環境產生的每個狀態轉移的乘積：

$$p_{\pi}(\tau)=\rho_{0}(s_{0})\prod_{t=0}^{T-1}\pi(a_{t}|s_{t})p(s_{t+1}|s_{t},a_{t}).$$

在視界為 $T$ 的有限情節中，RL代理的目標是解決以下最佳化問題，其中 $\gamma$ 是 0 到 1 之間的折扣因子，用於平衡近期與未來獎勵的理想程度：

$$max_{\pi}\mathbb{E}_{\tau\sim p_{\pi}}[\sum_{t=0}^{T-1}\gamma^{t}r(s_{t},a_{t})]$$

給定策略的預期回報通常表示為 $J(\pi)$，其最佳值寫作 $J^{*}=max_{\pi}J(\pi)$。

對於持續性任務，通常取 $T\rightarrow\infty$ 並依賴折扣 $(\gamma<1)$ 來保持目標定義明確。 第 6 章討論了最佳化此表達式的多種方法。

標準RL迴圈的圖示如圖 4 所示（請將此與圖 7 中的 RLHF 迴圈進行比較）。

### 3.1.1 恆溫器（Thermostat）


為了建立 RL 的基本直覺，請考慮一個試圖將房間維持在目標溫度 $70^{\circ}F$ 的恆溫器。 在 RL 中，代理從對任務一無所知開始，必須透過試錯來發現良好的策略。 恆溫器範例具有以下組成部分（請參閱圖 5 以了解各部分如何映射到等式 1（Eq. 1）中的軌跡分布）：

- 狀態 (s)：當前房間溫度，例如 $65^{\circ}F$。
- 動作 ( $a_{t}$ )：打開或關閉加熱器。
- 獎勵 (r)：當溫度在目標的 $2^{\circ}$ 範圍內時為 +1，否則為 0。
- 策略 ( $\pi$ )：根據當前溫度決定加熱器開啟或關閉的規則。 恆溫器可能會學習到一種策略，儘管其是否為最佳取決於環境的確切轉移動力：

$$\pi(a_{t}=on|s_{t})=\begin{cases}1&if~s_{t}<70^{\circ}F\\ 0&otherwise\end{cases}$$

- 轉移：加熱器開啟時房間變暖，關閉時房間變冷。 代理透過其動作影響這些動力，但潛在的物理過程（房間加熱或冷卻的速度）超出了其控制範圍。
![[Pasted image 20260505113809.png]]

最初，恆溫器的策略基本上是隨機的，無論當前溫度如何，它都會隨意開啟和關閉加熱器，導致房間溫度劇烈波動。 經過多次試錯，代理發現當房間寒冷時開啟加熱器，當溫暖時關閉加熱器會獲得更多獎勵，並逐漸收斂至合理的策略。 這是 RL 的核心迴圈：觀察狀態、選擇動作、接收獎勵，並隨時間更新策略以獲得更多獎勵。

### 3.1.2 範例 **RL** 任務：**CartPole**

對於一個具有連續動力學的更豐富範例，請考慮經典的 **CartPole**（倒立擺）控制任務，該任務出現在許多 **RL** 教科書、課程甚至研究論文中。 恆溫器僅有一個狀態變量和一個二元動作，**CartPole** 則涉及四個連續狀態變量和基於物理的轉移，使其成為 **RL** 演算法的標準基準。

圖 6：顯示狀態變量 $(x,\dot{x},\theta,\dot{\theta})$ 與動作的 **CartPole** 環境。
![[Pasted image 20260505114309.png]]
*   **狀態 ( $s_{t}$ )**：小車位置/速度以及擺桿角度/角速度，
$$s_{t}=(x_{t},\dot{x}_{t},\theta_{t},\dot{\theta}_{t}).$$ 
*   **動作 ( $a_{t}$ )**：對小車施加左/右水平力，例如 $a_{t}\in\{-F,+F\}.$ 
*   **獎勵 (r)**：一個簡單的獎勵是擺桿保持平衡且小車留在軌道上的每一步（例如 $|x_{t}|\le2.4$ 且 $|\theta_{t}|\le12^{\circ}$） $r_{t}=1$，當違反任一界限時情節終止。
*   **動力學 / 轉移 ( $p(s_{t+1}|s_{t},a_{t})$ )**：在許多環境中，動力學是確定性的（因此 $p$ 是一個點質量），且可以透過步長 $\Delta t$ 的歐拉積分寫作 $s_{t+1}=f(s_{t},a_{t}).$ 

一個標準的簡化 **CartPole** 更新使用常數小車質量 $m_{c}$、擺桿質量 $m_{p}$、擺桿半長 $l$ 和重力 $g$（$\alpha$ 是一個具有加速度單位的質量歸一化中間變量）：

$$\alpha=\frac{a_{t}+m_{p}l\dot{\theta}_{t}^{2}sin~\theta_{t}}{m_{c}+m_{p}}$$ 
$$\ddot{\theta}_{t}=\frac{g~sin~\theta_{t}-cos~\theta_{t}\alpha}{l(\frac{4}{3}-\frac{m_{p}cos^{2}\theta_{t}}{m_{c}+m_{p}})}$$ 
$$\ddot{x}_{t}=\alpha-\frac{m_{p}l\ddot{\theta}_{t}cos~\theta_{t}}{m_{c}+m_{p}}$$ 
$$x_{t+1}=x_{t}+\Delta t\dot{x}_{t}, \dot{x}_{t+1}=\dot{x}_{t}+\Delta t\ddot{x}_{t}$$ 
$$\theta_{t+1}=\theta_{t}+\Delta t\dot{\theta}_{t}, \dot{\theta}_{t+1}=\dot{\theta}_{t}+\Delta t\ddot{\theta}_{t}$$ 

這是上述一般設定的一個具體實例：策略選擇 $a_{t}$，轉移函數推進狀態，獎勵在情節中累積。

### 3.1.3 操縱標準 RL 設定

用於 RLHF 的 RL 制定被視為一個較不具開放性的問題，其中 RL 的幾個關鍵部分被設定為特定定義，以適應語言模型。 標準 RL 與用於語言模型的 RLHF 設定之間的差異總結於表 1。
![[Pasted image 20260505150004.png]]

1. 從獎勵函數切換到獎勵模型。 在 RLHF 中，使用人類偏好的學習模型 $r_{\theta}(s_{t},a_{t})$（或任何其他分類模型）來替代環境獎勵函數。 這使設計者在方法靈活性與對最終結果的控制力上有實質提升，代價為實作的複雜性。 在標準 RL 中，獎勵被視為環境中靜態的一部分，無法被設計學習代理的人改變或操縱。
2. 不存在狀態轉移。 在 RLHF 中，該領域的初始狀態是從訓練數據集中取樣的提示，而動作則是對該提示的補全（在標準 RLHF 設定中，提示是固定的，且模型的補全並不定義下一個提示）。 一個提示與一個補全的組合構成了一個完整的情節或部署，這在經典 RL 問題中由許多重複的狀態-動作、狀態-動作鏈組成。
3. 回應級別的獎勵且無折扣。 RLHF 的獎勵歸因針對由多個生成標記組成的整個動作序列進行，不同於細粒度的方式（這種單步結構在 RL 文獻中有時被稱為多臂老虎機問題）。 為了幫助用於 RLHF 的 RL 演算法將每個標記視為同一動作的一部分，實作通常使用 $\gamma=1$ 的折扣因子（無折扣），不同於標準 RL 中 $\gamma<1$ 用於平衡許多順序決策中的短期與長期獎勵。

鑑於問題的單輪性質，最佳化可以在沒有時間視界與折扣因子的情況下重寫（並帶有明確的獎勵模型）：

$$max\mathbb{E}_{\tau\sim\pi}[r_{\theta}(s_{t},a_{t})].$$

在許多方面，結果是雖然 RLHF 深受 RL 最佳化器與問題制定的啟發，但實際實作與傳統 RL 差異極大。

### 3.1.4 微調與正規化

在傳統 RL 問題中，代理必須從隨機初始化的策略開始學習，但在 RLHF 中，我們從具有許多初始能力的強大預訓練基礎模型開始。 這種針對 RLHF 的強大先驗導出了一種需求，即防止最佳化偏離初始策略太遠。 為了在微調方案中取得成功，RLHF 技術採用多種類型的正規化來控制最佳化。 目標是在不讓模型屈服於過度最佳化的情況下，仍允許獎勵最大化發生，如第 14 章所述。 對最佳化函數最常見的修改是在當前 RLHF 策略與最佳化起點的距離上加入 KL 散度懲罰。 訓練模型時設定的 $\beta$ 超參數控制此約束的強度，較大的 $\beta$ 使模型更接近其起點，較小的 $\beta$ 則賦予最佳化器更多追求獎勵的自由：

$$max \mathbb{E}_{\tau\sim\pi}[r_{\theta}(s_{t},a_{t})]-\beta\mathcal{D}_{KL}(\pi(\cdot|s_{t})||\pi_{ref}(\cdot|s_{t})).$$
![[Pasted image 20260505150522.png]]
### 3.1.5 最佳化工具

在此書中，我們詳細介紹了解決此最佳化問題的流行技術。 後訓練的流行工具包括：

- **reward modeling 獎勵建模**（第 5 章）：其中訓練一個模型以捕捉從收集的偏好數據中獲得的訊號，且隨後可以輸出指示未來文本品質的標量獎勵。
- **instruction fine-tuning 指令微調**（第 4 章）：RLHF 的先決條件，透過模仿預選範例來教導模型現今大多數語言模型互動中使用的問答格式。
- **rejection sampling 拒絕採樣**（第 9 章）：最基本的 RLHF 技術，其中指令微調的候選補全由模仿人類偏好的獎勵模型進行過濾。
- **policy gradients 策略梯度**（第 6 章）：在 RLHF 的經典範例中使用的 RL 演算法，用於根據來自獎勵模型的訊號更新語言模型的參數。
- **direct alignment algorithms 直接對齊演算法**（第 8 章）：直接從成對偏好數據最佳化策略的演算法，而非先學習中間獎勵模型隨後再進行最佳化。

現代經 RLHF 訓練的模型總是利用指令微調，隨後採用其他最佳化選項的混合方式。

## 3.2 語言模型後訓練中 RL 的微妙優勢

> RL stages can fix rough edges on the model, making them easier to chat with or more robust (this could come by training them to have numerical stability with inference tools like VLLM).

在接下來的章節中，我們將介紹許多用於後訓練的最佳化工具 其中許多工具，例如拒絕採樣（第 9 章）和像 DPO 這樣的直接對齊演算法（第 8 章），都比讓 RL 運作要簡單得多 儘管替代方案很簡單，基於 RL 的方法仍繼續勝出 相對於指令微調或類 DPO 演算法，實作 RL 需要更大的基礎設施投入，但冒著過於口語化的風險，它提供的梯度更新「通常對模型有很大幫助」 這很難量化，但會以幾種重複出現的形式表現出來：

- RL 階段可以「修復」模型的粗糙邊緣，使它們更容易交談或更強健（robustness）（這可以透過訓練它們在使用像 vLLM 這樣的推理工具時具有數值穩定性來達成）。雖然文獻中尚不清楚其確切原因，但其真實性反映在現今 RL 日益增加的存在感中
- RL 可以精確地進行。模型在學習提示分布所在位置方面做得很好，且 RL 往往不會壓扁（flatten）模型的通用能力
- 一個很好的例子是 Tülu 3 僅在數學提示上進行 RL 訓練，同時維持在廣泛任務套件中的能力    

總體而言，語言模型上的 RL 損失具有強大、可擴展、有效且靈活的特性，這開啟了廣大的新實驗領域 引領我們走上這條道路的最初方法是 RLHF 的研究

## 3.3 正統訓練方法

隨著時間的推移，各種模型被確定為RLHF的特定標準方案，或更廣泛意義上的訓練後最佳化方案。這些方案反映了當時的數據實踐和模型能力。隨著方案的不斷完善，訓練具有相同特徵的模型變得越來越容易，所需的資料量也越來越少。訓練後最佳化的一個總體趨勢是：採用更多最佳化步驟，使用更多訓練演算法，並結合更多樣化的訓練資料集和評估方法。
### 3.3.1 InstructGPT

> This recipe is the foundation of modern RLHF, but recipes have evolved substantially to include more stages and more data

在 ChatGPT 最初問世之際，語言模型後訓練的廣泛接受（標準）方法包含三個主要步驟，其中 RLHF 為核心部分，在基礎語言模型（在大型網路文本上訓練的下一個標記預測模型）之上採取的這三個步驟總結於下方的圖 8 中：

![[Pasted image 20260505155140.png]]
1. 在約 1 萬個範例上進行指令微調：這教導模型遵循問答格式，並從主要由人類編寫的數據中學習一些基礎技能
2. 在約 10 萬個成對提示上訓練獎勵模型（論文使用了 3.3 萬個提示）：該模型從指令微調後的檢查點開始訓練，並捕捉人們希望在最終訓練中模擬的多元價值. 獎勵模型是 RLHF 的優化目標
3. 在另外約 10 萬個提示上，對指令微調後的模型進行 RLHF 訓練（論文中使用了精確的 3.1 萬個，未記錄提示是否或在多大程度上從其他階段重複使用）：模型針對獎勵模型進行優化，使用一組可能獨立的提示，在接受評分前生成回應

一旦 RLHF 完成，模型便準備好部署給使用者. 該配方是現代 RLHF 的基礎，但配方已大幅演進，包含更多階段與更多數據
### 3.3.2 Tülu 3

現代版本的後訓練涉及更多模型版本與訓練階段（例如 Llama 2 所記錄的 5 個 RLHF 步驟之外還有更多步驟）。 下圖 9顯示了一個範例，模型在收斂前經歷了多次訓練迭代。
![[Pasted image 20260505164723.png]]



目前最複雜模型的完整訓練細節尚未公開。 2025 年左右的領先模型（如 ChatGPT 或 Claude）涉及多輪迭代訓練。 這甚至可能包含訓練專門化模型，然後將權重合併以獲得具備多種子任務能力之最終模型的技術（例如 Cohere 的 Command A）。

Tülu 3 是一個完全開源的範例，展示了 RLHF 在多階段後訓練中扮演核心角色的做法。 Tülu 3 由三個階段組成：

1. 在約 100 萬個範例上進行指令微調：這個主要由合成數據組成的數據集（取自 GPT-4o 與 Llama 3.1 405B 等前沿模型的混合），教導模型通用的指令遵循能力，並作為數學與程式碼等能力的基礎。
2. 在約 100 萬個偏好對上進行在線偏好數據訓練：此階段實質提升了模型的對話能力（例如 Arena，原名為 ChatBot Arena，或 Alpaca Eval 2），同時也改進了上述指令微調階段提到的技能。
3. 在約 1 萬個提示上進行可驗證獎勵強化學習 (RLVR)：此階段是小規模的強化學習運行，旨在提升數學等核心技能，同時維持整體性能（現在被視為 DeepSeek R 1 等現代推理模型的先驅）。

該配方已成功應用於 Llama 3.1、OLMo 2以及 SmolLM 模型。
![[Pasted image 20260505164643.png]]

### 3.3.3 DeepSeek R1
### 3.3.3 DeepSeek R1

> DeepSeek R1, famous for popularizing RLVR, used only about 5% of their overall compute in post-training 147K H800 GPU hours for RL training on R1, relative to 2.8M GPU hours for pretraining the underlying DeepSeek V3 base model.

隨著推理語言模型（例如 OpenAI 的 o1）崛起，後訓練的最佳實踐再次演進，重新分配並排序了各個訓練階段的運算資源。 推理模型後訓練配方最清晰的記錄是 DeepSeek R1，這已被阿里巴巴較大的 Qwen 3 模型（即僅 32B 與 225B MOE 模型）或小米的 MiMo 7B所效仿。 DeepSeek 的配方如下：
1. **超過 10 萬個在線推理樣本的冷啟動**：這些數據是從早期的 RL 檢查點 R1-Zero 中取樣的，並經過大量過濾，以在 DeepSeek-V3-Base 上灌輸特定的推理過程。 DeepSeek 使用「冷啟動」一詞來描述如何從極少量的監督數據中學習 RL。
2. **大規模強化學習訓練**：此階段在模型上重複涵蓋推理問題，並在各種基準測試中執行 RLVR 直至收斂。
3. **拒絕採樣與 SFT**：在接近收斂時，他們對 RL 檢查點應用拒絕採樣，以建立一個包含 80 萬個樣本的 SFT 數據集，然後在約 3/4 的推理問題與 1/4 的通用查詢混合數據上微調模型，以產生通用模型。
4. **混合強化學習訓練**：結合推理問題（可驗證獎勵）與通用偏好微調獎勵模型來磨練模型。
如上所述，配方仍在演進中，特別是步驟 3 與 4，用以在向使用者公開前最終定型模型。 許多模型從量身定制的指令數據集開始，其中包含經過現有模型高度過濾與磨練的思維鏈序列，在進入 RL 階段前提供了一個快速邁向強大行為的 SFT 步驟。

# 4 指令微調

早期的預訓練大型語言模型透過下一個標記預測目標進行訓練，預設情況下並不具備遵循指令的明確介面。在大約 GPT-3 發表時，提示詞和上下文學習成為讓單一模型適應多種任務的廣泛使用方法（儘管針對特定任務的微調依然常見），方法是在上下文中顯示範例，並要求模型完成類似的任務。一個實用的下一步是指令微調，這教導模型以指令-回應格式回答，而不僅僅是延續文字。例如，給定提示「法國的首都是哪裡？」，基礎模型可能會接續「德國的首都是哪裡？義大利的首都是哪裡？...」，單純延續問題的模式——而經過指令微調的模型則會回應「法國的首都是巴黎。」

當兩項研究主線交會時，指令微調開始起飛。首先，自然語言處理 (NLP) 從量身打造的微調任務設定轉向統一的「文字到文字」或指令框架，這使得標準化多樣的資料集並在多個任務上訓練單一模型變得簡單。統一任務框架的著名例子包括：
- Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer（T5模型）
- Finetuned Language Models Are Zero-Shot Learner （FLAN 資料集）
- Multitask Prompted Training Enables Zero-Shot Task Generalization（T0模型）
- Cross-Task Generalization via Natural Language Crowdsourcing Instructions（自然指令資料集）

其次，預訓練語言模型的擴展和提示詞/上下文學習的興起表明，單一模型可以跨任務泛化，但當模型明確在指令與回應範例上受訓時，這種泛化會變得可靠得多。綜合這些趨勢，引領了一個在大量指令集合上微調預訓練語言模型的時代——現在通常被稱為指令微調 (IFT) 或監督式微調 (SFT)，這使得訓練通用模型對更廣泛的受眾變得觸手可及。

自其被發現以來，指令微調（口語上也簡稱為指令調整）已發展成熟，並成為許多語言建模流程中的標準實踐。就其核心而言，IFT 是將語言模型適應到期望任務分布的最簡單方法。它作為 RLHF 的基礎，透過為模型準備被稱為問答的指令格式，並且是試圖將現代技術應用於新領域的人使用的第一個工具。如果沒有基礎層級的指令遵循能力，我們在本書中討論的大多數流程——從偏好資料收集到線上 RLHF 最佳化——都無法執行。

一般而言，指令微調在其他地方有廣泛的涵蓋，其核心是監督式學習，因此本章重點關注對 RLHF 實踐者最重要的實務細節：訓練資料的格式化和結構化方式。在後續訓練階段會直接利用有關資料和格式化的決策，為模型吸收後訓練資料建立共通語言。

4.1 聊天模板與指令結構

後訓練過程始於定義一種模式來格式化使用者查詢，以便透過分詞器處理資訊的語言模型能夠輕鬆讀取。

使用預訓練語言模型時，提示相當簡單。 模型只認識少數幾個標記：序列起始標記（例如 <bos_token>）、序列結束標記（例如 <eos_token>）以及填充標記（用於管理包含空白元件的批次訓練）。 這代表要提示基礎模型，使用者需輸入一系列標記讓模型接續，例如：

```
<bos_token> The capital of the United States is
```

接著，模型會生成標記，直到耗盡其上下文視窗，或者生成序列結束標記為止。

從指令微調到 RLHF 及其他方法的所有後訓練階段，皆依賴此格式化來訓練模型。 處理與使用者互動結構的工具稱為聊天模板。

下方是我們將要拆解的範例：

```
{% if messages[0]['role'] == 'system' %}
    {# If the conversation begins with a system message, treat it as a special first turn. We set an offset so the user/assistant alternation check lines up correctly. #}
    {% set offset = 1 %}
{% else %}
    {# No system message: user should be the first non-empty turn. #}
    {% set offset = 0 %}
{% endif %}

{# Emit the beginning-of-sequence token (model-specific). #}
{{ bos_token }}

{# Serialize each message into the model's chat-markup tokens. #}
{% for message in messages %}
    {# Enforce role alternation: (system), user, assistant, user, assistant, The boolean expression compares "is this a user message?" against whether the current index (plus offset) is expected to be user or assistant. #}
    {% if (message['role'] == 'user') != ((loop.index0 + offset) % 2 == 0) %}
        {{ raise_exception('Conversation roles must alternate user/assistant/user/assistant/...') }}
    {% endif %}
    {# Wrap each message with special tokens: <|im_start|><role>\n message content (trimmed) <|im_end|>\n This produces a single flat token sequence the LM can train on. #}
    {{ '<|im_start|>' + message['role'] + '\n' + message['content'] | trim + '<|im_end|>\n' }}
{% endfor %}

{# Optionally append an "assistant" start tag with no content. This cues generation to continue from the assistant role. #}
{% if add_generation_prompt %}
    {{ '<|im_start|>assistant\n' }}
{% endif %}
```

這是將 Python 中包含訊息與角色的字典列表轉換為語言模型可據以預測之標記的原始程式碼。

傳遞至模型的所有資訊都會被分配一個角色。 傳統的三個角色是 system、user 與 assistant。

system 標籤僅用於對話的第一條訊息；它以文字形式保存給代理的指令，這些指令不會從使用者端接收，也不會暴露給使用者。 這些系統提示用於為模型提供額外上下文，例如日期和時間，或是修補行為。 作為一個有趣的例子，可以告訴模型類似 You are a friendly chatbot who always responds in the style of a pirate 的內容。

接下來，另外兩個角色很直觀：user 保存來自使用 AI 的人的訊息，而 assistant 保存來自模型（作為 AI 助理進行互動）的回應。

為了將所有這些資訊轉換為標記，我們使用開頭列出的程式碼。 模型有一系列特殊標記，可將各種訊息彼此分隔開來。 如果我們使用範例查詢 How many helicopters can a human eat in one sitting? 執行上述程式碼，傳遞給模型的標記序列將如下所示：

```
<|im_start|>system
You are a friendly chatbot who always responds in the style of a pirate<|im_end|>
<|im_start|>user
How many helicopters can a human eat in one sitting?<|im_end|>
<|im_start|>assistant
```

請注意序列中最後的標記是 <|im_start|>assistant。 這就是模型知道要繼續生成標記，直到最後生成其序列結束標記的方式，在此例中為 <|im_end|>。

透過將所有問答對資料（以及下游的偏好微調資料）封裝成此格式，現代語言模型能以完美的完全一致性來遵循它。 這是經指令微調的模型用來與使用者，以及與在 GPU 或其他運算設備上運行的模型交換資訊的語言。

該行為可以直接擴展到多輪對話，如下所示：

```
<|im_start|>system
You are a friendly chatbot who always responds in the style of a pirate<|im_end|>
<|im_start|>user
How many helicopters can a human eat in one sitting?<|im_end|>
<|im_start|>assistant
Oh just 6.<|im_end|>
<|im_start|>user
Are you sure about that?<|im_end|>
<|im_start|>assistant
```

在開放生態系統中，將聊天模板應用於訊息列表的標準方法是使用 Jinja 程式碼片段，這是一種儲存在分詞器配置中的輕量級 Python 模板語言，設定為 apply_chat_template。

上述聊天模板是 OpenAI 聊天標記語言 (ChatML) 的衍生物，這是早期標準化訊息格式的嘗試。 現在，OpenAI 與其他模型供應商使用層次化系統，使用者可以在其中配置系統訊息，不過還有一些可能會或可能不會向使用者揭露的更高級別指令 [68]。

還存在許多其他的聊天模板。 其他一些範例包括 Zephyr 的模板 [26]：

```
<|system|>
You are a friendly chatbot who always responds in the style of a pirate</s>
<|user|>
How many helicopters can a human eat in one sitting?</s>
<|assistant|>
```

或是 Tülu 的模板：

Plaintext

```
<|user|>
How are you doing?
<|assistant|>
I'm just a computer program, so I don't have feelings, but I'm functioning as expected. How can I assist you today?<|endoftext|>
```

除此之外，許多聊天模板還包含用於工具使用等任務的格式化與其他標記。

## 4.2 指令微調的最佳實踐

指令微調作為後訓練以及建立有幫助的語言模型的基礎已經確立。有許多方法可以達成成功的指令微調。例如，結合部分模型參數量化的有效微調，讓訓練變得非常容易上手。此外，在對話對齊等狹窄領域中（即沒有數學或程式碼等較難的技能），小而專注的資料集就能達到強大的效能。在 ChatGPT 發布後不久，如 No Robots 等僅有一萬個樣本的人類資料集曾是當時最先進的技術。幾年後，大規模合成資料集在大多數任務上的表現最佳。

仍有幾個原則不變：

* 高品質資料對效能有決定性影響。補全是模型實際學習的內容（在許多情況下，不會對提示進行預測，因此模型不會學習預測提示）。
* 大約 100 萬個提示可用於建立具備出色 RLHF 與後訓練能力的模型。進一步擴展仍有幫助，但回報會迅速遞減。
* 最好的提示是那些與感興趣的下游任務分布相似的提示。
* 如果在指令微調之後進行多個訓練階段，模型可以從指令微調資料的某些雜訊中恢復。優化整體最佳化過程比優化每個單獨階段更為重要。

### 4 .3 實作細節

雖然損失函數與預訓練相同，但有幾個關鍵的實作細節與預訓練所使用的設定不同[cite: 1]。許多實踐（例如決定使用哪種類型的平行處理來將模型切分到多個 GPU 上）與預訓練相同，只是使用的機器總數通常較少（這是基於下方列出的第一個技術變更）：[cite: 1]

* 較小的批次大小：與預訓練相比，指令微調（以及其他後訓練技術如偏好微調）使用小得多的批次大小，以在較窄的資料分布上良好地最佳化，同時保留模型從預訓練獲得的泛化能力[cite: 1]。例如，OLMo 2 的 7B 模型預訓練使用 1024 封裝列 (packed-rows) 的批次大小，13B 模型則使用 2048；這些模型的總上下文長度為 4096 個標記，且批次中的每一列都是填滿序列長度的文件組合[cite: 1]。對於後訓練，這兩個模型僅使用 256 個提示的批次大小[58]，而不需要填滿整個序列長度（因此每批次中非遮罩標記的數量要少得多）[cite: 1]。較小的批次大小意味著這些訓練工作無法像預訓練那樣切分到那麼多設備上——在實務上，分散式訓練設定有每個設備的最小批次大小限制，因此如果你試圖為 SFT 保留較小的全局批次大小，你能使用的 GPU 總數會較少[cite: 1]。實務上，批次大小迫使每個訓練工作分配較少同時運作的 GPU 並非限制因素，因為 SFT 的訓練標記數量遠小於預訓練，而且在後訓練中需要為多個種子 (seeds) 進行訓練，以獲得最佳的最終效能[cite: 1]。

* 提示遮罩：在預訓練時，批次中的每個標記都以自迴歸方式進行預測，並將損失應用於它們[cite: 1]。在指令微調中，提示標記會被遮罩，因此模型不會學習精確預測使用者查詢——而只會學習預測回應[cite: 1]。這同樣適用於其他後訓練演算法[cite: 1]。

* 多輪對話遮罩：對於多輪對話，常見的遮罩選擇有兩種[cite: 1]。(1) 僅最後一輪：損失僅包含最後一輪助理回覆的標記，而所有先前的上下文（包含先前的助理回覆）皆被遮罩[cite: 1]。長對話仍可被「展開」成多個訓練樣本：對於 N 輪的對話，每個範例預測一個助理回應，同時遮罩所有先前的上下文並排除任何未來的對話輪次[cite: 1]。(2) 僅遮罩使用者輪次：所有使用者輪次皆被遮罩，但每個助理輪次皆包含在損失中[cite: 1]。如果您希望獲得更多（較短）的訓練範例，您仍可在此設定中進行展開，但主要差異在於模型會直接針對中間的助理回覆進行訓練[cite: 1]。

* 與預訓練相同的損失函數：指令微調使用與預訓練語言模型相同的自迴歸損失函數，但在資料處理、遮罩（僅針對完整序列進行訓練，而預訓練文件可拆分至不同批次）等方面有實質差異[cite: 1]。

* 學習率：SFT 通常使用比預訓練小一到兩個數量級的學習率，以最佳地管理不同的最佳化動態（較小的資料集、較小的批次，以及強大的預訓練初始化，都傾向於較保守的更新）[cite: 1]。例如，OLMo 2 預訓練的峰值學習率為 $3\times10^{-4}$，而 SFT 則為 $1\times10^{-5}$ [58][cite: 1]。OLMo 3 使用較高的 SFT 學習率 $5-8\times10^{-5}$ [18]，部分原因是其訓練基礎設施使用了序列封裝 (sequence packing) 技術，將多個範例裝入每個訓練序列中，從而增加了以有用標記衡量之有效批次大小[cite: 1]。較大的批次會產生變異較低的梯度估計，進而支援較高的學習率而不會破壞訓練穩定性——這種關係被稱為線性縮放法則[cite: 1]。通常會在訓練步驟的一小部分內對學習率進行預熱 (warmed up)，然後線性衰減[cite: 1]。在實務上，團隊通常會掃描多個學習率，並在保留的評估套件上選擇最佳的檢查點 [18][cite: 1]。