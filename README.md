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

Feedback loops update beliefs with backpropagation:

```
let perhaps-visit(a: Belief, b: Belief, c: Belief): Nil = (
   (a and b or not c).observe(true)
);
```

Feedback loops can be guarded by traditional boolean logic or even rational logic:

```
let perhaps-visit(a: Belief, b: Belief, c: Belief): Nil = (
   if global-array.length<321 and not a then (a and b or not c).observe(true)
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
* jitter by default: need to add an epsilon to break symmetry for initial deadlock in operators like xor
* $$XOR(A, B) = (A \lor B) \land \neg(A \land B)$$ ... necessary to be linearly separable
* NOT(A) $1 - A$
* A AND B $A \times B$
* A OR B $A + B - (A \times B)$
* dimensionality: not all beliefs originate from simple booleans or map to simple booleans... what about classifiers etc.

Demos:
* active learning for MNIST classifications... see how long it takes to learn an alphabet from trial/error
* active CartPole learning
* collaborative art idk
   * a little pet or person you can train and incentivize with a Steam game?

Goals:
* maybe this will be the first major non-core library for LM/LSTS
* it seems most interesting to me right now
