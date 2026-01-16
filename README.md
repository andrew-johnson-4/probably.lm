Rational programming library for LM.

### Problem Statement

* modern neural networks can be thought of as general function approximators
* blackbox learning takes a long time to train, but can learn a lot of interesting things with just many training examples
* whitebox learning converges faster
* network topology affects what functions can be learned with that model and how fast it will converge

so

* what is the most ergonomic way to define and interact with neural network programming?
* not just a library
* what would the ideal *language* look like?
* remember, these are just a different kind of function
* a type of function that learns and adapts

### Examples

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

let variable1 = idk();
let variable2 = idk();
perhaps-print(variable1, variable2);
```

Feedback loops update beliefs with backpropagation:

```
let perhaps-visit(a: Belief, b: Belief, c: Belief): Nil = (
   (a and b or not c).observe(true)
);

let variable1 = idk();
let variable2 = idk();
let variable3 = idk();
perhaps-visit(variable1, variable2, variable3);
```

Feedback loops can be guarded by traditional boolean logic or even rational logic:

```
let perhaps-visit(a: Belief, b: Belief, c: Belief): Nil = (
   if global-array.length<321 and not a then (a and b or not c).observe(true)
);
```

NOTE: feedback loops do not need to be Bayesian
* example `a and b or not c` can be learned in a code path that has arbitrary priors that are not conditioned in learning directly
* the conditioning constraint could be a separately proven property, but this library won't force it or even attempt to
* non-Bayesian learning is potentially very useful too

A belief behaves like a boolean in most situations, however it can be a dynamically updated value that the program learns.

The data structure is fairly simple:
* roots: a simple confidence value [0,1]
* compounds: logic chaining based on truth tables for standard operations not/and/or/xor/implies etc.
* since we are hard-coding the logic operators we don't need to learn them generally which makes the learning and math a lot simpler
* `x and y` from the above example creates a new compound dynamically from the existing belief chain or roots supplied
* learning can backpropagate information towards the roots
* non-default activation functions can be implemented as field/methods `(a and b).sigmoid`
* can performance be good enough for active learning?
* jitter by default: need to add an epsilon to break symmetry for initial deadlock in operators like xor
   * jitter at fact initialization or jitter as smaller epsilon during learning?
* $$XOR(A, B) = (A \lor B) \land \neg(A \land B)$$ ... necessary to be linearly separable
* NOT(A) $1 - A$
* A AND B $A \times B$
* A OR B $A + B - (A \times B)$
* product T-NORM is differentiable for and
* Blame for A: $\frac{\partial f}{\partial A} = B$
* Blame for B: $\frac{\partial f}{\partial B} = A$
* just need to ensure a smooth gradient at all gates across all possible probability states
* dimensionality: not all beliefs originate from simple booleans or map to simple booleans... what about classifiers etc.
   * `Belief<range>` for non-boolean beliefs

Demos:
* active learning for MNIST classifications... see how long it takes to learn an alphabet from trial/error
* active CartPole learning
* collaborative art idk
   * a little pet or person you can train and incentivize with a Steam game?

Goals:
* maybe this will be the first major non-core library for LM/LSTS
* it seems most interesting to me right now
