%.dvi: %.tex
	latex $^

%.svg: %.dvi
	dvisvgm --no-fonts $^ $@

all: actual_execution.svg simulated_execution.svg

.PHONY: all
