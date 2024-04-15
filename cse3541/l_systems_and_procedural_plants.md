# L-Systems and Procedural Plants

## L-systems
- models morphogenesis
- based on formal grammars
- **self-similarity:** when a piece of a shape is geometrically similar to the whole, both the shape and the cascade that generate it are called self-similar (Mandelbrot, 1982)
- a string rewriting system

![[Pasted image 20240415114630.png]]

## Types of L-systems
- **context-free:** production rules refer only to an individual symbol
- **context-sensitive:** production rules apply to a particular symbol based on its neighbors
- **deterministic:** exactly one production for each symbol
- **stochastic:** multiple productions for each symbol, chosen randomly

## D0L-systems
- `D0L`
- simplest class of L-systems
- deterministic
- context free
```
alphabet: a, b
rules: a -> ab, b -> a
axiom: b

b
a
ab
aba
abaababa
```

## Graphic interpretation of L-systems
- graphic interpretation of strings, based on turtle geometry
- **turtle geometry:** turtle has a position and direction (`x`, `y`, `α`)
- given step size `d` and angle increment `δ`, the turtle can respond to commands represented by symbols:
    - `F`: move forwards by `d`, drawing a line
    - `f`: move forwards by `d`, without drawing a line
    - `+`: turn left by `δ`
    - `-`: turn right by `δ`
![[Pasted image 20240415115903.png]]

## Bracketed L-systems
- represent branching structures
- extends the turtle system with new commands:
    - `[`: push current state of turtle onto a stack
    - `]`: pop a state from the stack and apply it (instantly changes turtle's position and heading)
![[Pasted image 20240415120148.png]]

