## Introduction
![Screenshot](cannonballs.png)
![Screenshot](Granular_matter_examples.png)
Sphere packing is an old problem in discrete geometry with a rich history. The problem is seeking to maximize the fraction of $R^D$ covered with non-overlapping spheres. Sphere packing has applications in communication theory, error correcting codes, toy models of granular material, modular forms, and even biology. Despite how simple is to state the problem, it is a notoriously hard problem to solve. $D = 1, 2, 3, 8$, and $24$ are the only dimensions for which the sphere packing has been solved. The solution in two dimensions $D=2$ is the hexagonal lattice and was rigorously proved in 1942 by Toth. $D=3$ is the famous Kepler conjecture. When asked about the optimal arrangement of cannonballs, Kepler conjectured that a family of lattices consisting of fcc and hpc lattices is the optimal packing in three dimensions. The Kepler conjecture was proved almost four centuries later by Thomas Hales in 1998. The densest packing of spheres in $D=8, 24$ correspond to the well-known E8 and Leech lattices, respectively. Maryna Viazovska established the proof for D=8 in 2016. A week later, Cohn, Kumar, Miller, Radchenko, and Viazovska proved the optimal packing in 24 dimensions. Viazovska was awarded the Fields in 2022 for proving the E8 lattice is the optimal packing in 8 dimensions.

In all other dimensions, the sphere packing problem has not been solved. There is not much geometric intuition about higher dimensions.
However, there are several known packing density bounds. Some bounds are based on explicit constructions, others are more abstract.
In general, every dimension appears to be different, and fewer results are known in high dimensions.
An important class of bounds for all dimensions are based on linear programming (LP). In the LP method, the space of functions acting on
an arbitrary hypothetical packing is considered.  Using Fourier analysis and working in dual space of these functions,
An upper bound on packing density is found. See references for more details.

## What is being computed in the code?

The mathematical formulation of this repository is the following: Given dimension $D$, solve $2N$ non-linear equations below for $2N$ variables,
$$f_k(0) + \sum_{n=1}^{N} d_n f_k(\Delta_n) = 0 \qquad \text{for}\qquad 1 \le k \le 2N$$
where 
$$f_k(\Delta) = L\_{2k-1}^{D/2-1} (4\pi \Delta) e^{-2 \pi \Delta}$$
and ${L^{D/2-1}}\_{2k-1}$ are Legendre Polynomials. The unknowns are $\Delta_n, d_{\Delta_n}$ for $n=1,2,\cdots, N$. Solving these equations for $\Delta_1$ gives an upper bound on packing density $\rho$,
$$\rho \le \frac{\pi^{D/2}}{(D/2)!} \left(\frac{r}{2}\right)^{D},\qquad r = \sqrt{2\Delta_1}$$
We choose the variable $N$. The upper bound on density improves as $N$ increases, however we need to solve more non-linear equations.

## A few considerations for using the code

The code computes packing density by solving the above-mentioned non-linear equations. The goal for any $D$ is to solve $\Delta_1$ for a higher $N$.
I solve them using Newton's method. Newton's method is fast and stable, but it requires matrix inversion, and every element of the matrix has to be
stored with high precision. I have tried gradient descent (without Momentum or Adam) in the past, but the convergence was very slow.

The problem is very sensitive to an initial guess for $\Delta_i$ when solving equations. In particular, if we sort $\Delta_i$, 
the equations would not converge unless one guesses the first few $\Delta_i$ with mathematical precision $10^{-3}$ as the fractional error.
In practice, once $N \sim D$, the bounds for a given $D$ are converged. Due to the non-linear nature of equations,
they have to be solved with high precision computing. For $D=2000$, I used about 1000-digit precision.
I originally used the Mathematica file for this problem in writing the paper, which can do arbitrary-precision arithmetic.
I repeated the calculation using Python's mpmath library. However, I found matrix inversion in mpmath to be much slower than in Mathematica and Julia.
Since Mathematica is not open-source, I share the Julia notebook here. 

## References

**High-dimensional sphere packing and the modular bootstrap**, Nima Afkhami-Jeddi, Henry Cohn, Thomas Hartman, David de Laat, Amirhossein Tajdini,
June 3, 2020, arXiv preprint: 2006.02560 , Published in:  JHEP 12 (2020) 066

**New upper bounds on sphere packings I**, Henry Cohn, Noam Elkies, arXiv preprint: 0110009, Published in: Annals of Mathematics 157 (2003), 689-714

