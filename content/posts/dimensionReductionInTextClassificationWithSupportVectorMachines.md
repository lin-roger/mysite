---
title: "Dimension Reduction in Text Classification with Support Vector Machines"
summary: "非經典古文，可以不用看，主要是我的筆記。SVM被公認是許多任務中效果最好的分類方法之一，SVM的學習能力和訓練計算複雜度與特徵空間維度無關，但在文本分類任務中，降低複雜度是有效處裡大量詞語的一個要點。該論文採用新的降維方法降低文檔向量的維度。還為基於中心的分類算法和SVM分類器引入決策函數，處理一個文檔可能屬於多個class的問題。分析大量的實驗結果表明使用為聚類資料設計的降維算法，使輸入維度降低，可在不犧牲預測精度的情況下取得更好的訓練效率。"
date: 2022-11-07T02:46:26+08:00
lastmod: 2023-06-26T02:46:26+08:00
# tags: 
# - prompt-engineering 
# - LLM
draft: false
---
## 摘要

SVM被公認是許多任務中效果最好的分類方法之一，SVM的學習能力和訓練計算複雜度與特徵空間維度無關，但在文本分類任務中，降低複雜度是有效處裡大量詞語的一個要點。該論文採用新的降維方法降低文檔向量的維度。還為基於中心的分類算法和SVM分類器引入決策函數，處理一個文檔可能屬於多個class的問題。分析大量的實驗結果表明使用為聚類資料設計的降維算法，使輸入維度降低，可在不犧牲預測精度的情況下取得更好的訓練效率。

## 簡介

文本分類是一項監督是任務，將文本分類到預定義的class，用來從大樣文本中尋找有價值的資訊，base on Vctor Space的方法有以下特性，input高維且稀疏(one hot, bag of word)，線性可分性(存在超平面將資料分割)，少數特徵不相關(多數相關)。有人猜測，積極降維會導致資訊嚴重損失，導致分類效果不佳。

給定訓練資料:

$$(x_i, y_i)$$

$$1\le y_i \le 1$$

$$1\le i\le n$$

具K, C的soft margin SVM之對偶式(Loss(target) funtion, Constraint(條件、約束))為:

$\max_{\alpha_i} \sum_{i=1}^{n} \alpha _i -\frac{1}{2}\sum_{i,j=1}^{n} \alpha_i \alpha_j y_i y_jK(\mathrm {x}_i, \mathrm {x}_j),$  

$s.t. \sum_{i=1}^{n} \alpha_iy_i=0, 0\le \alpha_i\le C, i=1,\dots ,n.$

K is (<, > 矩陣乘法，處裡非線性可分, $\phi$是一個mapping)

$K(\mathrm {x}_i, \mathrm {x}_j) = < \phi(\mathrm {x}_i), \phi(\mathrm {x}_j)>$

如上式所述，SVM的複雜度取決於訓練樣本數，為$O(n)$。並且因K函數的使用不受dim of feature space影響，然而K函數的計算複雜性被忽略了其取決於dim of input space，就算在最佳分割超平面的情況K函數的計算複雜性也無法被省略。因此降維必定可以使訓練SVM和預測帶來更高效率。

該論文假設文件集合表示為Document-Term Matrix(Bag of Word)，加權兩倍，假設資料的聚類已經進行。

下一章回顧LSI，使用svd分解做a的低秩近似，但忽略了資料的聚類結構。第三節中，回顧幾種對聚類資料特別有效的降維演算法:兩種群中心方法和使用GSVD(廣義奇異值分解)的泛化LDA。通過降維SVM()和K-mens(計算向量對距離)等分類器的計算複雜度皆可大降低。

多數文本資料集中，文本可被分入多個類，為更有效處理此問題，在第四節中介紹基於閥值的分類算法擴展，實驗表明，該論文提出的cluster preserving降維演算法沒有造成訊息損失，反而提升了分類器的預測精度(推斷具有去噪效果)。

## 低秩近似使用隱含語意索引

LSI假設:Document-Term Matrix中存在隱含的語意結構，其被文件中出現各種詞所破壞(polysemy and synonymy)。基本概念:若兩個文檔向量代表同一主題，其會共享許多與關鍵詞相關的關聯詞，通過SVD其語意結構會十分接近(term vectors表示為左奇異向量document vectors表示為右奇異向量)。然而，LSI再降維時忽略了聚類結構，且並沒有理論最佳的參數選擇，需多次實驗來確定最佳維度(如第五章所述)。實驗結果證實，當資料已被聚類時，下節介紹的降維法對新資料的分類效果更好。

## 聚類資料的降維演算法

為提升高維度資料的處裡效率，須將資料降維，此節回顧三種保留聚類結構的降維算法

### Centroid-based Algorithms for Dimension Reduction of Clustered Data

給定一Document-Term Matrix，找一變換映射每個document vector從m維空間降到l維空間(m>l)，兩種方法:

1. 線性轉換($G^T_{l\times m}$)
2. 低秩近似(分解為兩個矩陣)

$A\approx BY$

只要計算給定資料的降維表示，就無須從B計算降維變換G，若確定矩陣B，Y即可用最小平方法求解。

$\min_{B,Y}\left\|BY-A\right\|_{F.}$

給定任意文本$q\in\mathbb{R}^{m\times1}$透過解最小化問題轉換到低維空間。

$\min_{\hat{q}\in\mathbb{R}^{l\times1}}\left\| B\hat{q}-q\right\|_{2.}$

在Centroid降維法中(A1)，B的第$i, (1\le i\le p)$列是第$i$個群的中心(均值中心)點向量，任一向量$q$，可在$p$維空間表示成$\hat{q}$即為最小平方法的解。

在Orthogonal Centroid演算法中(A2)，使用$p$維表示資料向量$q\in \mathbb{R}^{m\times 1}$被給定為$\hat{q}=Q_{p}^{T}q$，$Q_p$為$B$的正交基底(QR分解)。

以上兩種等Centroid-based降維法在計算成本比LSI更低，且在聚類資料下的效果更好。雖然此方案只能在線性可分的資料使用，但文本資料扔然可用，因文本資料通常是線性可分的(非線性可分in 18)

### Generalized Discriminant Analysis based on the Generalized Singular Value Decomposition

近期(2003)，出現了GSVD base 的 cluster-preserving降維法，使SVM泛化到高維空間資料。

經典判別分析透過最大化聚類之間的散度和最小化即群內的散度，維持聚類結構。為此，其定義聚類內散度矩陣$S_w$和聚類間散度矩陣$S_b$，$N_i$表示集群$i$的列索引集合$n_i$表示集群$i$的列數，$C$表全域中心點。目標使群內散度最小化，和降維後群間散度最大化。再次請出$G^T\in \mathbb{R}^{l\times m}$將A的每列m維向量映射到l維的變換，目標表示為最小化$trace(G^T S_wG)$和最大化$trace(G^T S_bG)$。

當$S_w$可逆(nonsingular、invertible)，可視為解最大化問題。
全局最大成立於:$G$的列是$S_{w}^{-1}S_b$的特徵向量並對應於$l$個最大特徵值。
when $l\le p-1$ is equals $λ_1 + \dots +λ_{p−1},$ each $λ_i ≥ 0$。設Document-Term Matrix $A$被分為$A=[A_1,\ \dots , \ A_p]$，$A_i \in \mathbb{R}^{m\times n_i}\  \text{in cluster}\ i$

$H_w = [a_1-c_1, a_2-c_2,\dots ,a_n-c_p]\in \mathbb{R}^{m\times n}$

$H_b = [\sqrt{n_1}(c_1-c),\dots ,\sqrt{n_p}(c_p-c)]\in \mathbb{R}^{m\times p}$

$S_w=H_wH_w^T\ \& \ S_b=H_bH_b^T$

當詞數(terms) $m$ > 文本數(doc) $n$，$S_w$不可逆(singular)，經典SVM失效。將問題(特徵值) $S_w^{-1}S_b\mathrm{x}_i = λ_i\mathrm{x}^i$ 改寫為 $\mathrm\beta_i^2H_bH_b^T\mathrm{x}_i = \mathrm\alpha_i^2H_wH_w^T\mathrm{x}_i$即可透過GSVD處理(LDA/GSVD in A3)。其中最複雜的計算部分複合矩陣$H$的完全正交分解，當$\max (p, n)\ll m$，$H=[H_b^T,H_w^T]\in \mathbb{R}^{(p+n)\times m}$ 的SVD分解可被計算為:

1. 計算$H_t$的QR分解$Q_HR_H$
2. 計算$R_H\in \mathbb{R}^{(p+n)\times (p+n)}$ 的SVD分解 $=Z\begin{pmatrix} \sum_H & 0 \\ 0 & 0 \end{pmatrix}P^T$，使$H=R_H^TQ_H^T=P\begin{pmatrix} \sum_H & 0 \\ 0 & 0 \end{pmatrix}Z^TQ_H^T$。其中$Q_HZ\in\mathbb{R}^{m\times (p+n)}$的列之間為標準正交的(內積0、距離1)，存在正交$Q\in \mathbb{R} ^{m\times m}$其前$p+n$列等同$Q_HZ$。將式整理為：  
$H = P\begin{pmatrix} \sum_H & 0 \\ 0 & 0 \end{pmatrix}Q^T$  
式中$\sum_H$的右邊有$m-t$個0列，因$R_H\in \mathbb{R}^{(p+n)\times (p+n)}$遠小於$H\in \mathbb{R}^{(p+n)\times m}$，記憶體需求大減，複雜度也降至$O(mn^2)+O(n^3)$

## 分類方法

為測試降維效果，使用三種分類器測試:中心點分類、kNN和SVMs。皆引入閥值進行修改，以確保文本被判有多重類別資格時可以正確分類。

### Centroid-based

設新的文本資料為$q$，訓練資料共有$p$個群，$c_i$為第$i$個群的中心點向量:

$arg \max_{1\le i\le p} \frac{q^Tc_i}{\|q\|_2 \|c_i\|_2}$

多類別擴展($\theta$為閥值):

$y(\mathrm{x}, j) = \text{sign}\{ sim(\mathrm{x}, \mathrm{c}_i)-\theta_j^c \}$  
$y(\mathrm{x}, j)\in \{ +1,-1 \}$  
$$
\begin{cases}
\text{Class is j}     & \text{:} & y(\mathrm{x}, j)>0  \\
\text{Class is not j} & \text{:} & y(\mathrm{x}, j)\le 0
\end{cases}
$$

![sign](https://upload.wikimedia.org/wikipedia/commons/c/c0/Signum_function.png)

### k-Nearest Neighbor

設新的文本資料為$q$，訓練文本資料共有$p$個群:

1. 在訓練資料中，使用餘弦相似度計算與$q$最近的k個文本向量
2. 在這k個向量中，計算屬於各個群的數量，$q$將被分配到最多的那個。

多類別擴展($\theta$為閥值，$kNN$為文本$x$的$k$個鄰近向量集合):

$y(\mathrm{x}, j) = \text{sign}\{ \sum_{\mathrm{d}_i\in kNN} sim(\mathrm{x}, \mathrm{d}_i) y(\mathrm{d}_i, j) -\theta_j^{kNN} \}$  

### SVM

OvR策略(為每個Class建一個分類器)的二元分類器的最佳分割超平面可透conventional SVM取得。引入多類別擴展:

$$
y(\mathrm{x}, j) = \text{sign}\{
     \sum_{\mathrm{x}_i\in SV}
     \alpha_i y_i K(\mathrm{x}, \mathrm{x}_i)+ b -\theta_j^{SVM} \}\\
K=<\mathrm{x}, \mathrm{x}_i> \\
K=[<\mathrm{x}, \mathrm{x}_i>+1]^d \\
K=\exp(-\gamma\|\mathrm{x}, \mathrm{x}_i\|^2)
$$

$SV$為支援向量的集合，$\theta$為閥值，$\gamma$與高斯函數寬度成反比。

## 實驗結果

預測結果包含:

1. 無降維
2. LSI/SVD
3. Centroid
4. Orthogonal Centroid
5. LDA/GSVD

SVM優化:

* 正則參數$C$
* polynomial 角度$d$
* Gaussian RBF $\gamma$

### 資料集

1. subset of MEDLINE database: 5個class、每個class各有500份文本、每份文本只有一個class、train:test = 50:50、做詞型還原和處理剔除字(此為國家教育研究院翻譯版，俗稱為停用詞)後訓練集有22095個不重複的詞。
2. Reuter-21578 文本集的"ModApte"分割: 90個class、每份文本可能有多個class、每個class至少有一個train和一個test、共7769個train和3019個tess、做詞型還原和處理剔除字(此為國家教育研究院翻譯版，俗稱為停用詞)後訓練集有11941個不重複的詞、引入閥值模型。

DTM不用BoW，改用TF-IDF並歸一化。

### 表一

對MEDLINE資料集使用LSI/SVD，用使用centroid-based, kNN和SVMs分類器分類的結果。觀察到:

1. kNN使用L2 norm在$l$為100-500時效果不佳，與餘弦相似性在未正則化資料表現更好的印象相符，且5NN明顯落後其他更高的K，表明k=5太小。

### 表二

SVM使用不同的K(核)函數與降維演算法在MEDLINE資料集的結果。觀察到:

1. 降維後的預測結果與原始空間的預測結果相似且複雜度降低。
2. 正交中心降維演算法對K函數的選擇不敏感，可以選複雜度更低的線性K函數。

### 表三

選擇不同分類演算法與降維演算法在MEDLINE資料集的結果。觀察到:

1. 使用LDA/GSVD降維演算法時，使用餘弦相似度的centroid-based與kNN分類演算法效果較差，而使用L2-norm的效果較好，因跡最佳化使用L2-norm表示。
2. 因LDA/GSVD最小化群內散點的跡(距離)，自同一(相似)類別的文檔向量會被變換為一個緻密的群甚至一個點，使得SVM難以找到泛化性高的超平面。

### 表四

MEDLINE資料集中使用SVM配合不同降維演算法在5個不同類別中的準確率。觀察到:

1. colon cancer與oral cancer難以被區分。

### 表五

選擇不同分類演算法與降維演算法(Centroid、Orthogonal Centroid)在REUTERS資料集的結果。觀察到:

1. Orthogonal Centroid 效果無明顯下降，Centroid則明顯降低，推測因將各個聚類中心映射至單位矩陣導致每個class間的資訊消失所造成。

## 結論與討論

本文使用三種降維方法Centroid、Orthogonal Centroid和LDA/GSVD，皆是為集群資料所設計，做為比較也使用了LSI/SVD這種不保留集群結構的降維演算法。其也測試了三種不同的分類演算法SVMs、kNN與centroid-based classification測試在不同降維法中的分類效果。測試結果取得了高的準度，即使在強力的降維下，也可與未降維的狀態近似。

該論文還引入了基於閥值的分類器，用於centroid-based和SVM做到一對多的分類。centroid在Class互不關聯的情況下表現更好。

### 結論

不犧牲精度的情況下，可以做到對文本大幅降為。Orthogonal Centroid在保留聚類結構的能力做的極佳，降為前後的預測準確率幾乎不變，在KNN或SVM使用可以極大減少計算複雜度。
