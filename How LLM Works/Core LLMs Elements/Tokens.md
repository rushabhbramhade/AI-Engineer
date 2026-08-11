# Token — AI Engineer Notes

> **Core idea:** An LLM does not directly process your text as words. It processes **tokens represented as numbers (token IDs)**.

### 1. Character vs Word vs Token

| Unit          | Meaning                           | Example           |
| ------------- | --------------------------------- | ----------------- |
| **Character** | Single symbol/letter              | `A`, `?`, `7`     |
| **Word**      | Human-language word               | `Artificial`      |
| **Token**     | Chunk of text chosen by tokenizer | `Art` + `ificial` |

**Important:**
`1 token ≠ 1 word`

A token can be:

* a whole word → `cat`
* part of a word → `token` → `tok` + `en`
* punctuation → `.`
* whitespace/space-related text
* sometimes multiple characters

---

### 2. Why use Tokens instead of whole words?

If AI stored only complete words:

* Vocabulary would become **huge**
* New/rare words would be difficult to represent
* Different word forms need separate entries
* Unknown words become a problem

**Tokens solve this using subwords.**

Example:

`unhappiness` → `un` + `happi` + `ness`

So the model can understand **new words from familiar pieces**.

**AI Engineer takeaway:**

> Tokenization gives LLMs a practical balance between **vocabulary size + ability to represent new text**.

---

### 3. How Tokenization Works

```text
User Text
   ↓
Tokenizer
   ↓
Split into tokens
   ↓
Token IDs (numbers)
   ↓
Embeddings
   ↓
LLM
```

Example:

```text
"I love AI"
      ↓
["I", " love", " AI"]
      ↓
[40, 1845, 2876]   ← example IDs
```

> Token IDs are then converted into **vectors (embeddings)** that the neural network processes.

---

### 4. Why common words can be tokens

Frequent text patterns are often represented efficiently.

```text
"the" → likely one token
"playing" → may be one token or multiple
"unbelievable" → may be multiple tokens
```

Tokenizer vocabulary is learned/designed from text patterns, so **frequent patterns tend to get efficient representations**.

---

### 5. Different Languages → Different Tokenization

Token efficiency varies by language.

```text
English sentence
→ often relatively efficient

Rare language / unusual text
→ may require more tokens
```

Factors include:

* vocabulary used to build the tokenizer
* writing system
* frequency of text patterns
* language morphology

**Important:** Same meaning ≠ same token count.

---

### 6. Token Limits & Context Window

**Context window = maximum amount of tokens an LLM can consider in one request/context.**

```text
Context Window
┌──────────────────────────────┐
│ system instructions          │
│ conversation                 │
│ user prompt                  │
│ documents                    │
│ tool results                 │
│ model output                 │
└──────────────────────────────┘
          ↓
       Token limit
```

If context becomes too large:

```text
Too many tokens
      ↓
Context limit reached
      ↓
Need to shorten / summarize / retrieve relevant data
```

**AI Engineer relevance:**
Long documents, RAG, chat history, agents and tool outputs all consume context.

---

### 7. Token ≠ Memory

**Token = unit of text processing.**
**Context = information currently available to the model.**

A model doesn't automatically remember everything from previous conversations just because tokens existed before.

```text
Tokens → current input/context
Memory → system mechanism for storing/retrieving information
```

---

### 8. Tokens & API Cost

Many LLM APIs charge based on token usage.

```text
Input tokens
+
Output tokens
=
Total token usage
```

More tokens → generally **higher cost + potentially more latency**.

Example:

```text
100K tokens input
+ 10K tokens output
= 110K tokens processed
```

**AI Engineer optimization:**

* reduce unnecessary prompt text
* summarize old conversation
* retrieve only relevant documents
* avoid sending duplicate context
* control output length

---

## ⭐ Remember This

```text
TEXT
 ↓
TOKENIZER
 ↓
TOKENS
 ↓
TOKEN IDs
 ↓
EMBEDDINGS / MODEL PROCESSING
 ↓
LLM OUTPUT
```

### Interview Answer

**What is a token?**

> A token is a small unit of text produced by a tokenizer. It can represent a complete word, part of a word, punctuation, or other text patterns. LLMs convert tokens into numerical representations and process them to generate output.

### AI Engineer Mental Model

**Tokenization → Context → Cost → Latency → Model limits**

This is why **token awareness matters when building LLM apps, RAG systems, chatbots and AI agents.**
