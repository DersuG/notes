# Imperative Languages
- read/modify/write cycle from memory
- **expressions:** used to produce values
      - constants, variables, operators, function calls, etc.
      - some have side effects
      - are **evaluated**
- **statements:** don't produce values
      - only used for side effects
      - assignment
      - are **executed**

## Values of expressions
- normally, an expression *E* designates a **value**
      - this is the **r-value** of *E*
        - if *E* is on the right side of an assignment statement, *E* stands for this value
- sometimes *E* is a memory location
      - only if *E* is on the left side of an assignment (e.g. array indexing)
      - the **l-value** of *E* is that "chunk of memory"
![[Pasted image 20240209130923.png]]

## Pointers in C
- pointers are "handles" to memory areas
- `&E`: find the l-value of *E* and create a handle to it
- `*E`: use the r-value of E to get to the memory

## References in Java
![[Pasted image 20240209132211.png]]

## Expressions
- **elements:** names for chunks of memory, constants, function calls, operators
- **operators:**
      - and **operands**
      - **arity:** unary, binary, ternary (e.g. `e1 ? e2 : e3`)
- **functions:**
      - prefix notation (e.g. `pow ( e1, e2 )`)
      - typically shouldn't have side effects

## Side effects of evaluating expressions
- **referential transparency:** can replace an expression with the r-value of this expression
      - not possible with side effects
        - e.g. `x = 5; y = 1 + x++; if (y == x) printf ("OK");`
        - can't become `x = 5; y = 1 + 5; if (y == x) printf ("OK");`
- in c: `=`, `++`, `--`, `+=`, etc. have side effects

## Order of Evaluation
- precedence and associativity aren't enough
      - `a - f(b) - c*d`
        - is `a` evaluated before or after `f(b)`?
        - what if `f(b)` changes `a`?
      - `f(a, g(b), h(c))`
- language must define this order

### Defined order of evaluation in C
- boolean expressions (short circuit evaluation)
- comma operator: `e1, e2`
      - `e1` is evaluated before `e2`
      - `f(a, g(b), h(c))`
- conditional operator: `e1 ? e2 : e3`

## Statements
- assignment statements
- control flow
      - selection
      - iteration
      - jump
- unstructured vs. structured control flow

## Procedures
- **procedure:** subroutine doesn't return a value
- **function:** subroutine returns a value
- **method:** subroutine in OOP

### Basic mechanism
- **caller**: calls the **callee**
      - provides **arguments** (aka **actual parameters**)
        - usually expressions that are evaluated immediately before the call
- **parameter passing:** actual parameters are "mapped" to **formal parameters**
- memory is allocated for the formal parameters and local vars of **callee**

## Scopes in imperative languages
- what entities are accessible where?
- what are their lifetimes?
- **shadowing**
      - generally bad
- at compile time, we consider scopes and their nesting
      - determines which entities are accessible where
- at run time, each scope has a **lifetime**
      - anything declared here has a lifetime, it dies at the end of the scope

### Implementing scopes
- memory regions
      - **code segment:** code for all procedures
      - **global (static) segment:** global variables
        - in Java, classes are stored in the global segment, and can be accessed/modified (aka **reflection**)
- **run-time call stack:** local variables
- **heap:** dynamically allocated memory

## Lifetimes and memory management
- **static allocation:** address determined once and exists forever
      - e.g. global variables, static fields, large constants, etc.
- **stack-based allocation:** address determined when the call happens, lifetime ends when the call ends
      - push activation record on run-time call stack
        - sometimes called a **stack frame**
      - local variables
      - relative addresses within the stack frame are determined at compile time
- **heap-based allocation:** manually allocated/freed space

### Implementing call stacks
- when a procedure `P` is called:
    - an **activation record** is pushed to the stack
        - has space for local vars
    - during execution, the **activation record pointer (AP)** register contains the starting address of the activation record
    - the **stack pointer (SP)** register contains the address of the location immediately beyond the activation record
- when `P` finishes, control returns to the caller, `SP` is set to the current `AP`, and `AP` is set to the address of the caller's activation record
![[Pasted image 20240214132310.png]]

### Calling convention
1. push return address (1)
2. push caller's `AP` (2)
3. set `AP` to address of (1)
4. push formal parameters
5. push (implicit) return variable
6. push local variables
7. do the call
9. set `AP` to caller's `AP` (value of (2))
10. jump to return address (value of (1))

- the caller sets up the activation record for the callee
- after a call is done, 

### How compilers generate code for call stacks
- calls:
    - save return address: `Mem[SP] = PC+4`
    - save pointer to caller's activation record: `Mem[SP+4] = AP`
    - allocate space for new activation record: `AP = SP` and `SP = SP+n`
    - jump to P: `PC =` address of first instruction in P, known at compile time
- returns:
    - pop activation record from stack and go back to caller

## Parameter passing modes
- **call-by-value**
- **call-by-reference**
    - argument must have an l-value, this is what's passed in the call


