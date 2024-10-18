1.2

a)

$L_{VP} \subseteq \{0,1,\# \}$
$L_{VP} = \left\{ bin(W)\#\#bin(k)\#\#bin(w_{1})\#\dots \# bin(w_{n}) | W,k \in \mathbb{N}, w_{1},\dots,w_{i} \in \mathbb{N}: \Sigma_{i=1}^{k} W_{i} \leq \Sigma^{n}_{j=1} w_{j}  \right\}$

b)

Sei G ein ungerichteter Graph
Sei A eine $n\times n$- Adjanzenzmatrix des Graphen G, wobei für alle $i,j \in V$ gilt: 

$$A[i,j] := \begin{cases} 1 \text{ wenn } \{i,j\} \in E \\ 0 \text{ wenn } \{i,j\} \notin E \end{cases}$$

Die Zerkelbedinung ist nun folgendermaßen formuliert:

$A[i_{1},i_{2}] = A[i_{2},i_{3}] = \dots = A[i_{k},i_{1}]$

$L_{KS} \subseteq \{0,1,\# \}$
$L_{KS} $