# Temperature

## 1. What is Temperature?

**Temperature** is a sampling parameter that controls **how randomly the LLM selects the next token**.

> **Low temperature → more predictable output**
> **High temperature → more diverse/creative output**

---

## 2. How It Works

An LLM predicts probabilities for the next token.

Example:

```text
Prompt:
"The capital of France is"

Possible tokens:

Paris      → 95%
London     → 2%
Berlin     → 1%
Other      → 2%
```

### Low Temperature

The model strongly prefers the highest-probability token.

```text
Temperature ↓
     ↓
Less randomness
     ↓
More predictable output
```

### High Temperature

The model is more willing to select lower-probability tokens.

```text
Temperature ↑
     ↓
More randomness
     ↓
More diverse output
```

---

## 3. Temperature Range

The exact allowed range depends on the model/API, but commonly:

```text
0 ─────────────────────── 1+
│                          │
Low                      High
│                          │
Predictable              Creative
```

### Typical Understanding

| Temperature | Behavior           | Suitable For                   |
| ----------- | ------------------ | ------------------------------ |
| **0–0.2**   | Very deterministic | Classification, extraction     |
| **0.3–0.5** | Controlled         | Summarization, Q&A             |
| **0.6–0.8** | More creative      | General generation             |
| **0.9+**    | Highly diverse     | Creative writing/brainstorming |

> These are **practical guidelines**, not universal rules. Model behavior varies by provider and model.

---

## 4. Real-World Example

### Task: Extract information

```text
"Is this email urgent? Answer Yes or No."
```

Use:

```text
Temperature = 0
```

Because you want:

```text
Yes
```

not:

```text
Yes, I believe this email is probably urgent...
```

---

### Task: Generate marketing ideas

```text
"Give me 10 creative names for an AI startup."
```

Use a higher temperature.

You want different possibilities:

```text
NeuraFlow
MindSync
CortexAI
Synapse...
```

---

# 5. Temperature in Syncra

Suppose Syncra generates a **daily briefing**.

### Factual task

```text
Summarize today's important messages.
```

Use a **low temperature** because you want:

* Consistency
* Accuracy
* Less randomness
* Reliable formatting

```text
Gmail + Slack
      ↓
     RAG
      ↓
LLM (Low Temperature)
      ↓
Daily Briefing
```

### Creative task

Suppose Syncra has:

> "Suggest creative ways to organize my tasks."

A higher temperature can be useful.

---

# 6. Important: Temperature Does NOT Mean Intelligence

A common misconception:

> "Higher temperature makes the model smarter."

❌ **Not necessarily.**

Temperature mainly changes **sampling/randomness**, not the underlying intelligence or knowledge of the model.

```text
Temperature
     ↓
Sampling behavior
     ↓
Different possible outputs
```

It does **not**:

```text
Temperature
     ↓
More knowledge ❌
```

---

# 7. Temperature vs Deterministic Tasks

For tasks such as:

* JSON extraction
* Classification
* Data extraction
* Tool calling
* Structured outputs

generally prefer **low temperature** when the API/model supports it.

Example:

```text
Email
  ↓
LLM
  ↓
{
  "priority": "high",
  "action_required": true
}
```

You want consistent results.

---

# 8. Quick Mental Model

```text
              TEMPERATURE
                   │
        ┌──────────┴──────────┐
        ↓                     ↓
      LOW                   HIGH
        ↓                     ↓
   Less random            More random
        ↓                     ↓
   Consistent              Diverse
        ↓                     ↓
  Extraction              Creativity
  Classification          Brainstorming
  Structured tasks        Creative writing
```

## One-line Interview Answer

> **“Temperature is a sampling parameter that controls the randomness of an LLM's output. Lower temperature produces more predictable responses, while higher temperature produces more diverse and creative responses.”**
