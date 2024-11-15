No, it would not make sense to use the binary cross-entropy error function if you replace the sigmoid activation function with a ReLU activation function in a logistic regression model.

The binary cross-entropy loss is defined as 
$$\text{BCE} = -\frac{1}{N} \sum_{i=1}^N \left[ y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i) \right]$$

Where 
- $y_{i} \in \{0,1\}$ is the true binary label
- $\hat{y}_{i} \$