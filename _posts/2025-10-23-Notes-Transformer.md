---
layout: post
title: Large Models and Training Knowledge Points
date: 2025-10-23 18:30:00-0400
description: A quick note on Transformer Model
tags: LLM AI math
related_posts: false
toc:
  sidebar: left
---

[Open PDF](/assets/pdf/Transformer_Notes.pdf)


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/Transformer_1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The transformer architecture.
</div>


# **1. Model Principles**

## **1.1 Transformer**

### Architecture Overview

The Transformer consists of an **Encoder–Decoder** structure, primarily composed of the following modules:

* **Encoder:**  
  - Multi-head self-attention mechanism  
  - Feedforward neural network  
  - Layer normalization  
  - Residual connection  

* **Decoder:**  
  - Masked multi-head self-attention  
  - Encoder–decoder attention  
  - Feedforward neural network  

### Encoder

**Functional overview of each component:**

- **Positional Encoding** – absolute or relative position encoding enables sequence modeling. It maps the semantic and positional information of tokens into the **same vector space**, allowing self-attention to jointly process both types of information. This makes the attention mechanism directly exploit their joint similarity while keeping parameter count constant and improving training efficiency.  
  $$ z_i = x_i + p_i $$  
  Here, $$x_i$$ encodes semantic meaning and $$p_i$$ encodes positional information.

- **Multi-head attention** – the Multi-Head mechanism improves representational capacity by splitting parameters into multiple attention heads, allowing parallel feature extraction.

- **Feedforward neural network (FNN)** – applied after the attention mechanism to independently transform each token representation through nonlinear mappings:  
  $$ \text{FNN}(x) = W_2 \, \text{ReLU}(W_1 x + b_1) + b_2 $$
  Example dimensions: $$d_{\text{model}} = 512$$, $$d_{\text{ff}} = 2048$$.  
  For each token:
  - $$W_1: (2048 \times 512)$$  
  - $$W_2: (512 \times 2048)$$  
  Used for:  
  (a) learning nonlinear mappings,  
  (b) extracting per-position features,  
  (c) expanding and compressing feature space to enhance expressivity.  

- **Layer normalization**

- **Residual connection**

### Decoder

The decoder has a similar structure to the encoder but introduces **an extra attention layer** to connect with encoder outputs.

1. **Masked multi-head self-attention:**  
   Since text is generated sequentially, the model must satisfy the **causal constraint** — each token can only depend on previous tokens.  
   This is achieved by masking future positions in the attention weights, enforcing **auto-regressive generation**.

2. **Encoder–decoder attention:**  
   Acts as a bridge between the **encoder’s contextual representation** and the **decoder’s generated output**.  
   - **Query (Q):** from the decoder (currently generated sequence).  
   - **Key (K), Value (V):** from the encoder (semantic representation of the input).

---

## **1.2 Attention Mechanism**

**Advantages:**
- Captures long-range dependencies (no vanishing gradient as in RNNs).
- Enables parallel computation (tokens processed independently).
- Attention weights are interpretable and visualizable.
- Handles variable-length input sequences.

---

### Mathematical foundation (Q–K–V matrices + softmax normalization)

#### QKV Matrix Computation

Each input token is linearly projected to obtain Query (Q), Key (K), and Value (V):

$$
Q = X W_Q, \quad K = X W_K, \quad V = X W_V
$$

where  
$$
X \in \mathbb{R}^{n \times d_{\text{model}}}
$$
$$n$$ = sequence length, $$d_{\text{model}}$$ = embedding dimension.

- $$W_{Q}$$, $$W_{K}$$, $$W_{V}$$ are trainable projection matrices.  
- $$d_k$$ = attention dimension, usually set as $$d_k = \frac{d_{\text{model}}}{h}$$.

---

### Self-Attention Mechanism

Compute attention weights as scaled dot-products between queries and keys, then use them to weight values:

$$
\text{Attention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}}\right) V
$$

The scaling factor $$\sqrt{d_k}$$ prevents gradient vanishing/exploding during training.

---

### Multi-Head Attention

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) W_O
$$

where each head is:
$$
\text{head}_i = \text{Attention}(Q W_i^Q, K W_i^K, V W_i^V)
$$

- $$h$$: number of heads  
- Each head has independent learnable matrices $$W_i^Q$$, $$W_i^K$$, $$W_i^V$$.  
- Outputs are concatenated and projected to $$d_{\text{model}}$$.  
- This allows the model to attend to different representation subspaces and features simultaneously.

---

### Masked Self-Attention

Used during generation to prevent tokens from attending to future positions:

$$
\text{MaskedAttention}(Q, K, V) = \text{softmax}\!\left(\frac{QK^\top}{\sqrt{d_k}} + M\right)V
$$

where the mask matrix $$M = [M_{ij}]$$ satisfies:
$$
M_{ij} =
\begin{cases}
0, & i \ge j \\
-\infty, & i < j
\end{cases}
$$

This ensures causal, left-to-right generation.

---


# **2. BERT, GPT, T5 architectures**
Pretrained language models based on Transformers have achieved major breakthroughs in the field of natural language processing (NLP).
Architecture Categories:
* Encoder models: contain only the encoder component; suitable for extracting contextual representations (e.g., BERT).
* Decoder models: contain only the decoder component; suitable for autoregressive generation tasks (e.g., GPT).
* Encoder–Decoder models: combine both components; suitable for sequence-to-sequence tasks (e.g., T5).


## BERT: Bidirectional Encoder Representation from Transformers

1. **Encoder-only architecture** — uses only Transformer encoder layers.  
2. **Pretraining process:** conducted through unsupervised learning, using **Masked Language Modeling (MLM)** and **Next Sentence Prediction (NSP)** tasks.  
   - **Masked Language Model (MLM):** randomly masks certain words in the input text, and the model must predict the masked words.  
   - **Next Sentence Prediction (NSP):** trains the model to determine whether two given sentences are consecutive in the original text, improving contextual understanding.  
3. **Fine-tuning process:** adapts the pretrained model to specific downstream tasks (e.g., text classification, QA).

## GPT: Generative Pretrained Transformer

1. **Decoder-only architecture** — uses a unidirectional autoregressive model composed solely of decoder layers.  
2. **Pretraining:** uses the **Causal Language Modeling (CLM)** task — predicting the next token based on all preceding tokens.  
3. **Fine-tuning:** primarily used for text generation tasks, such as dialogue generation and summarization.

## T5: Text-to-Text Transfer Transformer

1. **Full Encoder–Decoder architecture.**  
2. **Pretraining:**  
   - **Text infilling:** similar to BERT’s MLM but can mask multiple consecutive tokens.  
   - **Denoising:** randomly deletes segments or words in the input text, and the model must reconstruct the original text.  
3. **Fine-tuning:** applicable to a wide variety of text generation tasks, including summarization, translation, and text classification.

---

# **3. Pretraining**

Pretraining refers to learning language patterns on large-scale raw corpora (typically internet-scale data) through self-supervised tasks (e.g., next-word prediction or Masked LM). Its goal is to equip the model with general language understanding and generation capabilities, rather than solving a specific task.

## Data Scale: From Millions to Trillions of Tokens

The amount of data required for pretraining mainly depends on the model size (number of parameters).  
Empirical rules come from the Scaling Laws (Kaplan et al., 2020): the larger the model, the more training tokens are needed in proportion; otherwise, the model becomes undertrained.  

| Model Size    | Representative Model        | Training Tokens (Empirical) | Estimated Data Volume |
| ------------- | --------------------------- | --------------------------- | ------------------- |
| ~100M params  | Small language models (e.g., DistilGPT) | 10⁹ (1B)                   | Tens of GB          |
| ~1B params    | GPT-2 Small                | 10¹⁰ (10B)                  | Hundreds of GB      |
| ~6B params    | GPT-J / GPT-NeoX           | 10¹¹ (100B)                 | TB scale            |
| ~70B params   | LLaMA-2 / Falcon-70B       | 2×10¹² (2T)                 | Tens of TB          |
| ~1T params    | GPT-4 level                | 10¹³ (10T)                  | Hundreds of TB      |

* **Empirical ratio rule:** Ideal training tokens ≈ **20×number of parameters (in tokens)**. For example, a 70B model should ideally be trained on ~1.4T tokens.

## Training Time

Assuming we use A100 GPUs (80GB) or H100 GPUs (80GB):  

| Model          | GPU Type       | Number of GPUs | Data Volume (Tokens) | Estimated Training Time |
| -------------- | -------------- | --------------- | ------------------- | ---------------------- |
| GPT-2 (1.5B)   | V100 × 32      | 32 GPUs         | 40B                 | ~1 week                |
| GPT-J (6B)     | A100 × 64      | 64 GPUs         | 400B                | ~2-3 weeks             |
| LLaMA-2 70B    | A100 × 2048    | 2048 GPUs       | 2T                  | ~3-4 weeks             |
| GPT-4 level    | H100 × 10,000+ | Tens of thousands | 10T               | Several months         |

## Data Preparation Workload

Pretraining corpora usually come from a mix of sources:

| Data Type        | Proportion | Source                        | Purpose              |
| ---------------- | ---------- | ----------------------------- | ------------------ |
| Web Text         | 50-70%     | Common Crawl, Wikipedia       | Language diversity  |
| Books / Papers   | 10-20%     | BooksCorpus, ArXiv            | Long context, logic |
| Code             | 10-20%     | GitHub, StackOverflow         | Program understanding |
| Dialogue / QA    | 5-10%      | OpenAssistant, Reddit         | Interactive structure |

Data cleaning often takes more time than training itself. These steps include:

* Deduplication  
* Language detection  
* Spam filtering  
* Text normalization (tokenize, normalize)


---
# **4. Fine-Tuning and Instruction Alignment**

## 4.1  SFT (Supervised Fine-Tuning) and Continued Pretraining

**Supervised Fine-Tuning (SFT)** adapts a pretrained model to a specific task using labeled data or instruction-response pairs.

### Step 1 – Define Tasks and Instructions
- **Task definition:** specify what the model should do (e.g., text classification, summarization, translation).  
- **Instruction design:** build prompt templates such as  
  *“Classify the following text as positive or negative.”*

### Step 2 – Collect Raw Data
- Use public datasets, domain-specific corpora, or crowdsourcing.  
- Clean and normalize data (remove duplicates, fix encoding, etc.).

### Step 3 – Label Data
- Establish clear annotation rules for consistency.  
- Apply human labeling or semi-automatic labeling tools.

### Step 4 – Construct Instruction Dataset
- Split data into training / validation / test sets.  
- Store in structured formats (JSON, CSV, etc.).

### Step 5 – Enhance Data Diversity
- Use synonym replacement or paraphrasing for augmentation.  
- Cover multiple use-case scenarios.

### Step 6 – Evaluate and Iterate
- Run small-scale evaluation to check performance.  
- Refine prompts or add examples for better alignment.

> ⚠ SFT may cause **over-specialization**—the model can lose generality and exploratory ability.  
> A common remedy: mix **instruction + pretraining data**, and follow SFT with preference optimization (DPO / PPO / ORPO).


---

