# Context Window & Tokenization

## 1. How Tokenization Works in Context

**Context = all tokens currently given to the LLM for generating the next response.**

**Context window = the AI's short-term working memory, measured in tokens.**

Better interview wording:

&#x20;

> **"The context window is the amount of tokenized information the model can consider at one time."**

Because **context ≠ permanent memory**.



```
Context Window → Short-term working memory
Memory System  → Information stored/retrieved separately
```

```text
User message
     ↓
Tokenizer
     ↓
Token IDs
     ↓
┌─────────────────────────────┐
│       CONTEXT WINDOW        │
│ System instructions         │
│ Conversation history        │
│ Retrieved documents (RAG)   │
│ Tool/API results             │
│ Current user message        │
└─────────────────────────────┘
     ↓
LLM processes tokens
     ↓
Output tokens
```

### Key point

The model doesn't see:

> "What is tokenization?"

as a human-readable sentence internally.

It receives something conceptually like:

```text
Text → Tokens → Token IDs → Model
```

***

# 2. How User Actions Affect Context

Every time the user interacts with an AI application, **new tokens can enter the context**.

Example:

```text
User: Explain RAG
        ↓
Context grows

User: Give example
        ↓
Previous conversation + new message
        ↓
Context grows

User: Now write code
        ↓
Conversation + code + previous output
        ↓
Context grows more
```

So in a long conversation:

```text
Turn 1 → 1K tokens
Turn 2 → 3K tokens
Turn 3 → 7K tokens
Turn 4 → 15K tokens
...
```

The exact behavior depends on how the application manages history; **the entire conversation is not necessarily sent every time**.

***

# 3. What Makes Up a Context Window?

Think:

```text
CONTEXT WINDOW
│
├── System prompt
├── Developer instructions
├── Conversation history
├── Current user input
├── RAG retrieved documents
├── Tool/API results
└── Other application data
```

All of these compete for the model's available context.

### AI Engineer Rule

> **Context is a limited resource. Don't send everything; send what the model needs.**

***

# 4. What is Context Length?

**Context length = maximum number of tokens a model can process as its context.**

Example:

```text
Model context = 128K tokens

System + history + RAG + tools + user input + output
                 ↓
             must fit
```

⚠️ The exact context size **depends on the model/provider** and can change as models are updated.

So don't memorize one universal number.

***

# 5. Context Window vs Memory

Very important for AI engineering:

| Context                                 | Memory                                  |
| --------------------------------------- | --------------------------------------- |
| Information currently supplied to model | Information stored for future retrieval |
| Limited by context length               | Can be stored externally                |
| Directly available to model             | Retrieved when needed                   |
| Usually token-based                     | Database/vector store/etc.              |

Example:

```text
Long-term user information
        ↓
Database / Memory Store
        ↓
Retrieve relevant information
        ↓
Context Window
        ↓
LLM
```

This is a major pattern in **AI agents and RAG systems**.

***

# 6. What Should an AI Engineer Keep in Mind?

### ① Context is limited

Don't blindly put the entire conversation/database into the prompt.

***

### ② More context ≠ better answer

```text
Too little context
→ Model lacks information

Useful context
→ Better answer

Too much irrelevant context
→ Higher cost + latency + possible confusion
```

**Goal = relevant context, not maximum context.**

***

### ③ Token budget

Always think about:

```text
Input tokens
+ Output tokens
+ Tool/RAG tokens
= Token usage
```

This affects **cost and latency**.

***

### ④ RAG retrieval

For large knowledge bases:

```text
User question
      ↓
Retrieve relevant chunks
      ↓
Put selected chunks into context
      ↓
LLM
```

Don't send the entire database.

***

### ⑤ Conversation management

Long chats can become expensive.

Common techniques:

```text
Old conversation
      ↓
Summarization
      ↓
Compact context
```

Other approaches:

- keep recent messages
- summarize older messages
- retrieve relevant past messages
- remove unnecessary tool outputs

***

### ⑥ Tool outputs can explode context

Agent:

```text
LLM
 ↓
Search API
 ↓
10,000 lines returned
 ↓
Context becomes huge
```

**Better:**

```text
Tool
 ↓
Filter / summarize
 ↓
Relevant results only
 ↓
LLM
```

***

# 7. Major Context Challenges for AI Engineers

You don't need to memorize dozens. Know these **8 core challenges**:

| Challenge            | Problem                            | Common solution               |
| -------------------- | ---------------------------------- | ----------------------------- |
| **Context limit**    | Too many tokens                    | Truncate/summarize            |
| **Context bloat**    | Unnecessary information            | Filter/compress               |
| **Lost information** | Important old info disappears      | Memory/retrieval              |
| **RAG noise**        | Irrelevant chunks retrieved        | Better retrieval/reranking    |
| **Cost**             | Too many tokens                    | Reduce context                |
| **Latency**          | Large prompts are slower           | Smaller context               |
| **Tool output**      | APIs return huge data              | Filter/transform              |
| **Context quality**  | Correct info but poor organization | Structure/context engineering |

***

# 8. Real-World AI Engineer Example

Suppose you're building a **customer-support AI agent**.

Customer has 200 previous messages.

❌ Bad architecture:

```text
200 messages
+ entire product documentation
+ entire database results
+ huge API responses
        ↓
       LLM
```

Problems:

**expensive + slow + noisy + context limit risk**

### Better architecture

```text
Customer question
       ↓
Retrieve relevant history
       ↓
Retrieve relevant documentation
       ↓
Fetch required customer data
       ↓
Filter / rank / compress
       ↓
Build context
       ↓
LLM
       ↓
Answer
```

### ⭐ AI Engineer mindset

> **Don't ask "How much context can I fit?"**
> Ask **"What is the minimum relevant context needed to solve this task correctly?"**

That's one of the most important practical ideas in LLM application development.
