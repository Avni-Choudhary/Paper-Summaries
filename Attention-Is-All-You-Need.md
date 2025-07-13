# 📄 Paper Summary: Attention Is All You Need
**Authors:** Vaswani et al. (2017)  
**Published at:** NeurIPS 2017
**Citations:** 180k+  
**🔗 Paper:** [Attention Is All You Need (arXiv)](https://arxiv.org/abs/1706.03762)


INDEX
1. Overview  
2. Features  
3. Architecture
    -  Component 1: Embedding + Positional Encoding  
    - Component 2: Encoder  
    - Component 3: Decoder  
    - Final Layers  
    - Feed Forward Layer  
4. Attention Mechanism  
   - Scaled Dot Product (formula)
   - Multi-Head Attention (formula)
   - Three uses (table or bullets)
5. Hyperparameters  
6. Advantages  
7. Conclusion

---
# Overview

This seminal paper introduces the **Transformer** ,a deep learning architecture that **eliminates recurrence and convolutions**, relying entirely on a **self-attention mechanism**. Its goal is to **reduce sequential computation** and improve the efficiency and scalability of sequence transduction models (like machine translation).

The Transformer follows a traditional **encoder-decoder** structure:
- **Encoder:** Processes input sequence
- **Decoder:** Generates output sequence

Each is composed of **N = 6 identical layers** including:
- **Multi-head self-attention**
- **Feed-forward networks**
- **Residual connections + Layer Normalization**

---
# Key Features:

1. Fully parallelizable (no recurrence)
2.  Captures global context using self-attention
3.  Reduces training time significantly
4.  Backbone for BERT, GPT, T5, and modern LLMs

---
# Complete Architecture

- **Input**
    - ⮕ *Embedding + Positional Encoding*
- **Encoder (6 layers)**
    - ⮕ Outputs Encoder Representations
- **Decoder (6 layers)**
    - ⮕ Uses encoder output + masked attention
- **Linear Layer**
    - ⮕ Maps to vocabulary size
- **Softmax**
    - ⮕ Produces final output probabilities
      

---

###  COMPONENT 1: Input Embedding + Positional Encoding

####  Word Embedding

- Each token is converted into a fixed-size vector.
- The embeddings are learned.

#### Positional Encoding

- Since there's no recurrence or convolution ,a **sinusoidal positional encoding** is added to the input embedding to give each token **a sense of its position** in the sequence.


$$PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d_{model}}}\right) $$

$$PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d_{model}}}\right)$$

where:
- `pos` is the position
- `i` is the dimension
- `d_model` is the embedding size (e.g. 512)

---

### COMPONENT 2: Encoder Stack (6 Layers)

Each encoder layer has:

| Sub-layer                 | Description                                         |
| ------------------------- | --------------------------------------------------- |
| Multi-Head Self-Attention | Each token attends to all other tokens in the input |
| Add & Norm                | Residual connection + Layer Normalization           |
| Feed-Forward Network      | Two-layer fully connected network                   |
| Add & Norm                | Another residual + norm                             |

---

### COMPONENT 3: Decoder Stack (6 Layers)

Each decoder layer has:

| Sub-layer                        | Description                             |
| -------------------------------- | --------------------------------------- |
| Masked Multi-Head Self-Attention | Prevents looking ahead (causal masking) |
| Add & Norm                       |                                         |
| Encoder-Decoder Attention        | Attends to encoder’s output             |
| Add & Norm                       |                                         |
| Feed-Forward Network             |                                         |
| Add & Norm                       |                                         |
|                                  |                                         |

 The decoder can only “see” previous output tokens during training (teacher forcing).

---

###  FINAL LAYERS
####  Linear Layer

- Outputs are passed through a linear layer (dense layer) that maps to vocabulary size.

#### Softmax

- Converts logits to probabilities over the vocabulary.

---
#### Feed-Forward Network

A simple neural network( usually a 2 layer MLP) applied to each position separately and identically.

$$FFN(x) = \max(0, xW_1 + b_1)W_2 + b_2$$

---
# Attention Mechanism

Attention mechanism works to answer a question that when it is given a word(token), which other worlds in the sequence should it focus on and how much?

Attention doesn't treat all the words equally, instead it assigns different importance levels (weights) to words when processing a given token.

**Features:**
1. Captures long-range dependencies effectively
2. Learns context dynamically
3. Is fully parallelizable
4. Works well in both text and vison 

**Types:**
1. Self Attention (Intra Attention)
2. Cross Attention
3. Scalar Dot-Product Attention
4. Multi-head Attention
5. Masked Multi-head Attention

---
###  Scaled Dot-Product Attention

$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$

Where:

- Q = Queries (What are we trying to understand)
- K = Keys (What are we comparing it to)
- V = Values (The information to pull out)
- d_k = dimension of key vectors (e.g. 64)

Steps:
1. Take dot product of Q and K to get similarity scores
2. Scale by under root dk to avoid large gradients.
3. Apply softmax, this gives attention weights.
4. Multiply those weights with V to get final output.

Output:
Each word ends up with a weighted summary of all the other words based on how relevant they are.

###  Multi-Head Attention

$$\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, ..., \text{head}_h)W^O$$

Each head:

$$\text{head}_i = \text{Attention}(QW^Q_i, KW^K_i, VW^V_i)
$$

Allows the model to attend to different parts of the sequence with different representation subspaces.

---
### **Three Uses of Multi-Head Attention in the Transformer**

1. **Encoder-Decoder Attention:**
- Queries come from the decoder,
- Keys and values come from the encoder output.
- Lets each decoder position attend to all encoder positions — like classic seq2seq attention.

1. **Encoder Self-Attention:**
- Queries, keys, and values all come from the encoder itself.
- Each position attends to **all positions** in the input sequence.

3. **Decoder Self-Attention:**
- Similar to encoder, but **masked** to prevent attending to future positions.
- Preserves the **auto-regressive** (left-to-right) property for generation.

---

# Model Hyperparameters

| Term                | Value |
| ------------------- | ----- |
| Layers (N)          | 6     |
| Embedding dim       | 512   |
| Attention heads     | 8     |
| FFN inner-layer dim | 2048  |
| Dropout             | 0.1   |

---

# Advantages of This Architecture

- Fully parallelized training
- Global dependency capture through attention
- Scales well to very deep models
- Foundation of BERT, GPT, T5, and more



---

# Conclusion 

The Transformer revolutionized NLP by eliminating recurrence and enabling **fast, parallelizable training** while capturing global dependencies via attention. It set new benchmarks in translation tasks (WMT 2014 English-to-German and WMT 2014 English-to-French ) and has become the foundation of modern NLP architectures.

---








