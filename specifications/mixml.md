Below is a concise snapshot of **MIXML**, our pedagogical core language, that all examples will target.

### Language Essence

- **Untyped, Applicative, ML‑inspired**
- \*\*Always‑recursive \*\*`` blocks (`let f = … and g = …`)—mirrors `let rec`.
- **Unicode‑friendly mixfix operators** declared with underscore holes (*×*, if\_then\_else\_, \_², etc.).
- **Minimal data types**: integers, booleans, symbols; lists if needed for pattern demos.
- **Pattern matching** via `match … with` (wildcard `_`, literal symbols, cons operator `_::_`).

### Example Fragments We Will Showcase

- Tail‑recursive factorial via `if_then_else_` and `_×_`.
- Operator ambiguity puzzle: `_++_` vs `_+_` on `1 + 2 ++ 3`.
- Postfix power example using `_²`: compare parses of `5² × 2`.

---

## 5  BNF Grammar (MIXML)

*Angle‑brackets are non‑terminals; terminals are ****bold**** keywords or symbols.*

```bnf
<program>       ::= <decl-list>
<decl-list>     ::= <decl> | <decl-list> **and** <decl>

<decl>          ::= **let** <binding>
<binding>       ::= <lhs> **=** <expr>

<lhs>           ::= <mixfix-ident> | <ident> <param-list>
<param-list>    ::= <ident> | <param-list> <ident>

<expr>          ::= <lambda>
                |  <match>
                |  <iflike>
                |  <mixfix-seq>

<lambda>        ::= **fun** <ident> **->** <expr>
<match>         ::= **match** <expr> **with** <pat> **->** <expr> { **|** <pat> **->** <expr> }
<iflike>        ::= <ident> (**_** <expr>)+          -- expands to mixfix parse of holes

<mixfix-seq>    ::= <app> { <operator-token> <app> }
<app>           ::= <atom> { <atom> }
<atom>          ::= <ident>
                |  <int> | <bool> | <symbol>
                |  **(** <expr> **)**

<pat>           ::= <ident> | <symbol> | **_** | <pat> <mixfix-op> <pat>

<ident>         ::= letter { letter | digit | **'** }
<mixfix-ident>  ::= operator-token with ≥1 underscore
<operator-token>::= sequence of Unicode Sm/Lo chars & punctuation excluding letters/digits
```

*Precedence & associativity rules* are injected from operator declarations of the form:

```ml
infixl 7 _×_
infixr 5 _++_
```