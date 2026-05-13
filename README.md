# 🧠 english-csv-prompt-generator

> A precision-engineered LLM prompt for generating large-scale, production-quality English conversational datasets in structured CSV format — built for NLP training, machine translation pipelines, and language learning apps.

---

## What Is This?

This is not a chatbot toy. This is a **dataset factory**.

`english-csv-prompt-generator` is a carefully structured system prompt designed to turn any capable LLM (Claude, GPT-4, etc.) into a deterministic, high-throughput English text generation engine. Feed it a starting ID, tell it to continue, and watch it produce clean, diverse, human-like conversational English — batch by batch — all the way to **100,000 entries** if you need it.

The prompt enforces strict CSV output (`id,english_text`), tracks its own progress, self-verifies quality, and comes with a built-in command interface so you stay in control of what gets generated and when.

---

## ✨ Key Features

### Structured Output, Every Time
The prompt enforces a single strict format: valid CSV wrapped in a fenced code block, no preamble, no filler text inside the output, no broken rows. You get data you can `pd.read_csv()` immediately.

### Built-in Quality Assurance
Before each batch is finalized, the prompt runs a self-verification checklist:
- Sequential, non-duplicate IDs
- Grammar and spelling verified
- Domain and topic diversity maintained
- Natural conversational tone preserved

No post-processing step required to catch obvious issues.

### Domain Diversity Engine
Each batch draws from a wide spread of real-world conversational contexts:

| Domain | Example Entry |
|---|---|
| Tech Support | *"Have you tried restarting the router? That usually fixes the connection issue."* |
| Healthcare | *"The doctor said I should come back in two weeks for a follow-up."* |
| Education | *"Could you explain that concept again? I didn't quite follow the second part."* |
| Hospitality | *"Your room is ready. Would you like help with your luggage?"* |
| Daily Life | *"I forgot to buy milk again. Can you add it to the shopping list?"* |
| Professional | *"Let's move the standup to 10 AM so the Singapore team can join."* |

### Difficulty Stratification
Entries are distributed across three levels:
- **Beginner** — short, high-frequency phrases and simple Q&A
- **Intermediate** — explanations, multi-clause sentences, polite requests
- **Advanced** — nuanced discussions, implicit context, idiomatic phrasing

This makes the output directly usable for curriculum-aligned language learning apps without a separate labeling pass.

### Command Interface
A conversational control layer is baked into the prompt so you never lose your place:

```
"continue"            → Next 250 entries from where you left off
"start at 4501"       → Jump to a specific ID
"focus on healthcare" → Themed batch for targeted domain coverage
"send script"         → Get a Python template for local generation
"quality check"       → Re-run verification on the last batch
```

### Progress Tracking
Every batch ends with a progress summary block:

```
✅ Batch complete: entries 1001-1250 (250 entries) delivered.
✅ 1250 of 100,000 entries completed (1.25% done)
```

No spreadsheet needed to track where you are in a multi-session generation run.

---

## 🚀 Quickstart

> **Recommended platform: [Qwen Chat](https://chat.qwen.ai)** — free, fast, and built to follow structured prompts precisely. This is what the prompt was designed and tested on.

1. Go to [chat.qwen.ai](https://chat.qwen.ai) and open a new chat
2. Copy the full prompt from [`prompt.md`](./prompt.md)
3. Paste it as the **system prompt** (or send it as your first message if system prompt isn't exposed)
5. Send your first message:
   ```
   start at 1
   ```
6. The model will begin generating your first 250 entries
7. Keep sending `continue` until your dataset is the size you need

That's it. No setup. No dependencies. No API calls to configure.

---

## 🏗️ Use Cases

- **Machine Translation Training** — Use as the English source side of a parallel corpus. Pair with any translation pipeline to produce sentence pairs for fine-tuning MarianMT, NLLB, or a custom Transformer encoder-decoder model.
- **Language Learning Apps** — Feed directly into a spaced repetition system, translation quiz engine, or vocabulary driller. Difficulty levels are already embedded.
- **LLM Fine-tuning** — Use as supervised fine-tuning (SFT) data for instruction-following or conversational style transfer.
- **Dialogue Systems** — Seed a retrieval-augmented chatbot with realistic, domain-tagged utterances.
- **Benchmark Construction** — Build evaluation sets for grammar correction, paraphrase detection, or semantic similarity models.
- **Data Augmentation** — Supplement small existing corpora for low-resource language pairs.

---

## 📊 Output Format

```csv
id,english_text
1,"Can you help me reset my password? I've been locked out since yesterday."
2,"The meeting has been rescheduled to Thursday at three in the afternoon."
3,"I'm not sure I understand the assignment. Could you walk me through it again?"
4,"We ran out of printer paper. I'll grab some from the supply room."
5,"Your prescription is ready for pickup at the pharmacy counter."
```

Each entry:
- Has a globally unique integer ID
- Contains 5-25 words of natural, grammatically correct English
- Is independently meaningful (no cross-entry dependencies)
- Is safe, neutral, and appropriate for all audiences

---

## ⚙️ Technical Notes

**Optimized for:** [Qwen Chat](https://chat.qwen.ai) — use this. Seriously.

> This prompt was developed and tested exclusively on **Qwen** (Alibaba Cloud's LLM platform). Other models may work to varying degrees, but Qwen is the recommended runtime for one specific reason: it is exceptionally good at strict prompt adherence. When the prompt says "output only CSV inside a code block," Qwen does exactly that — no preamble, no filler, no format drift across long sessions. That reliability is what makes 100k-entry generation actually tractable.
>
> Qwen Chat is also **free**, with premium capabilities that other platforms lock behind $20+/month subscriptions. There is no reason to run this on a paid API when Qwen handles it better for nothing.

**Recommended model:** Qwen3 (latest, available at [chat.qwen.ai](https://chat.qwen.ai))

**Recommended settings:**
- Temperature: `0.8–1.0` (lower = more repetition, higher = more variety)
- Max tokens: `4000+` per response to fit a full 250-entry batch

**Batch size:** 250 entries per call is the sweet spot — large enough to be efficient, small enough to stay within context windows and avoid quality degradation toward the end of long outputs.

**Scaling to 100k:** A full 100,000-entry run requires 400 batches. For automation, extract each CSV block programmatically and concatenate. The `id` field is globally sequential across all batches, so merging is trivial:

```python
import pandas as pd
import glob

files = sorted(glob.glob("batch_*.csv"))
df = pd.concat([pd.read_csv(f) for f in files], ignore_index=False)
df.sort_values("id").to_csv("full_dataset.csv", index=False)
```

---

## 🤝 Contributing

Pull requests are welcome. If you extend the prompt for a specific domain (medical, legal, STEM) or a different target language pair, feel free to open a PR or raise an issue.

---

## 📄 License

MIT — use it, fork it, train on it, ship it.

---

*Built with intention. Designed for scale.*
