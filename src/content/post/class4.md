+++
date = "22 Feb 2018"
draft = false
title = "Class 4: Differential Privacy In Action"
author = "Team Panda"
slug = "class4"
+++

Two weeks ago we took a look at [privacy in machine learning](https://secml.github.io/class2/) and introduced differential privacy as one possible approach to perform statistical analysis on data while maintaining user privacy.  Today we explore three applications of differential privacy: Google's RAPPOR for obtaining user data from client-side software, the FLEX system to enforce differential privacy for SQL queries, and an algorithm for training deep neural networks that can provide differential privacy guarantees. 

## Google's RAPPOR 

> Erlingsson, Úlfar, Vasyl Pihur, and Aleksandra Korolova. _Rappor: Randomized aggregatable privacy-preserving ordinal response_. ACM CCS 2014. [[PDF](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/42852.pdf)] 

The goal of Randomized Aggregatable Privacy-Preserving Ordinal Response (RAPPOR) is to ensure anonymity for those participating in crowd-sourced statistics with a strong privacy guarantee. For example, if student who has cheated is participating in a study on cheating in school it is unlikely they would respond truthfully without strong plausible deniability.


A simple algorithm that constructs such plausible deniability is shown below:
 
    STEP ONE

    flip coin
    if (coin==HEADS):
        answer truthfully
    else:
        go to step 2

    STEP TWO

    flip coin
    if (coin==HEADS):
        answer yes
    else:
        answer truthfully

This algorithm will result in a truthful response rate of 75%. Therefore if we let \\(Y = [\text{Number of raised hands}] / [\text{Size of class}]\\) then \\([\text{Number of true cheaters}] \approx 2(Y − 0.25)\\).

This means we have differential privacy with the level \\(\epsilon = \ln(0.75/(1 − 0.75)) = \ln(3)\\), but only for the first time responses are collected. This guarantee degrades if the same survey is administered repeatedly (where the same respondants generate new randomness for each query). RAPPOR is a way to ensure strong privacy protection even for a single respondent who is surveyed often.

On a high level, RAPPOR achieves this by having each client machine report a “noisy” representation of the true value \\(v\\) by submitting a \\(k\\)-sized bit array to a server. This representation of \\(v\\) is selected in order to reveal a specific amount of information about \\(v\\) in order to limit the information the server learns from the \\(k\\)-sized bit array. Importantly the server does not learn the true value \\(v\\) with confidence even when an infinite number of reports are submitted by the client. 

This is achieved through the the following three steps:

1. _Signal_ - hash the client’s value \\(v\\) onto the Bloom filter \\(B\\) of size \\(k\\) using \\(h\\) hash functions. 

<p align="center">
<img src="/images/class4/rappor_signal.png" width="500" >
<br> <b>Figure:</b> Bloom Filter
</p>

A Bloom filter is a probabilistic data structure optimized for determining set membership.

2. _Permanent_ Randomized Response
For each value \\(v\\) from a client, and for each bit \\(i\\) in the bloom filter \\(B\\), create a binary reporting value \\(B_i’\\) such that TODO:missing equation here??? where \\(f\\) is a user-defined tuning variable, \\(f \in (0,1)\\). This \\(B’\\) is memorized and reused as the basis for all future reports on value \\(v\\).

<p align="center">
<img src="/images/class4/rappor_perm_prob.png" width="500" >
<br> <b>Figure:</b> Permanent B'
</p>

3. _Instantaneous Randomized Response_. Next, allocate a bit array \\(S\\) of size \\(k\\) and initialize to 0. Set each bit \\(i\\) in \\(S\\) with the following probabilities: TODO:???

<p align="center">
<img src="/images/class4/rappor_inst_prob.png" width="500" >
<br></p>

4. _Report_. Send the bit array \\(S\\) to the server.

How private are RAPPOR’s aggregated results in reality? If you assume infinite sampling, there is a privacy guarantee of:

<p align="center">
<img src="/images/class4/rappor_epsilon_inf.png" width="500" >
<br></p>

An attacker with "unlimited collection capabilities... is also bounded by the privacy guarantee of ε∞ and cannot improve upon this bound with more data collection."

## Towards Practical Differential Privacy for SQL Queries 

> Johnson, Noah, Joseph P. Near, and Dawn Song. _Towards Practical Differential Privacy for SQL Queries_. Proceedings of the VLDB Endowment, January 2018. [[PDF](https://arxiv.org/pdf/1706.09479.pdf)]  (Images below are taken from this paper.)

#### Motivation
The recent increase in data collection in large organizations has exposed personal user data to analysis that have an impact on user privacy. This paper investigates the methods that can be used to increase differential privacy, protecting individual privacy. By taking a practical approach to differential privacy, the authors are able to provide an implementation of differential privacy that is compatible with any existing database.

#### Contributions
- Empirical study of 8.1 million real-world SQL queries
- Elastic sensitivity
- FLEX: An end-to-end differential privacy system
- Experimental evaluation of FLEX

#### Elastic sensitivity
Elastic sensitivity is the paper’s main contribution and proposed approach to enforcing differential privacy on a system that is locally sensitivity-based. Local sensitivity is the “maximum of the difference between the query run on a true database and any neighbor of it.” The practical nature of their design exists because instead of being a property of all possible databases, local sensitivity is a property of one true database.
<p align="center">
<img src="/images/class4/ls.png" width="300" >
<br> <b>Figure:</b> Local Sensitivity
</p>
The paper describes and proves their theorem shown below using aspects of database local sensitivity. 


<p align="center">
<img src="/images/class4/proof.PNG" width="500" >
<br> <b>Figure:</b> Elastic Sensitivity
</p>

#### Limitation of existing approaches
Two main requirements of differential privacy are stated. The first is that the implementation of differential privacy must be compatible with existing databases, and have support for heterogeneous databases. Also it should have robust support for equijoins, which include self and non-self joins, relationship types and an arbitrary number of nested joins.

<p align="center">
<img src="/images/class4/g1.PNG" width="600" >
<br> <b>Figure:</b> Comparison of various differential privacy implementations
</p>

As seen in the chart above, other approaches address parts of the requirements, and elastic sensitivity seems to cover all of the requirements set forth in this paper for differential privacy. One lacking requirement among previous approaches was adequate database compatibility.

#### Unsupported Queries and other aggregate functions


<p align="center">
<img src="/images/class4/sqlcode.PNG" width="500" >
<br> <b>Figure:</b> SQL non-equijoin statement
</p>

Since non-equijoins need information about both datasets, the operation is not supported in elastic sensitivity. Elastic sensitivity also sometimes fails to provide support for max-frequency metrics. However, not having support for either of these operations does not impact a majority of operations in either case.

#### FLEX

<p align="center">
<img src="/images/class4/flexPerf.PNG" width="500" >
<br> <b>Figure:</b> FLEX design structure
</p>

FLEX is an implementation of elastic sensitivity that is highly compatible with existing databases. The FLEX database structure is shown in the figure above. TODO: this is the wrong figure

By targeting support for specific SQL queries, their system can enforce differential privacy on most real world queries to databases. For a given SQL query, FLEX calculates its elastic sensitivity given an analysis of the query. FLEX then applies smooth sensitivity to the elastic sensitivity and adds noise drawn from the Laplace distribution to the original query results. 

<p align="center">
<img src="/images/class4/flexPerf.PNG" width="600" >
<br> <b>Figure:</b> FLEX Performance
</p>

- Elastic sensitivity for 76% of queries
- Large error for unsupported queries: 14.14%
- Parsing errors: 6.58%


#### Comparison with wPINQ


<p align="center">
<img src="/images/class4/wPINQComp.PNG" width="1000" >
<br> <b>Figure:</b> wPINQ and Flex performance comparison
</p>

This paper moves towards more practical approaches of differential privacy in SQL queries by proposing elastic sensitivity. The concept of elastic sensitivity was then tested through their implementation in FLEX. A comparison of FLEX and wPINQ is shown in the figure above. Adoption of this method by Uber for internal data analytics demonstrates the potential of their approach for having a large impact on data privacy.


## Deep learning with differential privacy

> Martin Abadi, Andy Chu, Ian Goodfellow, H Brendan McMahan, Ilya Mironov, Kinal Talwar, and Li Zhang. _Deep learning with differential privacy_. ACM CCS 2016 [[PDF](https://arxiv.org/pdf/1607.00133.pdf)] (All figures below are taken from this paper)

#### Differential Privacy Recap

Differential Privacy can be defined in terms of the application-specific concept of adjacent databases. Suppose, for adjacent databases where each training dataset contains a set of image-label pairs, we say that two of these sets are adjacent if one image-label pair is present in one training set while absent in the other. A randomized mechanism \\( \mathcal{M}: D \rightarrow R\\) with domain D and range R satisfies \\( (\epsilon, \delta) \\)-differential privacy if for any two adjacent inputs \\( d,d'~\epsilon ~D\\) and for any subset of outputs \\( S \subseteq R\\) it holds that

$$ Pr[\mathcal{M}(d) ~\epsilon ~\mathcal{S}] \leq \mathcal{e}^{\epsilon} Pr[\mathcal{M}(d') ~\epsilon ~\mathcal{S}] + \delta $$ 

Authors used Dwork et al.[1] privacy definition of allowing the possibility that plain \\( \epsilon \\)-differential privacy is broken with probability \\( \delta \\) in their work. 


## Approach

Their proposed technique consists of three components: differentially private SGD (Stochastic Gradient Descent) Algorithm, Moments Accountant and Hyperparameter Tuning.

#### Differentially private SGD Algorithm

Algorithm 1 describes their method for training a model with parameters \\( \theta \\) by minimizing the loss function \\( L(\theta) \\). At each step of computing of SGD, they calculated the gradient \\( \nabla \theta L(\theta, x_i)\\) for a random subset then clip the \\(L_2\\) norm of each gradient. After that they computed the average and for ensuring the privacy they added noise in those. They took the opposite direction of this average noisy gradient. Finally, they ouput the model with the privacy loss. 

<p align="center">
<img src="/images/class4/algorithm_diff_priv_sgd.png" width="600" >
<br> <b>Figure:</b> Code snippet of Differentially private SGD
</p>

#### Moments Accountant


Each lot is \\( (\epsilon, \delta)\\)-DP if we choose \\(\sigma \\) in Algorithm 1 as \\( \sigma \\) = \\(\frac{\sqrt{ 2\log(\frac{1.5}{\delta})}} {\epsilon} \\) for Gaussian noise. Thus, each step is \\( \mathcal{O}((q \epsilon),q\delta) \\)-DP over the dataset, where q = \\( \frac{L}{N}\\) is the sampling probability of a lot over the dataset and \\(\epsilon \leq 1 \\).
The result in the literature which gives the best overall bound is the strong composition theorem [2]. Strong composition theorem does not take into account any particular noise distribution under consideration. Authors invent a stronger accounting method, which is the moments accountant. In Algorithm 1, over \\(T\\) iterations, naive composition gives the bound of \\( \mathcal{O}((qT \epsilon),qT\delta) \\)-DP. Over \\(T\\) iterations, strong composition gives the bound of \\( \mathcal{O}((q \epsilon \sqrt{T \log \frac{1}{\delta}}),qT\delta) \\)-DP. Whereas, over \\(T\\) iterations, moments accountant gives a tighter bound of \\( \mathcal{O}((q \epsilon \sqrt{T}),\delta) \\)-DP.

**Theorem:** There exist constants \\(c_1\\) and \\(c_2\\) so that given the sampling probability q = L/N and the number of steps T, for any \\( \epsilon < c_1q^{2}T \\), Algorithm 1 is \\( (\epsilon,\delta) \\)-differentially private for any \\(\delta > 0\\) if we choose $$ \sigma \geq c_2 \frac{q \sqrt{T\log(\frac{1}{\delta})}}{\epsilon} $$

If we use the strong composition theorem, we will then need to choose \\( \sigma = \Omega(q\sqrt{T\log(1/\delta)\log(T/\delta)}/\epsilon)\\)
For \\(L = 0.01N\\), \\(\sigma\\) = 4, \\(\delta\\) = \\(10^{-5}\\), and \\(T = 10000\\), we have \\(\epsilon\\) ≈ 1.26 using the moments accountant, and \\(\epsilon\\) ≈ 9.34 using the strong composition theorem.

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
<br> <b>Figure:</b> Code snippet of DPSGD_Optimizer and DPTrain
</p> 


The above figure contains the TensorFlow code snippet (in Python) of DPSGD_Optimizer which iteratively invokes DPSGD_Optimizer using a privacy accountant to bound the total privacy loss.


#### Differentially private PCA

Differentially Private Principal Component Analysis is a very useful method for capturing the features of the input data. Authors implemented the differentially private PCA algorithm according to Dwork et al.[4]. They took a random sample from the training examples and treated them as vectors. They normalized each vector to unit \\(l_2\\) norm to form the matrix \\(A^{T}A\\). After that they added Gaussian noise to the matrix A. Then for each input example they applied the projection to the principal directions of noisy covariance matrix before feeding them into the neural network. 


## Experimental Results


#### MNIST

<p align="center">
<img src="/images/class4/result_accuracy_mnist_fig_3.png">
</p> 

**Figure:** Results on the accuracy for different noise levels on the MNIST dataset. In all the experiments, the network uses 60 dimension PCA projection, 1,000 hidden units, and is trained using lot size 600 and clipping threshold 4. The noise levels \\(\sigma, \sigma_{p}\\) for training the neural network and for PCA projection are set at (8, 16), (4, 7), and (2, 4), respectively, for the three experiments.

In the above Figure, they showed the results for different noise levels. In
each plot, they showed the evolution of the training and testing
accuracy as a function of the number of epochs as well as
the corresponding \\(\delta\\) value, keeping \\(\epsilon\\) fixed. 


<p align="center">
<img src="/images/class4/result_accuracy_mnist_fig_4.png" width="600" >
</p> 

**Figure:** Accuracy of various \\( (\epsilon,\delta) \\) privacy values
on the MNIST dataset. Each curve corresponds to a different \\(\delta \\) value.

In the above figure, they showed the accuracy that they can obtain for different value of \\(\delta\\) and \\(\epsilon\\). From the figure, one can observe that keeping the value of \\(\delta\\) fixed and varying the value of \\(\epsilon\\) can have large impact on the accuracy. 


<p align="center">
<img src="/images/class4/result_accuracy_mnist_fig_5.png" >
</p> 

**Figure:** MNIST accuracy when one parameter varies, and the others are fixed at reference values.

In the above figure, they showed the accuracy of the model on MNIST dataset by by varying a particular parameter while keeping the other parameter constant. For training the neural network parameters and for PCA projection they set the reference values like - 60 PCA dimenstions, 600 lot size, 1000 hidden units, initial learning rate of 0.1, final learning rate 0.052 in 10 epochs, gradient norm bound of 4, and noise equal to 4 and 7 respectively. 


#### CIFAR-10

<p align="center">
<img src="/images/class4/result_accuracy_cifar_fig_6.png" >
</p> 

**Figure:** Results on accuracy for different noise levels on CIFAR-10. With \\(\delta \\) set to \\(10^{-5}\\) , achieve accuracy 67%, 70%, and 73%, with \\(\epsilon \\) being 2, 4, and 8, respectively. The first graph uses a lot size of 2,000, (2) and (3) use a lot size of 4,000. In all cases, \\(\sigma \\) is set to 6, and clipping is set to 3.


In the above figure, authors showed the result of the accuracy and privacy cost of the model on the CIFAR-10 dataset as a function of the number of epochs for different parameter settings. In MNIST dataset, the difference in accuracy between a non-private baseline and a private model is about 1.3% whereas the corresponding drop in accuracy in CIFAR-10 dataset is about 7%. 





--- Team Panda
Christopher Geier, Faysal Hossain Shezan, Helen Simecek, Lawrence Hook, Nishant Jha


## References

[[1]](http://www.wisdom.weizmann.ac.il/~naor/PAPERS/odo.pdf) C. Dwork, K. Kenthapadi, F. McSherry, I. Mironov,and M. Naor. Our data, ourselves: Privacy via distributed noise generation. In EUROCRYPT, pages 486–503. Springer, 2006.\

[[2]](https://privacytools.seas.harvard.edu/files/privacytools/files/05670947.pdf) C. Dwork, G. N. Rothblum, and S. Vadhan. Boosting and differential privacy. In FOCS, pages 51–60. IEEE, 2010.\

[[3]](https://arxiv.org/pdf/0903.4510.pdf) A. Gupta, K. Ligett, F. McSherry, A. Roth, and K. Talwar. Differentially private combinatorial optimization. In SODA, pages 1106–1125, 2010.\

[[4]](http://kunaltalwar.org/papers/PrivatePCA.pdf) C. Dwork, K. Talwar, A. Thakurta, and L. Zhang. Analyze Gauss: Optimal bounds for privacy-preserving principal component analysis. In STOC, pages 11–20. ACM, 2014.\
