---
title: "🚧Advanced Prompting Engineering 進階提示工程🚧"
summary: "🚧目前進度:deepl部分翻譯🚧"
date: 2023-06-26T02:38:25+08:00
lastmod: 2023-06-26T02:38:25+08:00
tags: 
- prompt-engineering 

draft: flase
---
## Zero-Shot Prompting & Few(N)-Shot Prompting

### How it work

Shot意指範例，顧名思義，Zero-Shot就是提示不包含任何範例，反之N-Shot就是提示包含N個範例，在簡單任務中您使用Zero-Shot Prompting通常也可獲得不錯的效果，但當問題開始複雜起來時，Few-Shot Prompting 可以更好的解決問題

The model has somehow learned how to perform the task by providing it with just one example (i.e., 1-shot). For more difficult tasks, we can experiment with increasing the demonstrations (e.g., 3-shot, 5-shot, 10-shot, etc.).

模型通過僅提供一個示例（即 1-shot）以某種方式學會瞭如何執行任務。對於更困難的任務，我們可以嘗試增加演示次數（例如 3 次、5 次、10 次等）。

Following the findings from [Min et al. (2022)](https://arxiv.org/abs/2202.12837), here are a few more tips about demonstrations/exemplars when doing few-shot:

根據 Min 等人的研究結果，這裡有一些關於進行小樣本時演示/範例的更多提示：

"the label space and the distribution of the input text specified by the demonstrations are both important (regardless of whether the labels are correct for individual inputs)"

“演示指定的標籤空間和輸入文本的分佈都很重要（無論標籤對於各個輸入是否正確）”

the format you use also plays a key role in performance, even if you just use random labels, this is much better than no labels at all.

您使用的格式對性能也起著關鍵作用，即使您只是使用隨機標籤，這也比根本沒有標籤要好得多。

additional results show that selecting random labels from a true distribution of labels (instead of a uniform distribution) also helps.

其他結果表明，從真實的標籤分佈（而不是均勻分佈）中選擇隨機標籤也有幫助。

### Limitations

當問題牽扯到具有多步驟的複雜推裡時，Few(N)-Shot 的效果會減弱，這時將指令拆解會是更好的選擇

## Chain-of-Thought Prompting

## Self-Consistency

## Generated Knowledge Prompting

## Tree of Thoughts (ToT)

## Retrieval Augmented Generation (RAG)

## Automatic Reasoning and Tool-use (ART)

## Automatic Prompt Engineer (APE)

## Active-Prompt

## Directional Stimulus Prompting

## ReAct Prompting

## Multimodal CoT Prompting

## Graph Prompts 

