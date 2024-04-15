# Parser and Language Types

## Recursive descent parser
- LL languages
- parses `5+5+5` as `5+(5+5)`

```
// expr => expr + expr | int
// evaluates left-most non-terminals first
expr => expr + expr
     => 5 + expr
     => 5 + expr + expr
     => 5 + 5 + expr
     => 5 + 5 + 5
```

![[Diagram.svg]]

## Shift-reduce parser
- LR languages
- parses `5+5+5` as `(5+5)+5`

```
// expr -> expr + expr | int
// evaluates right-most non-terminals first
expr => expr + expr
     => expr + 5
     => expr + expr + 5
     => expr + 5 + 5
     => 5 + 5 + 5
```

## Funky examples

```
L = {a^n b^m c^k | m = n+k}
  = {a^n b^n b^k c^k}

L -> X Y
X -> a X b | empty
Y -> b Y c | empty
```

```
L = {a^n b^m c^k | m = n+k, n>0, k>0}

L -> X Y
X -> a X b | a b
Y -> b Y c | b c

// maybe?
L -> X Y
X -> X' | empty
X' -> a X' b
Y -> Y' | empty
Y' -> b Y' c
```

## Famous grammar
```
EXP ::= EXP + TERM | TERM
TERM ::= TERM * FACTOR | FACTOR
FACTOR ::= ID | ++ID
```