# 🧠 Large Language Models (LLMs) - Presentation Notes

## 1. 📜 History & Evolution

### 🔁 RNN (Recurrent Neural Networks)

* Processes words **one at a time** in sequence.
* Remembers only recent context.
* Example:

  > Input: "The river bank"
  > vs. "The ICICI bank"
  > RNN can't distinguish meanings without broader context.

### 🔁 LSTM (Long Short-Term Memory)

* Improvement over RNN.
* Better memory for longer sequences.
* Still struggles with very long texts.

### 🔄 Transformers (Introduced by Google - 2017)

* Built initially for **Google Translate**.
* Solved the limitations of RNN/LSTM.
* Uses **Self-Attention** to understand full context at once.

---

## 2. 🤖 What is a Transformer?

Transformers are deep learning models based on **attention mechanisms**.
They are the backbone of LLMs like **GPT (Generative Pretrained Transformer)**.

### 🔹 Main Components:

#### 1. **Tokenization (Encoding Phase)**

* Breaks text into chunks (tokens).
* Example:

  > Input: "I love pizza"
  > Tokens: `["I", "love", "pizza"]`
* Try: [TikTokenizer](https://tiktokenizer.vercel.app/)

#### 2. **Vector Embedding**

* Converts tokens into numbers (vectors).
* Captures semantic meaning.
* Example:

  > "king" → `[0.12, -0.88, ...]`
  > "queen" → similar vector → shows semantic similarity

#### 3. **Positional Encoding**

* Adds information about word order.
* Example:

  > "The dog chased the cat" ≠ "The cat chased the dog"

#### 4. **Self-Attention**

* Looks at the **entire sentence** to decide how much focus each word should have.
* Example:

  > In "The bank was full of water", focus on "river" gives right meaning.

#### 5. **Multi-Head Attention**

* Multiple attention layers run in parallel.
* Understands **multiple types of relationships**.
* Example:

  > One head may focus on grammar, another on meaning.

#### 6. **Normalization & Layers**

* Stabilizes training.
* Each layer refines the results further.

---

## 3. 🎯 Why These Steps Are Needed

* 🔹 Tokenization: Convert raw text → model-understandable form
* 🔹 Embedding: Understands meaning beyond literal tokens
* 🔹 Positional Encoding: Order matters
* 🔹 Attention: Captures relationships
* 🔹 Multi-Head: Adds depth to understanding

---

## 4. 🏋️ Training Phase

### 🧠 Goal: Learn from lots of text data

#### Example:

* Input: "Hi, how are you?"
* Encoder processes the full sentence.
* Decoder begins with `<start>` token.
* Target output: "I am fine"
* Model calculates loss between prediction and actual → Backpropagation updates weights

---

## 5. 🚀 Inference Phase

### 🎯 Goal: Generate new text

#### Example:

* Input: "Hi, how are you?"
* Model output (step by step):

  1. "I"
  2. "am"
  3. "fine"
* It generates each word **one at a time**, using previous words as context.

---

## 🔚 Summary

| Step                | Purpose                                    |
| ------------------- | ------------------------------------------ |
| Tokenization        | Split input text into model-friendly units |
| Embedding           | Represent meaning numerically              |
| Positional Encoding | Retain word order info                     |
| Self-Attention      | Capture context across sentence            |
| Multi-Head          | Boost contextual depth                     |
| Training            | Learn to predict next tokens accurately    |
| Inference           | Generate meaningful responses              |

---

> 🎤 Use this structure for your slides and explain each step with small, relatable examples.

