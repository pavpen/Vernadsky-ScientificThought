NAME = thought

# LATEXMK_OPTS = -pdf
LATEXMK_OPTS = -pdf -pdflatex=lualatex

all: $(NAME).pdf

re: clean
	$(MAKE) all

$(NAME).pdf: *.tex bib/*.bib *.cbx
	latexmk $(LATEXMK_OPTS) $(NAME)

clean:
	rm -f $(NAME).blg $(NAME).bbl $(NAME).out $(NAME).bcf $(NAME).toc \
		$(NAME).dvi $(NAME).pdf $(NAME).run.xml *.log *.fdb_latexmk \
		*.gdb_latexmk *.aux *-blx.bib

view: $(NAME).pdf
	evince $(NAME).pdf
