---
title: "Reinforcement Learning"
summary: "從馬可夫決策過程到雙重深度Q網路"
date: 2024-01-27T15:36:25+08:00
lastmod: 2024-01-27T15:36:25+08:00
tags: 
-  Reinforcement Learning
# - LLM

cover:
    image: "cover.png"
    caption: "強化學習中代理與環境的互動"
    alt: ""
    relative: false
draft: false
---

## 馬可夫決策過程

強化學習(Reinforcement Learning, RL)藉由互動學習如何行動以實現目標，而馬可夫決策過程(Markov decision processes, MDPs)，是RL數學化的一種理想表示。MDPs中，代理(agent)與其環境(environment)在一連串分立的時步互動，其中代理是完全可知且可控的，而環境是不完全可控且很有可能難以理解的。代理與環境之互動遵循以下定義:

1. 代理選擇動作(action)
2. 動作的選擇基於狀態(states)
3. 選擇的驗證基於回饋(rewards)

策略(policy)定義代理在給定狀態$S_t$下如何選擇動作$A_t$，而代理的目標為最大化期望回饋$G_t$。

$$G_t = R_{t+1}+R_{t+2}+R_{t+3}+\dotsb+R_T$$
$T$為最終時步，該形式如果再$T$極大時令$G_t$發散至無窮，所以實務上會使用以下形式:

$$
G_t = R_{t+1}+\gamma R_{t+2}+\gamma^2 R_{t+3}+\dotsb=\sum_{k=0}^{\infty}\gamma^kR_{t+k+1}, \text{ in }
0 \le \gamma \le 1
$$

$\gamma$為衰減係數。上式以遞迴形式表達:

$$
\begin{align*}
G_t &= R_{t+1}+\gamma R_{t+2}+\gamma^2 R_{t+3}+\dotsb \newline
&= R_{t+1}+\gamma (R_{t+2}+\gamma R_{t+3}+\dotsb) \newline
&= R_{t+1}+\gamma G_{t+1}
\end{align*}
$$

範例:
$$
\begin{align*}
\mathcal{S} &= \set{ \text{high}, \text{low} } \newline
\mathcal{R} &= \set{ -3, 0, r_{\text{wait}}, r_{\text{search}} } \newline
\mathcal{A(\text{hight})} &= \set{ \text{search}, \text{wait} } \newline
\mathcal{A(\text{low})} &= \set{ \text{search}, \text{wait}, \text{recharge} } \newline
\end{align*}
$$

MDPs以表表示:

![table1](table1.png)

以轉移機率圖表示:

![fig1](fig1.png)

### 策略與價值函數

大部分的強化學習演算法需要***價值函數***，其估計代理在給定的狀態(或狀態-操作對)在未來的期望回饋總和，與策略有關。

***策略***將狀態$S$映射為每個可能操作的機率，如果代理在時間$t$遵循策略$\pi$，則$\pi(a|s)$即為在狀態$S_t=s$下操作為$A_t=a$機率。

策略$\pi$的*狀態-價值函數* :

$$
\begin{align*}
v_{\pi}(s) &= \mathbb{E}\_{\pi} [G_t|S_t=s] \newline
&= \mathbb{E}\_{\pi} [\sum_{k=0}^{\infty}\gamma^kR_{t+k+1}|S_t=s], \text{for all } s \in \mathcal{S}
\end{align*}
$$

策略$\pi$的*動作-價值函數* :

$$
\begin{align*}
q_{\pi}(s, a) &= \mathbb{E}\_{\pi}[G_t|S_t=s, A_t=a] \newline
&= \mathbb{E}{\pi}[\sum_{k=0}^{\infty}\gamma^kR_{t+k+1}|S_t=s, A_t=a]
\end{align*}
$$

## 最佳化策略和最佳化價值函數

解RL任務即為找一可以在長久運行下取得大量回饋的策略。
