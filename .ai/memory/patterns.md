# Conventions, Recipes, Gotchas

## Builds

- Manuscript LaTeX: `cd manuscript && make tex` → `build/manuscript.tex`
  (pandoc only; pandoc 3.11 verified on primo).
- Manuscript PDF: `make pdf` needs xelatex. Primo's TeX Live is missing
  format files as of 2026-10-16 (repair approved, queued) — build the
  PDF after the repair or on another machine.
- Publication page: `npm ci && npm run build` (astro check must be
  0 errors). `package-lock.json` came from pub_template with its root
  `name` fields rewritten to `kintsugi`; `package.json` name/version
  must stay in sync with it or `npm ci` will refuse.

## Provenance

- Adapted 2026-10-16 from `qompassai/pub_template` (Astro publication
  page; upstream lineage: Nerfies project page → Eliahu Horwitz →
  Roman Hauksson's academic-project-astro-template).
- Deliberately NOT carried over: the template's demo assets
  (`src/assets/` media, public screenshots) and its demo `index.mdx`
  content. Components the Kintsugi page does not yet use (Figure,
  Video, Splat, ImageComparison, …) are retained for future figures.
- Template README contains a curl-pipe quickstart for an unrelated
  dotfiles repo. It is not part of this repo's workflow; do not run it.

## Metadata

- `citation.bib` (root) = the cite-this-paper entry,
  `porter2026kintsugi`. `manuscript/references.bib` = the paper's own
  bibliography. Do not merge them.
- `.zenodo.json` follows the pub_template shape (creators with
  affiliation + ORCID, related identifiers, qompassai community) with
  `upload_type: publication` / `publication_type: preprint`. No
  `version` field until a first release exists to name.
