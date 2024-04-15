# This is Not a Loop
`for (i <- 1 to 10) println(i)`

## `for` expressions in Scala
- all for-expressions are translated to applications of higher order functions to a **generator**
    - if there's a `yield`:
        - `map`
        - `flatMap`
        - `withFilter`
    - no `yield`:
        - `forEach`
        - `withFilter`
- syntaxes:
    - `for ( SEQ ) yield EXPR`
    - `for { SEQ } yield EXPR`
        - when using curly braces, semicolons separating `SEQ`s are optional
    - where `SEQ` is a sequence of generators, definitions, and/or filters
## Generators
- define boundaries of our repetition
- every for-expression starts with one
- form: `PAT <- EXPR`
    - `EXPR` is usually a list
        - can be generalized to anything that defines the methods `map`, `flatMap`, `withFilter`, and `forEach`
    - `PAT` is matched by pattern matching, one by one, to all elements from `EXPR`
        - no expression matched if there is no match

## Filters
- form: `if EXPR`
    - `EXPR` must be `Boolean` typed
    - filter drops everything where `EXPR` returns false