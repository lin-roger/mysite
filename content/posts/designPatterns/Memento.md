---
title: "Memento"
# summary: "文章介紹"
date: 2023-09-27T15:06:53+08:00
lastmod: 2023-09-27T15:06:53+08:00
# tags: 
# - prompt-engineering 
# - LLM

# cover:
#     image: "" #图片路径：posts/tech/文章1/picture.png
#     caption: "" #图片底部描述
#     alt: ""
#     relative: false
draft: true
---

### Memento

存储发端对象的内部状态。纪念物可根据需要存储或多或少的发起者内部状态，由发起者自行决定。

保护，防止发起者以外的其他对象访问。纪念物实际上有两个界面。看护者看到的是纪念物的狭窄界面，它只能将纪念物传递给其他对象。与此相反，追忆者看到的是一个宽广的界面，它可以访问所有必要的数据，将自己恢复到以前的状态。理想情况下，只有制作纪念品的起源者才可以访问纪念品的内部状态。

### Originator

创建一个包含其当前内部状态快照的 memento。

使用 memento 来恢复其内部状态。

### Caretaker

负责memento的保管。

永远不要操作或检查memento的内容。
