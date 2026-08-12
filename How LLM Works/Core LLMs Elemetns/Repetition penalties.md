# Repetition Penalties

## 1. What are Repetition Penalties?

**Repetition penalties** are sampling controls used to reduce the model's tendency to **repeat the same words, tokens, or phrases**.

> They help make generated text more diverse and avoid unwanted repetition.

---

## 2. Why Are They Needed?

Without repetition control, an LLM can sometimes produce:

```text
The meeting is important.
The meeting is very important.
The meeting is really important.
The meeting is important...
```

This can happen especially during long generations.

### Problem

```text
LLM
 ↓
Repeating tokens/phrases
 ↓
Boring / low-quality output
```

### With repetition penalty

```text
LLM
 ↓
Penalize repeated tokens
 ↓
More diverse output
```

---

## 3. Common Types

Different LLM APIs use different controls.

### 1. Repetition Penalty

Penalizes tokens that have already appeared.

```text
Token appeared before
       ↓
Higher penalty
       ↓
Lower chance of repeating it
```

A common interpretation:

```text
1.0 → No penalty
>1.0 → More repetition discouraged
```

---

### 2. Frequency Penalty

Penalizes tokens based on **how many times they have already appeared**.

Example:

```text
"AI AI AI AI"
```

The more often `AI` appears, the stronger the penalty becomes.

```text
Appeared 1 time → Small penalty
Appeared 5 times → Larger penalty
```

---

### 3. Presence Penalty

Penalizes a token simply because it has **already appeared at least once**.

It encourages the model to introduce new words/topics.

```text
Token appeared once
       ↓
Penalty applied
       ↓
Encourage different token
```

---

# 4. Frequency vs Presence Penalty

| Penalty                | Main Idea                       | Effect                      |
| ---------------------- | ------------------------------- | --------------------------- |
| **Repetition Penalty** | Discourages repeated tokens     | Less repetition             |
| **Frequency Penalty**  | More repetition → more penalty  | Reduces frequent words      |
| **Presence Penalty**   | Token appeared before → penalty | Encourages new words/topics |

> Exact behavior and parameter names depend on the model provider.

---

# 5. Real-World Example

### Task: Generate a product description

Without enough repetition control:

```text
This laptop is powerful.
This laptop is powerful and fast.
This laptop is powerful for powerful performance.
```

With appropriate repetition control:

```text
This laptop delivers strong performance,
fast multitasking, and efficient battery life.
```

The output becomes more natural.

---

# 6. Repetition Penalties in Syncra

Suppose Syncra generates a daily briefing.

Bad output:

```text
Important update:
Rahul discussed the deployment.

Important update:
Rahul discussed the deployment.

Important update:
Rahul discussed the deployment.
```

A repetition penalty can help prevent this.

But don't make the penalty too strong.

### Why?

Too much penalty can produce unnatural wording:

```text
❌ "The deployment discussion was significant regarding the release."
```

when simply saying:

```text
✅ "Rahul discussed the deployment."
```

would be clearer.

---

# 7. Temperature vs Repetition Penalty

These solve **different problems**.

| Parameter              | Controls                                  |
| ---------------------- | ----------------------------------------- |
| **Temperature**        | Randomness / diversity of token selection |
| **Repetition Penalty** | Repetition of previously generated tokens |

### Mental Model

```text
Temperature
    ↓
"How random should the output be?"

Repetition Penalty
    ↓
"How strongly should repeated tokens be discouraged?"
```

---

# 8. Important Practical Rule

Don't increase repetition penalties automatically.

First ask:

> **Is the model actually repeating unwanted content?**

For many tasks, default settings may already work well.

Use repetition controls when you observe:

* Repeated phrases
* Repeated sentences
* Loops
* Excessive word reuse

---

# 9. Quick Mental Model

```text
              SAMPLING PARAMETERS
                     │
        ┌────────────┴─────────────┐
        ↓                          ↓
   Temperature              Repetition Penalty
        ↓                          ↓
   Randomness                 Repetition
        ↓                          ↓
Low = predictable         Low = normal repetition
High = diverse            High = less repetition
```

## One-line Interview Answer

> **“Repetition penalties are sampling controls that reduce the probability of repeatedly generating tokens or phrases, helping produce more varied and natural output.”**
