# Real world model checking using SAT solvers

Adrien Bustany

::

## What is model checking anyway?

>Get an exhaustive proof of a given set of properties on a model

::

## Yeah, like what?

::

## Yeah, like what?

### Interlocking systems

Check that the signals around a junction on a train track keep it safe

::

## Yeah, like what?

### Elecronic circuits

Check that a circuit behaves according to its specification

::

## Yeah, like what?

### Absence of undefined behaviour in code

Integer overflow, null pointer dereference…

::

## Yeah, like what?

### Automatic test case generation

Find the right set of inputs to trigger a given situation

::

## Yeah, like what?

### Problem optimization

To some extent, iteratively proving that a better solution exists

::

## How do you describe a model?

No standard format, usually using a declarative language that declares inputs,
outputs and the logical relations between them

::

## How do you check a model?

Many approaches:

- BDD (binary decision diagrams)
- Abstract interpretation
- SMT
- SAT

Explore the set of all possible states to prove that no properties are violated.

::

## How do you check a model?

In other words:

1. Be smart (teach "semantics" to the prover): saves on computation, but
   increases complexity and is potentially less generic
2. Be brutal: explore all states ← What we do ☺

::

## Let's model a Sudoku

Rules of Sudoku:

<img src="Sudoku-by-L2G-20050714.svg" width="120px" height="120px"/>

1. A grid is 9x9 cells, 3x3 groups
1. All cells must be filled with a value
1. The value of each cell is in [1, 9]
1. A value can appear at most once in a row, column and group

::

## Let's model a Sudoku

A bit more formal

<pre>
	int [1,9] grid [9,9];

	∀ value ∈ [1, 9] (
		∀ line ∈ [0, 8] (
			∃ column ∈ [0, 8] (grid[line, column] = value)
		) ⋀
		∀ column ∈ [0, 8] (
			∃ line ∈ [0, 8] (grid[line, column] = value)
		) ⋀
		∀ gline ∈ [0, 2], gcolumn ∈ [0, 2] (
			∃ line ∈ [0, 2], column ∈ [0, 2] (
				grid[gline * 3 + line, gcolumn * 3 + column] = value
			)
		)
	)
</pre>

::

## Formal problem solving using SAT

So we've formalized our problem using simple propositional logic... How do we
solve it?

Let's *lower* it to simple forms!

::

## Expansion rules

The expansion rules describe the process of transforming a set of statements in
the "high level" logic to a lower level one.

::

## Expansion rules

### Composites data structures

<pre>
	int [1,9] array [3];
</pre>

gets expanded to

<pre>
	int [1,9] array_0;
	int [1,9] array_1;
	int [1,9] array_2;
</pre>

::

## Expansion rules

### Quantifiers

<pre>
	∀ value ∈ [1, 3] P(x)
</pre>

gets expanded to

<pre>
	P(1) ⋀ P(2) ⋀ P(3)
</pre>

and

<pre>
	∃ value ∈ [1, 3] P(x)
</pre>

gets expanded to

<pre>
	P(1) ⋁ P(2) ⋁ P(3)
</pre>

::

## Expansion rules

### Integers

<pre>
	int [0,8] value;
</pre>

gets expanded to

<pre>
	bool value_0; // bit 0
	bool value_1; // bit 1
	bool value_2; // bit 2
	bool value_3; // bit 3
</pre>

...and integer operations to bitwise operations

::

## Expansion rules

Rinse and repeat until you reach a form that your SAT solver understands!

::

## Not everything in life is combinational

A Sudoku is fun, but it's pretty static.

What if we have inputs changing over time?

::

## Not everything in life is combinational

Let's add one additional dimension: time

<pre>
	// x is a stream of value over time
	// one could see it as x0, x1, …, xN
	int [0, 5] x;

	// y has value FALSE at t = 0, and then flips at each cycle
	y := FALSE, ~y;

	// z contains the value of y at the previous cycle
	z := pre(y);
</pre>

Welcome to the world of sequential logic!

Subject for a future talk?
