TARGET=paper

DEPS=$(wildcard ./*.tex ./*.bib ./*.cls ./*.sty)


all: $(TARGET).pdf

$(TARGET).pdf: $(DEPS)
	pdflatex $(TARGET)
	bibtex $(TARGET)
	pdflatex $(TARGET)
	pdflatex $(TARGET)

clean:
	rm -rf $(TARGET).pdf $(TARGET).aux $(TARGET).bbl $(TARGET).blg $(TARGET).toc $(TARGET).log $(TARGET).out
