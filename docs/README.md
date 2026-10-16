# About this repository's layout

This repo holds the Kintsugi research in submittable form:

- `manuscript/` — the full manuscript (single Markdown source,
  bibliography, pandoc Makefile). `SUBMISSION.md` at the root is the
  submission checklist.
- `src/pages/index.mdx` — the publication web page, adapted from
  [qompassai/pub_template](https://github.com/qompassai/pub_template).

## Template provenance and credits

The publication page machinery comes from pub_template, Qompass AI's
adaptation of Roman Hauksson's
[academic-project-astro-template](https://github.com/RomanHauksson/academic-project-astro-template),
itself adapted from Eliahu Horwitz's academic project page template and
Keunhong Park's [Nerfies](https://nerfies.github.io/) project page.
That lineage is credited here, as upstream requests.

Deliberate departures from the template, made for this publication:

- No demo assets or demo content are carried over; the page presents
  this paper only.
- The template has no manuscript pipeline; this repo adds one
  (`manuscript/` + `make check` honesty gate over `RESULTS PENDING`
  markers).
- Root `citation.bib` cites the *paper* (the template's cites the
  template software); the paper's own bibliography lives in
  `manuscript/references.bib`.
- `.ai/memory/` is seeded with this project's real state rather than
  left as scaffolding.
