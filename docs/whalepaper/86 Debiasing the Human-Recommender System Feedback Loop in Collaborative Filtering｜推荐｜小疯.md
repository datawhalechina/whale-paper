
## Introduction
### Bias and Fairness
![[Pasted image 20230831171356.png]]

### Popularity Bias
Popularity Bias: Popular items are recommended even more frequently than their popularity would warrant.

The long-tail phenomenon is common in RS data: in most cases, a small fraction of popular items account for the most of user interactions [3]. When trained on such long-tailed data, the model usually gives higher scores to popular items than their ideal values while simply predicts unpopular items as negative. As a result popular items are recommended even more frequently than their original popularity exhibited in the dataset

## Methodologies

### Human-Recommender System Feedback Loops

![[Pasted image 20230831155338.png]]

### Conventional MF
Conventional Matrix Factorization
![[截屏2023-08-31 15.55.35.png]]

### Conventional MF + Active Learning
1. Conventional Matrix Factorization_
2. + Actively Learning_
![[Pasted image 20230831155600.png]]

### Propensity
1. PopularityPropensityMF
2. Popularity Propensity MF + Active Learning
3. PoissonPropensityMF
4. Poisson Propensity MF + Active Learning

![[Pasted image 20230831155747.png]]
![[Pasted image 20230831155750.png]]
![[Pasted image 20230831155756.png]]
![[Pasted image 20230831155800.png]]
![[Pasted image 20230831155803.png]]


### Blind Spot Aware Matrix Factorization (Author’s)
![[Pasted image 20230831155600.png]]
![[Pasted image 20230831155643.png]]


## Experiments


### datastet
![[截屏2023-08-31 15.48.24.png]]
### evaluation
![[截屏2023-08-31 15.48.48.png]]

### methods

![[截屏2023-08-31 15.49.04.png]]

### experiments
![[截屏2023-08-31 15.49.38.png]]

![[截屏2023-08-31 15.49.45.png]]
