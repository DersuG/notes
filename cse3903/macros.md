# Macro Preprocessor

## Naive algorithm
- 2-pass approach is tempting
    - permits forward references
- pass 1: build table of macro names and definitions
- pass 2: expand macros
- doesn't allow nested definitions
    - lets you do things like conveniently define a bunch of macros depending on your OS
```
HPOS .MAC
READ .MAC
    AND R0,R0,#0
    ;...
    .MND
WRITE .MAC
    ST R0,Buff
    ;...
    .MND
    .MND
```

## Better algorithm
- single pass
- 2 states:
    - definition mode
        - entered when you come across `.MAC`
        - exited when you come across a corresponding `.MND`
    - expansion mode
        - entered when you come across a macro call
            - by this point the macro will be defined

## Algorithm for nested invocations
- need an "ArgStack" data structure
- as nested expansions are encountered, arguments are pushed to the stack

## Run only the macro preprocessor on GCC
- `gcc -E test.c > test.i`