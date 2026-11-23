# The Urdu Tokenizer Tax

**How many more tokens do popular LLM tokenizers need for Urdu and Roman Urdu than for English, and does a small model also perform worse on Urdu?**

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR-USERNAME/urdu-tokenizer-tax/blob/main/urdu_tokenizer_tax.ipynb)

> **Status:** replace every `<!-- FILL -->` marker below with your real results before publishing.

## Why this matters

Language models read text as *tokens*, not words. If a tokenizer splits Urdu into many more tokens than English for the same meaning, Urdu users pay more per request, fit less text in the context window, and may get worse model quality. This project measures that gap across popular tokenizers and checks whether it shows up in model quality too.

## Research questions

1. **Tokenizer tax:** For the same sentences, how many times more tokens do Urdu and Roman Urdu need compared with English, across different tokenizers?
2. **Model quality:** Is a small language model measurably worse at Urdu than English, and does the gap change under 8-bit and 4-bit quantization?

## What's in this repo

| File | Description |
|---|---|
| `urdu_tokenizer_tax.ipynb` | One-click Colab notebook that runs the whole study |
| `results/tokenizer_stats.csv` | Token counts and ratios per tokenizer, language, and dataset |
| `results/tokenizer_cost_chart.png` | Headline chart for Part A |
| `results/model_quality.csv` | Bits per byte per precision and language (Part B) |
| `results/model_quality_chart.png` | Chart for Part B |

## Method

### Data
- **Curated set:** 20 hand-written parallel sentences in English, Urdu, and Roman Urdu.
- **OPUS-100 (en-ur test split):** up to 500 real-world parallel pairs, filtered to 30-250 characters. This data is noisy (web, subtitle, and religious text).

### Part A: Tokenizer comparison
Tokenizers compared: GPT-2, GPT-4 (cl100k), GPT-4o (o200k), Qwen2.5, BLOOM, XLM-R, mBERT, mT5, Llama-2 (TinyLlama), Phi-3, XGLM.

For each tokenizer and language I computed:
- **Tokens per word** (whitespace-split words)
- **Bytes per token**
- **Cost vs. English** = total tokens for the language / total tokens for the English version of the same sentences (1.0 means no extra cost)

### Part B: Model quality
- **Model:** Qwen2.5-0.5B
- **Precisions:** full precision (fp16 on GPU), 8-bit, and 4-bit NF4 (bitsandbytes)
- **Metric:** **bits per byte** (total negative log-likelihood in bits divided by total UTF-8 bytes). I use bytes instead of tokens so English and Urdu are comparable even though they are tokenized differently.
- Evaluated on up to 100 English and 100 Urdu sentences.

## Results

### Part A: Tokenizer tax
<!-- FILL: paste the table printed by the notebook (cost vs. English per tokenizer) -->

![Tokenizer cost chart](results/tokenizer_cost_chart.png)

**Key findings**
- <!-- FILL: e.g. "Urdu needs between X and Y times as many tokens as English depending on the tokenizer." -->
- <!-- FILL: which tokenizer is best and worst for Urdu -->
- <!-- FILL: Roman Urdu vs. Urdu script comparison -->

### Part B: Model quality
<!-- FILL: paste the table from results/model_quality.csv -->

![Model quality chart](results/model_quality_chart.png)

**Key findings**
- <!-- FILL: English vs. Urdu bits per byte at baseline -->
- <!-- FILL: whether quantization widened or narrowed the gap, or if the differences are within noise -->

## How to reproduce

1. Click the **Open In Colab** badge above (or upload `urdu_tokenizer_tax.ipynb` to [Google Colab](https://colab.research.google.com)).
2. Set **Runtime → Change runtime type → T4 GPU** (a CPU runtime works but skips the 8-bit and 4-bit tests).
3. Click **Runtime → Run all**. It takes about 10 minutes.
4. Results are saved to `/content/results/` and downloaded as a zip.

## Limitations

- The curated set has only 20 sentences, so its numbers are noisy.
- Sentences were written by the author. They may not represent all registers of Urdu.
- OPUS-100 contains noisy and domain-skewed text.
- Part B uses a single small model (0.5B parameters), so findings may not generalize to larger models.
- Bits per byte on short sentences has high variance. Small differences between precisions may be noise.
- Word counts use whitespace splitting, which is an imperfect measure for Urdu.
- Other studies on multilingual tokenizer fairness exist. This project is a small, reproducible Urdu-focused measurement, not a claim of a first-of-its-kind result.

## Future work

- Add Punjabi, Pashto, and Sindhi sentences
- Evaluate more models and larger sizes in Part B
- Add GPTQ/AWQ quantization and a calibration-language comparison
- Build a larger, professionally reviewed Urdu parallel set


## License

MIT License. 
