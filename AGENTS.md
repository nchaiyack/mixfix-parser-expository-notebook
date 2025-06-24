# Project goal.

I am interested in creating an expository book, written in Python Quarter (.qmd) about mixfix operators and parsing: that is, all of the definitions needed to express mixfix operators, as well as all of the concepts, data structures, and algorithms used to create operational parses for mixfix operators. Ultimately, I want to explain how mixfix operator implementations work in the practical situation where (a) an underlying language with a grammar exists (e.g. an untyped ML-like core calculus with "equation" style definitions), and (b) multiple operators exist in a same scope -- some perhaps that lead to multiple possible parses when they overlap syntactically -- and so the parser must fail and present all possible parses as a practical aid for programmers to fix this problem.

# Files/directories

  - `references/`. This folder contains PDFs that you can use as technical references on mixfix operator parsing and implementation.

  - `chapters/`. This folder contains '.qmd' files for each chapter in the book.

  - `specifications/`. This folder contains important instructions you must follow: `specifications/overall.md` specifies the goals of the book and its writing; `specifications/mixml.md` specifies the toy language "MIXML" that will be used to illustrate mixfix operators in use.

  - `templates/chapter.qmd`. A template you must use to create a new chapter.

  - `_quarto.yml`, `index.qmd`. Respectively Quarto notebook configuration,and first chapter of the book.

  - `build.sh`. Build script for the whole book.

# Ultimate work product.

A Python Quarto book that combines exposition and implementation, as specified in `specifications/overall.md`.

Throughout, it is important to implement pretty-printing that concisely expresses the important technical details involved with data structures. As a stretch goal, it would be very nice to "pretty-print" how key algorithms work in specific executable cases.

# Final book requirements.

- My audience is an undergraduate student that has taken a first compilers course.

- All explanations and implementations must balance the two criteria:

  -  You must be parsimonious in the data structures, concepts, and algorithms you introduce, in order to explain the "essence" of mixfix operator definition and parsing.

  -  Simultaenously, you must be "complete" in your explanations and practical, in the sense that the reader should leave with an actual working didactic implementation of a mixfix parser that meets the goals above.

- For didactic purposes, I also want to provide pretty-printing functions for data types and algorithm runs that concisely express the key details of data structures required.

# Instructions

## When you are done, always:

- Verify that the book will render by executing `build.sh`. Make sure that it exists successfully and shows no errors.

## When you need to research about mixfix parsing

- Read the reference PDFS in `references/`.

## When writing

- Write in prose paragraphs in a didactic style, as if you were giving a semi-formal lecture to undergraduate students.

- Think long and hard about pedagogical and explanatory/expository approaches to complex concepts and processes.

- Consider multiple alternatives in order to satisfy the requirements of the task, but ultimately select one that is best pedagogically.
