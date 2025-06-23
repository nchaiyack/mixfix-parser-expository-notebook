GOAL.
I am interested in creating an expository Python Quarto notebook about mixfix operators and parsing: that is, all of the definitions needed to express mixfix operators, as well as all of the concepts, data structures, and algorithms used to create operational parses for mixfix operators. Ultimately, I want to explain how mixfix operator implementations work in the practical situation where (a) an underlying language with a grammar exists (e.g. an untyped ML-like core calculus with "equation" style definitions), and (b) multiple operators exist in a same scope -- some perhaps that lead to multiple possible parses when they overlap syntactically -- and so the parser must fail and present all possible parses as a practical aid for programmers to fix this problem.

HOW TO THINK.
Think long and hard about pedagogical and explanatory/expository approaches. Consider multiple alternatives in order to satisfy the requirements of the task, but ultimately select one that is best pedagogically.

REQUIREMENTS.
My audience is an undergraduate student that has taken a first compilers course.

All explanations and implementations must satisfy the following criterion: you must be parsimonious in the data structures, concepts, and algorithms you introduce, in order to explain the "essence" of mixfix operator definition and parsing. However, you must be "complete" in your explanations and practical, in the sense that the reader should leave with an actual working didactic implementation of a mixfix parser that meets the goals above.

For didactic purposes, I also want to provide pretty-printing functions for data types that concisely express the key details of data structures required.

ULTIMATE WORK PRODUCT:
A Python Quarto notebook that combines exposition and implementation. Throughout, it is important to implement pretty-printing that concisely expresses the important technical details involved with data structures. As a stretch goal, it would be very nice to "pretty-print" how key algorithms work in specific executable cases.