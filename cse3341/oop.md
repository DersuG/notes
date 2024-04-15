# Object Oriented Programming
- **member:** anything in a class
    - **member**
    - **field**
- **class:** blueprint for creating objects
- `new` creates instances

## Java references / C++ pointers
- objects are manipulated indirectly through references (pointers)
- in c++, references and pointers are different
    - `Rectangle x = new Rectangel();` creates a reference
    - `Rectangle *x = new Rectangle();` creates a pointer

## Object creation
- new instance created on heap
- a reference (pointer) to this instance is produced
- the appropriate constructor is called
- the reference (pointer) is returned

## Object destruction
- in c++, each `new` must have a `delete`
    - e.g. `delete x;`
    - c++ has destructors (e.g. `~Rectangle() {}`)
- in java, objects that cannot be reached by the stack are garbage collected
    - java has finalizers (e.g. `void finalize() {}`)

## Members
- **instance member:** each instance has its own copy
- **static member:** shared across instances

## Instance methods
- `this` is an implicit formal parameter
    - `double area() { return height * width; }` is actually:
        - `double area(Rectangle this) { return this.height * this.width; }` in java
        - `double area(Rectangle *this) { return this->height * this->width; }`
    - calling `x.area()` or `x->area()` is actually calling `area(x)`

## Inheritance
- **extending:**
    - in java, `class B extends A {}`
    - in c++, `class B : public A1, A2, A3 {}`
- every member of `A` is **inherited** by `B`

![[Pasted image 20240221132850.png]]

## Constructors and inheritance
- constructors aren't inherited
- a constructor in subclass `B` must invoke a constructor in the superclass `A`
    - technically not always but yeah
- `super()`

## Method inheritance
- **overloading:** same name but different signature
- **overriding:** same name and signature

## Reference polymorphism
- references (pointers) can point to instances of `A` or any direct or indirect subclass of `A`

## Method invocations at compile time
- `x.m(a, b);`
- the actual method is determined based on the type of `x`
- this is the **compile-time target**

```
// B extends A
A x;
x = new B();
x.m(1, 2);
```
- since x has declared type `A`, the compile-time target is the method `m` in class `A`
- java bytecode encodes this like `virtualinvoke x,<A: void m(int,int)>`

## Method invocations at run time
- (in java)
- look at the concrete class of `x` and try to find a method with a matching signature
- if not found, go to the superclass and repeat until found
    - does not search implemented interfaces
- this is the **run-time target** (aka dynamic target)
- this process is called **virtual dispatch** (aka method lookup)
- each class has a reference to its superclass
![[Pasted image 20240223131015.png]]

## Abstract classes
- in java, `abstract void m(int x);`
- in c++,  `virtual void m(int x) = 0;`

## Interfaces (java)
- interface methods can be compile-time targets

## Virtual methods (c++)
![[Pasted image 20240223132415.png]]
- `x` has declared type `A*`, so the compile-time target is `m` in `A`
- the run-time target is `m` in `B`
    - without `virtual`, the run-time target will be the compile-time target

## Diamond problem
- multiple inheritance
- ambiguous run-time target
- in java, possible because of `default` method bodies in interfaces

## JVM
- interpreting bytecode
- compiling bytecode to native code
    - faster
    - JIT  for hot code

## Compilation within a VM
- **just-in-time (JIT):** the first time bytecode is executed, it is compiled to native code on the fly
    - usually done at method level
    - problem: overhead
- **profile-driven:** after a certain threshold, "hot spots" are compiled

## Lifetimes & memory management
- **static allocation:** address determined once and stays there
    - e.g. static fields
- **stack-based allocation:** local vars and formal parameters (including `this`) of methods
- **heap-based allocation:** manually allocated/deallocated
    - e.g. `malloc()`
    - e.g. `new`

![[Pasted image 20240304120437.png]]
![[Pasted image 20240304120347.png]]
![[Pasted image 20240304120403.png]]
![[Pasted image 20240304120420.png]]


