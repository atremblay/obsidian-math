

Highlight from [Introduction to Graph Theory](https://store.doverpublications.com/0486678709.html)

# Graph
A graph is an object consisting of two sets called its *vertex set* and its *edge set*. The vertex set is a finite **nonempty** set. The edge set may be emtpy, but otherwise its elements are **two-element** subsets of the vertex set.

![[Images/graph.svg]]
# Pseudograph
Loops are not allowed in a graph because that would be an edge `{X,X}` and this is not a proper set; the same element is there twice.

If we allow loops. then we are talking about a *pseudograph*

![[Images/pseudograph.svg]]

# Multigraph
Skeins (repeated edges) are not allowed either because that would mean having the edge `{X,Y}` repeated, but a graph is defined by an **edge set**, cannot have `{{X,Y}, {X,Y}}`.

If we allow skeins, then we are talking about a *multigraph*

![[Images/multigraph.svg]]

# Hypergraph
An hypergraph is an object consisting of two sets called its *vertex set* and its *edge set*. The vertex set is a finite **nonempty** set. The edge set may be emtpy, but otherwise its elements are subsets of the vertex set.

The main difference with a standard graph is that the elements of the edge set can have more than one vertice. This is a generalization of an edge called an hyperedge.

![[Images/hypergraph.svg]]

# Cyclic graph
If *v* is an integer greater than or equal to 3, the *cyclic* graph on *v* vertices, denoted $C_v$ , is the graph having vertex set *{1,2,...,v}* and the edge set *{{1,2}, {2,3}, ..., {v-1, v}, {v,1}}*.


# Null graph
If *v* is a positive integer, the *null* graph on *v* vertices, denoted $N_v$ , is the graph having vertex set *{1,2,...,v}* and no edges.


# Complete graph
If *v* is a positive integer, the *complete* graph on *v* vertices, denoted $K_v$ , is the graph having vertex set *{1,2,...,v}* and all possible edges

# Complement of a graph
If *G* is a graph, its complement, denoted $\overline{G}$ , is a graph having the same vertex set; the edge set of $\overline{G}$ consists of all two-element subsets of the vertex set which have not already been included in the edge set of *G*. 

If two vertices are connected in *G*, they are not connected in $\overline{G}$ and vice-versa.

# Subgraph

A graph $H$ is a *subgraph* of $G$ if the vertex set of $H$ is a subset of the vertex set of $G$ and the edge set of $H$ is a subset of the edge set of $G$.

# Supergraph
Since a subgraph is the result of selective erasing it follows that a supergraph is the result of selective augmentation. Given a graph *G* you can form a supergraph of *G* by adding new vertices and/or connecting nonadjacent (new or old) vertices.

# Degree of a vertex
Number of edges connected to the node/vertex

# Isomorphism
Two graphs are isomorphic if there is a one-to-one correspondance between bertex sets that *preserves vertex adjacency*. In other words, relabel one graph so that it is the same as the other one. A label is just a label, it contains no information on the structure of a graph. The edge set must be updated accordingly.

Left grpah is isomorphic to the right graph. Intermediate steps in-between


![[../Machine Learning/isomorphism 1.svg]]


1. Flip B and D
2. Rotate 90 degrees clockwise
3. Match the labels on the right
$$
\begin{align}
	A & \longleftrightarrow  3\\
	B & \longleftrightarrow  1\\
	C & \longleftrightarrow  2\\
	D & \longleftrightarrow  4\\
\end{align}
$$


## Properties preserved by isomorphism
Recognizing that two graphs are isomorphic is not always easy. All these properties must be the same:

1. The number of vertices
2. The number of edges
3. The distrubitions of degrees
4. The number of "pieces" (disconnected parts) of a graph

> A pair of graphs can share all four properties and still not be isomorphic; but having checked that they do not differ in these four bovious ways, it is at least less of a gabmle to invest some time searching for an isomorphism.

# Planar graphs
A graph is *planar* if it is isomorphic to a graph that has been drawn in a plane without edge-crossings. Otherwise a graph is *nonplanar*.

Useful theorem

###### Jordan Curve Theorem
If C is a continuous simple closed curve in a plance, then C divicdes the rest of the plane into two regions having C as their common boundary. If a point P in one of these regions is joined to a point Q in the other by a continuous curve L in the plane, then L intersects C.

###### Corollary
If C is a continuous simple closed curve in a plane and two points of C are joined by a continuous curve L in the plane having no other points in common with C, then except for its endpoints L is entirely contained in one of the two regions determined by C.

###### Corollary
Every [[#Supergraph|supergraph]] of a nonplane graph is nonplanar

# Expansion
If some new vertices of degree 2 are added to some of the edges of a graph *G*, the resulting graph *H* is called an *expansion of G*. This different than [[#Supergraph|supergraph]]. In making an expansion there is only one thing we are allowed to do (provided we do anything at all): splice new vertices of degree 2 into the edges.

Right image is an expansion of the left. 

![[Images/expansion.svg]]

# Euler walk

A walk in a graph is a sequence $A_1, A_2, \ldots, A_n$ of not necessarily distinct vertices in which $A_{k}$ is joined by an edge to $A_{k+1}$. The walk is said to join $A_1$ and $A_n$.

A graph is said to be *connected* if every pair of vertices is joined by a walk. Otherwise a graph is said to be disconnected.

A walk $A_1, A_2, \ldots, A_n$ is *closed* if $A_1$ and $A_n$ are the same vertex, otherwise a walk is *open*.

An **euler walk** is a walk that uses every edge in the graph exactly once.

###### Theorem
If a connected graph has a closed euler walk, then every vertex is even. Conversly, if a graph is connected and every vectex is even, then it has a closed euler walk.

# Faces
When a planar graph is actually drawn in a place without edge-crossings, it cuts the plane into regions called *faces* (denoted *f*) of the graph. The exterior [[../Ordinary differential equations/Definitions/Region]] is counted as a face.

# Polygonal graph
A graph is *polygonal* if it is planar, cnnected and has the property that every edge borders on two different faces. An edge can border on no more than two faces, so what we are excluding are planar connected graphs having one or more edges that border on only one face.

![[Images/polygonal.svg]]

##### Eurler's formula
No matter what graph you draw, provided that it is corrsing-free and in one piece, there is the basic cimplicity of $v + f - e = 2$


# Regular graph
A graph is *regular* if all the vertices have the same degree. If the common value of the degrees of a regular graph is the number *d*, we say that the graph is *regular of degree d*.

Examples
1. Any [[#Cyclic graph|cyclic graph]] $C_v$ is regular of degree 2
2. Any [[#Complete graph|complete graph]] $K_v$ is regular of degree $v-1$
3. Any [[#Null graph|null graph]] $N_v$ is regular of degree 0

# Planotic graph

A graph is *platonic* if it is [[#Polygonal graph|polygonal]], [[#Regular graph|regular]], and has the additional property that all of its [[#Faces|faces]] are bounded by the same number of edges.

Other than $K_1$ and the *cyclic graphs* there are only five platonic graphs, also know as Platonic solids
1. Tetrahedron or $K_4$ (d4)
2. Hexahedron or cube (d6)
3. Octahedron (d8)
4. Dodecahedron (d12)
5. Icosahedron (d20)
