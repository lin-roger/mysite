---
title: "Linking Representations with Multimodal Contrastive Learning"
summary: "文章介紹"
date: 2023-09-04T22:57:28+08:00
lastmod: 2023-09-04T22:57:28+08:00
tags: 
- Contrastive Learning 
- record linkage
- multimodal

# cover:
#     image: "" #图片路径：posts/tech/文章1/picture.png
#     caption: "" #图片底部描述
#     alt: ""
#     relative: false
draft: true
---

## 摘要

許多應用須將不同文件資料集中的實體分組。目前常用的方法大多並不基於深度學習且不利用文件資料固有的多模態(multimodal)特性。值得注意的是，紀錄連結(record linkage)任務通常被視為機率性(兩筆 record 的相似度)/確定性(Deterministic, 兩筆 record 是否相同)字串匹配問題。在紀錄連結問題中，類別的數量龐大，無法預知或不斷變化。為解決前述問題，該文提出了 CLIPPINGS (**C**ontrastively **LI**nking **P**ooled **P**re-trained Embedd**ings**)。 CLIPPINGS 使用端到端訓練的對稱視覺語言雙向編碼器，透過語言-影像對比預訓練對其，以學習向量空間，其中給定實體的語言-影像表徵組合與該與和實體同類別的表徵相近，不同則遙遠。推理時，可以透過檢索其線下樣本嵌入索引的最近鄰或對其表徵聚類將實體連結起來。該文測試了兩個頗具挑戰性的應用:通過連接企業級財務記錄建構20世紀中期日本的綜合供應鏈--每個公司名稱都由其在文檔影像中的裁剪和相應的OCR表示、以及辨識在龐大的美國歷史報紙語料庫中的圖片-圖說對是否來自同一根本的圖片來源。CLIPPINGS的性能遠遠優於目前常用的字串匹配方法，也優於單模態方法。此外，僅在影像-OCR對上訓練的純自監督模型雖然低於監督黃金標準，但在不需要任何標記資料的情況下仍然優於流行的字串匹配方法。

## 簡介

## 文獻探討

## 模型架構

## 紀錄連結

## 圖片-圖說 重複噪音

## 結論

