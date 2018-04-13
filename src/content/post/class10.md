+++
date = "06 Apr 2018"
draft = false 
title = "Class 10: Formal Verification Methods"
author = "Team Bus"
slug = "class10"
+++

## Motivation

Similar to what we saw in [Class 6](https://secml.github.io/class6/), we would like to have formal bounds on how robust a machine learning model is to attack. In the following two papers, we aim at achieving this robustness by means of proving properties about the underlying neural networks. Such attempts aim to end the arms race of attacks and defenses commonly seen in literature, and to provide formal guarantees of defenses with respect to any type of adversary.

## Differential Privacy and Adversarial Robustness
> Mathias Lecuyer, Vaggelis Atlidakis, Roxana Geambasu, Daniel Hsu, Suman Jana. _On the Connection between Differential Privacy and Adversarial Robustness in Machine Learning_. February 2018. arXiv e-print [[PDF]](https://arxiv.org/abs/1802.03471)

In the privacy domain, we are able to provide formal guarantees of the privacy of records by introducing the property of differential privacy. Where for two data sets that differ in one record, the difference of the output predictions on each one is less than some bounded amount.

In this paper, the authors extend this property to apply to adversarial examples by relating changes in a data set to changes in an input. Where for two _inputs_ that differ in some perturbation, the difference of the output predictions on each one is less than some bounded amount.

More formally,
$$ P(A(x) = i) \le e^{\epsilon} P(A(x’) = i) + \delta  $$
Where, \\( x’ \\) is some perturbed version of \\( x \\). \\( A \\) is the algorithm in question, and \\( P(A(x) = i) \\) is the probability that A assigns to \\( x \\) taking on value \\( i \\).


The authors apply this property to images and call it PixelDP. They go on to prove that if a model satisfies PixelDP with respect to the 0-norm metric for a set of inputs, then it is robust to 0-norm attacks on those inputs. (0-norm here indicating how many pixels were changed).

They accomplish this by also establishing an upper bound on the second highest class assignment for the perturbation, \\( \max_{j \ne i} p_j(x’) \\), using the same principle of robustness as defined above.

$$ P(A(x’) = j) \le e^{\epsilon}P(A(x) = j) + \delta $$ 

From these two bounds, they arrive at the core proposition that an algorithm satisfies PixelDP robustness to 0-norm attacks if the following holds:

$$ p_{i}(x) > e^{2 \epsilon} \max_{j: j \ne i} p_{j}(x) + (1+e^{\epsilon}) \delta $$	 	 	 		
			
With this proposition the authors devise a method for attaining this property, primarily by adding and accounting for noise during training and testing.

### Noise Layer

The authors’ basic approach is to add a noise layer to the neural network. They first establish that the noise added should be proportional to the sensitivity of the layer, which is more difficult to compute for deeper layers. They recommend placing the noise in the first few layers of the network as it is easiest to bound the sensitivity, and they acknowledge that trade-offs of accuracy and robustness play a role in the decision process.

The noise layer itself simply uses Laplacian and Gaussian mechanisms to add noise, and the network is trained with the noise layer to allow decent accuracy.

### Testing Strategy

To account for the noise during testing, the authors run the input through the network multiple times. The label is the highest predicted score is chosen and these results are counted and aggregated. This allows the variance of each label to also be computed.

The idea of making multiple queries may seem contradictory to the idea of privacy, where each query takes away from a privacy budget. However, here we are not concerned with privacy leakage, only with bounding the output of the function.

### Robustness

Finally, to have a measure of robustness, the authors propose a way deriving a bound for the maximum perturbation allowed. This is accomplished by finding the maximum \\( L \\) such that the proposition defined above still holds. The resulting \\( L_{max} \\) can be compared against some threshold \\( T \\) such that if \\( L_{max} \ge T \\) then the model is robust against that input. That is to say, that perturbation to the given image would not be sufficient to change its label. This metric is, of course, dependent on a given test set, but it allows the claim that a certain portion of the test set is “safe”, meaning the prediction is robust.

### Evaluation

To evaluate their approach, the authors used three accuracy metrics. 

* Conventional accuracy: the proportion of correct predictions
* Robust accuracy: the proportion of correct and robust predictions
* Robust precision: the proportion of correct predictions relative to the number of robust predictions.
	
They first tested accuracy on benign samples with different noise levels, \\( L \\) and different thresholds for robustness \\( T \\). 

<p align="center">
<img src="/images/class10/benign.png" width="500" >
<br/>
</p>

They found that, with higher \\( T \\), robust accuracy decreases but robust precision increases, and that the threshold should be tuned based on the amount of noise.

They then tested the accuracy on malicious samples, compared against the Madry defense [[3]](https://arxiv.org/pdf/1608.04644.pdf)

<p align="center">
<img src="/images/class10/malicious.png" width="500" >
<br/>
</p>

This shows that for a 2-norm attack, their defense is comparable to the Madry defense, but for an inf-norm attack, the Madry defense is better. These results make sense because the Madry defense was built around \\( L_{\infty} \\) attacks, while PixelDP is for \\( L_0 \\) and \\( L_2 \\) attacks. This begs the question of if we can have a differential privacy based defense in the \\(L_{\infty} \\) space, but at the moment there is no clear mapping between the two. 

## Piecewise Linear Neural Network Verification

> Rudy Bunel, Ilker Turkaslan, Philip H.S. Torr, Pushmeet Kohli, M. Pawan Kumar. _Piecewise Linear Neural Network verification: A comparative study_. arXiv e-print November 2017. [[PDF]](https://arxiv.org/pdf/1711.00455.pdf)

In this paper, the authors look at providing guarantees for piecewise linear neural networks for the purposes of verification. They show how to approach this through several approaches. For all of these approaches, the problem is defined the same general way. We are given a network that implements a function \\( y = f(x) \\) and some bounded input domain \\( C \\) in order to prove some property \\( P \\) about the network. This can be formulated as shown below:
$$ x \in C, y = f(x) \implies P(y) $$

In this problem definition, we see that we are only considering piecewise-linear neural networks (PL-NNs). This is justified by the observation that PL-NNs represent the majority of networks used in practice. In addition, the properties mentioned in the problem statement are defined to be Boolean formulas over linear inequalities. 

### Verification Methods

The authors then go on to discuss a couple different verification methods, which all leverage the piecewise linear structure of PL-NN. All of these methods that are compared follow the same general principle: discover a counterexample to make the given property false. If this counterexample problem is unsatisfiable, then we have shown that no counterexample exists and hence the property must be true. 

#### Mixed Integer Programing Encoding

In the general problem we are taking a look at, we have an issue with non-linearities as we must deal with a max function on the output of our network. In this method, the authors bypass this by replacing these nonlinear max functions with a set of inequalities. These can then be passed off to the solver to see if we can achieve some counterexample.

#### Reluplex

This method essentially assigns values to all the variables, even if some of the constraints are violated. It then goes through and tries to fix the violated constraints at each step. Reluplex is covered more extensively in our [previous blog post](https://secml.github.io/class6/).

#### Planet

This method operates by attempting to find an assignment to the phase of the non-linearities. Similar to Reluplex, it assigns values to the variables and then at each step verifies the feasibility of the assignment. Unlike Reluplex, this also has the advantage of being easily extended to networks containing MaxPooling units. In order to detect incoherent assignments, the author also employs a global linear approximation to the neural network. This is done by approximating the nonlinear constraints as a set of linear constraints that represent the convex hull of the nonlinearities. 

<p align="center">
<img src="/images/class10/planet.png" width="500" >
<br/>
</p>

#### Branch and Bound Optimization for Verification

This method looks at transforming this whole satisfiability problem into an optimization problem. To approach this, we look at adding extra neurons to the end of the network such that we can take a  look at the global minimum and use that information to determine whether the original problem was satisfiable. Essentially, we are formulating any Boolean formula over linear inequalities on the output of the network as a sequence of additional layers, and thus reducing the verification problem to a global minimization problem. 

Optimization algorithms, such as stochastic gradient descent, are not appropriate for this minimization problem as they do not provide the guarantee of a global minimum. The authors thus propose a branch and bound algorithm. This algorithm essentially is a breadth-first search algorithm. The algorithm computes the upper and lower bounds of the minimum of the output. The best upper-bound found so far will serve as a candidate for the global minimum. The iterative splitting process of the BFS type search will allow us to get a tighter and tighter lower bound.  

### Evaluation

For the evaluation, they consider a couple of data sets: Airborne Collision Avoidance System, Collision Detection, and Twin Stream. For each of these, they attempt to solve the satisfiability problem with a timeout of two hours. They define their success rate to be the proportion of properties successfully solved. A problem determined as SAT means that the property was false and that a counterexample exists and hence, UNSAT means that the property is true. Another metric used is labeled “Number of Wins.” This counts the number of times a solver was the fastest to solve a property. The results for these data sets are shown below:

<p align="center">
<img src="/images/class10/acas.png" width="500" >
<br/>
<caption align="bottom">ACAS data set</caption>
<br/>
</p>

<p align="center">
<img src="/images/class10/collision.png" width="500" >
<br/>
<caption align="bottom">Collision Detection data set</caption>
<br/>
</p>

<p align="center">
<img src="/images/class10/twinstream.png" width="500" >
<br/>
<caption align="bottom">Twin Stream data set</caption>
<br/>
</p>

Overall, we can see that the proposed branch and bound method is able to compete with other verification methods present in literature and so this proposes a new strategy for formal verification techniques. 

## Suman’s Talk

For the last section of class, Suman Jana from Columbia University gave us a talk on his upcoming research. He also discussed some of the core motivations for why it is important and difficult to study this area. Citing the differences between testing and verifying regular programs, which can have formal specifications and SMT solvers. In machine learning, the idea is to learn the specification, and they are fundamentally much more opaque.



--- Team Bus:  
Anant Kharkar,
Atallah Hezbor,
Bhuvanesh Murali,
Mainuddin Jonas,
Weilin Xu


#### References

[[1]](https://arxiv.org/pdf/1802.03471.pdf) M. Lecuyer, V. Atlidakis, R. Geambasu, D. Hsu, S. Jana. "On the Connection between Differential Privacy and Adversarial Robustness in Machine Learning" _arXiv preprint arXiv:1802.03471_, February 2018.

[[2]](https://arxiv.org/pdf/1711.00455.pdf) R. Bunel, I. Turkaslan, P-H.S. Torr, P. Kohli, M.-P, Kuymar. "Piecewise Linear Neural Network verification: A comparative study" _arXiv preprint arXiv:1711.00455_, November 2017.

[[3]](https://arxiv.org/pdf/1706.06083.pdf) A. Madry, A. Makelov, L. Schmidt, D. Tsipras, A. Vladu. "Towards Deep Learning Models Resistant to Adversarial Attacks" _arXiv preprint arXiv:1706.06083_, June 2017.


