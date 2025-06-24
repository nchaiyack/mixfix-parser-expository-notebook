Below is a concise snapshot of **MIXML**, our pedagogical core language, that all examples will target.

### Language Essence

- **Untyped, Applicative, ML‑inspired, Eagerly-evaluated**
- **Minimal data types**: lambdas, lisp-style symbols (e.g. `'abc`)
- **Unicode‑friendly mixfix operators** declared with underscore holes (e.g. `_×_`, `if_then_else_,` `_²`, etc.).

### Syntax demonstration

The following are valid MIXML programs of increasing syntactic
complexity that should evaluate to the same symbol `'abc`:

```
'abc
```

```
let abc = 'abc in abc
```

```
let abc = 'abc and
let def = 'def in
abc
```

```
let _↩_ = fun l r -> l in
_↩_ `abc `def
```

```
let _↩_ = fun l r -> l in
'abc ↩ 'def
```

```
let ⟦_⟧ = fun x -> x in
⟦ 'abc ⟧
```

```
let _✓_✕ = fun x y -> x in
'abc ✓ 'def ✕
```
---

## Lexical analysis and base grammar of base MIXML 

One of the takeaways of our mixfix parser implementation is that, as the set of mixfix operators
in scope changes, so does the effective grammar being parsed.

The following "pseudo-lex" specification shows how tokenization might work:

```{lex}
$id_part            = (all non-whitespace graphic Unicode other than "_")

'                   <quote>
(                   <open_paren>
)                   <close_paren>

(floating point #)  <float>

($id_part)+ |
    _ ($id_part)+ (_($id_part)+)* (_)+

                    <ident>

$white+             (discard)

in | and | let | = | end | infixl |
     infixr | infix |fun | ->

                    (retain keyword verbatim)
```

After lexical analysis and producing a token stream, the following
BNF grammar then recognizes valid programs:


```{bnf}
<program>   ::= <decl_list> "in" <expr> | <expr>
<decl_list> ::= <decl> ("and" <decl>)*
<decl>      ::= "let" <ident> "=" (<expr>)+ "end" | "infixl" <float> <ident> 
              | "infixr" <float> <ident> | "infix" <float> <ident>

<expr>      ::= <lambda> | <ident> | <quote> <ident>
              | <open_paren> <expr> <close_paren>

<lambda> ::= "fun" (<ident>)+ "->" <expr>
```
