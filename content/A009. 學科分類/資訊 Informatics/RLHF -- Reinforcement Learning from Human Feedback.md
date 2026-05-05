---
cssclasses:
  - center-titles
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