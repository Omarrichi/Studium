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

The binary cross-entropy loss relies on a probabilistic interpretation of $$