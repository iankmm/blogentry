Date: 10-17-25
Title: Solving integer relation using LLL
Category: math
---
The infamous integer relation problem is,

**Problem.** (Integer Relation Problem) given $a_1,...,a_n\in\mathbb{R}$, finding a non-trivial solution $x_1,...,x_n\in\mathbb{Z}$ such that:
$$
a_1x_1+...+a_nx_n=0
$$

Another integer problem called the subset sum problem can be considered a subset (no pun intended) of the integer relation problem.

**Problem.** (Subset Sum Problem). Given a set $A\subset Z$ and a natural number $M$. Find a subset $S$ of $A$ such that $\sum{S}=M$

This problem can be reconsidered such that if $A=\{a_1,...,a_n\}$ then the set $S=\{a_i|x_i=1\}$ where:
$$
a_1x_1+...+a_nx_n-M=0
$$

Hence, SSP has a stronger definition than IRP.

Now, lattice is a way of looking at these problems geometrically.

**Def.** (Lattice). Given the basis $B=\{b_1,...,b_n\}$, lattice $\mathcal{L}(B)$ is defined as:
$$
\mathcal{L}=\{\sum_i{a_ib_i}|a_i\in\mathbb{Z}\}
$$

todo:
- explain svp, cvp baesd off lattice and how to solve them under poly time
- explain lattice reduction
- explain LLL algorithm
