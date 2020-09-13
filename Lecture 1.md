---
title: "Lecture 1 --- Introduction"
author: Kenton Lam
---

# COMP4500 &mdash; Advanced Algorithms & Data Structures

Lectured by Ahmad Abdel-Hafez. Currently works full-time as a data scientist. First time teaching at UQ. Not a mathematician.

# Introduction and Background

## Motivation

- _Creating_ more efficient algorithms.
- _Justifying_ choice of algorithms using theory.
- _Improving_ problem solving skills.
- This is a prerequisite for a job at Google, Amazon, Oracle, etc.

### Research Areas

These areas are currently being researched and developed:

- distributed/parallel algorithms,
- neural networks/pattern recognition,
- bioinformatics, and
- quantum computing.

### Outline

- Background and asymptotic notation.
- Recurrences, divide and conquer.
- Graph algorithms (3 weeks).
- Dynamic programming (2 weeks). **Assignment 1 published.**
- **Midsemester exam.**
- **Assignment 2 published.**
- Greedy algorithms.
- Amortised analysis.
- Complexity classes.
- Randomised algorithms.

### Other

- Textbook is Cormen at al. (CLRS).
- Tutorials start in Week 2.
- Piazza will be used.
- Consultation with lecturer can be organised by email.

# Running Time Analysis and Asymptotic Notation

- An **algorithm** is a well-defined computation procedure which takes some values as _input_ and produces some value as _output_.
- As an example, _insertion sort_ is an algorithm taking an array as input and returning a sorted array. It works by building the sorted array from left to right by inserting the next value in its sorted position.
- _Loop invariants_ can be used to prove an algorithm is correct.

- **Execution time** depends on input size and the input itself. Generally, we want an upper bound on the execution time.
  - There are _worst_, _average_, and _best_ case execution times. Average case is the average over all inputs, weighted by the probability of that input.
- For example, insertion sort requires $n(n-1)/2$ comparisons in the worst case and $n-1$ in the best case.

## Asymptotic Notation

- For sufficiently large $n$, the constants are negligible.

- Example: $2n^2 \in O(n^3 - n^2)$.
- Theorems:
  - If $f \in O(g)$, then $g + f \in \Theta(g)$. If $f$ grows no larger than $g$, it doesn't affect the big-$\Theta$ bound of $g$.
  - If $k > 0$, then $k n^a \in \Theta(n^a)$.
  - If $k > 0$ and $0 \le a \le b$, then $k n^a \in O(n^b)$.

- $f$ is asymptotically non-negative if $f(n) \ge 0$ for $n \ge n_0$.
- Theorem: If $f, g$ are asymptotically non-negative and $\lim_{n \to \infty} f(n)/g(n) = c$, then: 
  - $f(n) \in O(g(n))$ if $c < \infty$, 
  - $f(n) \in \Theta(g(n))$ if $0 < c < \infty$, and 
  - $f(n) \in \Omega(g(n)) $ if $c > 0$.

