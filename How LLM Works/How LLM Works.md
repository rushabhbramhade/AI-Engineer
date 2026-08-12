# How LLMs Work

> Notes on the fundamentals of Large Language Models — from raw text to token-by-token generation.

---

## Table of Contents

1. [What is an LLM?](#1-what-is-an-llm)
2. [Foundation Model vs LLM](#2-foundation-model-vs-llm)
3. [What Makes an LLM?](#3-what-makes-an-llm)
4. [Transformer Architecture](#4-transformer-architecture)
5. [How Does an LLM Generate Text?](#5-how-does-an-llm-generate-text)
6. [Training vs Inference](#6-training-vs-inference)
7. [Pre-training](#7-pre-training)
8. [Fine-tuning](#8-fine-tuning)
9. [Prompting vs RAG vs Fine-tuning](#9-prompting-vs-rag-vs-fine-tuning)
10. [Business Applications of LLMs](#10-business-applications-of-llms)
11. [Mental Model](#11-mental-model)
12. [Key Takeaways](#12-key-takeaways)
13. [Learning Path](#13-learning-path)

---

## 1. What is an LLM?

An **LLM (Large Language Model)** is a type of **Foundation Model** trained on a huge amount of text and code to learn patterns in language and generate useful outputs.

**Simple definition:**
> A neural network trained on large amounts of text/code to understand and generate human-like language.

**Examples:** GPT · Claude · Gemini · Llama

---

## 2. Foundation Model vs LLM

### Foundation Model

A model trained on large, diverse datasets that can be adapted for many different tasks.

```
Large Dataset
     ↓
Pre-training
     ↓
Foundation Model
     ↓
Prompting / RAG / Fine-tuning
     ↓
Different Applications
```

### LLM

An LLM is a **Foundation Model specialized for language tasks**:

- Text generation
- Question answering
- Summarization
- Translation
- Code generation
- Reasoning

---

## 3. What Makes an LLM?

```
LLM = Data + Architecture + Training
```

| Component | Description |
|---|---|
| **Data** | Books, articles, websites, documentation, conversations, source code — mostly learned via **self-supervised learning** (patterns learned from the data itself, no manual labeling needed) |
| **Architecture** | Defines *how* the network processes information — modern LLMs typically use the **Transformer** |
| **Training** | The process of adjusting parameters to reduce prediction error |

**Training loop:**
```
Training Data → Model Prediction → Calculate Error → Update Parameters → Repeat
```

---

## 4. Transformer Architecture

The **Transformer** underlies most modern LLMs. Its key idea is **Attention**.

### Attention

Attention lets the model determine:
> Which other tokens matter for understanding the current token?

**Example:**
```
"The developer fixed the bug because he found the error."
```
The model uses surrounding context to resolve what **"he"** refers to.

### Simplified LLM Flow

```
Text
 ↓
Tokenization
 ↓
Token Embeddings
 ↓
Transformer Layers (Attention + Neural Net Processing)
 ↓
Output Probabilities
 ↓
Next Token
```

---

## 5. How Does an LLM Generate Text?

LLMs generate text **one token at a time**.

**Example:**
```
Input:  "The capital of France is"
Output: "Paris"
```

The generated token is appended to the context, and the model predicts again:
```
"The capital of France is Paris" → predicts next token...
```

### Generation Loop

```
Input Tokens → Transformer → Probability of Next Token
     → Select Token → Add Token to Context → Repeat → Final Output
```

> **Core idea:** LLMs generate responses by repeatedly predicting the next token based on context.

---

## 6. Training vs Inference

| | Training | Inference |
|---|---|---|
| **What happens** | Model *learns* patterns from data | Trained model is *used* to generate output |
| **Flow** | `Huge Dataset → Training → Learned Parameters` | `User Prompt → Trained Model → Output` |
| **Cost** | Computationally expensive | Relatively cheap per request |

> **Training = Learning** · **Inference = Using the learned model**

---

## 7. Pre-training

The initial large-scale training phase where the model learns general language patterns.

```
Books + Web + Code + Articles
              ↓
         Pre-training
              ↓
        Base / Foundation Model
```

Learns:
- Grammar & language structure
- Relationships between concepts
- Code patterns
- General world knowledge present in the data

---

## 8. Fine-tuning

Adapts a **pre-trained** model for a specific task, domain, or behavior.

```
Pre-trained Model → Task-specific Dataset → Fine-tuning → Specialized Model
```

**Example:**
```
General LLM → Customer Support Dataset → Fine-tuning → Customer Support Model
```

> Fine-tuning usually means **adapting** an existing model — not training an LLM from scratch.

---

## 9. Prompting vs RAG vs Fine-tuning

Three different ways to adapt/control an LLM's behavior:

| Method | Purpose | Flow |
|---|---|---|
| **Prompting** | Tell the model what to do | `Prompt → LLM → Response` |
| **RAG** | Give the model relevant external/current info | `Question → Search KB → Relevant Info → LLM → Answer` |
| **Fine-tuning** | Adapt model behavior via training data | `Training Examples → Fine-tuning → Specialized Model` |

---

## 10. Business Applications of LLMs

### 10.1 Customer Support
```
Customer Question → LLM → Answer
```

### 10.2 Coding Assistants
```
Developer Request → LLM → Code / Explanation
```

### 10.3 Document Summarization
```
Large Document → LLM → Summary
```

### 10.4 AI Agents
```
User Goal → LLM → Tool Selection → API / Database / Service → Result → LLM → Final Response
```

### 10.5 Syncra Example
```
Gmail + Slack + WhatsApp + GitHub
              ↓
        Data Processing
              ↓
             RAG
              ↓
             LLM
              ↓
       AI Daily Briefing
```

---

## 11. Mental Model

```
                    LLM
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
       DATA     ARCHITECTURE    TRAINING
        │            │            │
   Text + Code    Transformer   Learn Patterns
                     │
                  Attention
                     │
                     ↓
                 Parameters
                     ↓
                  Inference
                     ↓
               Next Token → Next Token → ...
                     ↓
               Final Output
```

---

## 12. Key Takeaways

- **LLM = Data + Architecture + Training**
- LLMs are a type of **Foundation Model**.
- Modern LLMs commonly use the **Transformer architecture**.
- **Attention** lets the model use contextual relationships between tokens.
- LLMs generate text **token by token**.
- **Training** teaches the model; **Inference** uses the trained model.
- **Pre-training** creates a general-purpose model.
- **Fine-tuning** adapts a pre-trained model for a specific task/behavior.
- **RAG** injects external information at inference time.
- **Prompting** controls model behavior through instructions.

---

## 13. Learning Path

Suggested order for going deeper:

1. Tokenization
2. Tokens & Vocabulary
3. Embeddings
4. Transformer
5. Attention
6. Parameters
7. Pre-training
8. Next-token Prediction
9. Sampling Parameters
10. Context Window
11. Fine-tuning
12. RAG
13. Function / Tool Calling
14. AI Agents

> **Mental Model:** Data → Training → Transformer → Parameters → Context → Next-token Prediction → Output





