# Current Work

Kintsugi — the joint-attention research line extending MATE: byte-size
local models as the perceptual layer of a deterministic focus protocol,
holding a shared goal through verified uptake and visible repair.

## Status (2026-10-16)

- Named by Matt 2026-10-16 ("Name it Kintsugi"). Repo created the same
  day: github.com/qompassai/kintsugi (public).
- Manuscript: complete draft in `manuscript/manuscript.md` (~4,550
  words) — design, graduation ladder, pilot evidence (§5), and a
  pre-registered evaluation protocol (§6). Two `RESULTS PENDING`
  markers stand in §6 (Trial 0, Trial 1); `make check` lists them.
- Publication page adapted from qompassai/pub_template (Astro);
  `src/pages/index.mdx` carries the paper's page. Site build NOT yet
  verified in this repo: run `npm ci && npm run build` on primo.
- Manuscript build: `make tex` is the verified route (pandoc 3.11 on
  primo). `make pdf` needs a working LaTeX engine — primo's TeX Live
  currently lacks format files; its repair is approved in the estate's
  maintenance queue.

## Queued

- Trial 0 (Kintsugi step zero): convert the §5.2 LoRA pilot adapter to
  GGUF form, serve on the granite `-tools` base via rose, re-score the
  same 20 held-out windows at the endpoint. Depends on the rose store
  re-provisioning of the four cured `-tools` GGUFs (approved 2026-10-16).
- Trial 1: two-agent uptake trial per §6, after Trial 0.
- Venue decision (Matt's): design-and-protocol submission now, or
  results paper after the trials. See `SUBMISSION.md`.
