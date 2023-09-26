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
draft: false
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

推薦系統利用使用者與項目間的的關係推算使用者可能喜歡的項目。然而使用者和項目的數量龐大且使用者通常只會與少量的向產生關係，所以這些資訊是稀疏(大部分都是0)且高維(但不同維度間有相關性，冗餘維度)的，需透過降維解決這些問題。

#### PCA

將有相關性的維度合併，去除資訊中攏餘的維度。

#### SVD

將原始資料矩陣$A$轉換為三個較小的矩陣$U\Sigma V^\top$表示。

SVD推薦系統\[[1]、[2]\]

## 2. 資料分析

### 分類

* kNN: 離樣本$q$最近的$k$個點，他們屬於甚麼類別(取最多)
* [決策樹][4]: C&RT(CART)、C4.5、ID3。
<!-- * 規則 -->
* [貝葉斯][5]
* [ANN][6]
* [SVM][7]

將多個分類器集成\[[8]\]可以取得更好的效果

### 分群

* [k-means][9]

<!-- ### 關聯規則探勘 -->

## 總結

回顧推薦系統中常用的資料探勘技術，

## reference

[\[1\] SVD 實作推薦系統][1]

[\[2\] SVD++ 與混合模型][2]

[\[3\] SVD_推薦系統_原理][3]

[\[4\] Machine Learning Foundations/Techniques: Decision Tree / Random Forest][4]

[\[5\] \[機器學習\] MultinomialNB 貝氏(貝葉氏)分類-理論篇][5]

[\[6\] Machine Learning Foundations/Techniques: Neural Networks and Matrix Factorization][6]

[\[7\] Machine Learning Foundations/Techniques: Linear Support Vector Machine][7]

[\[8\] Machine Learning Foundations/Techniques: Blending / Bagging / Boosting][8]

[1]: https://medium.com/ai-academy-taiwan/svd-%E5%AF%A6%E4%BD%9C%E6%8E%A8%E8%96%A6%E7%B3%BB%E7%B5%B1-f90f98b9831b "SVD 實作推薦系統(SVD、Funk SVD)"

[2]: https://medium.com/ai-academy-taiwan/svd-%E8%88%87%E6%B7%B7%E5%90%88%E6%A8%A1%E5%9E%8B-c58ed9248c97 "SVD++ 與混合模型(SVD++)"

[3]: https://medium.com/data-scientists-playground/svd-%E6%8E%A8%E8%96%A6%E7%B3%BB%E7%B5%B1-%E5%8E%9F%E7%90%86-c72c2e35af9c "SVD_推薦系統_原理(Funk SVD、Bais SVD、SVD++)"

[4]: https://www.youtube.com/live/8yRp2lETKyE?si=oYm8V52ivTugB3Gb "Machine Learning Foundations/Techniques: Decision Tree / Random Forest"

[5]: https://medium.com/@d101201007/%E6%A9%9F%E5%99%A8%E5%AD%B8%E7%BF%92-multinomialnb-%E8%B2%9D%E6%B0%8F-%E8%B2%9D%E8%91%89%E6%B0%8F-%E5%88%86%E9%A1%9E-df1d59b6fd9d "[機器學習] MultinomialNB 貝氏(貝葉氏)分類-理論篇"

[6]: https://youtu.be/PM8vwXiQL78?si=cGldMSTsliZ5HiNm "Machine Learning Foundations/Techniques: Neural Networks and Matrix Factorization"

[7]: https://www.youtube.com/live/ripP9uMPpfE?si=zw2Yg1vYaP3ETa9Q "Machine Learning Foundations/Techniques: Linear Support Vector Machine"

[8]: https://www.youtube.com/live/lzpMyabZV1I?si=aDvUEywfKKqk_jGo "Machine Learning Foundations/Techniques: Blending / Bagging / Boosting"

[9]: https://en.wikipedia.org/wiki/K-means_clustering "Wiki: k-means clustering"
