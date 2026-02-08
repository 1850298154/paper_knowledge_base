export SHELL = /bin/bash
article = tagunif.en
inputs  = $(wildcard *.tex)
classes = $(wildcard *.cls)
styles  = $(wildcard *.sty)
figures = $(wildcard *.fig)
biblios = $(wildcard *.bib)
bibsty  = $(wildcard *.bst)

all: $(article).pdf

.PRECIOUS: $(patsubst %.fig,%.pdf,$(figures))\
	   $(article).bbl

%.ftex: %.fig %_tex.pdf
	echo '\begin{picture}(0,0)%' > $@
	echo "\includegraphics{$(patsubst %.fig,%_tex.pdf,$<)}%" >> $@
	echo "\end{picture}%" >> $@
	fig2dev -L pdftex_t $< >> $@

%_tex.pdf: %.fig
	fig2dev -L pdftex $< > $@

rtgderivation.etex: rtgderivation.fig
	echo "\documentclass{book}" > $@
	echo "\usepackage{color}" >> $@
	echo "\usepackage[final]{graphicx}" >> $@
# 	echo "\usepackage{amsmath}" >> $@
# 	echo "\usepackage{amssymb}" >> $@
# 	echo "\usepackage{ifthen}" >> $@
# 	echo "\usepackage{xspace}" >> $@
	echo "\usepackage{macros}" >> $@
	echo '\begin{document}' >> $@
	echo '\pagestyle{empty}' >> $@
	echo '\begin{picture}(0,0)%' >> $@
	echo "\includegraphics{$(patsubst %.fig,%.pstex,$<)}%" >> $@
	echo "\end{picture}%" >> $@
	fig2dev -L pstex_t -m 0.8 $< >> $@
	echo "\end{document}" >> $@

rtgderivation.pstex: rtgderivation.fig
	fig2dev -L pstex -m 0.8 $< > $@

%.etex: %.fig
	echo "\documentclass{book}" > $@
	echo "\usepackage{color}" >> $@
	echo "\usepackage[final]{graphicx}" >> $@
# 	echo "\usepackage{amsmath}" >> $@
# 	echo "\usepackage{amssymb}" >> $@
# 	echo "\usepackage{ifthen}" >> $@
# 	echo "\usepackage{xspace}" >> $@
	echo "\usepackage{macros}" >> $@
	echo '\begin{document}' >> $@
	echo '\pagestyle{empty}' >> $@
	echo '\begin{picture}(0,0)%' >> $@
	echo "\includegraphics{$(patsubst %.fig,%.pstex,$<)}%" >> $@
	echo "\end{picture}%" >> $@
	fig2dev -L pstex_t -m 1 $< >> $@
	echo "\end{document}" >> $@

%.pstex: %.fig
	fig2dev -L pstex -m 1 $< > $@

%.edvi: %.etex %.pstex
	latex $<
	mv `basename $*.dvi` $*.edvi

%.eps: %.edvi
	mv $*.edvi $*.dvi
	dvips -E $*.dvi -o $@

%.pdf: %.eps
	epstopdf $<

%.aux: %.tex # cancel implicit rule

$(article).out: $(article).tex $(patsubst %.fig,%.pdf,$(figures))\
	        $(classes) $(styles)
	pdflatex $(article)

$(article).bbl: $(article).out $(biblios) $(bibsty)
	bibtex $(article)

%.dvi: %.tex # cancel implicit rule

$(article).pdf: $(article).out $(article).bbl
	pdflatex $(article)
	pdflatex $(article)

#	if [ -z "`ps | grep xpdf | grep -v grep`" ]; then xpdf -paper A4 $@ & fi

%.ind: %.out indexstyle
	makeindex -s indexstyle $*

%_fig.pdf: %.ftex %_tex.pdf
	pdflatex $<
	mv `basename $*.pdf` $*_fig.pdf

%.tar.gz: Makefile %.tex $(figures) $(classes) $(styles) $(biblios)
	mkdir -p $*
	cp Makefile $*.tex $(figures) $(classes) $(styles) $(biblios) $*
	tar cvfz $@ $*
	rm -rf $*

update: clean
	cvs -z3 update
	${MAKE}

bibcount: $(article).bbl
	grep bibitem $< | wc -l

dist: $(article).tar.gz $(article).pdf clean

distclean: clean
	rm -f *.pdf\
	  $(patsubst %.fig,%.pdf,$(figures))

clean:
	rm -f *.aux *.log *~ *.bak SAVE.* *.toc *.bmt *.brf figures/*.bak\
	  *.dvi *.ps *.bbl *.blg *.out *.mtc* *.ilg *.idx *.ind *.ptc \#*\
	  $(patsubst %.fig,%.ftex,$(figures))\
	  $(patsubst %.fig,%.etex,$(figures))\
	  $(patsubst %.fig,%_tex.pdf,$(figures))\
	  $(patsubst %.fig,%_fig.pdf,$(figures))\
	  $(patsubst %.fig,%.dvi,$(figures))
