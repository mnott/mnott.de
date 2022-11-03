---
title: APA Style BibDesk
menu_order: 1
post_status: publish
post_excerpt: This is a solution for using an APA-Style preview in BibDesk.
taxonomy:
    category:
        - LaTeX
        - BibDesk
        - MacOS
        - Dissertation
    post_tag:
        - LaTeX
        - BibDesk
        - MacOS
comment_status: open
---

# Motivation

When previewing literature database entries in [BibDesk](https://bibdesk.sourceforge.io/), I'd like to have precisely the same format as I'm going to have a dissertation. To do that, I found [this](https://github.com/nbrewk/biblatex-apa-preview-bibdesk). 

# Installation

You can simply open the `TeX Preview` preferences of BibDesk, and click on `Edit...` next to `TeX template`.

Then standard template is this:

```latex
\documentclass[letterpaper]{article}
\usepackage{url}
\pagestyle{empty}
\textwidth = 6.5in
\voffset = -105pt
\hoffset = -120pt
\renewcommand{\refname}{}

\begin{document}
\nocite{<<CiteKeys>>}
\bibliography{<<File>>}
\bibliographystyle{<<Style>>}
\end{document}
```

Note that I've added `\usepackage{url}` to the standard template so that Bibtex would not balk out when I was using that tag in e.g. a note field.

Replace that content by this:

```latex
\documentclass[letterpaper]{article}

\pagestyle{empty}
\textwidth = 6.5in
\voffset = -105pt
\hoffset = -120pt

\renewcommand{\refname}{}

 % Times-like font
\usepackage{mathptmx}

% biblatex-apa specific
\usepackage[american]{babel}
\usepackage[autostyle]{csquotes}
\usepackage[style=<<Style>>,backend=biber]{biblatex}
\DeclareLanguageMapping{american}{american-apa}

\addbibresource{<<File>>.bib} % you have to add ``.bib'' here because,
                              % for some reason, it won't find the
                              % file without it

% The following command is provided for LaTeX2RTF compatibility with amslatex.
\newif\iflatextortf
\iflatextortf
\providecommand{\bysame}{\_\_\_\_\_}
\fi

\renewbibmacro*{addinfo2}{%
   %\printtexte[brackets]{% DELETED
   \iffieldequalstr{entrysubtype}{nonacademic}
     {}
     {\iffieldbibstring{entrysubtype}
        {\bibcplstring{\thefield{entrysubtype}}}
        {\printfield{entrysubtype}}}%
    \setunit*{\addsemicolon\addspace}%
    \ifentrytype{report}{}{\printfield{note}}%
    %} % DELETED
    }

\DeclareSourcemap{
  \maps[datatype=bibtex]{
    \map{
      \step[fieldsource=note, final]
      \step[fieldset=addendum, origfieldval, final]
      \step[fieldset=note, null]
    }
  }
}
\DeclareFieldFormat{journaltitle}{\mkbibemph{#1},} % italic journal title
\DeclareFieldFormat[article]{title}{`#1',} % title of journal article is
\DeclareFieldFormat[misc]{title}{\mkbibemph{#1}	}
\DeclareFieldFormat[article]{volume}{#1~}
\DeclareFieldFormat[article]{pages}{#1}
\defbibenvironment{bibliography}
{\list
 {}
 {\setlength{\leftmargin}{\bibhang}%
  \setlength{\itemindent}{-\leftmargin}%
  \setlength{\itemsep}{\bibitemsep}%
  \setlength{\parsep}{\bibparsep}}}
{\endlist}
{\item}

\setlength\bibitemsep{1.7\itemsep}
\renewbibmacro*{pageref}{%
  \addperiod% NEW
  \iflistundef{pageref}
    {}
%    {\printtext[parens]{% DELETED
    {\newline\footnotesize\printtext[parens]{% NEW
       \ifnumgreater{\value{pageref}}{1}
         {\bibstring{backrefpages}\ppspace}
     {\bibstring{backrefpage}\ppspace}%
 %      \printlist[pageref][-\value{listtotal}]{pageref}}}}% DELETED
       \printlist[pageref][-\value{listtotal}]{pageref}\addperiod}}}% NEW

  \DeclareCiteCommand{\citeauthor}
  {\boolfalse{citetracker}%
   \boolfalse{pagetracker}%
   \usebibmacro{prenote}}
  {\ifciteindex
     {\indexnames{labelname}}
     {}%
   \printtext[bibhyperref]{\printnames{labelname}}}
  {\multicitedelim}
  {\usebibmacro{postnote}}

  \DeclareCiteCommand{\citeyear}
  {\boolfalse{citetracker}%
   \boolfalse{pagetracker}%
   \usebibmacro{prenote}}
  {\ifciteindex
     {\indexnames{labelname}}
     {}%
   \printtext[bibhyperref]{\printdate}}
  {\multicitedelim}
  {\usebibmacro{postnote}}




\begin{document}

\nocite{<<CiteKeys>>}
\printbibliography[heading=none] % I don't want a heading on my previews

\end{document}
```

The referenced example is much shorter, but I've fixed some other things while doing this; e.g., making sure that Note fields are not wrapped in square brackets.

To use it, back in the BibDesk settings, as a style enter `apa` and also, for the BibTeX path next to `Full path to bibtex:`, enter `/Library/TeX/texbin/biber`.

# Observation

While that precisely works, it has a substantial downside: Biber is significantly slower than BibTeX. So I ended up switching back, and keeping both configuration files as a backup in `~/Library/Application Support/BibDesk/`. Unfortunately, BibDesk only applies for one template, `previewtemplate.tex` to exist, which means, I've to copy around the template should I need to, and also, change back and forth between Biber and BibTex as BibTeX path.

Anyway, it is not particularly nice, but workable.
