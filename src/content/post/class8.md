+++
date = "23 Mar 2018"
draft = false
title = "Class 8: Testing of Deep Networks"
author = "Team Panda"
slug = "class8"
+++

## DeepXplore: Automated Whitebox Testing of Deep Learning Systems

> Kexin Pei, Yinzhi Cao, Junfeng Yang, Suman Jana. 2017. _DeepXplore: Automated Whitebox Testing of Deep Learning Systems_. In Proceedings of ACM Symposium on Operating Systems Principles (SOSP ’17). ACM, New York, NY, USA, 18 pages. [[PDF]](https://arxiv.org/pdf/1705.06640.pdf)

As deep learning is increasingly applied to security-critical domains, having high confidence in the accuracy of a model's predictions is vital.  Just as in traditional software development, confidence in the correctness of a model's behavior stems from rigorous testing across a wide variety of possible scenarios.  However, unlike in traditional software development, the logic of deep learning systems is learned through the training process, which opens the door to many possible causes of unexpected behavior, like biases in the training data, overfitting, underfitting, etc.  As this logic does not exist as an actual line of code, deep learning models are extremely difficult to test, and those who do are are faced with two key challenges: 

1. How can all (or at least most) of the model's logic be triggered so as to discover incorrect behavior?
2. How can such incorrect behavior be identified without manual inspection?

To address these challenges, the authors of this paper first introduce _neuron coverage_ as a measure of how much of a model's logic is activated by the test cases. To avoid manually inspecting output behavior for correctness, other DL systems designed for the same purpose are compared across the same set of test inputs, following the logic that if the models disagree than at least one model's output must be incorrect.  These two solutions are then reformulated into a joint optimization problem, which is implemented in the whitebox DL-testing framework DeepXplore.  

### Limitations of Current Testing

The motivation for the DeepXplore framework is the inability of current methods to thoroughly test deep neural networks.  Most existing techniques to identify incorrect behavior require human effort to manually label samples with the correct output, which quickly becomes prohibitively expensive for large datasets.  Additionally, the input space of these models is so large that test inputs cover only a small fraction of cases, leaving many corner cases untested.  Recent work has shown that these untested cases near model decision boundaries leave DNNs vulnerable to adversarial evasion attacks, in which small perturbations to the input cause a misclassification.  And even when these adversarial examples are used to retrain the model and improve accuracy, they still do not have enough model coverage to prevent future evasion attacks. 

### Neuron Coverage

To measure the area of the input space covered by tests, the authors define what they call "neuron coverage," a metric analogous to code coverage in traditional software testing.  As seen in the figure below, neuron coverage measures the percentage of nodes a given test input activates in the DNN, analogous to the percentage of the source code executed on code coverage metrics.  This is believed to be a better measure of the robustness test inputs because the logic of a DNN is learned, not programmed, and exists primarily in the layers of nodes that compose the model, not the source code. 

<p align="center">
<img src="/images/class8/neuron_coverage.png" width="500">
<div class="caption"> <a href="(https://arxiv.org/pdf/1705.06640.pdf)"><em>Source</em></a> </div>
</p>

### Cross-referencing Oracles

To eliminate the need for expensive human effort to check output correctness, multiple DL models are tested on the same inputs and their behavior compared.  If different DNNs designed for the same application produce different outputs on the same input, at least one of them should be incorrect, therefore identifying potential model inaccuracies.
         
<p align="center">
<img src="/images/class8/oracle.png" width="500">
<div class="caption"> <a href="(https://arxiv.org/pdf/1705.06640.pdf)"><em>Source</em></a> </div>
</p>


### Method

The primary objective for the test generation process is to maximize the neuron coverage and differential behaviors observed across models.  This is formulated as a joint optimization problem with domain-specific constraints (i.e., to ensure that a test case discovered is still a valid input), which is then solved using a gradient ascent algorithm.  


### Experimental Set-up

The DeepXplore framework was used to test three DNNs for each of five well-known public datasets that span multiple domains: MNIST, ImageNet, Driving, Contagio/Virustotal, and Drebin.  Because these datasets include images, video frames, PDF malware, and Android malware, different domain-specific constraints were incorporated into DeepXplore for each dataset (e.g. pixel values for images need to remain between 0 and 255, PDF malware should still be a valid PDF file, etc.).  The details of the chosen DNNs and datasets can be seen in the table below. 

<p align="center">
<img src="/images/class8/datasets.png" width="700">
<div class="caption"> <a href="(https://arxiv.org/pdf/1705.06640.pdf)"><em>Source</em></a> </div>
</p>

For each dataset, 2,000 random samples were selected as seed inputs, which were then manipulated to search for erroneous behaviors in the test DNNs.  For example, in the image datasets, the lighting conditions were modified to find inputs on which the DNNs disagreed. This is shown in the photos below from the Driving dataset for self-driving cars. 
<p align="center">
<img src="/images/class8/driving.png" width="400">
<div class="caption"> <a href="(https://arxiv.org/pdf/1705.06640.pdf)"><em>Source</em></a> </div>
</p>
The top row has the original images with arrows indicating that all three DNNs agreed on the decision and the bottom row has the images with modified lighting conditions with arrows showing that at least one of the models made a difference decision than the other two.


## Deep k-Nearest Neighbors: Towards Confident, Interpretable and Robust Deep Learning

> Nicolas Papernot, Patrick McDaniel. 2018. _Deep k-Nearest Neighbors: Towards Confident, Interpretable and Robust Deep Learning_. [[PDF]](https://arxiv.org/pdf/1803.04765.pdf)

## k-Nearest Neighbors

Deep learning is ubiquitous. Deep neural networks achieve a good performance on challenging tasks like machine translation, diagnosing medical conditions, malware detection, and classification of images. In this research work, the authors mentioned about three 
well-identified criticisms directly relevant to the security. They are the lack of reliable confidence in machine learning, model interpretability and robustness. Authors introduced the Deep k-Nearest Neighbors (DkNN) classification algorithm in this research work. It enforces the conformity of the predictions made by a DNN model on the test data with respect to the model’s training data. For each layer in the neural network, the DkNN performs a nearest neighbor search to find training points for which the layer’s output is closest to the layer’s output on the test input. Then they analyze the assigned label of these neighboring points to make it sure that the intermediate layer's computations remain conformal with the final output model’s prediction. 

<p align="center">
<img src="/images/class8/Dknn.png">
<br> <b>Figure:</b> Intuition behind the Deep k-Nearest Neighbors (DkNN)
</p> 


Consider a deep neural network (in the left of the figure), representations output by each layer (in the middle of the figure) and the nearest neighbors found at each layer in the training data (in the right of the figure). Drawings of pandas and school buses indicate training points. We can observe that confidence is high when there is homogeneity among the nearest neighbors labels. Interpretability of the outcome of each layer is provided by the nearest neighbors. Robustness stems from detecting nonconformal predictions from nearest neighbor labels found for out-of-distribution inputs across different layers. 


### Algorithm

The psudo-code for their k-Nearest Neighbors (DkNN) that the authors introduced in ensuring that the intermediate layer's computations remain conformal with the respect to the final model's prediction is given below-

<p align="center">
<img src="/images/class8/algorithm_dknn.png" width="400">
<br> <b>Figure:</b> Code snippet of Deep k-Nearest Neighbor   
</p> 


## Basis for Evaluation

Basis for evaluating robustness, interpretability, Confidence are discussed below-
<p align="center">
<img src="/images/class8/basis_evalution.png" width="500">
<br> <b>Figure:</b> Basis for Evaluation
</p> 


## Evaluation of Confidence/ Credibility


In their experiments, they measured high confidence on inputs. From the experiment they observed that credibility varies across both in- and out-of-distribution samples. They tailored their evaluation to demonstrate that the credibility is well calibrated. They performed their experiments on both benign and adversarial examples. 

## Classification Accuracy

In their experiment, they used three datasets. First, hand written recognition task of MNIST dataset, SVHN dataset and the third one is GTSRB dataset. In the following figure, we can observe the comparison of the accuracy between DNN and DkNN model on three different dataset.

<p align="center">
<img src="/images/class8/classification_accuracy.png" width="400">
<br> <b>Figure:</b> Classification accuracy of the DNN and DkNN: the DkNN has a limited impact on or improves performance.    
</p> 

## Credibility on in-distribution samples

Reliability diagrams are plotted for the three different datasets (MNIST, SVHN and GTSRB) below:
<p align="center">
<img src="/images/class8/evaluation_confidence_dknn.png" width="500">
<br> <b>Figure:</b> Reliability diagrams of DNN softmax confidence
(left) and DkNN credibility (right) on test data—bars (left
axis) indicate the mean accuracy of predictions binned by
credibility; the red line (right axis) illustrates data density
across bins. The softmax outputs high confidence on most of
the data while DkNN credibility spreads across the value range.  
</p> 

On the left, they visualized the estimation of confidence output by the DNN softmax and it is calculated by the probability \\(arg ~max_{j} ~f_j(x)\\). On the right, they plotted the credibility of DkNN predictions. From the graph, it may appear that the softmax is better calibrated than the corresponding DkNN. Because its reliability diagrams are closer to the linear relation between accuracy and DNN confidence. But if the distribution of DkNN credibility values are considered then it surfaces that the softmax is almost always very confident on test data with a confidence above 0.8. DkNN uses the range of possible credibility values for datasets like SVHN (test set contains a larger number of inputs that are difficult to classify). 


<!-- ## Mislabeled Inputs

<p align="center">
<img src="/images/class8/mislabeled.png" width="600">
<br> <b>Figure:</b> Mislabeled inputs from the MNIST (top) and SVHN
(bottom) test sets: we found these points by searching for
inputs that are classified with strong credibility by the DkNN
in a class that is different than the label found in the dataset.    
</p>  -->

## Credibility on out-of-distribution samples


Images from NotMNIST is identical to MNIST but the classes are non-overlapping. For MNIST, the first set of out-of-distribution samples contains images from the NotMNIST dataset. For SVHN, the out-of-distribution samples contains images from the CIFAR-10 dataset. Again, they have the same format but there is no overlap between SVHN and CIFAR-10. For both the MNIST and SVHN datasets, they rotated all the test inputs by an angle of 45 degree to generate a second set of out-of-distribution samples. 

<p align="center">
<img src="/images/class8/dknn-on-test-data.png" width="600">
<br> <b>Figure:</b> DkNN credibility vs. softmax confidence on outof-distribution
test data: the lower credibility of DkNN
predictions (solid lines) compared to the softmax confidence
(dotted lines) is desirable here because test inputs are not part
of the distribution on which the model was trained—they are
from another dataset or created by rotating inputs.    
</p> 

In the above figure, the credibility of the DkNN on the out-of-distribution samples is compared with the DNN softmax on MNIST (left) and SVHN (right). The DkNN algorithm has an average credibility of 6% and 9% to inputs from the NotMNIST and rotated MNIST test sets respectively, compared to 33% and 31% for the softmax probabilities. We find the same observation for SVHN model. Here, the DkNN assigns an average credibility of 15% and 18% to CIFAR-10 and rotated SVHN inputs, compared to 52% and 33% for the softmax probabilities.

## Evaluation  of the interpretability


Here, they have considered the model being bias to the skin color of a person. In a recent study, Stock and Cisse demonstrate how an image of former US president Barack Obama throwing an American football in a stadium ResNet model. They reproduce their experiment and apply the DkNN algorithm to this model. They plotted the 10 nearest neighbors from the training data. These neighbors are computed by the last hidden layer of ResNet model. 

<p align="center">
<img src="/images/class8/evaluation-interpretability.png" width="600">
<br> <b>Figure:</b> Debugging ResNet model biases—This illustrates how
the DkNN algorithm helps to understand a bias identified by
Stock and Cisse [105] in the ResNet model for ImageNet. The
image at the bottom of each column is the test input presented
to the DkNN. Each test input is cropped slightly differently to
include (left) or exclude (right) the football. Images shown at
the top are nearest neighbors in the predicted class according
to the representation output by the last hidden layer. This
comparison suggests that the “basketball” prediction may have
been a consequence of the ball being in the picture. Also
note how the white apparel color and general arm positions of
players often match the test image of Barack Obama.    
</p> 


On the left side of the above figure, the test image that is processed by the DNN is the same as that of the one used by Stock and Cisse. It contains 7 black and 3 white basketball players. They are similar to the color and also located in the air. They assumed that the ball play an important role in prediction. So, they ran another experiment with the same image but now cropping the image to remove the ball. Now the model predictated it as he is playing racket. Neighbor in this training class are white players. Image share certain charateristics, such as the background is green and most of the people are wearing white dresses and holding there hands in the air. In this example, besides the skin color, the position and appearance of the ball also contributed to the model's prediction.


## Evaluation of Robustness

DkNN is a step towards correctly handling malicious inputs like adversarial inputs because: 

- outputs more reliable confidence estimates on adversarial examples than the softmax.
- provides insights as to why adversarial examples affect undefended DNNs.
- robust to adaptive attacks they considered


## Accuracy on Adversarial Examples


They crafted adversarial examples using three algorithms: Fast Gradient Sign Method (FGSM), Basic Iterative Method (BIM), and Carlini-Wagner 2 attack (CW).

All there test results are shown in the following table. They have also included the accuracy of both undefended DNN and DkNN. By observing the table, they made a conclusion that even though the attacks were successful in evading the undefended DNN, but when the model is integrated with DkNN, then some accuracy on adversarial examples is recovered. 


<p align="center">
<img src="/images/class8/evaluation-robustness.png" width="500">
<br> <b>Figure:</b> Adversarial example classification accuracy for
the DNN and DkNN: attack parameters are chosen according
to prior work. All input features were clipped to remain in their
range. Note that most wrong predictions made by the DkNN
are assigned low credibility (see Figure 6 and the Appendix).    
</p> 

They have plotted the reliability diagrams comparing the DkNN credibility on GTSRB adversarial examples with the softmax probabilities output by the DNN. For DkNN, credibility is low across all attacks. Here, the number of points in each bin is reflected by the red line. DkNN outputs a credibility below 0.5 for most of the inputs. This indicates a sharp departure from softmax probabilities, which classified most adversarial examples in the wrong class with a high confidence of above 0.9 for the FGSM and BIM attacks. They also made an observation that the BIM attack is more successful at introducing perturbations than that of the FGSM or the CW attacks. 

<p align="center">
<img src="/images/class8/evaluation_confidence_dknn_adversarial.png" width="600">
<br> <b>Figure:</b>  Reliability Diagrams on Adversarial Examples—
The DkNN’s credibility is better calibrated (i.e., it assigns low
confidence to adversarial examples) than probabilities output by
the softmax of an undefended DNN. All diagrams are plotted
with GTSRB test data. Similar graphs for the MNIST and
SVHN datasets are in the Appendix.    
</p> 

## Explanation of DNN Mispredictions 


<p align="center">
<img src="/images/class8/misprediction_dknn.png" width="600">
<br> <b>Figure:</b> Number of Candidate Labels among k = 75 Nearest
Neighboring Representations—Shown for GTSRB with clean
and adversarial data across the layers of the DNN underlying
the DkNN. Points are centered according to the number of
labels found in the neighbors; while the area of points is
proportional to the number of neighbors whose label matches
the DNN prediction. Representations output by lower layers
of the DNN are less ambiguous for clean data than adversarial
examples (nearest neighbors are more homogeneously labeled).    
</p> 

In the above figure, for both clean and adversarial examples, we can observe that the number of candidate labels decreases as we move up the neural network from its input layer all the way to its output layer. Number of candidate labels (in this case k = 75 nearest neighboring training representations) that match the final prediction made by the DNN is smaller for some attacks. For CW attack, the true label of adversarial examples that it produces is often recovered by the DkNN. Again, the lack of conformity between neighboring training representations at different layers of the model characterizes the weak support for the model’s prediction. 

<!-- ## Robustness of DKNN to Adaptive Attacks

<p align="center">
<img src="/images/class8/robustness_dknn_adaptive.png" width="600">
<br> <b>Figure:</b>  Feature Adversarial Examples against our DkNN
algorithm—Shown for SVHN (see Appendix for MNIST).
Adversarial examples are organized according to their original
label (rows) and the DkNN’s prediction (columns).   
</p>  -->

<!-- ## Conclusion -->


### Comparison to LID

We discussed similarities between the DkNN approach and Local
Intrinsic Dimensionality [<a
href="https://openreview.net/forum?id=B1gJ1L2aW">ICLR 2018</a>]. There
are important differences between the approaches, but given the
results reported in the _Obfuscated Gradients Give a False Sense of
Security: Circumventing Defenses to Adversarial Examples_ (discussed
in <a href="https://secml.github.io/class3/">Class 3</a>) reported on
LID, it is worth investigating how robust DkNN is to the same
attacks. (Note that neither of these defenses are really obfuscating
gradients, so the attack strategy reported in that paper is to just use high confidence adversarial examples.)



## The Secret Sharer: Measuring Unintended Neural Network Memorization & Extracting Secrets


> Nicholas Carlini, Chang Liu, Jernej Kos, Ulfar Erlingsson, Dawn Song. 2018. The Secret Sharer: Measuring Unintended Neural Network Memorization & Extracting Secrets.  arXiv:1802.08232. [[PDF]](https://arxiv.org/pdf/1802.08232.pdf)

This paper focuses an adversary targeting "secret" user information stored in a deep neural network. Sensitive or "secret" user information can be included in the datasets used to train deep machine learning models. For example, if a model is trained on a dataset of emails, some of which have credit card numbers, there is a high probability that the credit card number can be extracted from the model, according to this paper.

## Introduction 
Rapid adoption of machine learning techniques has resulted in models trained on sensitive user information or "secrets". The "secrets" may include “person messages, location histories, or medical information.” The potential for machine learning models to memorize or store secret information could reveal sensitive user information to an adversary. Even black-box models were found to be susceptible to leaking secret user information. As more deep learning models are implemented, we need to be mindful of the models ability to store information, and to shield models from revealing secrets.

## Contributions

The exposure metric defined in this paper measures the ability of a model to memorize a secret. The higher the exposure of a model, the more likely a model is memorizing secret user information. The paper uses this metric to compare the ability of different models to memorize with different hyper-parameters. An important observation was that secrets were found to be memorized early in training rather than in the period of over-fitting. The author found that this property was consistent between different models and hyper parameters. Models included in paper focus on deep learning generative text models. Convolution neural network were also tested, but perform worse generally on text-based data. Extraction of secret information from the model was tested with varying hyper-parameters and conditions.

## Perplexity and Exposure

Perplexity is a measurement of how well a probability distribution predicts a sample. A model will have completely memorized the randomness of the training set if the log-perplexity of a secret is the absolute smallest. 

Log-perplexity suggests memorization but does not yield general information about the extent of memorization in the model. Thus the authors define rank:

<p align="center">
<img src="/images/class8/secsha_risk.png" width="400">
<br> <b>Figure:</b>  Definition of rank. Requires computing log-perplexity of all possible secrets.
</p> 

To compute the rank, you must iterate over all possible secrets. To avoid this heavy computation the authors define multiple numerical methods to compute a value related to the rank of a secret, exposure.

<p align="center">
<img src="/images/class8/secsha_exposure.png" width="400">
<br> <b>Figure:</b>  Definition of exposure. More simply, the negative log-rank of a secret, s[r].
</p> 

## Secret Extraction Methods

The authors identify four methods for extracting secrets from a black-box model. 

1. Brute Force
2. Generative Sampling
3. Beam Search
4. Shortest Path Search

Brute force was determined to be too computationally expensive, as the randomness space is too large. Generative sampling and beam search both fail to give the most optimal solution. The author's used shortest path search to guarantee the lowest log-perplexity solution. Their approach was based on Dijkstra's graph algorithm, and is explained in the paper. The figure below demonstrates the process of maximizing the log-perplexity for the purpose of finding the secret.

<p align="center">
<img src="/images/class8/secsha_dsk.png" width="500">
<br> <b>Figure:</b>  Shortest path search algorithm. Blue path points to location of largest perplexity, the location of the secret.
</p> 

## Characterizing Memorization of Secrets

In order to better understand model memorization the authors tested different numbers of iterations, various model architectures, multiple training strategies, changes in secret formats and context, and memorization across multiple simultaneous secrets.

#### Iterations

As showing in the figure below, the authors collected data relating to the exposure of the model throughout training. The exposure of the secret can clearly be seen rising until the test error starts to rise. This increase in exposure expresses the model's memorization of the training set early in the training process. 
<p align="center">
<img src="/images/class8/secsha_epoch.png" width="500">
<br> <b>Figure:</b>  Estimated exposure of secret and loss during training.
</p> 


#### Model architectures

The authors observed, through their exposure metric, that memorization was a shared property among recurrent and convolutional neural networks. Data relating to the different model architectures and exposure is shown in the figure below. 
<p align="center">
<img src="/images/class8/secsha_modelComparison.png" width="500">
<br> <b>Table:</b>  Model Exposure Comparison 
</p> 


#### Training strategies

As seen in the figure below, smaller batch sizes resulted in lower levels of memorization. Although having a smaller batch size slows down distributed processing of models, it is clear that their results suggest that a smaller batch size can reduce memorization. 
<p align="center">
<img src="/images/class8/secsha_batchsize.png" width="400">
<br> <b>Table:</b>  Estimated exposure of various model and batch sizes.
</p> 


#### Secret formats and context

The table below interestingly suggests that the context of secrets significantly impacts whether an adversary can detect memorization. As there become more characters or information associated with the secret, the adversary has an easier time extracting randomness. 
<p align="center">
<img src="/images/class8/secsha_secret.png" width="400">
<br> <b>Table:</b>  Estimated exposure with different contexts. 
</p> 


#### Memorization across multiple simultaneous secrets

The table below shows the effect of inserting multiple secrets into the dataset. As the number of insertions increase, the model becomes more likely to memorize the secrets that were inserted.
<p align="center">
<img src="/images/class8/secsha_multiplephrases.png" width="500">
<br> <b>Table:</b> Percentage of phrases that can be extracted from the model
</p> 

## Evaluating word level models

An interesting observation was that the capacity of a large word-level model produced better results than the smaller character-level model. By applying the exposure metrics to a word-level model, the authors found that representation of numbers mattered heavily in the memorization of information. When the authors replaced replaced "1" with "one," they saw the exposure dropped by more than half from 25 to 12 for the large word-level model. Because the word-level model was more than 80 times the size of the character-level model, the authors found it surprising that "[the large model] has sufficient capacity to memorize the training data completely, but it actually memorizes less."

## Conclusion
This paper examines the extent to which memorization has occurred in different types of algorithms and how sensitive user information can be revealed through this memorization. The metrics used in the paper can be easily transferred to existing models that have a well-defined notion of perplexity. They also demonstrate that these metrics can also be used to extract secret user information from black-box models. 

--- Team Panda: Christopher Geier, Faysal Hossain Shezan, Helen Simecek, Lawrence Hook, Nishant Jha

### Sources

> Kexin Pei, Yinzhi Cao, Junfeng Yang, Suman Jana. 2017. DeepXplore: Automated Whitebox Testing of Deep Learning Systems. In Proceedings of ACM Symposium on Operating Systems Principles (SOSP ’17). ACM, New York, NY, USA, 18 pages. https: //doi.org/10.1145/3132747.3132785 [[PDF]](https://arxiv.org/pdf/1705.06640.pdf)

> Nicholas Carlini, Chang Liu, Jernej Kos, Ulfar Erlingsson, Dawn Song. 2018. The Secret Sharer: Measuring Unintended Neural Network Memorization & Extracting Secrets.  arXiv:1802.08232. Retrieved from https://arxiv.org/pdf/1802.08232.pdf [[PDF]](https://arxiv.org/pdf/1802.08232.pdf)


> Nicolas Papernot, Patrick McDaniel. 2018. Deep k-Nearest Neighbors: Towards Confident, Interpretable and Robust Deep Learning. Retrieved from [[PDF]](https://arxiv.org/pdf/1803.04765.pdf)