# Peer Review

## Preprint — internal review (author-requested; diver manuscript-review protocol)

### Manuscript Number: —

### Title: *Kintsugi: Joint Attention for Byte-Size Models — Shared Goals in Multi-Agent Systems Through Verified Uptake and Visible Repair*

---

## Review Scores

| Category | Score |
|---|---|
| Reviewer Recommendation | **Minor Revision** |
| Overall Manuscript Rating | **78/100** |
| Topic importance | 4/5 |
| Conclusions supported | 4/5 |
| Novelty | 4/5 |

---

## Confidential Comments to the Editor

Provenance: this review was conducted at the author's request by the author's agent, applying the author's own diver manuscript-review skill and the diver `lua/research/` tooling (BibCheck/BibDuplicates, CitationScan, headless diagnostics, Crossref/arXiv verification). It is a validation pass, not an independent external review, and should not be represented as one. Every external claim was checked against its primary source and every reported number was recomputed against the underlying run record (battery + LoRA pilot RESULTS.md, 2026-10-16). Three factual reporting defects were found in §5 (Major Comments 1–3). All corrections were applied to the manuscript in the same pass, 2026-10-16; the comments below describe the draft as submitted for review.

---

## General Comments to Authors

The manuscript adapts joint attention to multi-agent systems built from byte-size local models: a deterministic focus protocol holds commitments, small adapted models serve as a perceptual layer that must restate the shared goal before acting, restatements are verified against the record and each other, and divergence is preserved in the log as a first-class repair event. Its empirical core is a pilot in which a LoRA adapter trained on deterministic targets from the coordination log took a granite-4.2-8b base from 0/20 to 19/20 clean held-out digests. The authors are credited with practices reviewers rarely get to assume: an identical automated scorer for both conditions (no rater-bias channel), deterministic targets with zero model-generated content, evaluation trials pre-registered in the text with visible RESULTS PENDING markers, and limitations stated in the authors' own voice. The defects found are local to §5's reporting; none touches the design, and all are corrected by returning to the run record.

---

## Major Comments

## Comment 1 — manuscript §5.2, Method (manuscript.md line 120) — pilot's evaluation form misstated, reporting incomplete

The draft stated the pilot base model was "served locally in GGUF form." The run record shows the base was granite-4.2-8b held as local HF safetensors, evaluated through the transformers stack (torch 2.14.1, peft 0.21.2); it was not served, and the distinction is material because the served configuration is Trial 0's entire question. The draft also omitted the training configuration a replication requires (learning rate, epochs, target modules, library versions, dataset construction). Fix: §5.2 now reports the safetensors/transformers account, the dataset construction (64 frozen events → 32 disjoint two-event micro-windows, seeded split; 12 training windows / 36 examples; deterministic targets, zero model-generated content), and the full configuration (rank 8, alpha 16, all linear modules, lr 1e-4, 3 epochs). Applied 2026-10-16.

## Comment 2 — manuscript §5.2, Results (manuscript.md line 122) — "zero invented facts" contradicted by the run record

The draft claimed the adapted model produced "zero invented facts — no F1-class fabrications … on any held-out window." The run record's held-out table shows the adapter invented **1 worker** (base: 3 workers, 4 timestamps; adapter: 1 worker, 0 timestamps). The timestamps claim was correct; the blanket claim was not, and in a paper whose taxonomy names fabrication its most serious failure class, the overclaim is exactly the kind the text itself exists to catch. Fix: §5.2 now reports the per-class counts for both models, and the same correction was applied to the repository README and the publication page's pilot table, which had inherited the overclaim. Applied 2026-10-16.

## Comment 3 — manuscript §5.3, companion battery paragraph — blind spot overgeneralised to all four models

The draft stated all four served models "exhibited a shared blind spot" for refusal-phrased contention. The run record shows the blind spot in the two models that produced parseable text answers (hf-qwen3-coder-30b missed both contention items; qwen2.5-coder:7b missed one); the other two failed the tier on different classes (markup leakage; thinking-budget exhaustion) and produced no readable contention judgement. Fix: §5.3 now scopes the claim to the parseable responders and names the other failure classes; the publication page's summary sentence was corrected to match. Applied 2026-10-16.

---

## Minor Comments

1. §6, Trial 0 criterion demanded "zero F1 fabrications" for parity, a bar the laboratory run itself (one invented worker) would fail. Criterion aligned to the laboratory counts, and the fallback's claim scope is now stated: a merged-model result does not support an adapter-serving parity claim. Fixed.
2. `manuscript/references.bib`, tomasello1995: page range omitted. The chapter's pages (103–130) were verified against multiple independent bibliographies and added.
3. §5.1: the 37-events / 7-annotations figure carried no date. "(October 2026)" added.
4. §5.2: "content recall" renamed to the scorer's actual metric name, "mean incident recall", in the manuscript and on the publication page.
5. Unit of analysis: the 20 held-out windows are disjoint but drawn from a single system's log; they are not independent samples of a population of systems. Section 7 already bounds generalisation accordingly; no change requested, noted for the record.
6. Tooling note (not a manuscript defect): on the author's workstation, BibCheck's bibtool route lacks the `check.rsc` resource it expects, so bibliography validation ran as: citeproc parse inside the pandoc build (all 10 citations resolved), BibDuplicates clean on both .bib files, CitationScan clean, and 0 headless-Neovim diagnostics on the manuscript under the live diver config. Full BibCheck awaits the resource file (and the workstation's pending TeX Live format repair, which also gates `make pdf`).

---

## Suggested Editorial Disposition

Minor revision. With the corrections above applied and re-verified (pandoc build re-run; `make check` still shows exactly the two designed RESULTS PENDING markers), the manuscript is submittable in its design-and-protocol shape. The results-paper shape awaits Trial 0 and Trial 1, and the submission PDF awaits the workstation TeX repair.

---

## Key External Sources Used in This Review

- Crossref record — Scaife & Bruner (1975), *Nature* 253, 265–266 (api.crossref.org)
- Crossref record — Tomasello et al. (2005), *Behavioral and Brain Sciences* 28(5), 675–691
- Crossref record — Mundy & Newell (2007), *Current Directions in Psychological Science* 16(5), 269–274
- Crossref record — Bratman (1992), *The Philosophical Review* 101(2), 327–341
- Crossref record — Pickering & Garrod (2004), *Behavioral and Brain Sciences* 27(2), 169–190
- Crossref record — Clark & Brennan (1991), in *Perspectives on Socially Shared Cognition*, 127–149
- Tomasello (1995), "Joint attention as social cognition", pp. 103–130 — corroborated by independent bibliographies (Benjamins; SAGE; PMC4258841 reference list)
- Hu et al., LoRA — https://arxiv.org/abs/2106.09685 (fetched, HTTP 200)
- llama.cpp — https://github.com/ggerganov/llama.cpp (fetched, HTTP 200)
- Internal primary source for all pilot and battery figures — the run record `~/workspace/phlow-trials/battery-20261016/RESULTS.md` (2026-10-16)
