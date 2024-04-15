# Types & Typing

## Types
- **type:** set of computational entities with uniform behavior

## Type checking
- operands should get operands of correct type
- lvalue and rvalue
- procedure calls
- return type

## Static typing
- **static type:** claim about runtime values
- declared or inferred

## Dynamic typing
- **dynamic type checking:** right before operation is executed

## Type/memory safety
- **type-safe language:** bad operations aren't performed
    - things can't fail silently
    - errors are detected and clearly reported
    - java is type safe
    - c/c++ is not type safe
- independent of static/dynamic

## Types as sets of values
- e.g. numeric types with ranges
- class type (not the same as class)
    - any object of class `C` or a subclass of `C`
    - "type `C`" = set of all instances of `C` or any transitive subclass of `C`
    - "class `C`" = just a blueprint for objects
- *subtypes are subsets:* `B` is a subtype of `A` if `B`'s values are a subset of `A`'s values

## Polymorphism
- **monomorphism:** each computational entity is exactly 1 type
- **polymorphism:** each computational entity can be multiple types
- types:
    - universal
        - parametric
        - inclusion (subset)
    - ad hoc
        - overloading
        - coercion

### Coercion
- values of one type are silently converted to another type
- **widening:** coercing into a larger type
    - e.g. subclass to superclass
- **narrowing:** coercing into a smaller type
    - loses information

### Overloading
?

### Parametric polymorphism
- e.g. generics in java

### Inclusion (subset)
- subtype relationships among types
    - "`Y` is a subset of `X`"
- a computational entity of a subtype can be used in any context that expects the supertype
- 
## Type erasure causes problems
```java
String[] slist = new String[3];
Object[] mine = slist;
mine[0] = new Integer();
slist[0].length(); // wtf!!!
```

```java
List<String> slist = new LinkedList<String>();
List<Object> mine = slist; // doesn't compile
```

```java
/* inside AbstractList<E> */

public boolean equals (Object obj) {
    if (!obj instanceof java.util.List<E>)) return false; // this won't work, after compilation, declared/static type info of E is removed
    if (!obj instanceof java.util.List<?>)) return false;
    List<?> other = (List<?>) obj;
    E entry = other.get(0); // this won't work either
    Object entry = other.get(0);
}
```