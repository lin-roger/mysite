---
title: "Recommender systems handbook || 閱讀紀錄 1. Data Mining Methods for Recommender Systems"
summary: "推薦系統中的資料探勘方法。本章介紹推薦系統中常用的資料探勘(Data mining, DM)技術。首先描述的是常見的資料預處理方法，如抽樣與降維。接著回顧推薦系統中重要的分類/分群技術，如:貝葉斯網路和支援向量機(SVM)/k-mean。"
date: 2023-09-18T05:59:25+08:00
lastmod: 2023-09-18T05:59:25+08:00
tags: 
- Recommender systems handbook 
- 推薦系統

# cover:
#     image: "" #图片路径：posts/tech/文章1/picture.png
#     caption: "" #图片底部描述
#     alt: ""
#     relative: false
draft: true
---

## 摘要

本章介紹推薦系統中常用的資料探勘(Data mining, DM)技術。首先描述的是常見的資料預處理方法，如抽樣與降維。接著回顧推薦系統中重要的分類/分群技術，如:貝葉斯網路和支援向量機(SVM)/k-mean。

## 簡介

推薦系統可視為DM的一種特例，以下簡要回顧推薦系統常用的DM方法，DM通常有以下三步驟:

## 1. 資料預處理

資料為一組物件(record, item, point, sample, observation, or instance)和屬性(variable, field, characteristic, or feature)的組合。資料通常須經預處理，以便後續分析使用，

### 距離度量

衡量兩筆資料的差異性，不同技術須使用不同方法。

#### $L_p$-dist

$d(x, y) = (\sum^n_{k = 1}|x_k-y_k|^p)^{\frac{1}{p}}$

> $p = 1$，曼哈頓距離  
> $p = 2$，歐式距離

#### 餘弦距離

$\cos(x, y) = \frac{x \cdot y}{\left \|x\right \|^2  \left \|y\right \|^2}$

> 計算兩向量間的夾角

#### 皮爾森相關係數

$Peason(x, y) = \frac{\mathbf{cov}(x, y)}{\sigma_x \times \sigma_y}$

> 計算$x, y$的關係。  
> +1 代表$x, y$正相關  
> -1 代表$x, y$負相關  
> 0 代表$x, y$間沒有明確關係

#### 單純一致係數(Simple Matching Coefficient, SMC)、Jaccard index

$SMC = \frac{\mathbf{num\ of\ match}}{\mathbf{num\ of\ attribute}} = \frac{\mathbf{M11+M00}}{\mathbf{M01+M10+M00+M11}}$

> $\mathbf{M00: x=0, y=0}$  
> $\mathbf{M10: x=1, y=0}$  
> $\mathbf{M01: x=0, y=1}$  
> $\mathbf{M11: x=1, y=1}$

$Jaccard index = \frac{\mathbf{M11}}{\mathbf{M01+M10+M11}}$

$Jaccard index(bit\ vector) = \frac{x\cdot y}{\left \|x\right \|^2 + \left \|y\right \|^2 - x \cdot y}$

### 抽樣

80/20法則: training $80\%$ / testing $20\%$。  
k-fold(k折交叉驗證): 將資料集拆為k份，在訓練時將其中一份作為testing，其餘作為training。

### 降維



## 2. 資料分析

* 預測 >> 分類
  * kNN
  * 決策數
  * 規則
  * 貝葉斯網路
  * SVM
  * ANN
* 描述
  * 關聯規則探勘
  * 分群
  
## 3. 解釋

## 總結
