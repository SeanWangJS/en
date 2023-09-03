---
layout: article
title: Some Thoughts on Attention Mechanism
tags: Transformer, Attention
---

Recently, I have been working on implementing a plain transformer on my own for practice. While writing about the attention function, I became curious about how it works and wanted to gain a deeper understanding.

As we all know, the attention formula is:

$$
Attn(Q, K, V) = softmax(\frac{QK^\top}{\sqrt{d_k}})V
$$

Assuming that all matrices are of shape $[L, d_k]$, the computation is quite simple. The following figure illustrates the idea of attention, ignoring the divide and softmax operation.

![](/resources/2023-09-01-attention/attention_attn.png)

The $Q, K,$ and $V$ matrices represent the semantic information of corresponding tokens. The first step of the attention function computes $S = QK^\top$, where each row vector represents the relationship between a specific token and all other tokens in the sequence. A higher value indicates a stronger relationship.

However, the second step, $Attn = S V$, may not be immediately intuitive. I searched some materials but not found any one which satisfied me. So I have to give some explanation to convince myself.

My solution is pushing the situation to it's limit, that is, we assume the each row of $S$ is like $[0, ...,1,..., 0]$, i.e. a vector with only one element is 1 and others are 0, which means that every token is most related to the $i$-th token but without any relationship with others. Then the $Attn$ matrix is just a row repeat of the $i$-th row of $V$ which implies that the $i$-th token is the most important part in the sentence.

From this thought experiment, we can conclude that the attention operation can extract the most important information from a sentence, leading to a better representation of it. This insight suggests that we should observe a reduction in entropy from $V$ to $Attn$ due to the attention transform. Am I right? need to be verified.