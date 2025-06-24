# MIXML Pedagogical Python Quarto Notebook – Master Plan

## 1  Overall Objective

Create a Python Quarto book that **teaches and implements** mixfix parsing in a tiny, untyped, applicative ML‑style language called **MIXML**. The notebook must:

- Begin with an immediate, runnable demo ("wow factor")
- Build every component—from lexer to evaluator—in small, comprehensible increments
- Highlight ambiguities and their resolution via operator precedence/associativity
- Remain < 1000 LOC of library code so students can read it end‑to‑end in one sitting
- Provide hidden pretty-printing functions that provide concise symbolic representations of data structures.

### What you can rely on

Use the PDFs in the 'references' folder as technical references on mixfix operator parsing.

---


## 3  Structure/Flow

> **Guiding Principle**  Readers repeatedly *see* a concept, *code* it, and *test* it before moving on.

| Step | Section Title                         | Pedagogical Aim                                                | Transition Rationale                                  | **Aspect of MIXML + Implementation Introduced**            |
| ---- | ------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------- | ---------------------------------------------------------- |
| 0    | Preamble & Quick Demo                 | Hook interest with a runnable MIXML snippet producing a result | Capitalise on curiosity before diving into mechanics  | Running MIXML code via **bootstrap parser & evaluator**    |
| 1    | Tokenisation Fundamentals             | Distinguish identifiers, Unicode operator symbols, numerals    | Lexical categories are prerequisite for parsing rules | **MIXML lexer skeleton**; `Token` classes; Unicode support |
| 2    | Baseline Expression Grammar           | Parse variables, application, `let`, and lambdas (no mixfix)   | Establish core AST before adding operator complexity  | **AST nodes**; always‑recursive `let`; environment basics  |
| 3    | Mixfix Declaration Syntax             | Parse underscore‑hole patterns & build operator table          | Shows how syntax like `_×_` maps to arity/fixity data | **OperatorTable**; underscore split; arity derivation      |
| 4    | Ambiguous Operator Sequences          | Parse sequences without precedence → produce forest            | Makes ambiguity concrete; motivates precedence rules  | **Ambiguous AST node**; simple sequence parser             |
| 5    | Precedence & Associativity Resolution | Implement precedence‑climbing / Pratt algorithm                | Resolves forest into tree or raises `AmbiguityError`  | **Resolver** using operator table; tests with `_×_`, `_+_` |
| 6    | Pattern‑Matching Expressions          | Introduce `match … with` syntax & evaluation                   | Required for `if_then_else_` implementation           | **Pattern AST**; evaluator pattern‑match logic             |
| 7    | Mixfix Inside Patterns                | Allow operators in pattern position (`x :: xs`)                | Demonstrates same mixfix rules apply in patterns      | **Pattern parser extension**; shared operator table        |
| 8    | Evaluator for MIXML                   | Implement interpreter with closures, ints, bools, symbols      | Completes language runtime; enables full examples     | **eval()**; built‑ins (`_×_`, `_+_`, `_²`) as Python funcs |
| 9    | Pretty‑Printing & REPL Helpers        | Provide `pretty()` for AST & values; minimal REPL loop         | Improves UX; aids debugging & demonstration clarity   | **pretty()**; colorised Unicode output                     |
| 10   | Demo/Implementation Reconciliation    | Swap out bootstrap, validate equivalence, celebrate            | Confirms student code correctness; ends learning arc  | **assert\_equivalent\_asts**; set `USING_DEMO = False`     |

---

## 3  Quick‑Demo Implementation Plan (“hidden setup cell” strategy)

1. **Bootstrap module**  Ship `mixfix_demo.py` (≤1000 LOC) exposing `Operator`, `parse`, `pretty`, `AmbiguityError`, `eval`.
2. **Hidden *****init***** cell**  First cell (collapsed via Quarto metadata) runs:
   ```python
   import importlib, sys, types, pathlib
   USING_DEMO = True
   sys.path.insert(0, str(pathlib.Path().resolve()))  # ensure we see mixfix_demo.py
   from mixfix_demo import *
   ```
3. **Wow‑factor demo**  Immediately evaluate:
   ```python
   fact = parse_and_eval("""
       let fact n = if_then_else_ (n == 0) 1 (_×_ n (fact (_- n 1)))
       fact 5
   """)
   print(fact)  # ⇒ 120
   ```
4. **Handoff**  At notebook end, after the student re‑implements everything, run:
   ```python
   assert_equivalent_asts(parse, student_parse)
   USING_DEMO = False
   ```
   Subsequent runs use the freshly implemented parser.
