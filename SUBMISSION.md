# Submission checklist — Kintsugi manuscript

The repository is arranged so the manuscript can be submitted as a full paper. This file tracks what "ready" means and what remains an author decision.

## What is in place

- Full manuscript text: `manuscript/manuscript.md` (design, graduation ladder, pilot evidence, pre-registered evaluation protocol, discussion, limitations).
- Bibliography: `manuscript/references.bib`, wired through pandoc citeproc.
- Build: `manuscript/Makefile` — `make tex` produces the LaTeX source (arXiv-ready route); `make pdf` produces the submission PDF.
- Metadata: `CITATION.cff` and `.zenodo.json` carry the title, abstract, keywords, license (Apache-2.0), and the author's ORCID.
- License: canonical Apache-2.0 in `LICENSE`.

## Gates before submitting

1. **Build gate.** `make tex` must succeed; `make pdf` must succeed on a machine with a working LaTeX engine. (Known environment note: the author's primary workstation currently has a TeX Live installation with missing format files; the repair is already approved in the workstation maintenance queue. Build the submission PDF after that repair, or on any other machine with xelatex.)
2. **Honesty gate.** Run `make check`. Every `RESULTS PENDING` marker names a trial that has not yet produced the outcome the text promises to report. Two submission shapes are legitimate:
   - *Design-and-protocol paper* — submit with the trials presented as protocol (the current text is written to support this reading: the marked sections report no unmeasured outcomes).
   - *Results paper* — run Trial 0 (served-adapter trial) and Trial 1 (two-agent uptake trial) per §6, replace each marker with the measured outcome, and re-run the gate.
   Deleting a marker without running its trial is not a third option.
3. **Reference gate.** Every entry in `references.bib` is a real, checkable source. Before submission, re-verify each entry against its publisher or index record, and add DOIs where the venue's style requires them.
4. **Author gate.** The author confirms: affiliation line as it should appear in print; venue; and whether the deposit is preprint-first (arXiv / Zenodo) or journal-first.

## Venue notes (author's call)

- **arXiv** (cs.MA / cs.AI): accepts the LaTeX source produced by `make tex`. Fastest route to a citable public record.
- **Zenodo**: the deposit metadata is ready in `.zenodo.json`. Note the known estate-wide issue: the GitHub→Zenodo webhook integration has been failing account-side (HTTP 500 from the legacy receiver), so a direct upload of the built PDF may be required until that is repaired in the author's Zenodo settings.
- **Journal / conference**: the text is venue-neutral; a venue style (CSL) drops into the pandoc build without restructuring the source.

## Decisions owed by the author

- Venue and submission shape (design-and-protocol vs results) — see gate 2.
- Affiliation line wording for the title page.
- Timing relative to Trial 0 / Trial 1 execution.
