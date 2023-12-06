# Compilers

## Steps
- lexical analysis creates stream of tokens
- parser creates parse tree
- code generator creates object file

## Tokens
- has a type and a value
    - value may be null
- usually use regular expressions to define tokens

## Regular expressions
- a formal mechanism for defining a language
- precise, unambiguous, well-defined
- e.g. strings that contain only letters, numbers, and underscores, start with a letter, don't contain consecutive underscores, and don't end with underscores: `[a-zA-Z](_[a-zA-Z0-9]|[a-zA-Z0-9])*`

### Literals
- characters from the alphabet
- escape codes for other stuff: `\t`, `\n`, `\r`, `\\`

### Operators
- parenthesis for grouping
- `|` for choice
    - e.g. `J(ava|AVA)`

### Character class
- set of possible characters
- e.g. `[0123456789]`
- ranges are allowed: `[0-9]`
- negate with `^`: `[^A-Z]` (matches any character that isn't a capital letter)
- shorthands:
    - `\d` for digits
    - `\s` for whitespace (`[ \t\r\n]`)
    - `\w` for word character (`[0-9a-zA-Z_]`)
    - `\D`, `\S`, `\W` for `[^\d]`, etc.
    - POSIX has its own shorthands too

### Wildcards
- `.` matches any character, except newlines

### Repetition
- `a*` matches 0 or more `a`
- `a+` matches 1 or more `a`
- `?a` matches 0 or 1 `a`

## Finite state automota (FSA)
- is an "accepting rule"
    - finite set of states
    - transition function (relation) between states based on next character in string
        - DFA vs. NFA
    - start state $s_0$
    - set of accepting states
- a string is accepted if you can start in $s_0$ and end up in any accepting state, consuming 1 character per transition
- not all states are accepting
    - accepting states are circled
- transitions are labeled by the character they consume

## Limitations of RE
- expressive power of RE is same as FSA, and both are limited
- writing an RE for strings of balanced parens is impossible
- FSA has predefined limit on how much nesting can be encoded

## REs in practice
- used to find a match

### Anchors
- specify where matching string should be with respect to a line of text
- `^` beginning of line
- `$` end of line
- `\A` beginning of string
- `\z` end of string

### Greedy vs. lazy
- repetition allows multiple matches to begin at same place
- which match is selected depends on algorithm:
    - greedy: matches as much as possible
    - lazy: matches as little as possible
 - default is usually greedy
 - for lazy matching use `*?` or `+?`

## Chomsky's hierarchy
- type 3: regular
    - left side is 1 non-terminal
    - right side has at most 1 non-terminal
    - `S -> aaB`
    - finite state machine (FSA)
- type 2: context-free
    - left side is 1 non-terminal
    - `S -> aBBb`
    - push-down automaton (PDA)
- type 1: context-sensitive
    - left side contains 1 non-terminal that is modified by rule
    - `aSBb -> aBBb`
    - bounded turing machine
- type 0: general
    - no restrictions
    - `aSBb -> BSbaa`
    - turing machine

## Context free grammar (CFG)
- all rules have 1 non-terminal on the left
- more powerful than RE
- efficient algorithms
- epsilon symbol means empty string


