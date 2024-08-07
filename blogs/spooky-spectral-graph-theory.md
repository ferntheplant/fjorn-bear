---
title: Spooky Spectral Graph Theory
link: spooky-spectral-graph-theory
published_date: 2024-05-21T00:00:00.000Z
meta_description: "boo \U0001F47B"
make_discoverable: false
tags: math
---
Table of contents

<!-- toc -->

- [Motivation](#motivation)
  * [Background](#background)
- [The Graph Laplacian](#the-graph-laplacian)
  * [Eigenvalues](#eigenvalues)
  * [Graph Properties](#graph-properties)
  * [Connectivity](#connectivity)
    + [Fiedler Vector](#fiedler-vector)
- [But why the Laplacian?](#but-why-the-laplacian)
  * [The 2nd Derivative](#the-2nd-derivative)
  * [Discretizing the Number Line](#discretizing-the-number-line)
  * [Vectors as Functions](#vectors-as-functions)
  * [Eigenfunctions](#eigenfunctions)
  * [Change of Change and Connectivity](#change-of-change-and-connectivity)
- [Further Reading](#further-reading)
<br></br>

<!-- tocstop -->

The following is a basic primer and introduction to spectral graph theory. I'll be using math-y words like "eigenvector" and "graph connectivity." I would advise knowing at least these 2 words before proceeding.

## Motivation

Despite the ghostly 👻 name, "spectral graph theory" is just the application of linear algebra to graphs (i.e. collections of vertices and the edges between them). The eigenvalues of a matrix are often referred to as the matrix's _spectra_, so you can probably infer that our goal will be to study the eigenvalues of matrices derived from graphs.

Usually you'd motivate a new area of math with some specific problem in need of solving. Newton gave us calculus when trying to describe the motion of objects. Galois gave us Galois theory when trying to find an equation for solutions of arbitrary quintic polynomials. I will introduce spectral graph theory under the principle of "just trust me, bro." It makes the final results much spookier 👻.

### Background

For the remainder of this article I'll be referring exclusively to unweighted, undirected graphs and as such will be omitting those qualifiers. Given a graph $G(V,E)$ its adjacency matrix $A_G$ is defined as:

$$
A_G = \begin{pmatrix}
a_{1,1} & \cdots & a_{1,n} \\
\vdots & \ddots & \vdots \\
a_{n,1} & \cdots & a_{n,n}
\end{pmatrix} \quad \textrm{with} \quad a_{i,j}=
\begin{cases}
1 & i\sim j \\
0 & i\not\sim j
\end{cases}
$$

Here $i \sim j$ means that the vertex $i$ is adjacent to the vertex $j$. To get an intuitive feel for this matrix imagine labeling every vertex of the graph with some index $i\in\mathbb{N}$. Now the $j^{th}$ entry of the $i^{th}$ column tells you whether the vertex with label $i$ is connected to the vertex with label $j$. Such labels can be arbitrarily assigned as the core information of the graph is encoded by the connectivity between labelled vertices rather than by the labels themselves.

Concretely, given the following graph $G$

![Hi This Is Fjorn](https://bear-images.sfo2.cdn.digitaloceanspaces.com/fjorn-1716265963-0.svg)

we would write its adjacency matrix as so:

$$
A_G = \begin{pmatrix}
0 & 1 & 1 & 0 \\
1 & 0 & 1 & 1 \\
1 & 1 & 0 & 1 \\
0 & 1 & 1 & 0 \\
\end{pmatrix}
$$

Notice we could shift all the labels clockwise by one vertex (leaving the edges in place) to create a new graph $H$. The adjacency matrix of this new graph is actually equivalent to the old one, i.e. $A_H = PA_GP^{-1}$ for some other 4x4 matrix P. As such, we can think of relabeling our graph as being some mechanism for changing the basis of the vector space over which the adjacency matrix acts (we'll discuss what this vector space even represents later).

Similarly to the adjacency matrix, we define the degree matrix of a graph as follows where $deg(i)$ measures the degree of the vertex labeled $i$

$$
D_G = \begin{pmatrix}
deg(1) & 0 & \cdots & 0 \\
0 & deg(2) & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & deg(n)
\end{pmatrix}
$$

## The Graph Laplacian

The primary matrix of study in spectral graph theory is known as the _Graph Laplacian_. The Laplacian of a graph $G$ is defined as

$$
L_G = D_G - A_G
$$

While the Laplacian matrix seems like a contrived definition, bear with me as we work through some of its useful properties to see anyone would care about it.

### Eigenvalues

Notice that the adjacency and degree matrices are, by definition, symmetric with real number entries. Therefore, the Laplacian is real-symmetric too. It is a well known result of the Spectral Theorem (oooo spooky 👻) that such matrices are diagonalizable, and their eigenvectors can form an orthonormal basis for the vector space over which they operate. Let's try to identify said eigenvectors and eigenvalues.

For a graph $G$ over $n$ vertices, given a normalized vector $u^T = (\begin{smallmatrix} u_1 & \cdots & u_n \end{smallmatrix})$ we can see that each entry $(Lu)_i$ of the Laplacian applied to $u$ has the following form:

$$
(Lu)_i = u_i * deg(i) - \sum\limits_{j,j \sim i} u_j = \sum\limits_{i,i \sim j}(u_i-u_j)
$$

Plugging this into the equation for a normalized eigenvector $u$ with eigenvalue $\lambda$ gives us

$$\lambda u = Lu$$
$$\lambda = u^TLu$$
$$\lambda = \sum\limits_{i} (u_i)(Lu)_i$$
$$\lambda = \sum\limits_{i} (u_i)(\sum\limits_{i,i \sim j}(u_i-u_j))$$

This nested sum can be simplified by noticing that each vertex $i$ contributes a term of $u_i^2-u_iu_j$ for each vertex $j$ adjacent to it. Similarly an adjacent vertex $j$ would contribute a term of $u_j^2-u_iu_j$. Combining these gives a familiar difference of squares $(u_i-u_j)^2$. As such we can reduce the equation for an eigenvalue to

$$\lambda = \sum\limits_{i<j,i \sim j} (u_i-u_j)^2$$

Some immediate consequences of this formula are:

- all eigenvalues are non-negative
- $\lambda = 0$ for "constant" vector $\mathbb{1}^T = (\begin{smallmatrix} 1 & \cdots & 1 \end{smallmatrix})$
- Each vertex contributes some non-negative amount to the eigenvalue for every connection it has, so small eigenvalues for a given vector generally imply the graph is not well connected

### Graph Properties

Ok cool, so we know some stuff about the eigenvalues of $L$. What do they tell us about the original graph though? Turns out they tell us a lot. A core result of spectral graph theory is that the multiplicity of the smallest eigenvalue $\lambda_1 = 0$ is exactly equal to the number of connected components of the graph.

We can actually see this directly from our analysis above. Since we can relabel vertices arbitrarily we can write the adjacency matrix for a graph with multiple connected components in a "block" form. The following graph $G$ has the following adjacency matrix:

![Hi This Is Fjorn](https://bear-images.sfo2.cdn.digitaloceanspaces.com/fjorn-1716476458-0.svg)

$$
A_G = \begin{pmatrix}
0 & 1 & 1 & 0 & 0 \\
1 & 0 & 1 & 0 & 0 \\
1 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 1 \\
0 & 0 & 0 & 1 & 0 \\
\end{pmatrix}
$$

Notice that the first 3 columns and rows of the matrix form a block representing the connectivity of the triangular connected component while the 4th and 5th rows and columns represent the connectivity of the other connected component. We can do this in general with arbitrarily many connected components.

In our eigenvalue formula, the only contributions that a vertex $i$ can make to an eigenvalue are due to incorporating values from connected vertices. So the "semi-constant" vector $u_1^T=(\begin{smallmatrix} 1 & 1 & 1 & 0 & 0\end{smallmatrix})$ is an eigenvector with eigenvalue $\lambda=0$. Similarly, the vector $u_2^T=(\begin{smallmatrix} 0 & 0 & 0 & 1 & 1 \end{smallmatrix})$ is also an eigenvector with eigenvalue 0. Therefore, the multiplicity of the zero eigenvalue tells us the number of connected components of the graph.

### Connectivity

Another important property of graphs elucidated by their Laplacian eigenvalues is their connectivity. A standard measurement of general connectivity is the minimum number of edges you need to remove from a connected graph in order to break it into 2 or more connected components. We define the _conductance_ $\phi(S)$ of a graph cut to be the ratio of the size of the cut (i.e. number of edges removed) to the number of vertices in the smaller partition.

Formally for a cut $S$ that divides a graph $G$ into sets of vertices $S$ and $\bar{S}$ such that all edges $\partial(S)=\{(u,v)\in V: u\in S \textrm{and} v\in\bar{S}\}$ (edges that have one endpoint each in $S$ and $\bar{S}$) are removed, its conductance is defined as

$$
\phi(S) = \frac{|\partial(S)|}{\min(|S|,|\bar{S}|)}
$$

Similarly, the conductance of an entire graph $G$ is defined as the minimum conductance of any cut of the graph.

$$
\phi(G) = \min\limits_{S\in G}\phi(S)
$$

Another fundamental result of spectral graph theory is the Cheeger Inequality. The inequality states that for $\lambda_2 > 0$ being the second-smallest eigenvalue of the Laplacian of graph $G$ with maximum vertex degree $d$ we have

$$
\lambda_2 \leq \phi(G) \leq \sqrt{2d\lambda_2}
$$

The proof is a little too technical to dive into here but let's instead look what this inequality tells us about a graph at a high level. For small values of $\lambda_2$ we see that the graph will have low conductance and as such can be separated by removing relatively few edges. Just like the multiplicity of $\lambda_1=0$ encoded information about the connected components of the graph, $\lambda_2$ encodes information about how well connected "nearly" separate components are.

#### Fiedler Vector

The eigenvector $u_2$ for eigenvalue $\lambda_2>0$ of a graph's Laplacian also encodes connectivity information. This eigenvector, sometimes called the Fiedler Vector, will have some positive and some negative coefficients. The coefficients of matching sign actually correspond to vertices in "nearly" separate components that would be divided if one applied a cut whose conductance was close to the graph's overall conductance. An example may be more illustrative.

![Hi This Is Fjorn](https://bear-images.sfo2.cdn.digitaloceanspaces.com/fjorn-1716489795-0.svg)

The above graph will have Laplacian

$$
L = \begin{pmatrix}
2 & -1 & -1 & 0 & 0 & 0 \\
-1 & 2 & -1 & 0 & 0 & 0 \\
-1 & -1 & 3 & -1 & 0 & 0 \\
0 & 0 & -1 & 3 & -1 & -1 \\
0 & 0 & 0 & -1 & 2 & -1 \\
0 & 0 & 0 & -1 & -1 & 2 \\
\end{pmatrix}
$$

The second-smallest eigenvalue is $\lambda_2 \approx 0.438$ with eigenvector $u_2^T=(\begin{smallmatrix} -1 & -1 & -0.561 & 3.561 & 1 & 1 \end{smallmatrix})$. Notice that the first 3 coefficients of the eigenvector are all negative while the last 3 coefficients are positive. This tells us that if we cut the graph using a cut that (nearly) minimized conductance then the vertices labeled 1-3 would form a connected component and the vertices 4-6 would form a separate connected component. Visually we can see that the lone edge between vertices 3 and 4 effectively splits the graph into 2 separate clusters, but with the Laplacian eigenvalues and eigenvectors we can detect this analytically.

## But why the Laplacian?

We've shown the graph Laplacian to be a fairly powerful tool, but why the hell is it called the Laplacian? In continuous settings the Laplacian usually denotes something conceptually equivalent to the 2nd derivative. In fact, in single variable calculus the Laplacian is exactly equal to the 2nd derivative. How do we translate this continuous setting concept into something graph-theoretic?

### The 2nd Derivative

To start, let's imagine the function $f(x)=x^2$ over the real numbers. Its second derivative $f''(x)=2$ is strictly positive and thus implies that $f(x)$ is _concave up_ everywhere. Visually, one can intuit that concavity measures whether the graph of a function is bowed upwards like a bowl (positive concavity) or downward like a mountain.

Analytically, however, we can approximate concavity at a point as whether the average value of the function in a small neighborhood of the point is generally greater than or less than the value of the function at that point. In algebra, we could express the concavity of a function $f$ at some point $a$ as

$$
\sum\limits_{b,|b-a|<\epsilon} (f(b)-f(a))^2
$$

We use $(f(b)-f(a))^2$ since we want the concavity measurement to be a smooth function itself and squaring guarantees positive values on the reals.

### Discretizing the Number Line

The above expression for concavity looks eerily similar to the expression for eigenvalues of the graph Laplacian. Let's try to turn the domain of $f(x)$, i.e. the real number line, into a graph. The most straightforward way to do so would be to just create an infinitely long list of vertices where each is connected to exactly 2 neighbors. Maybe something like the following:

![Hi This Is Fjorn](https://bear-images.sfo2.cdn.digitaloceanspaces.com/fjorn-1716492228-0.svg)

To approximate the reals we could now label each vertex with arbitrarily small real numbers such that for two connected vertices $i$ and $j$ with $j>i$ there exists no real number $k$ such that $i<k<j$. This graph now looks like a pixelated version of the number line.

Applying $f(x)=x^2$ to this graph would be equivalent to squaring the label on each vertex. The concavity of $f(x)$ at some vertex $a$ on this graph could be measured the same way as before, except instead of summing over points $b$ in some $\epsilon$ neighborhood of $a$, we sum over vertices $b$ that are adjacent to $a$.

$$
\sum\limits_{b,b\sim a}(f(b)-f(a))^2
$$

For finer and finer labeling of the graph this will approximate the actual concavity and 2nd derivative of $f(x)$ on the number line. But what would happen if we chose a different graph? While a straight-line graph does a great job at simulating the real number line, all the graphs we discussed above do not. Does the concavity or 2nd derivative calculation work for functions applied over them too?

### Vectors as Functions

While we traditionally think of functions as applying over the real number line (or other Euclidean spaces like $\mathbb{R}^2$), functions can just as well apply to graphs as shown above. Now the domain would be the set of vertices, usually indexed by some discrete label, and range would be the usual outputs of the function. Since graphs can be arbitrarily connected they allow us to apply functions like $f(x)=x^2$ to space more "topologically complex" than a traditional number line.

As discussed before, a point on the number line is only "influenced" by its immediate neighbors directly to its left and right. However, with arbitrary graphs a vertex can be "influenced" by arbitrarily many other vertices that themselves may be connected to points traditionally considered "far away."

The graph Laplacian thus represents a sort of 2nd derivative on functions of graphs when the functions are represented as vectors onto which the Laplacian may act. The function $f(x)=x^2$ could thus be represented by a vector like $f^T=(\begin{smallmatrix} 1 & 4 & 9 & \cdots & 36 \end{smallmatrix})$ when acting on a 6 vertex graph.

Just as the traditional derivative measures the change of a function $f(x)$ as you move along the number line, the 2nd derivative measures the change of that change. In the graph-theoretic view the vector $g=Lf$ is itself also a function, in this case the function that measures the change of the change of $f$ as you move from vertex to vertex on the graph.

### Eigenfunctions

An eigenvector of a matrix is a vector that is exclusively scaled (and not "rotated") by the application of said matrix and its eigenvalue is the scaling factor. If the Laplacian is effectively taking the 2nd derivative of a function over a graph then its eigenvectors are _eigenfunctions_ whose second derivatives are scalar multiples of themselves. This seems like an underwhelming insight on its own, but recall the Spectral Theorem from earlier: these eigenvectors form an orthonormal basis over the vector space of functions on the graph. This means we can represent _any_ function on our graph as a linear combination of the Laplacian's eigenfunctions.

On the real number line, such functions that are multiples of their 2nd derivatives are of family $e^x$, $\sin (x)$, and $\cos (x)$ (which are all actually the same family for complex numbers). Thinking about these families as an orthonormal basis of the space of all functions over the real number line tells us that any function can be written as a sum of scalar multiples of sines and cosines. Sound familiar? Such a sum is the Fourier series of a function!

So a graph's Laplacian eigenfunctions give us an orthonormal basis for _all_ functions over the graph. This lets us find Fourier series any function over a graph. Traditional Fourier series grant us mastery over functions on real (or complex) numbers. But, as mentioned above, graphs let us think of functions over more exotic topologies where points can influence many more neighbors than usual. So the graph Laplacian gives us a starting point for mastering functions over more connected spaces too!

### Change of Change and Connectivity

Admittedly the above discussion was fairly "hand-wavy." But the intuition still holds. Yet one question remains: if the graph Laplacian measures the change of the change of a function then how do its eigenvalues encode graph connectivity?

Let's start with analyzing how the zero eigenvalue $\lambda_1=0$ describes graph connectivity. We know its multiplicity tells us the number of connected components, but let's examine it algebraically. Just like on the number line, the 2nd derivative (i.e. application of the Laplacian) of a constant is the zero function. However, on a graph vertices in disconnected components cannot "influence" each other, so there can exist multiple orthonormal constant functions (i.e. semi-constant vectors as discussed earlier). So if a function's domain can produce orthogonal "constant" functions then the domain contain separated connected components.

The relationship between $\lambda_2$ and a graph's conductance is a little harder to explain. Algebraically, the eigenfunction $f_2$ with eigenvalue $\lambda_2$ has a 2nd derivative equal to a scalar multiple of itself. Small $\lambda_2$ would mean the 2nd derivative is "shrunken" by a small multiple so let's think about it as being near 0. But if $f_2$ has 2nd derivative of 0 then it must be constant?

Think about the [Fiedler Vector example](#fiedler-vector). The eigenvector for $\lambda_2$ actually did "look" constant; the coefficients of matching sign were generally near, if not equal, in value. The coefficients that corresponded to the edge whose removal separates the graph are the "farthest away" from the constant value of the other coefficients on their side of the graph. Just like with a truly disconnected graph, a graph with low conductance has extra dimensions of "constant"-like functions. The "constant-ness" of this extra dimension breaks down for function values on vertices near the "flimsy" connection.

Ultimately, we can parse "change of change" of a function as measuring the function's smoothness. On the number line smoothness at a point can only be affected by the points immediately greater or less than the point. But on a graph smoothness of a function is influenced by the graph's connectivity and the connectivity provides greater optionality when thinking about what kinds of function we consider to look "constant."

## Further Reading

So that was the spooky tale of spectral graph theory. If you're interested in learning more I highly recommend checking out the following reading material.

- [Dan Spielman's course on Spectral Graph Theory](http://www.cs.yale.edu/homes/spielman/eigs/)
- [Dan Spielman's (draft) book on Spectral Graph Theory](http://cs-www.cs.yale.edu/homes/spielman/sagt/sagt.pdf)
- [This video for a computer scientests intro to Spectral Graph Theory](https://www.youtube.com/watch?v=uTUVhsxdGS8)
- [Bogdan Nica's intro notes to Spectral Graph Theory](https://arxiv.org/pdf/1609.08072)
