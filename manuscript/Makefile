# Kintsugi manuscript build — pandoc + citeproc over a single Markdown source.
#
#   make tex    build/manuscript.tex  (needs only pandoc; always available)
#   make pdf    build/manuscript.pdf  (additionally needs a LaTeX engine)
#   make check  word count + list any RESULTS PENDING markers
#   make clean
#
# A version carrying RESULTS PENDING markers is a draft: it may circulate,
# but it must not be submitted as a results paper until the marked trials
# have run and the markers are replaced by measured outcomes.

PANDOC ?= pandoc
SRC    := manuscript.md
BIB    := references.bib
BUILD  := build

.PHONY: all pdf tex check clean
all: pdf

$(BUILD):
	mkdir -p $(BUILD)

pdf: $(BUILD)
	$(PANDOC) $(SRC) --citeproc --bibliography=$(BIB) \
		--pdf-engine=xelatex -o $(BUILD)/manuscript.pdf

tex: $(BUILD)
	$(PANDOC) $(SRC) --citeproc --bibliography=$(BIB) \
		-s -t latex -o $(BUILD)/manuscript.tex

check:
	@echo "words: $$(wc -w < $(SRC))"
	@echo "pending markers:"
	@grep -n 'RESULTS PENDING' $(SRC) || echo "  none"

clean:
	rm -rf $(BUILD)
