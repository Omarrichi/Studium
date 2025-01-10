Question 2:
a)
Formula for Entropy:
$H= - \Sigma^{n}_{i=1}p_{i}*\log_{2}(p_{i})$
frenquences:
- Flu: 2
- Cancer: 2
- Total instences in EC3: 4

probabilities $p_{i}$
- $p_{Flu} = \frac{2}{4}=0,5$
- $p_{Cancer} = \frac{2}{4}=0,5$

In the Formula:
$H= -(p_{Flu}* \log_{2}(p_{Flu})+p_{Cancer}*\log_{2}(p_{Cancer}))$
$H= -(0,5* \log_{2}(0,5)+0,5*\log_{2}(0,5))$
($\log_{2}(0,5)= -1$)
$\Rightarrow H = -(0,5*-1+0,5*-1)= 1.0$
$EC3: H=1.0$

$l$-Diversity: 
-  Compare H for each equivalence class to $\log_{2}(l)$
	- For EC3, $H = 1.0$, so $\log_{2}(l)\leq 1.0$
	- This implies $l \leq 2$ (since $\log_{2}(2)=1$)
$\Rightarrow$
Entropy of EC3: 1.0
The data satisfies entropy $l$-diversity for $l=2$

b)
Group by Quasi-Identifiers:
1. **(20 to 35, part-time)**:
    
    - Rows: 1, 10
    - Count: 2
2. **(36 to 65, part-time)**:
    
    - Rows: 2, 4, 7
    - Count: 3
3. **(20 to 35, full-time)**:
    
    - Rows: 3, 5, 8, 9
    - Count: 4
4. **(36 to 65, full-time)**:
    
    - Rows: 6
    - Count: 1

*k*-Anonymity

For $k$-anonymity, the size of the smallest equivalence class determines $k$. Here, the smallest equivalence class is **(36 to 65, full-time)** with **1 instance**.

1. **Equivalence Classes**:
    
    - (20 to 35, part-time): 2 instances
    - (36 to 65, part-time): 3 instances
    - (20 to 35, full-time): 4 instances
    - (36 to 65, full-time): 1 instance
2. **k-Anonymity**: *k*=1

Question 3:
a)
The **IGS** quantifies the reduction in uncertainty about the sensitive feature (BMI) after the split:
$$IGS=H(BMI)−H(BMI∣Split)$$

- $H(BMI)$: Entropy of BMI for the entire dataset.
- $H(BMI∣Split)$: Weighted entropy of BMI after the split.

Entropy Formula:
$$H= - \Sigma^{n}_{i=1}p_{i}*\log_{2}(p_{i})$$

Compute $H(BMI)$
BMI has 3 values: High, Low, Average:
- High: 2 instances
- Low: 2 instances
- Average: 1 instance
- Total: 5 instances

Proportions:

- $p(High)=\frac{2}{5}=0,4$
- $p(Low)=\frac{2}{5}=0,4$
- $p(Average)=\frac{1}{5}=0,2$
Entropy:
$H(BMI)=−(0.4⋅log2​(0.4)+0.4⋅log2​(0.4)+0.2⋅log2​(0.2))$