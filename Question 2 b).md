Question 2.b)
No, it would not make sense to use the binary cross-entropy error function if you replace the sigmoid activation function with a ReLU activation function in a logistic regression model.

The binary cross-entropy (BCE) loss is defined as 
$$\text{BCE} = -\frac{1}{N} \sum_{i=1}^N \left[ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) \right]$$
Where 
- $y_{i} \in \{0,1\}$ is the true binary label
- $\hat{y}_{i} \in [0,1]$ is the predicted probability output by the model
The key assumption here is that $\hat{y}_{i} \in [0,1]$, so:
- $\log(\hat{y}_i)$ is well defined
- $1 - \hat{y}_{i}\geq 0$, ensuring $\log(1 - \hat{y}_i)$ is also well defined.

Rectified linear unit activation function (ReLU) is defined as:
$$\text{ReLU}(x)=max(0,x)$$
$\hat{y}_i = \text{ReLU}(z_{i}) =\begin{cases} 0, & \text{if } z_i < 0, \\ z_i, & \text{if } z_i \geq 0. \end{cases}$
This implies that the output of ReLU is not constrained to $[0, 1]$ but instead lies in $[0, \infty)$.

This several issues:
- Undefined Logarithms:
	- When $\hat{y}_i = 0$, the term $\log(\hat{y}_i)$ in BCE becomes undefined, as $\log(0) \to -\infty$.
	- When $\hat{y}_i \geq 1$, the term $\log(1 - \hat{y}_i)$ becomes undefined, since $1 - \hat{y}_i < 0$.
- Violation of Probability Assumptions
	- The BCE loss assumes $\hat{y}_i \in [0, 1]$ so that it represents a valid probability. However, ReLU outputs $\hat{y}_i \in [0, \infty)$, which violates this assumption.


The binary cross-entropy loss relies on a probabilistic interpretation of $\hat{y}_{i}$ as a valid probability in $[0,1]$. ReLU does not satisfy this condition because:
- Its outputs are unbounded and can exceed 1, breaking the domain of the BCE formula.
- Logarithms in BCE can become undefined or invalid for ReLU outputs

Thus, BCE is incompatible with ReLU activation functions.