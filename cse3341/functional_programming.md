# Functional Programming

## Immutable quandary
- quandary without `mutable` keyword
- `int`
    - signed 64-bit (`Long` in java)
- `Ref`
    - `nil`
    - result of dot expression ("dot value")
- `Q` is parent type of `int` and `Ref`

### Dot operator
- `EXPR1 . EXPR2`
    - operands are of type `Q`
    - result is of type `Ref`
- built-in functions
    - `Q left(Ref)`
    - `Q right(Ref`
- explicit downcasts required
    - are dynamically checked
- useful for lists: `nil` -> `(3 . nil)` -> `(4 . (3 . nil))`

### Lists
- a value is a list iff:
    - it's `nil` (empty list), or
    - it's `x . y` where (`y` is a list implies `x` is the first element of the list, and `y` is the rest of the list)
- `(4 . (3 . nil)) . ((2 . (1 . nil)) . nil))`

![[nonlist_s_expression.excalidraw]]

## Dealing with immutability
```
Q main (int x)
{
    return sum (x);
}

// returns cumulative sum (1 to i)
// must loop with recursion for immutables
int sum (int i)
{
    if (i == 1) return 1;
    return i + sum (i - 1);
}
```

```
Q main (int x)
{
    return 4 . 3 . 2 . 1 . nil; // ((((4 . 3) . 2) . 1) . nil)
    return (4 . (3 . (2 . (1 . nil)))) // correct list order
}

Ref makelist (int x)
{
    if (x == 1) return (1 . nil);
    return x . makelist (x - 1);
}

Ref sumtotals (Ref l)
{
    //if (left (l) == 1) return 1 . nil;
    if (isNil (l) == 1) return nil;
    
    //Ref s = sumtotals (right (l));
    //return (left (l) + left (s)) . s;
    return sum ((int) left (l)) . sumtotals ((Ref) right (l));
}

Ref doubles (Ref l)
{
    if (isNil (l) == 1) return nil;
    return double ((int) left (l)) . doubles ((Ref) right (l));
}
int double (int i)
{
    return 2 * i;
}
```

```
// with map
Ref map (Ref l, Q f)
{
    if (isNil (l) == 1) return nil;
    return f ((int) left (l)) . map ((Ref) right (l), f);
}

Ref double (Ref l)
{
    return map (l, sum);
}
```

## Functional quandary examples
```
// get length of list
int length (Ref l)
{
    if (isNil (l) == 1) return 0;
    return 1 + length ((Ref) right (l));
}
```

```
// fib
// this is super inefficient!
// you already calculated fib(n-2) when calculating fib(n-1)
int fib (int n)
{
    if (n <= 2) return 1;
    return (fib (n - 1) + fib (n - 2));
}
```

```
// fib with optimized memoization
// could store all previous values
// but we really only need the last 2
// we want:
//     int curr = 1;
//     int prev = 0;
//     for (i = n; i == 1; i--)
//         curr, prev = curr + prev, curr;
int fib2 (int n, int curr, int prev)
{
    if (i == 1) return current;
    return fib2 (n - 1, curr + prev, curr);
}
Q main (int x)
{
    return fib2 (x, 1, 0);
}
```

- **tail recursion:** when the result of the recursive call isn't used in the body of the method
    - can reuse the stack frame
    - many functional languages have a `tail` keyword for functions to do this
- **memoization**
    - the quandary interpreter has cli options for memoization
    - pure functions can be readily memoized

## Functions as first-class entities
- ...


