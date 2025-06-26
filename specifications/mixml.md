Below is a concise snapshot of **MIXML**, our pedagogical core language, that all examples will target.

### Language Essence

The language is a "Scheme with ML syntax"-style applicative language that is untyped and eagerly evaluated. It is
extremely simple:

- A program is an (optional) sequence of `let` definitions, followed with an expression.
- The only datatypes are functions and symbols.
- The only expressions available are literals -- lambdas and symbols -- as well as function applications, whether
of infix, postfix, mixfix, etc. functions.

The key feature is this: **Unicode‑friendly mixfix operators** declared with underscore holes (e.g. `_×_`, `if_then_else_,` `_²`, etc.).

### Syntax demonstration


TBD:::::: how do we deal with nullary functions?

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

## Substantial examples

Although we only have functions and symbols, Church encoding allows us to obtain many base types, as well as sum and product types. Then, mixfix operators give us syntax. The result is a surprisingly easy-to-read language.

### Example: Booleans

```
let true  = fun x -> fun y -> x and
let false = fun x -> fun y -> y and
let if:_then:_else:_ =
    fun bool this-fn that-fn -> ((bool this-fn that-fn)) and
let ¬_    = fun p -> p false true in
if: (¬ true)
    then: (fun -> 'not-true)
    else: (fun -> 'not-false)

```

### Example: Conses

```
let true  = fun x -> fun y -> x and
let false = fun x -> fun y -> y and
let [_._] = fun x y -> fun z -> z x y and
let _::_. = fun x y -> [ x . y ] and
let []    = false and
let ℓswitch:_case-pair:_case-nil:_ =
    fun list on-pair on-nil ->
        (list on-pair on-nil)
ℓswitch: [] case-pair: fun -> true true case-nil: fun -> false
```



## Lexical analysis and base grammar of base MIXML 

One of the takeaways of our mixfix parser implementation is that, as the set of mixfix operators
in scope changes, so does the effective grammar being parsed.

The following "pseudo-lex" specification shows how tokenization might work. Note
that, as far digits are involved, non-floats will be parsed as identifiers.

```{lex}
$id_part            = (all non-whitespace printable Unicode other than special chars _ ( ) ' = ->.)

$white+             (discard)
(                   <open_paren>
)                   <close_paren>

in | and | let | = | end | infixl |
     infixr | infix |fun | ->
                    (retain keyword verbatim)

'                   <quote>

(floating point #)  <float>

($id_part)+ |
    _ ($id_part)+ (_ ($id_part)?)* (_)+
                    <ident>
```

After lexical analysis and producing a token stream, the following
BNF grammar then recognizes valid programs. Note that the `fun_app`
rule is where mixfix parsing/rewriting comes into play. Specifically,
the option for multiple expressions in order signal potential
applications of mixfix operators, and explicitly requires handling
of ambiguous left recursion.


```{bnf}
<program>   ::= <decl_list> "in" <expr> | <expr>
<decl_list> ::= <decl> ("and" <decl>)*
<decl>      ::= "let" <ident> "=" (<term_list>)+ "end" | "infixl" <float> <ident> 
              | "infixr" <float> <ident> | "infix" <float> <ident>

# The following rule is usually augmented with
# mixfix operator alternatives, and explicitly
# requires dealing with left recursion.
<term_list>   ::= (<expr>)+

<expr>      ::= <lambda> | <ident> | <quote> <ident>
              | <open_paren> <expr> <close_paren>

<lambda> ::= "fun" (<ident>)* "->" <term_list>
```
