---
title: "🚧Whitening Sentence Representations for Better Semantics and Faster Retrieval🚧"
summary: "🚧還在寫喔!!語意相似度計算🚧"
date: 2023-06-28T01:31:35+08:00
lastmod: 2023-07-02T22:32:35+08:00
tags: 
- text-similarity 
draft: false
---

## Abstract

BERT 等預訓練模型在許多自然語言處理任務中取得了巨大成功。然而，如何通過這些預訓練模型獲得更好的句表徵向量仍然值得探索。先前的工作表明，各向異性問題是基於 BERT 的句表徵的關鍵瓶頸，阻礙模型充分利用底層語義特徵。因此，一些增強句子分佈各向同性的嘗試，例如基於流的模型(BERT-flow)，已被應用於句表徵並取得了一些改進。在本文中，我們發現傳統機器學習中的白化(球化)變換同樣可以增強句表徵的各向同性並取得有競爭力的結果。此外，白化還能夠降低句子表示的維度。我們的實驗結果表明，它不僅可以實現良好的性能，而且可以顯著降低存儲成本並加快模型檢索速度。

## Introduction

深度神經語言模型的應用(BERT、GPT)近年來取得了巨大成功，因為它們創造了對隨前後文語境改變的詞彙表徵向量。這一趨勢也刺激了為長文本產生語義崁入(句崁入向量、段落崁入向量)的研究進展。然而句崁入已被證明不能完整捕捉句子的基本語義。據先前研究([Gao et al., 2019; ][1][Ethayarajh, 2019; ][2][Li et al., 2020][3])，所有的詞語的表徵向量都不是[各項同性][4]的：它們在方向上並非均勻分布的，而是在崁入空間中佔據一個狹小的錐體，呈現各向異性。[Ethayarajh, 2019; ][2]證明，取自預訓練模型的崁入向量的極度的各向異性，以致任兩個詞的崁入向量COS相似度平均為0.99(極度相似)。[Li et al., 2020][3]進一步調查發現，BERT的句崁入空間存在兩問題:

1. 詞頻令崁入空間產生偏移：高頻詞距離原點較近，低頻詞距離原點較遠。在相同權重下，低頻詞對整體句崁入向量的遠比高頻詞影響更強
2. 低頻詞分散且稀疏：低頻詞在嵌入空間中與其他詞的最近距離(L2-dist)相較高頻詞更遠，使低頻的同義詞相似性更低

  [1]: https://openreview.net/forum?id=SkEYojRqtm "Representation Degeneration Problem in Training Natural Language Generation Models 表徵退化"
  [2]: https://aclanthology.org/D19-1006/ "How Contextual are Contextualized Word Representations? Comparing the Geometry of BERT, ELMo, and GPT-2 Embeddings 崁入向量的幾何結構"
  [3]: https://aclanthology.org/2020.emnlp-main.733/ "On the Sentence Embeddings from Pre-trained Language Models BERT-flow"
  [4]: https://en.wikipedia.org/wiki/Isotropic_position "各項同性 wiki(英文)"
