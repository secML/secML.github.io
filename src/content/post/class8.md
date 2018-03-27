+++
date = "23 March 2018"
draft = true
title = "Class 8: Testing of Deep Networks"
author = "Team Panda"
slug = "class8"
+++

## DeepXplore: Automated Whitebox Testing of Deep Learning Systems

> Kexin Pei, Yinzhi Cao, Junfeng Yang, Suman Jana. 2017. DeepXplore: Automated Whitebox Testing of Deep Learning Systems. In Proceedings of ACM Symposium on Operating Systems Principles (SOSP ’17). ACM, New York, NY, USA, 18 pages. https: //doi.org/10.1145/3132747.3132785 [[PDF]](https://arxiv.org/pdf/1705.06640.pdf)



## Deep k-Nearest Neighbors: Towards Confident, Interpretable and Robust Deep Learning

> Nicolas Papernot, Patrick McDaniel. 2018. Deep k-Nearest Neighbors: Towards Confident, Interpretable and Robust Deep Learning. Retrieved from [[PDF]](https://arxiv.org/pdf/1803.04765.pdf)

## k-Nearest Neighbor


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
<img src="/images/class8/algorithm_dknn.png" width="600">
<br> <b>Figure:</b> Code snippet of Deep k-Nearest Neighbor   
</p> 


## Basis for Evaluation

Basis for evaluating robustness, interpretability, Confidence are discussed below-
<p align="center">
<img src="/images/class8/basis_evalution.png" width="750">
<br> <b>Figure:</b> Basis for Evaluation
</p> 


## Evaluation of Confidence/ Credibility


In their experiments, they measured high confidence on inputs. From the experiment they observed that credibility varies across both in- and out-of-distribution samples. They tailored their evaluation to demonstrate that the credibility is well calibrated. They performed their experiments on both benign and adversarial examples. 

## Classification Accuracy

In their experiment, they used three datasets. First, hand written recognition task of MNIST dataset, SVHN dataset and the third one is GTSRB dataset. In the following figure, we can observe the comparison of the accuracy between DNN and DkNN model on three different dataset.

<p align="center">
<img src="/images/class8/classification_accuracy.png" width="600">
<br> <b>Figure:</b> Classification accuracy of the DNN and DkNN: the DkNN has a limited impact on or improves performance.    
</p> 

## Credibility on in-distribution samples

Reliability diagrams are plotted for the three different dataset, i.e. MNIST, SVHN and GTSRB dataset in the following Figure. 
<p align="center">
<img src="/images/class8/evaluation_confidence_dknn.png" width="600">
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


On the left side of the above figure, the test image that is processed by the DNN is the same as that of the one used by Stock and Cisse. It contains 7 black and 3 white basketball players. They are similar to the color and also located in the air. They assumed that the ball play an important role in prediction. So, they ran another experiment with the same image but now cropping the image to remove the ball. Now the model predictated it as he is playing racket. Neighbor in this training class are white players. Image share certain charateristics, such as the background is green and most of the people are wearing white dresses and hodling there hands in the air. In this example, besides the skin color, the position and appearance of the ball also contributed to the model's prediction.


## Evaluation of Robustness

DKNN is a step towards correctly handling malicious inputs like adversarial inputs because: \\
- outputs more reliable confidence estimates on adversarial examples than the softmax.\\
- provides insights as to why adversarial examples affect undefended DNNs.\\
- robust to adaptive attacks they considered\\



## Accuracy on Adversarial Examples


They crafted adversarial examples using three algorithms. They are \\
 - Fast Gradient Sign Method (FGSM), \\
 - Basic Iterative Method (BIM), \\
 - Carlini-Wagner 2 attack (CW)

 All there test results are shown in the following table. They have also included the accuracy of both undefended DNN and DkNN. By observing the table, they made a conclusion that even though the attacks were successful in evading the undefended DNN, but when the model is integrated with DkNN, then some accuracy on adversarial examples is recovered. 


<p align="center">
<img src="/images/class8/evaluation-robustness.png" width="600">
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




## The Secret Sharer: Measuring Unintended Neural Network Memorization & Extracting Secrets


> Nicholas Carlini, Chang Liu, Jernej Kos, Ulfar Erlingsson, Dawn Song. 2018. The Secret Sharer: Measuring Unintended Neural Network Memorization & Extracting Secrets.  arXiv:1802.08232. Retrieved from https://arxiv.org/pdf/1802.08232.pdf [[PDF]](https://arxiv.org/pdf/1802.08232.pdf)

