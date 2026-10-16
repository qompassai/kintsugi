# Durable Decisions

## Name (2026-10-16, Matt)

**Decision**: the research is named **Kintsugi** — breaks in joint
attention are repaired in the open record as first-class events (the
gold seams); graduation requires real breaks, mended.

**Naming mechanics**: crates.io `kintsugi` is taken; the crate package
is `kintsukuroi` (金繕い, the craft's other name — verified free
2026-10-16), shipping the `kintsugi` bin. Repo, program, and MCP carry
the Kintsugi name. Runner-up candidate was Regard (joint regard).

## Architecture (2026-10-16)

**Decision**: the commitment/perception split is constitutive.
Deterministic protocol = commitment layer (claims, phases, contention
are protocol acts). Byte-size models = perceptual layer only
(goal-state compression, uptake restatement, divergence detection).
Authority never transfers; graduations are the operator's decisions.

## License (2026-10-16)

**Decision**: Apache-2.0 only — canonical apache.org text, appendix
`Copyright 2026 Qompass AI`; SPDX `Apache-2.0` in metadata. This repo
does not follow pub_template's dual-license arrangement.

## Manuscript honesty gates (2026-10-16)

**Decision**: §6 trials are pre-registered in the text with
`RESULTS PENDING` markers; a marker is replaced only by the measured
outcome of its trial. Two legitimate submission shapes:
design-and-protocol paper (markers as protocol) or results paper
(markers resolved by running the trials). See `SUBMISSION.md`.
