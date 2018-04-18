+++
date = "13 Apr 2018"
draft = false 
title = "Class 11: Formal Verification Methods"
author = "Team Bus"
slug = "class10"
+++

## Motivation

????

## Manipulating Machine Learning: Poisoning Attacks and Countermeasures for Regression Learning
> Matthew Jagielski, Alina Oprea, Battista Biggio, Chang Liu, Cristina Nita-Rotaru and Bo Li._Manipulating Machine Learning: Poisoning Attacks
and Countermeasures for Regression Learning_. April 2018. arXiv e-print [[PDF]](https://arxiv.org/pdf/1804.00308.pdf)


### Introduction
It is well understood that in machine learning models are easy to be manipulated by smart attackers. There are both test time evasion attack or training time data poisoning attacks and hence, studying and understanding these potential risks is beneficial for application of machine learning in security critical domains.  

In this paper, the authors studied specific problem of training data poisoning attack on linear regression models and subsequently propose an effective defensive strategy.  

A linear regression is defined as 
<p align="center">
<img src="/images/class11/linear_regression.png" width="250" >
<br/>
</p>

And the loss function being optimized during the training time is the mean-squared error (MSE), which is defined as:
<p align="center">
<img src="/images/class11/loss_func.png" width="500" >
<br/>
</p>

MSE is used here as the result of linear regression is a continuous value while traditional classification task outputs discrete categorical values. The term \\(\Omega(\mathbf{w})\\) denotes the regularization terms. When we take different norms, it corresponds to different regularization functions ( \\(L_{2}\\)-norm corresponds to ridge regression, \\(L_{1}\\)-norm correspond to LASSO regression and Elastic-net Regression takes the linear combination of \\(L_{1}\\)-norm and \\(L_{2}\\)-norm).  

### Attacker's Goal and Optimization-based Poisoning Attack
The attack aims to corrupt the machine learning in the training phase by introducing noisy training data points such that predictions at test time will be influenced. The author considered both the white-box and black-box attack. White-box attack means the attacker is aware of all the information including training dataset, feature values, training algorithm and trained parameters. In the black-box setting, only training dataset is assumed to be unknown while rest are the same as white-box setting. With all these described, we arrive at the following formualtion regarding training data poisoning attack:

<p align="center">
<img src="/images/class11/problem_formulation.png" width="500" >
<br/>
</p> 

where \\(\mathbf{\theta}_{p}^{*}\\) is the model parameter obtained by training the model on poisoned training dataset. \\(D_{tr}\\) denotes unpoisoned training data, anmd \\(D_{p}\\) are the poisoned training data, which typically takes around \\(10\%\\) of the clean training data. \\(W(D^{'},\mathbf{\theta}_{p}^{*})\\) is the loss function defined on test (or validation) dataset \\(D^{'}\\), which is free from the poisoning attacks. Note that, this is bilevel optimization problem and is different from traditional optimization we know in that the constraint itself is an optimization problem. Also, with the parameter \\(\theta_{p}^{*}\\) depends implicitly on the poisoned training dataset \\(D_{p}^{*}\\). To optimize the above problem with respect to a set of data points \\(D_{p}\\), the approach is to optimize each istance \\(\mathbf{x}_{c}\\) in the set \\(D_{p}\\). With this, the authors apply gradient aescend algorithms to fina an optiaml \\(\mathbf{x}_c\\) that can maximize the loss function \\(W(\cdot)\\). The gradient can be calculated as below:

<p align="center">
<img src="/images/class11/gradient_cal1.png" width="250" >
<br/>
</p> 

The calculation of first term is non-trivial as there is an implicit dependence of \\(\mathbf{\theta}\\) and \\(\mathbf{x}_c\\). With some tricks played by KKT equilibrium conditions, the author arrived at equation as follows:

<p align="center">
<img src="/images/class11/gradient_cal2.png" width="500" >
<br/>
</p> 

Combining the above two, we are able to approximately compute the gardient and update \\(\mathbf{x}_c\\). Notebally, unlike previous papers on training data poisoning attacks on classifiers, the problem setting of this paper is in linear regression and the author further propose to optimize both the data point \\(\mathbf{x}_c\\) and its response variable \\(y_c\\). Hence, a new variable \\(\mathbf{z}_{c}= (\mathbf{x}_{c},y_{c})\\) is introduced to repalce \\(\mathbf{x}_c\\). Details of the gradient of \\(W\\) with respect to varaible \\(\mathbf{z}_{c}\\) can be referred from equation (14) and equation (4). 

### Statistical-based Poisoning Attack  

Statistical Attack is in contrast to the previous optimization based attack. Statistical attack is computationally efficient and it operates by generating adversarial training poinst from a multivariate normal distribution with mean and covariance estimated from the training data. Then, the feature values are rounded to corner values and the finally the reponse value \\(y_{c}\\) for a given data point \\(\mathbf{x}_c\\) is rounded to boundary value (either 1 or 0). As can be seen from the description, it is computationally effective, but less accurate than the optimization based method.  

### TRIM algorithm
This is the defense method proposed by the author to tackle the above mentioned two attack strategies. TRIM algorithm operates byiteratively estimating the regression parameters while at the same time training on a subset of points with lowest residuals in each iteration. Here, residual means error in a result. For example, error is taken with respect to \\(\mathbf{x}_c\\) while residual with taken with respect to \\(f(\mathbf{x})\\). The intuitive understanding of this algorithm is in that, majority of the training points are non-correupted and poisoned data points typically exhibit larger outlier behavior. If poisoned data points do not have outlier behavior, then its effect on regression task is minimized and hence can be considered as less harmful. The TRIM algorithm actually provides solution to the following optimization problem:

<p align="center">
<img src="/images/class11/trim_formulation.png" width="500" >
<br/>
</p> 

Optimization problem above intuitively represents that we optimize the regression parameter \\(\theta\\) and the subset of points with smallest residuals at the same time. This problem is computionally challenging as simple approach of enumarating all possible subsets of \\(I\\) of size \\(n\\) is large. Also, the parameter \\(theta\\) is typically unknown without any prior assumptions (if we know \\(\theta\\) in advance, we can simply choose \\(I\\) as a set of \\(n\\) points with lowest residuals with respect to \\(theta\\)). 

The solution to the challeng above is to alternatively optimize parameter \\(\theta\\) and \\(I\\). Specifically, at the begining of iteration \\(i\\), an estimation of parameter \\(\theta\\) is obtained based on the current set \\(I\\). Next, with the selected parameter \\(\theta\\), the set \\(I\\) is updated by selecting \\(n\\) points with lowest residuals with respect to \\(\theta\\). As for side note, the first iteration, the set \\(I\\) is initialized with random set of size \\(n\\). \\(theta\\) is updated through some initialization technique or prior knowledge. 

The theorem 1 in the paper proves that the TRIM terminates in finite number of steps, and hence convergence of the algorithm is guaranteed. The following thereom (theorem 2 in the paper) provides an upper bound on the worst case loss of the regression model under adversarial noise injection.  

<p align="center">
<img src="/images/class11/loss_up_bound.png" width="500" >
<br/>
</p> 

The left hand side means the loss on clean data (test or vallidation dataset) with respect to parameters trained on noisy data is upper bounded by fraction \\(1 + \frac{\alpha}{1-\alpha}\\) with respect to best case training loss on clean data \\(D_{tr}\\) abd parameter \\(\theta^{*}\\) is obtained by training on the clean dataset. Hence, right hand side actually denotes a ideal-case scenario. Note that, an implicit assumption here is, \\(D^{''}\\) and \\(D_{tr}\\) follow same distribution. Also, note that \\(alpha\\) is the fraction of poisoned data points in the training set, which is typically very small. 

### Experimental Results
In this paper, the author used Health care dataset, Loan Dataset and House pricing dataset, which all suits well for regression tasks. The results in this blog only contains results for ridge regression tasks. As shown in figure below (Fig3 in the paper), the optimization based attack method and statistical attack method proposed in this paper outperform the baseline gradient desend ([BGD](http://www.cs.man.ac.uk/~gbrown/publications/xiao2015icml.pdf)) method  significantly (BGD is a method adapted from previous attack method on classification task. There are no existing methods on attacking regression models).  

<p align="center">
<img src="/images/class11/attack_fig.png" width="800" >
<br/>
</p> 

As for the defense method, following figure illustrates its effectiveness in face of the atatcks desciribed in previous sections. It is obvious that TRIM algorithm (defense method proposed in this paper) outperforms other defense methods significantly. Other three defense methods are from existing works that are suitable for countering the attacks proposed in this paper ([RANSAC](https://www.sri.com/sites/default/files/publications/ransac-publication.pdf) and [Huber](https://projecteuclid.org/download/pdf_1/euclid.aoms/1177703732) from robust statistics and RONI implemented by authors based on [this](http://proceedings.mlr.press/v28/chen13h.pdf) and [this](https://people.eecs.berkeley.edu/~tygar/papers/SML/Spam_filter.pdf)).     

<p align="center">
<img src="/images/class11/defense_fig.png" width="800" >
<br/>
</p> 

### Conclusion
As a concluding mark, this paper is the first paper to consider training data poisoning attacks on linear regression models. Both attack and defense methods for the linear regression task are provided with theoretical guarantees on the convergence and optimality of their attacka and defense algorithms. 


#### References

[[1]](https://arxiv.org/pdf/1804.00308.pdf) Matthew Jagielski, Alina Oprea, Battista Biggio, Chang Liu, Cristina Nita-Rotaru and Bo Li._Manipulating Machine Learning: Poisoning Attacks
and Countermeasures for Regression Learning_. April 2018.

[[2]](http://www.cs.man.ac.uk/~gbrown/publications/xiao2015icml.pdf) Xiao, Huang, et al. "Is feature selection secure against training data poisoning?." International Conference on Machine Learning. 2015.

[[3]](https://www.sri.com/sites/default/files/publications/ransac-publication.pdf) Fischler, Martin A., and Robert C. Bolles. "Random sample consensus: a paradigm for model fitting with applications to image analysis and automated cartography." Readings in computer vision. 1987. 726-740. 

[[4]](https://projecteuclid.org/download/pdf_1/euclid.aoms/1177703732) Huber, Peter J. "Robust estimation of a location parameter." The annals of mathematical statistics (1964): 73-101.

[[5]](http://proceedings.mlr.press/v28/chen13h.pdf) Chen, Yudong, Constantine Caramanis, and Shie Mannor. "Robust sparse regression under adversarial corruption." International Conference on Machine Learning. 2013.

[[6]](https://people.eecs.berkeley.edu/~tygar/papers/SML/Spam_filter.pdf) Nelson, Blaine, et al. "Exploiting Machine Learning to Subvert Your Spam Filter." LEET 8 (2008): 1-9.

