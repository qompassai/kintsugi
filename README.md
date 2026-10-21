# Kintsugi

**Joint attention for byte-size models — shared goals in multi-agent systems through verified uptake and visible repair.**

<a href="./LICENSE"><img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="License: Apache 2.0"></a>

Multi-agent systems built from language models coordinate through text, but text is not attention. An agent can receive a goal statement without taking it up; two agents can act on divergent readings of the same plan with neither noticing; and when the shared picture breaks, the break is usually invisible in the transcript until the damage is done.

Kintsugi adapts the developmental-psychology mechanism of *joint attention* — the process by which two minds come to attend to the same object, each aware that the other is attending to it — to small, locally served language models. The design splits the mechanism in two:

- **A deterministic protocol is the commitment layer.** It holds the record: who holds focus, in what phase, under what contention. Claims, releases, and phase transitions are protocol acts — explicit, ordered, auditable — never model utterances.
- **Byte-size models are the perceptual layer.** Each participating agent carries a small adapted model that maintains a compressed *goal-state* and must produce a canonical *uptake restatement* — the shared goal and its own role in it — before acting. Restatements are compared across agents and against the record. Agreement means joint attention is holding. Divergence is the contention signal, detected semantically rather than only structurally.

When attention breaks, the repair is written into the shared record as a first-class event, visible permanently — the gold seam in the vessel. The common ground is stronger for showing where it cracked and how it was mended, and the graduation rules for trusting the models widen only over a history of breaks survived and repaired. Authority never transfers: the models propose, the protocol disposes.

This repository holds the research as a submittable manuscript: full text, bibliography, and build tooling.

<details>
<summary>Repository layout</summary>

- `manuscript/manuscript.md` — the full manuscript source (single file, pandoc Markdown with citeproc citations).
- `manuscript/references.bib` — the bibliography.
- `manuscript/Makefile` — `make tex` (pandoc only), `make pdf` (pandoc + a LaTeX engine), `make check` (word count and a list of any `RESULTS PENDING` markers).
- `SUBMISSION.md` — submission readiness checklist and venue notes.
- `CITATION.cff`, `.zenodo.json` — citation and deposit metadata (Apache-2.0).
- `citation.bib` — the cite-this-paper BibTeX entry (the paper's own bibliography is `manuscript/references.bib`).
- `src/pages/index.mdx` — the publication web page (Astro; see the Publication page section below).
- `AGENTS.md`, `.ai/memory/` — the repo's agent protocol and project memory.

</details>

<details>
<summary>Status</summary>

The manuscript is a complete draft of the design, the graduation ladder, and the pilot evidence in hand. Two evaluation trials are specified in the text as a pre-registered protocol and are marked `RESULTS PENDING` where their outcomes belong; those markers are listed by `make check` and must be resolved — by running the trials, not by editing around them — before the manuscript is submitted as a results paper. The design-and-protocol version may be submitted as-is once the author decides the venue.

Pilot evidence already in hand (reported in §5 of the manuscript): a LoRA adapter trained on 36 deterministic-target examples from the coordination log's digest corpus took a granite-4.2-8b base from 0/20 to 19/20 clean held-out digests under an identical scorer, with mean incident recall rising from 0.175 to 1.000, invented timestamps eliminated (4 → 0), and one invented worker remaining in the single non-clean window — evidence that the perceptual task Kintsugi assigns to small models is trainable, which is the premise the rest of the design stands on.

</details>

<details>
<summary>Building the manuscript</summary>

```sh
cd manuscript
make tex    # build/manuscript.tex — needs only pandoc
make pdf    # build/manuscript.pdf — needs pandoc and xelatex
make check  # word count + RESULTS PENDING markers
```

</details>

## Publication page

The repo doubles as the paper's project page (Astro + Tailwind, adapted from [qompassai/pub_template](https://github.com/qompassai/pub_template)): `npm ci && npm run build`. The page source is `src/pages/index.mdx`; `docs/README.md` records the template provenance and the deliberate departures from it (no demo assets, a real manuscript pipeline, paper-first citation metadata, project memory seeded with real state).

## License

Apache-2.0. See `LICENSE`.

## Citation

See `CITATION.cff`. Author: Owner — ORCID [0000-0002-0302-4812](https://orcid.org/0000-0002-0302-4812).
