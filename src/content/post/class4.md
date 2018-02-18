+++
date = "16 Feb 2018"
draft = true
title = "Class 4: Differential Privacy In Action"
author = "Team Panda"
slug = "class4"
+++


(Short Summary of What topic's are covered)

## Google's RAPPOR

Erlingsson, Úlfar, Vasyl Pihur, and Aleksandra Korolova. "Rappor: Randomized aggregatable privacy-preserving ordinal response." [[PDF](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/42852.pdf)] 

- Rappors Goal
- Randomized Response in RAPPOR
- Bloom Filter
- Result Analysis
- Attack Models
- Limitations

## Towards Practical Differential Privacy for SQL Queries 

Johnson, Noah, Joseph P. Near, and Dawn Song. "Towards Practical Differential Privacy for SQL Queries." [[PDF](https://arxiv.org/pdf/1706.09479.pdf)] 

- Motivation
- Contribution
- Background (definition of global and local sensitivity and stability)
- Discussion on Five crucial Properties of Dataset
- Requirement for DP
- Limitation of existing approach
- Counting Triangles
- Unsupported Queries and other aggregate functions
- FLex
- Performance Analysis
- Compaison with wPINQ


## Deep learning with differential privacy


> Abadi, Martin, et al. "Deep learning with differential privacy." [[PDF](https://arxiv.org/pdf/1607.00133.pdf)]

#### What is Differential Privacy?

Differential Privacy can be defined in terms of the application-specific concept
of adjacent databases. Suppose, for adjacent databases where each training dataset contains a set of image-label pairs, we say
that two of these sets are adjacent if one image-label pair is present in one training set while absent in the other. A randomized mechanism M: D → R with
domain D and range R satisfies (ε, δ)-differential privacy if
for any two adjacent inputs d, d' ∈ D and for any subset of
outputs S ⊆ R it holds that

$$ Pr[\mathcal{M}(d) ~\epsilon ~\mathcal{S}] \leq \mathcal(e)^{\epsilon} Pr[\mathcal{M}(d') ~\epsilon ~\mathcal{S}] + \delta $$ 

Authors used Dwork et al.[1] privacy definition of allowing the possibility that plain ε-differential privacy is broken with probability δ in their work. 


## Approach

Their proposed technique consists of three components. They are Differentially private SGD (Stochastic Gradient Descent) Algorithm, Moments Accountant and Hyperparameter Tuning.

#### Differentially private SGD Algorithm


Algorithm 1 describes their method for training a model with parameters \\( \theta \\) by minimizing the loss function \\( L(\theta) \\). At each step of computing of SGD, they calculated the gradient \\( \nabla \theta L(\theta, x_i)\\) for a random subset then clip the 2 norm of each gradient. After that they computed the average and for ensuring the privacy they added noise in those. They took the opposite direction of this average noisy gradient. Finally, they ouputted the model with the privacy loss. 

<p align="center">
<img src="/images/class4/algorithm_diff_priv_sgd.png" width="600" >

</p>
<!-- ![](/images/class4/algorithm_diff_priv_sgd.png) -->


#### Moments Accountant


Each lot is \\( (\epsilon, \delta)\\)-DP if we choose \\(\sigma \\) in Algorithm 1 as \\( \sigma \\) = \\(\frac{\sqrt{ 2\log(\frac{1.5}{\delta})}} {\epsilon} \\) for Gaussian noise. Thus, each step is \\( \mathcal{O}((q \epsilon),q\delta) \\)-DP over the dataset, where q = \\( \frac{L}{N}\\) is the sampling probability of a lot over the dataset and \\(\epsilon \leq 1 \\).
The result in the literature which gives the best overall bound is the strong composition theorem [2]. Strong composition theorem does not take into account any particular noise distribution under consideration. Authors invent a stronger accounting method, which is the moments accountant. In Algorithm 1, over T iterations, naive composition gives the bound of \\( \mathcal{O}((qT \epsilon),qT\delta) \\)-DP. Over T iterations, strong composition gives the bound of \\( \mathcal{O}((q \epsilon \sqrt{T \log \frac{1}{\delta}}),qT\delta) \\)-DP. Whereas, over T iterations, moments accountant gives a tighter bound of \\( \mathcal{O}((q \epsilon \sqrt{T}),\delta) \\)-DP.

Theorem: There exist constants \\(c_1\\) and \\(c_2\\) so that given the sampling probability q = L/N and the number of steps T, for any \\( \epsilon < c_1q^{2}T \\), Algorithm 1 is \\( (\epsilon,\delta) \\)-differentially private for any \\(\delta > 0\\) if we choose $$ \sigma \geq c_2 \frac{q \sqrt{T\log(\frac{1}{\delta})}}{\epsilon} $$

If we use the strong composition theorem, we will then need to choose \\( \sigma = \Omega(q\sqrt{T\log(1/\delta)\log(T/\delta)}/\epsilon)\\)
For L = 0.01N, \\(\sigma\\) = 4, \\(\delta\\) = \\(10^{-5}\\), and T = 10000, we have \\(\epsilon\\) ≈ 1.26 using the moments accountant, and \\(\epsilon\\) ≈ 9.34 using the strong composition theorem.



#### Hyperparameter Tuning

Allows balancing of privacy, accuracy, and performance
Authors found model accuracy most sensitive to training parameters such as batch size and noise level 


## Implementation
#### Sanitizer
#### Privacy Accountant
#### PCA
## Experimental Results

--- Team Panda
Christopher Geier, Faysal Hossain Shezan, Helen Simecek, Lawrence Hook, Nishant Jha


## References

[[1]](http://www.wisdom.weizmann.ac.il/~naor/PAPERS/odo.pdf) C. Dwork, K. Kenthapadi, F. McSherry, I. Mironov,and M. Naor. Our data, ourselves: Privacy via distributed noise generation. In EUROCRYPT, pages 486–503. Springer, 2006.\

[[2]](https://privacytools.seas.harvard.edu/files/privacytools/files/05670947.pdf) C. Dwork, G. N. Rothblum, and S. Vadhan. Boosting and differential privacy. In FOCS, pages 51–60. IEEE, 2010.\



