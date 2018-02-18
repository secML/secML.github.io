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

Differential Privacy can be defined in terms of the application-specific concept of adjacent databases. Suppose, for adjacent databases where each training dataset contains a set of image-label pairs, we say that two of these sets are adjacent if one image-label pair is present in one training set while absent in the other. A randomized mechanism \\( \mathcal{M}: D \rightarrow R\\) with domain D and range R satisfies \\( (\epsilon, \delta) \\)-differential privacy if for any two adjacent inputs \\( d,d'~\epsilon ~D\\) and for any subset of outputs \\( S \subseteq R\\) it holds that

$$ Pr[\mathcal{M}(d) ~\epsilon ~\mathcal{S}] \leq \mathcal(e)^{\epsilon} Pr[\mathcal{M}(d') ~\epsilon ~\mathcal{S}] + \delta $$ 

Authors used Dwork et al.[1] privacy definition of allowing the possibility that plain \\( \epsilon \\)-differential privacy is broken with probability \\( \delta \\) in their work. 


## Approach

Their proposed technique consists of three components. They are Differentially private SGD (Stochastic Gradient Descent) Algorithm, Moments Accountant and Hyperparameter Tuning.

#### Differentially private SGD Algorithm


Algorithm 1 describes their method for training a model with parameters \\( \theta \\) by minimizing the loss function \\( L(\theta) \\). At each step of computing of SGD, they calculated the gradient \\( \nabla \theta L(\theta, x_i)\\) for a random subset then clip the (\\ l_2 \\) norm of each gradient. After that they computed the average and for ensuring the privacy they added noise in those. They took the opposite direction of this average noisy gradient. Finally, they ouputted the model with the privacy loss. 

<p align="center">
<img src="/images/class4/algorithm_diff_priv_sgd.png" width="600" >
<br> Figure: Code snippet of Differentially private SGD
</p>

#### Moments Accountant


Each lot is \\( (\epsilon, \delta)\\)-DP if we choose \\(\sigma \\) in Algorithm 1 as \\( \sigma \\) = \\(\frac{\sqrt{ 2\log(\frac{1.5}{\delta})}} {\epsilon} \\) for Gaussian noise. Thus, each step is \\( \mathcal{O}((q \epsilon),q\delta) \\)-DP over the dataset, where q = \\( \frac{L}{N}\\) is the sampling probability of a lot over the dataset and \\(\epsilon \leq 1 \\).
The result in the literature which gives the best overall bound is the strong composition theorem [2]. Strong composition theorem does not take into account any particular noise distribution under consideration. Authors invent a stronger accounting method, which is the moments accountant. In Algorithm 1, over T iterations, naive composition gives the bound of \\( \mathcal{O}((qT \epsilon),qT\delta) \\)-DP. Over T iterations, strong composition gives the bound of \\( \mathcal{O}((q \epsilon \sqrt{T \log \frac{1}{\delta}}),qT\delta) \\)-DP. Whereas, over T iterations, moments accountant gives a tighter bound of \\( \mathcal{O}((q \epsilon \sqrt{T}),\delta) \\)-DP.

Theorem: There exist constants \\(c_1\\) and \\(c_2\\) so that given the sampling probability q = L/N and the number of steps T, for any \\( \epsilon < c_1q^{2}T \\), Algorithm 1 is \\( (\epsilon,\delta) \\)-differentially private for any \\(\delta > 0\\) if we choose $$ \sigma \geq c_2 \frac{q \sqrt{T\log(\frac{1}{\delta})}}{\epsilon} $$

If we use the strong composition theorem, we will then need to choose \\( \sigma = \Omega(q\sqrt{T\log(1/\delta)\log(T/\delta)}/\epsilon)\\)
For L = 0.01N, \\(\sigma\\) = 4, \\(\delta\\) = \\(10^{-5}\\), and T = 10000, we have \\(\epsilon\\) ≈ 1.26 using the moments accountant, and \\(\epsilon\\) ≈ 9.34 using the strong composition theorem.



#### Hyperparameter Tuning

Authors used the insight from the version of a result from Gupta et al. [3] restated as Theorem D.1 in the Appendix to reduce the number of hyperparameter settings. From theorem 1, one can say that making batches size too large will increase the privacy cost. The learning rate in non-private training will also go downwards as the model will converge to a local optimum. But authors found that they didn't have to decrease the learning rate to a very small value because differentially private training never goes to an area where it would be justified. Moreover, from their experiment they came to know that there was a small advantage of starting with a large learning rate compare to small learning rate and then linearly decaying it to a smaller value in a few epochs. And after that the rate remained constant. 


## Implementation

They have implemented the differentially private SGD algorithms in TensorFlow. They made their code available to download under an Apache 2.0 license from github.com/tensorflow/models. Their whole implementation is divided into two components. One is Sanitizer which preprocesses the gradient to protect privacy and the other one is privacy accountant which keeps track of the privacy cost of training the model. They implemented differentially private PCA(Principal Directions) and applied pre-trained convolutional layers. Because the neural network model may benefit by projecting the inputs on the PCA or by feeding it through a convolutional layer.


#### Sanitizer

For achieving the privacy protection, the sanitizer performs the following two operations:

1. Limits sensitivity of each example by clipping the norm of the gradient
2. Adds noise to the gradient of a batch before updating network parameters


#### Privacy Accountant

The main purpose of their implementation is to keep track of privacy spending over the course of training and hence the implementation of Privacy Accountant is the most important component of their implementation. One can compute \\(\alpha (\gamma)\\) by two ways. First one is by applying asymptotic bound and then evaluating a closed-form expression and the second one is by applying numerical integration. In their implementation they used the second one to compute \\(\alpha (\gamma)\\). They also \\(\alpha (\gamma)\\) computed for a wide range of \\( \gamma's\\), which helped them to get the best result possible \\( (\epsilon, \delta) \\) values. They stated that for the parameters it is enough to calculate \\(\alpha (\gamma)\\) where \\(\gamma \leq 32\\).


<p align="center">
<img src="/images/class4/algorithm_dpsgd_optimizer.png" width="600" >
<br> Figure: Code snippet of DPSGD_Optimizer and DPTrain
</p> 


The above figure contains the TensorFlow code snippet (in Python) of DPSGD_Optimizer which iteratively invokes DPSGD_Optimizer using a privacy accountant to bound the total privacy loss.


#### Differentially private PCA

Differentially Private Principal Component Analysis is a very useful method for capturing the features of the input data. Authors implemented the differentially private PCA algorithm according to Dwork et al.[4]. They took a random sample from the training examples and treated them as vectors. They normalized each vector to unit \\(l_2\\) norm to form the matrix \\(A^{T}A\\). After that they added Gaussian noise to the matrix A. Then for each input example they applied the projection to the principal directions of noisy covariance matrix before feeding them into the neural network. 


## Experimental Results

--- Team Panda
Christopher Geier, Faysal Hossain Shezan, Helen Simecek, Lawrence Hook, Nishant Jha


## References

[[1]](http://www.wisdom.weizmann.ac.il/~naor/PAPERS/odo.pdf) C. Dwork, K. Kenthapadi, F. McSherry, I. Mironov,and M. Naor. Our data, ourselves: Privacy via distributed noise generation. In EUROCRYPT, pages 486–503. Springer, 2006.\

[[2]](https://privacytools.seas.harvard.edu/files/privacytools/files/05670947.pdf) C. Dwork, G. N. Rothblum, and S. Vadhan. Boosting and differential privacy. In FOCS, pages 51–60. IEEE, 2010.\

[[3]](https://arxiv.org/pdf/0903.4510.pdf) A. Gupta, K. Ligett, F. McSherry, A. Roth, and K. Talwar. Differentially private combinatorial optimization. In SODA, pages 1106–1125, 2010.\

[[4]](http://kunaltalwar.org/papers/PrivatePCA.pdf) C. Dwork, K. Talwar, A. Thakurta, and L. Zhang. Analyze Gauss: Optimal bounds for privacy-preserving principal component analysis. In STOC, pages 11–20. ACM, 2014.\

