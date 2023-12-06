# Macros

## Macro definitions
- new pseudo-ops:
    - `.MAC` for macro definition start, label is macro's name
    - `.MND` for macro definition end

```
; example
CLEAR   .MAC
        AND   R0,R0,#0
        AND   R1,R0,#0
        AND   R2,R0,#0
        AND   R3,R0,#0
        .MND
```

## Macros vs. functions
- macros are faster
- functions take less space

## Arguments
- use text substitution

```
; example
SWAP   .MAC  &A,&B ;args
       LD    R1,&A
       LD    R2,&B
       ST    R1,&B
       ST    R2,&A
       .MND

Prog   .ORIG
X      .FILL x10
Y      .FILL x0
       SWAP  X,Y
       .END
```

## Problems with labels in macros
- pure text substitution would result in duplicate labels when a macro is used more than once
- solution: labels are local within macros
    - special character prefix
    - macro processor does name mangling
    - each expansion has unique name

```
; example
SWAPR .MAC  &Val1,&Val2
      JMP   $Bgn
$T1   .BLKW #1
$T2   .BLKW #1
$Bgn  ST    &Val1,$T1
      ST    &Val2,$T2
      LD    &Val2,$T1
      LD    &Val1,$T2
```

Each expansion adds a unique prefix to the labels (`AA`, `AB`, etc.).
E.g. `Bgn` becomes `AABgn`.

## Variables
- evaluated at expansion time
- `.SET` defines variables (e.g. `&Test .SET 0`)
- now `&Test` can be used in macro body

## Conditional expansion
- new pseudo ops `.IF`, `.ELSE`, and `.ENIF`

```
; example
GOSUB .MAC  &Type,&Target ;
      .IF   &Type EQ near ;if (type == near) {
      JSR   &Target       ;
      .ELSE               ;} else {
      JMP   $Bgn          ;
$Dst  .FILL &Target       ;
$Bgn  LD    R7,&Dst       ;
      JSRR  R7            ;
      .ENIF               ;}
      .MND
```

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
- prevent infinite expansions
- need an "ArgStack" data structure
- as nested expansions are encountered, arguments are pushed to the stack

## C macros
- `#define INC(X,Y) do { X++; Y++ } while(0)`
- wrapping with do-while allows putting semicolons at the end

## Run only the macro preprocessor on GCC
- `gcc -E test.c > test.i`
