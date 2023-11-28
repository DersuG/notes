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
