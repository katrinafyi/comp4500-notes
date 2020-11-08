---
title: "Lecture 2 --- Recurrences"
author: Kenton Lam
---

# Recursive functions

There are three strategies we can use:

- substitution, where we guess an answer and prove it satisfies the recurrence,
- iteration, expanding into a sum and evaluating, and
- master theorem which covers some particular cases.

### Divide and conquer algorithms

Recall merge sort, defined as $\texttt{merge_sort}(A, p, r)$. This also uses a subroutine $\texttt{merge}(A, p, q, r)$ which merges the subarrays $A[p,q]$ and $A[q+1, r]$ into $A[p, r]$ (this algorithm is $\Theta(n)$ where $n=r-p+1$).

The time complexity of merge sort is $T(n) = 2T(n/2) + f(n)$ with $f(n) \in \Theta(n)$. 

## Recurrences

To be well-defined, a recurrence relation needs a base case and recursive case(s) which converge towards that base case. Sometimes, we omit the base case and implicitly take it as $T(n) = \Theta(1)$ for $n \le c$. This does not have a significant effect on the running time of the base case.

Consider a generic divide and conquer algorithm which 

- takes input of size $n$,
- breaks it into $a$ parts, each of size $n/b$,
- takes $D(n)$ time to divide the problem in this way, and
- takes $C(n)$ time to combine the subproblem results.

This results in the following recurrence:
$$
T(n) = \begin{cases}
aT(n/b) + D(n) + C(n), & n \ge c, \\ 
\Theta(1), & n \le c.
\end{cases}
$$

## Substitution

**Example:** Consider
$$
T(n) = \begin{cases}
2, & n=2,\\
2T(n/2) + n, &n=2^k,
\end{cases}
$$
and we guess that 
$$
T(n) = n \log_2 n.
$$
The base case is that $T(2) = 2 = 2 \log_2(2)$. The inductive step is
$$
\begin{aligned}
T(n) = 2T(n/2)+n &= 2(n/2)\log_2(n/2) + n \\ 
&= n(\log_2 n - \log_22) + n \\ 
&= n \log_2 n
\end{aligned}
$$
which is what we wanted to show.

**Example:** As a more complicated example using
$$
T(n) =\begin{cases}
1, & n=1, \\ 
2T(\lfloor n/2\rfloor) + n, & n>1.
\end{cases}
$$
We notice it resembles the earlier example and we guess that $T(n) \in O(n \lg n)$. We need to prove this using the definition. 

First, we need an $n_0$. We can see that $n_0=1$ doesn't work because $T(1) = 1$ which is not less than $0$. Suppose $n_0=2$, then for the cases which depend directly on $T(1)$, we have
$$
\begin{aligned}
T(2)& = 2T(1) + 2 = 4 \le c2 \lg 2\\
T(3)& = 2T(1) + 3 = 5 \le c 2 \lg 2\\
\end{aligned}
$$
for some $c \ge 2$.

Moving onto the induction, we assume that $T(n) \le c n \lg n$ for $\lfloor n/2\rfloor$ and we want prove this for the case of $n$. Substituting,
$$
\begin{aligned}
T(n) = 2T(\lfloor n/2\rfloor) + n  
&\le 2(c \lfloor n/2\rfloor \lg \lfloor n/2\rfloor) + n \\ 
&\le c  n \lg \lfloor n/2\rfloor + n \\ 
&=  c  n (\lg n - \lg 2) + n \\  
&=  cn\lg n - cn\lg 2 + n \\ 
&\le cn \lg n
\end{aligned}
$$
assuming $c \ge 1$ and $n \ge 0$.

**Example:** Guessing does not always work in this way. Consider,
$$
T(n) = T(\lfloor n/2 \rfloor) + T(\lceil n/2 \rceil) + 1
$$
and we guess that $T(n) = O(n)$ (which is actually correct). Trying to prove the inductive step, we get
$$
\begin{aligned}
T(n) = T(\lfloor n/2 \rfloor) + T(\lceil n/2 \rceil) + 1 
&\le c(\lfloor n/2 \rfloor) + c(\lceil n/2 \rceil) + 1  \\ 
&=cn+1 
\end{aligned}
$$
which is not $\le cn$, so we cannot prove it this way! We can solve this problem by strengthening the guess_, via subtracting a lower order term. Specifically, we assume that $T(n) \le cn-b$ for $b>0$. This does not affect the asymptotic bound but with this stronger assumption, we are able to probe a stronger result.

**Note:** In the inductive step, we need to be very precise. We need to prove that $T(n) \le cn$ very carefully, something like $cn + n = O(n)$ is not good enough.

#### Change of variables

Consider
$$
T(n) = 2 T(\lfloor \sqrt n \rfloor) + \lg n.
$$
This looks hard, but one trick we can use for some more unfamiliar cases is changing the variables. We try $m = \lg n$ which means $n = 2^m$. Making the change,
$$
T(2^m)=2T(2^{m/2}) + m \iff S(m) = 2S(m/2) + m
$$
where $S(m) = T^(2^m)$. The $S$ expression looks familiar and we know the solution is $S(m) \in \Theta(m \lg m)$. Changing back gives us
$$
T(n) = T(2^{m}) = S(m) = \Theta(m \lg m) = \Theta(\lg n \lg(\lg n)).
$$

## Iteration

## Master method

Suppose $T$ is of the form
$$
T(n) = aT(n/b) + f(n)
$$
where $n/b$ can also have a ceiling or floor. Then, we need to compare the work at each step, $f(n)$ with how many calls there are. Specifically,

- (1) if $n^{\log_b a}$ is polynomially *larger* than $f(n)$, then $T(n) \in \Theta(n^{\log_ba})$, 
- (2) if $n^{\log_b a}$ is the same asymptotic *tight* bound as $f(n)$, then $T(n) \in \Theta(n^{\log_ba}\lg n)$, and
- (3) if $f(n)$ is polynomially *larger* than $n^{\log_ba}$ and $f(n)$ is regular, then $T(n) \in \Theta(f(n))$.

We say a function $f$ is **polynomially larger than** $g$ if $f$ is lower bounded by $g$ times some polynomial with degree $\epsilon>0$. That is,
$$
f(n) \in \Omega(g(n) \times n^\epsilon) \quad \text{where }\epsilon > 0
$$
or equivalently,
$$
f(n) / g(n) \in \Omega(n^\epsilon).
$$
A function is **regular** if $af(n/b)< cf(n)$ for $c<1$. This just ensures that the subproblems are decreasing in complexity.

Putting this together, we have these three cases:

- (1) $f(n) \in O(n^{\log_{b}a-\epsilon})\implies T(n) \in \Theta(n^{\log_b a})$, 
- (2) $f(n) \in \Theta(n^{\log_ba})\implies T(n) \in \Theta(n^{\log_b a} \lg n)$, and
- (3) $f(n) \in \Omega(n^{\log_ba+\epsilon})$ and regularity $\implies T(n) \in \Theta(f(n))$.

**Example:** Consider merge sort from earlier with $T(n) = 2 T(n/2) + f(n)$ with $f(n) \in \Theta(n)$. We compare $f(n)$ to $n^{\log_22}=n$ and we see they are asymptotically the same. As a result, this is case 2 and $T(n) \in \Omega(n \lg n)$.