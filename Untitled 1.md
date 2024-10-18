1.2

a)

$L_{VP} \subseteq \{0,1,\# \}$
$L_{VP} = \left\{ bin(W)\#\#bin(k)\#\#bin(w_{1})\#\dots \# bin(w_{n}) | W,k \in \mathbb{N}, w_{1},\dots,w_{i} \in \mathbb{N}: \Sigma_{i=1}^{k} W_{i} \leq \Sigma^{n}_{j=1} w_{j}  \right\}$

b)

Sei G ein ungerichteter Graph
Sei A eine $n\times n$- Adjanzenzmatrix des Graphen G, wobei für alle $i,j \in V$ gilt: 
$$A[i,j] := \begin{cases} 1 \text{ wenn } \{i,j\} \in E \\ 0 \text{ wenn } \{i,j\} \notin E \end{cases}$$
Die Zirkelbedingung ist nun folgendermaßen formuliert:
$A[i_{1},i_{2}] = A[i_{2},i_{3}] = \dots = A[i_{k},i_{1}] = 1$ wobei $k \geq 3$ ist
Also in einer Sequenz von Knoten, die bei $i_1$ und $i_2,i_3,\dots,i_{k}$ durchläuft muss es bei $i_{1}$ enden, damit ein Kreis entsteht.
Für unseren zirkelfreien Graphen gilt nun:
$\forall i_{1},i_{2},\dots,i_{k}, k \geq 3:A[i_{1},i_{2}] = A[i_{2},i_{3}] = \dots = \ A[i_{k},i_{1}] \neq 1$
Sei $A = (a_{i,j}) \in \{0,1\}^{n\times n}$ die Adjazenzmatrix zu G, d.h. $a_{i,j} = 1$ genau dann, wenn ${i,j} \in E$ für $0 \leq i,j< n$. Wir kodieren G als Aneinanderreihung der Zeilen von A.
$L_{KS} \subseteq \{0,1,\# \}$
$L_{KS} = \{a_{1,1}\dots a_{n,n} |n \in \mathbb{N}: \forall i_{1},i_{2},\dots,i_{k}, k \geq 3,a_{i_{1},i_{2}} = a_{i_{2},i_{3}} = \dots = \ a_{i_{k},i_{1}} \neq 1$