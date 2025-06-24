Below is a concise snapshot of **MIXML**, our pedagogical core language, that all examples will target.

### Language Essence

- **Untyped, Applicative, ML‑inspired**
- **Unicode‑friendly mixfix operators** declared with underscore holes (`_×_`, `if_then_else_,` `_²`, etc.).
- **Minimal data types**: lambdas, lisp-style symbols (`'symbol`)

### Example Fragments We Will Showcase

- Tail‑recursive factorial via `if_then_else_` and `_×_`.
- Operator ambiguity puzzle: `_++_` vs `_+_` on `1 + 2 ++ 3`.
- Postfix power example using `_²`: compare parses of `5² × 2`.

---

## Lexical analysis and base grammar of base MIXML 

One of the takeaways of our mixfix parser implementation is that, as the set of mixfix operators
in scope changes, so does the gramma

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
