---
title: "ModerBERT: A Lightweight and Efficient BERT"
date: "2025-04-21"
Link: ""
skills: 
---

# BERT
BERT (Bidirectional Encoder Representations from Transformers) is an NLP model developed by Google. It is an encoder-only transformer model. It was released in October 2018 by Jacob Devlin et al. at Google AI Language.

## Architecture:
- Positional encoding: learned embeddings
- Self-attention mechanism

## Pre-training:
Trained on a large corpus of text data using two tasks:
- Masked Language Model (MLM)
- Next Sentence Prediction (NSP)

The training corpus is a combination of the BooksCorpus (800M words) and English Wikipedia (2,500M words, only text no list, table, headers).

## Model size:
### BERT-base:
- embedding size: 768
- layers: 12
- attention heads: 12
Total parameters: 110M
### BERT-large:
- embedding size: 1024
- layers: 24
- attention heads: 16
Total parameters: 345M

The input sequence length is 512 tokens. This is due to the positional encoding, which is limited to 512 tokens.

## BERT Variants:
- DistilBERT: A smaller, faster, and lighter version of BERT that retains 97% of its language understanding while being 60% faster and 40% smaller.
- RoBERTa: A robustly optimized version of BERT that removes the NSP objective and trains with much larger mini-batches and learning rates.
- ALBERT: Replaced NSP by Sentence Order Prediction (SOP), and factorized embedding parameterization to reduce the number of parameters.
- ELECTRA: Replaces the masked language model with a generator-discriminator setup, where a small generator model generates corrupted tokens and a larger discriminator model predicts whether each token is real or fake.

# ModernBERT
## Introduction
ModernBERT has been introduced in December 2024 by Answer AI and Lighton AI. It is a lightweight and efficient version of BERT, designed to be faster and more memory-efficient while maintaining high performance on various NLP tasks.

Here are the summary of the key features and improvements of ModernBERT:
- **RoPE positional encoding**: use of RoPE (Rotary Positional Encoding) to improve the model's ability to capture long-range dependencies in text.
- **Flash attention**: reduces the computational complexity of the self-attention mechanism, making it faster and more efficient.
- **Altenating attention**: alternate between global and local attention. 
- **Unpadding and Sequence Packing**
- **Paying Attention to Hardware**
- **Bias**: removed all bias terms in all linear layers, except for the final decoder layer, and also in the Layer Norms. 
- **Layer Norm**: added a normalization layer after the embedding layer.
- **Activation function**: replaced the GeLU activation function with a GeGLU.

## Alternating Attention
The attention mechanism only attends the full input sequence every three layers (global attention), while the other layers focus on the 128 tokens nearest to itself (local attention). This allows for a faster processing of the input sequence.

<figure style="text-align:center">
  <img src="images/ModernBert_Alternating_attention.png" alt="Attention" width="80%" />
    <figcaption>Source: <a href="https://huggingface.co/blog/modernbert" target="_blank">HF blog</a></figcaption>
</figure>


## Unpadding and Sequence Packing
For processing multiple sequences in a batch, we need to pad the sequences to the same length. Otherwise, some thread becomes idle while waiting for the longest sequence to finish. This is inefficient and lead to wasted computational resources. 

However, padding leads to a lot of wasted memory and computation. Then we remove them and pack them into a mini-batch size of 1. 

## Paying attention to Hardware
The architecture of the model is designed to be hardware-aware, meaning that it takes into account the specific hardware on which it will be run. This comes from these two observations:

1. Deep & Narrow vs Wide & Shallow: Research shows that deeper models with narrower layers, often perform better than shallow models with fewer, wider layers. However, this is a double-edged sword: the deeper the model, the less parallelizable it becomes, and thus, the slower it runs at identical parameter counts.

2. Hardware Efficiency: Model dimensions need to align well with GPU hardware for maximum performance, and different target GPUs result in different constraints.

## Training
**Tokenizer**: used the BPE tokenizer with a vocabulary size of 50304. This number does have an impact on the performance of the model, Karpathy found that the optimal vocabulary size that going from 50257 to 50304 which is the nearest multiple of 64 yields a 25% performance improvement.

**Data**: 2 trillion tokens of text data, including primarily English data from a variety of data sources, including web documents, code, and scientific literature.

**Optimizer**: StableAdamW


## Model Size:
### ModernBERT-base:
- embedding size: 768
- layers: 22
- attention heads: 12

### ModernBERT-large:
- embedding size: 1024
- layers: 28
- attention heads: 16