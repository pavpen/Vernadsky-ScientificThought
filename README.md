A Translation of Vernadsky's _Scientific Thought as a Planetary Phenomenon_ into English
======================================================================================

This translation is made from the Russian published at
[http://vernadsky.lib.ru/e-texts/archive/thought.html](http://vernadsky.lib.ru/e-texts/archive/thought.html).


LaTeX Packages Needed for Compilation
-------------------------------------

If you are using a packaged texlive distribution, you'll need to install, at
least

* texlive-bibtexextra
* texlive-langcyrillic
* texlive-luatex

You'll also need to have [biber](http://biblatex-biber.sourceforge.net/)
installed.


Known Trouble
-------------

If you see a “metric data not found or bad” error, e.g.:

	! Font \T2A/cmr/m/n/12=larm1200 at 12pt not loadable: metric data not found or 
	bad.
	<to be read again> 
			   relax 
	l.15 }
	      %
	? 


while running `make` in the `tex` directory:

*1.* Open `thought.tex`

*2.* Comment out the `\usepackage{bigfoot}` line:

```latex
%\usepackage{bigfoot} % install {ncctools} and {bigfoot}
```

*3.* Comment out the various `\DeclareNewFootnote` commands, and uncomment the corresponding `\newcommand` commands which provide replacements for these footnote commands:

```latex
%%
% Footnotes:
% Add a comma between the footnote marks:
\newcommand{\fncomma}{${\rule{-1pt}{0pt}}^{, }$}
%\DeclareNewFootnote{default} % Author's footnotes.
%\DeclareNewFootnote{Ed}[fnsymbol] % The editor's footnotes.
%%\DeclareNewFootnote{Ed}[greek] % The editor's footnotes.
%\MakePerPage{footnoteEd}
%\DeclareNewFootnote{Transl}[Roman] % Our (translator's) footnotes.
%\MakePerPage{footnoteTransl}

% Footnotes with original Russian phrases (e.g., which don't have a direct
% English language equivallent).
%\DeclareNewFootnote{Rus}[roman]
%\MakePerPage{footnoteRus}
\newcommand{\footnoteRus}[1]{%
	\footnoteTransl{%
	  \begin{otherlanguage}{russian}%
	    #1%
	  \end{otherlanguage}%
	}}

% Uncommend the following macros when running pdflatex to generate the font
% cache, instead of the above (when avoiding running out ouf TeX counters by
% disabling <bigfoot>).  Also, uncomment the substitute for \footnoteRus above.
%
\newcommand{\footnoteEd}[1]{\footnote{#1}}
\newcommand{\footnoteTransl}[1]{\footnote{#1}}
\newcommand{\footnotemarkTransl}{\footnotemark}
\newcommand{\footnotetextTransl}{\footnotetext}
```

*4.* Run `make fontcache`

*5.* Restore `thought.tex` to its initial state (i.e., reverse steps 2 and 3):


```latex
\usepackage{bigfoot} % install {ncctools} and {bigfoot}
```

```latex
%%
% Footnotes:
% Add a comma between the footnote marks:
\newcommand{\fncomma}{${\rule{-1pt}{0pt}}^{, }$}
\DeclareNewFootnote{default} % Author's footnotes.
\DeclareNewFootnote{Ed}[fnsymbol] % The editor's footnotes.
%\DeclareNewFootnote{Ed}[greek] % The editor's footnotes.
\MakePerPage{footnoteEd}
\DeclareNewFootnote{Transl}[Roman] % Our (translator's) footnotes.
\MakePerPage{footnoteTransl}

% Footnotes with original Russian phrases (e.g., which don't have a direct
% English language equivallent).
\DeclareNewFootnote{Rus}[roman]
\MakePerPage{footnoteRus}
%\newcommand{\footnoteRus}[1]{%
%	\footnoteTransl{%
%	  \begin{otherlanguage}{russian}%
%	    #1%
%	  \end{otherlanguage}%
%	}}

% Uncommend the following macros when running pdflatex to generate the font
% cache, instead of the above (when avoiding running out ouf TeX counters by
% disabling <bigfoot>).  Also, uncomment the substitute for \footnoteRus above.
%
%\newcommand{\footnoteEd}[1]{\footnote{#1}}
%\newcommand{\footnoteTransl}[1]{\footnote{#1}}
%\newcommand{\footnotemarkTransl}{\footnotemark}
%\newcommand{\footnotetextTransl}{\footnotetext}
```

*6.* Now your `make` should succeed.


###Explanation:

The input file uses UTF-8 to include the various languages in the document
(English, Russian, occasionally, French and German).  Also, the LaTeX packages,
and the macros used in the document use up all the TeX counters, and cause an
overflow.  These conditions presently require the use of LuaLaTex to compile
the document.

Unfortunately, the way `lualatex` comes in my Gentoo TeX Live installation it
doesn't compile fonts, and generate metric data.  The usual `latex` does that
properly, but I can't run, because of the above considerations.

Steps 1–3 make LaTeX run (if input encoding errors are ignored) by using
temporary replacement macros.  This fills the LaTeX font cache with the
required compiled fonts and their matadata.  After this has been accomplished,
`lualatex` can, finally, use the font chache to compile the disired output PDF.



(C) 2012–2015 Pavel M. Penev
