# Sampling Parameters — AI Engineer Notes

## 1. What is Sampling?

An LLM doesn't directly "choose a sentence."

It predicts the **probability of the next token**.

Example:

```text
Prompt:
"The sky is"

Possible next tokens:

blue    → 70%
cloudy  → 15%
dark    → 8%
green   → 2%
...
```

The model uses these probabilities to select the next token.

```text
Prompt
  ↓
LLM
  ↓
Probability distribution
  ↓
Sampling strategy
  ↓
Next token
  ↓
Repeat
  ↓
Complete response
```

### Core idea

> **Sampling parameters control how the model chooses tokens from its probability distribution.**

***

# 2. Why Sampling is Needed?

Without sampling, the model could always choose the highest-probability token.

```text
Highest probability
       ↓
Always choose it
       ↓
More deterministic
       ↓
Less variation
```

Sampling allows the model to sometimes choose other **plausible** tokens.

This creates variation and creativity.

***

# 3. Main Sampling Parameters

For AI engineering, know these:

| Parameter       | Controls                       | Main use                  |
| --------------- | ------------------------------ | ------------------------- |
| **Temperature** | Randomness                     | Creativity vs consistency |
| **Top-K**       | Number of candidate tokens     | Restrict choices          |
| **Top-P**       | Probability mass of candidates | Adaptive restriction      |
| **Min-P**       | Minimum probability threshold  | Remove unlikely tokens    |
| **Seed**        | Randomness reproducibility     | Testing/debugging         |

> Exact parameter availability depends on the model/API.

***

# 4. Temperature ⭐⭐⭐

### What is Temperature?

**Temperature controls how strongly the model favors high-probability tokens.**

### Low temperature

```text
Temperature ↓
      ↓
High-probability tokens favored
      ↓
More predictable
```

Example:

```text
Temperature = 0.1

"The capital of France is..."
→ Paris
```

Good for:

- factual answers
- classification
- structured output
- extraction
- predictable workflows

***

### High temperature

```text
Temperature ↑
      ↓
More probability spread
      ↓
More varied choices
```

Useful for:

- brainstorming
- creative writing
- generating ideas
- conversational variation

### Important

**High temperature does NOT mean the model becomes more intelligent.**

It mainly changes **output randomness/variation**.

***

# 5. Temperature Mental Model

Think of it as:

```text
LOW
🔥
"Choose the safest likely option."

HIGH
🔥🔥🔥🔥🔥
"Consider more possible options."
```

Example:

Prompt:

> Give me a startup name for an AI coding tool.

Low temperature:

```text
CodeAI
```

Higher temperature:

```text
NeuraForge
CodePilot
SynapseFlow
DevMind
```

***

# 6. Top-K

**Top-K limits the model to the K most probable next tokens.**

Example:

```text
Possible tokens:

A → 40%
B → 25%
C → 15%
D → 10%
E → 5%
F → 5%
```

If:

```text
Top-K = 3
```

Only:

```text
A
B
C
```

are considered.

```text
All tokens
   ↓
Top-K filter
   ↓
K most probable tokens
   ↓
Sampling
```

### Example

```text
Top-K = 1
→ only highest probability token

Top-K = 5
→ five most likely tokens

Top-K = 50
→ fifty candidates
```

### AI Engineer use

Useful when you want to **limit unlikely choices**.

***

# 7. Top-P / Nucleus Sampling ⭐⭐⭐

This one is extremely important.

Instead of saying:

> "Give me exactly K tokens."

Top-P says:

> **"Keep the smallest set of tokens whose combined probability reaches P."**

Example:

```text
Tokens:

A → 50%
B → 25%
C → 15%
D → 5%
E → 5%
```

If:

```text
Top-P = 0.90
```

We need:

```text
A + B + C
50% + 25% + 15%
= 90%
```

So:

```text
A ✓
B ✓
C ✓
D ✗
E ✗
```

***

# 8. Top-K vs Top-P

### Top-K

Fixed number:

```text
Top-K = 5
→ always consider 5 tokens
```

### Top-P

Dynamic number:

```text
Top-P = 0.9
→ number of tokens depends on probability distribution
```

### Easy memory trick

> **Top-K = fixed number of choices**

> **Top-P = probability-based number of choices**

***

# 9. Temperature + Top-P Together

They can be used together, depending on the API/model.

Conceptually:

```text
LLM probabilities
       ↓
Temperature
       ↓
Top-P / Top-K filtering
       ↓
Sample next token
```

But **don't blindly tune everything**.

For production systems:

> Change one parameter at a time and evaluate the actual task.

***

# 10. Min-P

Min-P is another filtering strategy supported by some inference systems.

It removes tokens whose probability is too small relative to the highest-probability token.

Example:

```text
Highest token = 50%

Min-P = 0.1

Minimum allowed:
50% × 0.1 = 5%
```

Tokens below that threshold are removed.

```text
50% ✓
20% ✓
10% ✓
5%  ✓
2%  ✗
```

### Why useful?

It can prevent very unlikely tokens from being sampled while still allowing the candidate set to adapt to the model's confidence.

***

# 11. Seed

A **seed** controls the starting point of the random number generator used during sampling.

If supported and the rest of the system is sufficiently deterministic:

```text
Same prompt
+
Same parameters
+
Same seed
        ↓
More reproducible output
```

Useful for:

- debugging
- testing
- evaluations
- experiments

⚠️ Same seed does **not always guarantee identical output**, because backend/model implementation, hardware, routing, or other nondeterminism can affect results.

***

# 12. Greedy Decoding

You should also know this.

**Greedy decoding = always choose the highest-probability token.**

```text
Probability:

A → 60%
B → 25%
C → 15%

Choose A
```

Then repeat for the next token.

```text
Greedy
  ↓
Most probable token
  ↓
Next prediction
  ↓
Most probable token
  ↓
...
```

It is highly predictable but can produce less varied text.

***

# 13. Sampling vs Greedy

```text
                 Token selection
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
       Greedy                    Sampling
          ↓                         ↓
Highest probability         Probability-based choice
          ↓                         ↓
Predictable                 More variation
```

***

# 14. Deterministic vs Creative Tasks

This is what matters in real AI engineering.

### Structured / deterministic tasks

Examples:

```text
JSON extraction
Classification
Data extraction
Intent detection
SQL generation
Tool calling
```

Usually prefer:

```text
Low randomness
+ strict output schema
+ validation
```

### Creative tasks

Examples:

```text
Story generation
Marketing ideas
Brainstorming
Names
Creative writing
```

Can benefit from:

```text
Higher randomness
+ broader candidate selection
```

***

# 15. Sampling Parameters ≠ Model Intelligence

Very important interview point:

> **Sampling parameters change how the model selects its output; they don't increase the model's underlying intelligence or knowledge.**

For example:

```text
Temperature ↑
```

doesn't mean:

```text
Model IQ ↑
```

It means:

```text
Output variation ↑
```

***

# 16. Real AI Engineer Example

Suppose you're building an **AI customer-support agent**.

You don't want:

```text
Customer:
Can I cancel my order?

AI:
Maybe you could cancel it...
Perhaps...
I think...
```

You want consistent behavior.

So you'd generally favor:

```text
Lower randomness
+
structured instructions
+
tool/API verification
+
output validation
```

For an AI marketing assistant:

```text
"Give me 10 creative campaign ideas."

```

More variation can be useful.

***

# 17. Sampling Parameters and Agents

This is especially important.

For an **AI agent**, different steps can require different behavior.

```text
                    AI AGENT
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
   Reasoning       Tool calling      Writing
       │               │                │
   Controlled       Stable          More creative
   generation       output           output
```

Don't assume one temperature is optimal for every step.

***

# 18. Common Mistakes

### ❌ Mistake 1

"Temperature = intelligence."

**Wrong.**

Temperature controls sampling behavior.

***

### ❌ Mistake 2

"Top-P = number of tokens."

**Wrong.**

Top-P controls **cumulative probability mass**.

***

### ❌ Mistake 3

"High temperature always gives better answers."

**Wrong.**

It gives more variation, which can sometimes make outputs worse.

***

### ❌ Mistake 4

"Set every parameter to maximum."

**Wrong.**

Sampling parameters interact and excessive randomness can reduce reliability.

***

### ❌ Mistake 5

"Same seed guarantees identical results everywhere."

**Wrong.**

It improves reproducibility when supported, but doesn't guarantee perfect determinism.

***

# 19. The Most Important Mental Model

Remember this:

```text
                 LLM
                  ↓
       Probability distribution
                  ↓
       ┌──────────┴──────────┐
       ↓                     ↓
  Temperature          Token filtering
                             │
                       ┌─────┴─────┐
                       ↓           ↓
                    Top-K        Top-P
                       │           │
                       └─────┬─────┘
                             ↓
                         Sampling
                             ↓
                       Next token
                             ↓
                    Repeat until done
```

***

# ⭐ AI Engineer Cheat Sheet

```text
Temperature
→ How much randomness/variation?

Top-K
→ How many highest-probability tokens?

Top-P
→ How much cumulative probability to consider?

Min-P
→ Remove tokens below relative probability threshold?

Seed
→ Help reproduce sampling behavior?

Greedy
→ Always choose highest-probability token?
```

### One-line interview answer

> **Sampling parameters control how an LLM selects the next token from its probability distribution. Temperature controls randomness, Top-K limits the candidate count, Top-P limits candidates by cumulative probability, Min-P removes very unlikely candidates, and seed helps with reproducibility.**

### 🔥 What you actually need to remember as an AI Engineer

**Don't spend most of your time memorizing parameter values.**

Understand this decision:

```text
Need consistency?
→ Reduce randomness + validate output

Need creativity?
→ Allow more variation

Need reliable agents?
→ Control generation + validate tool calls/results

Need production quality?
→ Evaluate outputs instead of tuning blindly
```

That is the practical knowledge that transfers directly to building **LLM apps, RAG systems, agents, and production AI features**.
