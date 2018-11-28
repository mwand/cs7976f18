# Generic Latex Makefile

.PHONY: edit open pdf clean distclean

####################
# 1. Public Region #
####################

MAIN      = $(basename $(firstword $(TEXFILES)))# basename of main tex file (on which latex is run). In case of several existing tex files, redefine this (the others should be includes)
ALL-AUX   =                                     # basenames of aux files bibtex needs to run over (empty: bibtex not used).  Common value: $(MAIN)
ALL-BIB   =                                     # bib files required by bibtex (= those listed in the bibliography command). Common value: $(wildcard *.bib)

EDITFILES = $(TEXFILES) $(MAIN).tex             # list of files to edit. MAIN is repeated at the end, so that it comes up first in emacs
    CLEAN =                                     # any additional commands to be executed by the     clean command (e.g.      cleaning subdirectories)
DISTCLEAN =                                     # any additional commands to be executed by the distclean command (e.g. dist-cleaning subdirectories)

# Override these variables (or add new ones) locally.
-include makefile-local-vars


#####################################
# 2. Private Region (do not change) #
#####################################

SHELL    = /bin/bash
LATEXBIN = pdflatex -halt-on-error
ALL-BBL  = $(patsubst %,%.bbl,$(ALL-AUX))
TEXFILES = $(wildcard *.tex)


# General targets

default: $(MAIN).pdf

edit:
	$(EDITOR) $(EDITFILES) &

new: clean default


# PDF

pdf: $(MAIN).pdf

# If you have tex files in cwd, etc. that the pdf does not depend on: MOVE THEM, they don't belong here. Keep things simple, not unnecessarily flexible
$(MAIN).pdf: #$(TEXFILES) $(ALL-BBL)
	$(LATEXBIN) $(MAIN)

# bibtex bib: $(ALL-BBL)

# A brief intro to bibtex: suppose your text source contains \bibliography{X}.
# To process this, latex (eventually) needs a bbl file. This is created using bibtex.
# Bibtex in return needs an aux file containing the citations being made, and file X.bib,
# which contains how these citations should be rendered in the bibliography.

# Below I don't list the .aux as a dependence, so that bibtex is not run every time the .aux changes
# (often for reasons that have nothing to do with the bibliography, such as new labels).
# Instead, run bibtex if the bbl file is missing OR if the bib file has changed.
# To force bibtex to run (say when a new \cite command is added), say "make bibtex".

# $(ALL-BBL): $(ALL-BIB)
# 	[ -r $(basename $@).aux ] || $(MAKE) latex
# 	bibtex $(basename $@)

# force latex to re-run, but don't remake any dependencies (thus this is different from "make -B pdf")
latex:
	$(LATEXBIN) $(MAIN).tex


# Opening

open: $(MAIN).pdf
	open $< &

# Cleaning

clean:
	rm -f *.aux *.log *.toc *.bak *.dvi *.lof *.loa *.lot *.bbl *.blg *.out *.nav *.snm *.vrb
	rm -f $(MAIN).pdf
	$(CLEAN)

distclean: clean
	rm -f $(foreach FILE,$(TEXFILES),$(basename $(FILE)).pdf) # must be foreach
	rm -f *~
	$(DISTCLEAN)

# Add new targets locally. This is included at the very end, to allow overriding (although intended use is to add new targets)
-include makefile-local-targets
