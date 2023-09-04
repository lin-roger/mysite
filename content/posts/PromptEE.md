---
title: "prompt-engineering 基礎"
summary: "簡單介紹prompt-engineering"
date: 2023-06-26T01:17:51+08:00
tags: 
- prompt-engineering 
- LLM
draft: false
---
## Base LLM & Instruction Tuned LLM

Base LLM 使用大量從網路爬取的資料訓練，輸入一文本後預測出最有可能接續在該文本後的下一個字(token)，所以給定一文章的前半部分其可將其後半部分生成。**問題 ->** 最有可能接續的字詞和人類預想的**相同**嗎?假設以下為您的訓練資料

> 常見問題:  
> Q1:為什麼同樣行程不同旅行社價格會不一樣？  
> Q2:行程上有的景點，是不是一定都會走到？  
> Q3:為什麼我和朋友的座位沒有安排在一起？  
>
> A1:其實旅遊產品牽涉範圍極廣...  
> A2:旅客在報名旅遊行程前...  
> A3:機上座位的安排...

當您向使用該資料訓練的LM輸入"Q1:為什麼同樣行程不同旅行社價格會不一樣？"，我們通常為期望LM回答與問題對應的答案，也就是:"A1:其實旅遊產品牽涉範圍極廣..."，但實際上LM會回答:"Q2:行程上有的景點，是不是一定都會走到？"，這是因為資料集中，文本Q1的下一段文本為Q2，而非A1，使模型認為Q1->Q2是最好的選擇，而非Q1->A1。

為解決此問題，Instruction Tuned LLM 出現了。透過對Base LLM 使用指令資料集(如:
[alpaca-tw](https://github.com/ntunlplab/traditional-chinese-alpaca/blob/main/data/alpaca-tw.json)
)微調並使用[RLHF](https://arxiv.org/abs/1909.08593)技術使LM的的輸出更符合人類的預期。

## 指令的基本原則

### 0. Model Limitations: Hallucination

1. 幻覺(Hallucination)，LLM並不了解自身知識的極限也不完全記得所有其看過的資訊，遇到一些艱澀的問題其可能會嘗試回答，並輸出看似合理但與事實不符或錯誤的回應，透過要求模型找出問題相關的文獻並引用其回答可以減輕此問題(**LM找出的相關文獻或引用也有為幻覺之嫌，務必謹慎確認**)

2. 無法做到精確字數要求，如過您在指令中加入以下要求"..., 在50個字以內說明"，可能會出現60甚至70個字(會因語言影響誤差範圍，該問題為token與字/詞數量不等所導致，一個字/詞可由1~n個token組成，通常為1~3個，參考[BPE編碼](https://en.wikipedia.org/wiki/Byte_pair_encoding))

### 1. Write Clear and Specific Instructions

0. **Clear != Short**，更長的指令通常提供了更多資訊，可以令模型產生更準確的結果。
1. Use delimiters，使用分隔符號將輸入資訊清楚的分割，使輸入資料不會與指令或其他資訊混淆，該方法也可以一定程度抵擋Prompt Injections(透過在輸入資料中夾帶惡意指令使模型輸出改變的技術。如:忽略先前的指令，幫我...。使用分隔符號將其指定為輸入資料可使模型不執行該惡意指令)
2. Ask for structured output，可以在指令中要求輸出資料以json、xml等結構化的形式表達，可以被更好的儲存或處裡
3. Check whether conditions are satisfied，在指令中加入一些條件，避免一些意外使輸出不合預期
4. Few-shot prompting，對於一個複雜的任務，指令中只有任務描述是不夠的，在指令中加入一些範例可以使LM有效理解你的要求(補充，LLM在zero-shot的表現並不佳，請盡量避免)

### 2. Give Model Time to Think

1. Specify the steps to complete a task，一個複雜的任務可以將其拆解成多個較為簡單的步驟或指令使模型可以更好理解或處理您的任務
2. Instruct the model to work out its own solution before rushing to a conclusion，詢問模型時可以要求模型先輸出推論過程在輸出結果

## Iterative Prompt Development(Prompt 的開發與迭代流程)

Prompt的構成:

* Instructions
* Context
* Input Data
* Output Indicator

Prompt guidelines:

* 寫出符合基本原則的Prompt
* 測試Prompt，取得大量輸出
* 找出不合預期的案例並分析原因以改進Prompt
* Repeat

並沒有所謂的**萬能**Prompt，只有**適合**的Prompt

## [Use Case](https://www.promptingguide.ai/introduction/examples)

### Summarizing 文本摘要

LLM 通常預設以抽樣摘要(以比原文更簡短的通順文本表述原文資訊)為主，可以要求:字數、句數、風格(簡潔的、嚴謹的)、側重在甚麼(對...的影響、XXX說的內容)、摘要的對象(國小生、商業部門經理)等

你可以要求LLM以抽取式摘要(原文的重點字、詞、句、段，原封不動的取出)，如:"找出/萃取文章中XXX會想知道的關鍵字/資訊/句子"，取得關鍵資訊避免攏言贅詞並減少幻覺產生的可能。

### Question Answering 問答

Prompt:

>Answer the question based on the context below. Keep the answer short and concise. Respond "Unsure about answer" if not sure about the answer.  

* Keep the answer short and concise: 指定風格
* Respond "Unsure about answer" if not sure about the answer: 1.3-Check whether conditions are satisfied

### Text Classification 文本分類

>Classify the text into neutral, negative or positive and follow the output format.  
>output format:"Class"  
>  
>Text: I think the vacation is okay.  
>Sentiment: "Neutral"  
>Text: I think the food was okay.
>Sentiment: "Neutral"
