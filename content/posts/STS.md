---
title: "🚧STS 資料集簡介🚧"
summary: "🚧介紹STS 2012-2016 和 STS benchmark, 目前進度:已完成STS 2012說明🚧"
date: 2023-07-06T22:06:01+08:00
tags: 
-  text-similarity
-  dataset
draft: true
---
## What is STS(Semantic Textual Similarity)

文本語意相似度(Semantic Textual Similarity, STS)任務的目的為衡量兩個句子間語意的相似程度。

## STS 2012

給定兩句子，預測兩個句子間語意的相似度，相似度有六個評級:

0. On different topics. 完全不同主題
1. Not equivalent, but on the same topic. 語意不相等，但主題相似
2. Not equivalent, but share some details. 語意不相等，但有一些細節相同
3. Roughly equivalent, but some important information differs/missing. 語意大致相同，但一些重要資訊缺失/不同
4. Mostly equivalent. but some unimportant details differ. 語意非常相近，但一些不重要的資訊不同
5. CompIetely equivalent, as they mean the same thing. 語意完全相等

**組成:**
| Name | 描述 | Original_Train/Val/Test | 採樣方法 | Adjust_Train/Val/Test |
| :---: | :---: | :---: | :---: | :---: |
| MSRpar([MSR](https://zh.wikipedia.org/zh-tw/%E5%BE%AE%E8%BB%9F%E7%A0%94%E7%A9%B6%E9%99%A2) Paraphrase) | 包含5801對句子，透過在18個月內從網路爬取大量新聞資料收集而成，資料集中有67%的句子對被以更簡短更清楚的風格改述。 | 70%/0%/30% | 分別從[0.4, ..., 0.8]五個相似度的範圍內抽出等量的未改述句子對與改述句子對，共抽出1500對 | 50%/0%/50% |
| MSRvid(MSR Video Paraphrase Corpus) | 資料集中含有2000部短片，並且每個短片配有多筆句子，每筆以一句話描述短片中的主要事件，並且不限語言，該資料集共收集了12萬條句子。 | NaN | 分兩組抽，第一組從同一影片蒐集句子對，第二組從不同影片蒐集句子對(第一組蒐集的句子對並非一定相似。反之，第二組蒐集的句子對並非一定不相關。許多影片主題是相似的)。作者盡量從兩組中自[0.5, ..., 0.8]四個相似度的範圍抽等量的樣本，以維持樣本的平衡性，共抽出1500對。 | 50%/0%/50% |
| SMT-eur | 2007年和2008年的ACL WMT(Workshops on Statistical Machine Translation) 法語-英語翻譯 shared task，資料中每筆包含機器翻譯句子和人類翻譯句子 | NaN | 2007年作為訓練集，共729對。2008年則作為測試集| NaN |
| ----- | ----- | **OUT-OF-DOMAIN TESTING(Without Train)** | ----- | ----- |
| On-WN | 2007年的ACL WMT 新聞對話測試集 法語-英語翻譯任務，資料中每筆包含機器翻譯句子和人類翻譯句子 | NaN | 共351對 | NaN |
| SMT-news | OntoNotes4.0 與 WordNet3.1 | NaN | 共750對 | NaN |

## STS 2013

## STS 2014

## STS 2015

## STS 2016

## STS benchmark
