---
title: "SELF-ICL: Zero-Shot In-Context Learning with Self-Generated Demonstrations"
summary: "A simple framework which bootstraps LMs' intrinsic capabilities to perform zero-shot ICL"
date: 2023-12-29T21:29:32+08:00
lastmod: 2023-12-29T21:29:32+08:00
# tags: 
# - prompt-engineering 
# - LLM

cover:
    image: "fig1.png" #图片路径：posts/tech/文章1/picture.png
    caption: "SELF-ICL 框架" #图片底部描述
    alt: ""
    relative: true
draft: false
---

## 摘要

大語言模型(LLM)表現出優秀的語境內學習(ICL, in-context learning)的能力。為提升LLM的ICL能力，許多透過取得現有訓練語料中的有效示範(demonstration, 可以說是context)的方法被提出，但終端使用者在使用LLM時並不能存取訓練語料或demonstration pool。
該文提出一個簡單框架SELF-ICL實現零樣本ICL:

1. 給定一個測試輸入
2. 利用指令引導模型產生多筆偽輸入
3. 令模型預測偽輸入的偽標籤
4. 以偽(輸入/標籤)作為ICL的context預測測試輸入的標籤

於23 BIG-Bench Hard tasks上的測試表明，SELF-ICL優於zero-shot的表現。此外，結合zero-shot CoT，SELF-ICL的表現甚至比肩來自真實語料的示範。