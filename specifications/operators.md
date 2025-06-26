# Specifications of mixfix operators

Per Danielsson and Norell (see `references/danielsson-norell-mixfix.pdf`), we have the following Agda defnitions:

## Operators

Danielsson and Norell give two equivalent ways of specifiying the syntactic operators. The first way of doing is by specifying the *holes* of a mixfix operator with underscores: i.e. `_+_`, `if_then_else_`, etc.

Agda's internal representation is a little more abstruse.

### Agda's internal representation of Operators

An `Operator` is defined by its fixity (i.e. whether an operator has holes before its first name part, and after its first name part), and by its "arity" (the number of ***internal*** holes, or equivalently `# of name parts - 1`) in the operator. More precisely, we have the Agda definitions:

```{agda}
data Fixity : Set where
    prefx: Fixity
    infx: Associativity -> Fixity
        -- Note that "left" or "right" associativity here refers to
        -- the left "outer hole" or right "outer hole" in an infix
        -- operator, i.e. one with holes on both sides.
    postfx : Fixity
    closed: Fixity

data Associativity : Set where
    left : Associativity
    right : Associativity
    non : Associativity

record Operator (fix : Fixity) (arity : Nat) : Set where
    field nameParts : Vec NamePart (1 + arity)
        -- note that "arity" here refers to the
        -- number of _internal_ holes.
```

More concretely, we have the following examples of what operators with a certain fixity and certain arity might look like:

| Fixity  | Arity = 0 | Arity = 1 | Arity = 2   |
| ------- | --------- | --------- | ----------- |
| Prefix  | `a_`    | `_a_b`  | `_a_b_c`  |
| Infix   | `_a_`   | `_a_b_` | `_a_b_c_` |
| Postfix | `_a`    | `a_b_`  | `a_b_c_`  |
| Closed  | `a`     | `a_b`   | `a_b_c`   |
