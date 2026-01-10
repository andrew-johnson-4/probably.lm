Rational programming library for LM.

Simple Boolean logic program:

```
let perhaps-print(x: Bool, y: Bool): Nil = (
   if x and y then print("Yes!")
);
```

Rational logic program:

```
let perhaps-print(x: Belief, y: Belief): Nil = (
   if x and y then print("Yes!")
);
```

A belief behaves like a boolean in most situations, however it can be a dynamically updated value that the program learns.

The data structure is fairly simple:
* roots: a simple confidence value [0,1]
* compounds: logic chaining based on truth tables for standard operations not/and/or/xor/implies etc.
* `x and y` from the above example creates a new compound dynamically from the existing belief chain or roots supplied
* learning can backpropagate information towards the roots
* non-default activation functions can be implemented as adverbs `a and b @sigmoid`
* can performance be good enough for active learning?

Goals:
* maybe this will be the first major non-core library for LM/LSTS
* it seems most interesting to me right now
