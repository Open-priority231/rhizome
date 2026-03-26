# Model selection guide

Rhizome embeds every note as a fixed-length vector and finds the closest
neighbours by cosine similarity.  The quality and behaviour of the entire
pipeline — which notes get linked, how many false positives appear, how long
encoding takes — depends almost entirely on the embedding model you choose.

This document explains how the model is used, what makes a model compatible,
and how to pick the right one for your vault.

---

## How rhizome uses the model

1. Each note is stripped of frontmatter and wikilinks, leaving clean prose.
2. The text is tokenised (max 128 tokens) and fed to the ONNX session.
3. Token embeddings are averaged with attention-mask weighting (**mean pooling**),
   producing a single vector per note.
4. Vectors are L2-normalised so that dot product equals cosine similarity.
5. The similarity matrix is searched to find the top-K neighbours above
   `SIMILARITY_THRESHOLD` for each note.

The model is loaded once, kept in memory for the full run, and cached to
`MODEL_DIR` so subsequent runs require no network access.

---

## Compatibility requirements

Rhizome downloads two files from HuggingFace:

```
https://huggingface.co/{MODEL_NAME}/resolve/main/onnx/model.onnx
https://huggingface.co/{MODEL_NAME}/resolve/main/tokenizer.json
```

A model is compatible if and only if:

| Requirement | Why |
|-------------|-----|
| Published on HuggingFace | Download path is hardcoded to `huggingface.co` |
| Has `onnx/model.onnx` in the repo root | This is the ONNX session rhizome loads |
| Has `tokenizer.json` in the repo root | Used by the Rust-backed tokenizer |
| Is a **sentence-transformer** (mean pooling) | CLS-token models produce poor similarity scores for prose |

The [Xenova organisation](https://huggingface.co/Xenova) publishes ready-to-use
ONNX exports for hundreds of sentence-transformer models in exactly this format.
It is the easiest source of compatible models.

> **Non-Xenova models** may follow a different directory layout or lack
> `tokenizer.json` in the expected location.  Using them would require modifying
> `inference/model.py` to point to the correct URLs — see
> [Contributing](../CONTRIBUTING.md) if you want to add support.

---

## Model comparison

All sizes are approximate ONNX file sizes on disk.  Embedding dimension
determines the size of the in-memory similarity matrix: at N notes,
memory use scales as N × dim × 4 bytes (float32).

| Model | Size | Dim | Languages | Notes |
|-------|------|-----|-----------|-------|
| `Xenova/paraphrase-multilingual-MiniLM-L12-v2` | ~250 MB | 384 | 50+ | **Default.** Trained on paraphrase pairs; very good at finding topically related notes across languages. |
| `Xenova/all-MiniLM-L6-v2` | ~90 MB | 384 | English | 6-layer model. Fast, small, good for English vaults on constrained hardware. Slightly lower recall than L12. |
| `Xenova/all-MiniLM-L12-v2` | ~130 MB | 384 | English | 12-layer model. Better recall than L6 with modest extra size. Solid default for English-only vaults. |
| `Xenova/all-mpnet-base-v2` | ~430 MB | 768 | English | MPNet architecture. Higher-quality embeddings than MiniLM for English. Recommended if link precision matters more than speed. |
| `Xenova/paraphrase-multilingual-mpnet-base-v2` | ~1.1 GB | 768 | 50+ | Best multilingual quality in this list. Noticeably slower and heavier than the default. Worth it for large vaults where precision counts. |

---

## Decision guide

### Is your vault primarily in one language?

**Yes — English only**
→ Use `Xenova/all-MiniLM-L12-v2` (balanced) or `Xenova/all-mpnet-base-v2`
  (higher quality).  The multilingual default works, but English-only models
  dedicate their full capacity to English and tend to produce tighter clusters.

**Yes — one non-English language**
→ Stick with the default (`paraphrase-multilingual-MiniLM-L12-v2`) or upgrade
  to `paraphrase-multilingual-mpnet-base-v2` for better precision.  Check
  [Xenova's HuggingFace page](https://huggingface.co/Xenova) for language-specific
  models — some (e.g. Japanese, Chinese) have dedicated fine-tuned exports.

**No — mixed or unknown**
→ Keep the default.  Multilingual models handle code-switching and transliterated
  content gracefully; monolingual models do not.

### How big is your vault?

**< 500 notes** — Any model works.  Rhizome uses the exact O(N²) numpy backend
and runs comfortably in memory even with 768-dim vectors.

**500 – 5 000 notes** — Prefer 384-dim models to keep the HNSW index fast and
memory-efficient.  `all-MiniLM-L12-v2` or the multilingual default are good
choices.

**> 5 000 notes** — 384-dim models are strongly recommended.  At 10 000 notes,
a 768-dim index requires ~240 MB just for the embedding matrix; 384-dim halves
that.

### Are you on constrained hardware?

Low RAM or slow CPU → `Xenova/all-MiniLM-L6-v2`.  It is the smallest compatible
model (~90 MB), encodes quickly, and still produces useful semantic links for
English vaults.

### Do you prioritise recall or precision?

**More links, fewer missed connections (recall)** → lower `SIMILARITY_THRESHOLD`
(try `low` = 0.60) and keep a smaller, faster model.

**Fewer links, higher confidence (precision)** → higher threshold (`high` = 0.88)
and a stronger model such as `all-mpnet-base-v2`.

---

## Switching models

1. **Update `.env`:**
   ```
   MODEL_NAME=Xenova/all-MiniLM-L12-v2
   ```

2. **Clear the model cache** — embeddings from the old model are in `MODEL_DIR`
   and are incompatible with the new one:
   ```bash
   rm models/model.onnx models/tokenizer.json
   ```

3. **Pre-download the new model** (optional but recommended before a full run):
   ```bash
   rhizome download-model
   ```

4. **Re-run the pipeline.** Because the new model produces different embeddings,
   all previously written `## Related Notes` sections may change.  Consider
   running `rhizome clean` first to remove stale links, then `rhizome run` to
   regenerate them.

   ```bash
   rhizome clean    # remove old sections
   rhizome audit    # preview what the new model would link
   rhizome run      # write new sections
   ```

---

## A note on embedding dimensions and `SIMILARITY_THRESHOLD`

384-dim and 768-dim models use the same cosine similarity scale (−1 to 1 after
L2 normalisation), but their score distributions differ.  A threshold of 0.75
that works well with one model may be too loose or too tight with another.

When switching models, run `rhizome audit` to inspect the potential-link count
and adjust `SIMILARITY_THRESHOLD` before committing to a full run.
