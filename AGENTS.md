# Project goal.

I am interested in creating an expository Python Quarto (.qmd) notebook about mixfix operators and parsing: that is, all of the definitions needed to express mixfix operators, as well as all of the concepts, data structures, and algorithms used to create operational parses for mixfix operators. Ultimately, I want to explain how mixfix operator implementations work in the practical situation where (a) an underlying language with a grammar exists (e.g. an untyped ML-like core calculus with "equation" style definitions), and (b) multiple operators exist in a same scope -- some perhaps that lead to multiple possible parses when they overlap syntactically -- and so the parser must fail and present all possible parses as a practical aid for programmers to fix this problem.

# Ultimate work product.

The file "mixfix-parsing.qmd": a Python Quarto (.qmd) notebook that combines exposition and implementation.

Throughout, it is important to implement pretty-printing that concisely expresses the important technical details involved with data structures. As a stretch goal, it would be very nice to "pretty-print" how key algorithms work in specific executable cases.

# Final notebook requirements.

- My audience is an undergraduate student that has taken a first compilers course.

- All explanations and implementations must balance the two criteria:
  -  You must be parsimonious in the data structures, concepts, and algorithms you introduce, in order to explain the "essence" of mixfix operator definition and parsing.
  -  Simultaenously, you must be "complete" in your explanations and practical, in the sense that the reader should leave with an actual working didactic implementation of a mixfix parser that meets the goals above.

- For didactic purposes, I also want to provide pretty-printing functions for data types and algorithm runs that concisely express the key details of data structures required.

# How to work

- Think long and hard about pedagogical and explanatory/expository approaches to complex concepts and processes.

- Consider multiple alternatives in order to satisfy the requirements of the task, but ultimately select one that is best pedagogically.



