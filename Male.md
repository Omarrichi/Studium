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