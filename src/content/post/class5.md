+++
date = "23 Feb 2018"
draft = true
title = "Class 5: Adversarial Machine Learning in Non-Image Domains"
author = "Team Nematode"
slug = "class5"
+++

## Introduction

## Audio

## Natural Language Processing
[Towards Crafting Text Adversarial Samples](https://arxiv.org/pdf/1707.02812.pdf)
Suranjana Samanta and Sameep Mehta

Adversarial samples are strategically modified samples, which are crafted with the purpose of fooling a classifier at hand. Although most of the prior works have been focused on synthesizing adversarial samples in the image domain, NLP based text classifiers can also be the focal point of such exploit. This paper introduces a method of crafting adversarial text samples by modification of the original samples. To be precise, the authors propose to modify the original text samples by deleting or replacing the important  words in the text or by introducing new words in the text sample. While crafting adversarial samples, the paper focuses on generating meaningful sentences which can pass off as legitimate from language viewpoint.  

### Approach
Initially, the authors calculate contribution of each word towards determining the class-label.  A word in the text is highly contributing if its removal from the text is going to change the class probability value to a large extent. In their proposed approach, modification of the sample text by considering each word at a time, in the order of the ranking based on the class-contribution factor. For the modification purpose, a candidate pool for each word in sample text is created considering consider synonyms and typos of each of the words as well as the genre or sub-category specific keywords. The assumption behind choosing sub-category or genre specific keywords based on the fact that certain words may contribute to positive sentiment for a particular genre but can emphasis negative sentiment for other kind of genre. For example, we can consider the sentence, "The movie was hilarious". This indicates a positive sentiment for a comedy movie. But the same sentence denotes a negative sentiment for a horror movie.  Thus, the word 'hilarious' contributes to the sentiment of the review based on the genre of the movie. Finally, replacement, addition or removal of words are performed on a given text sample at each iteration, so that the modified sample flips its class label.

### Experiments
The paper performs their experimental results on two datasets- IMDB movie review dataset for sentiment analysis and twitter dataset for gender classification. They compare the efficiency of their method with the existing method TextFool by measuring the accuracy of the model
obtained at different configurations. For both the cases, the proposed model was generated using Convolutional Neural Network (CNN). 

Figure below shows the results for IMDB dataset. From the figure, it is obvious that the proposed method of adversarial sample crafting for text is capable of synthesizing semantically correct adversarial text samples from the original text sample. In addition, the inclusion of genre specific keywords definitely boost up the quality of sample crafting. This is evident from the fact the drop in accuracy of the classifier before re-training for original text sample and the adversarialy crafted text sample is more when genre specific keywords are being used.
<p align="center">
<img src="/images/result_imdb.PNG" width="500" >
<br>  <b>Figure:</b> Performance results on IMDB movie review dataset.
</p>

The following figure shows a graph that indicates the number of changes required to create successful adversarial samples with and without using the genre-specific keywords. 
<p align="center">
<img src="/images/result_modifications.PNG" width="500" >
<br>  <b>Figure:</b> Plot showing the number of adversarial samples produced against the number of changes incurred
</p>

For the Twitter dataset, the proposed method shows similar performance as the IMDB dataset.
<p align="center">
<img src="/images/result_twitter.PNG" width="500" >
<br>  <b>Figure:</b> Performance results on Twitter gender classification dataset.
</p>

### Example
<p align="center">
<img src="/images/example_nlp.PNG" width="500" >
<br>  <b>Figure:</b> Examples of adversarial samples crafted from Twitter and IMDB dataset using (i)TextFool and
(ii) proposed method.
</p>

## Face Recognition
[Accessorize to a Crime: Real and Stealthy Attacks on State-of-the-Art Face Recognition](https://www.cs.cmu.edu/~sbhagava/papers/face-rec-ccs16.pdf)
Mahmood Sharif, Sruti Bhagavatula, Lujo Bauer, Michael K. Reiter

Face recognition and face detection algorithms are extensively used in surveillance and access control applications. This paper focuses on fooling the state-of-art machine learning algorithms that are practically deployed for these tasks. More concretely, the paper carries out two type of attacks, namely, dodging and impersonation. In dodging, the attacker tries to conceal his/her identity, whereas, in impersonation, the attacker tries to trick the algorithm into recognizing him/her as a different target individual. The authors realize these attacks by printing a wearable glass which, upon wearing, allows the attacker to successfully launch the attacks. The glasses used in the attack are shown below.

<p align="center">
<img src="/images/class5/glasses.png" width="500" >
<br> <b>Figure:</b> Glass frames used for the attacks
</p>

[Parikh et al.](https://www.robots.ox.ac.uk/~vgg/publications/2015/Parkhi15/parkhi15.pdf) proposed the state-of-art 39 layer deep neural network trained on 2,622 celebrities which achieved an accuracy of 98.95% for the task of face detection. The authors of this paper use this model to launch their attacks.

The objective function of the deep neural network's softmax layer is given as below:
<p align="center">
<img src="/images/class5/objective.png" width="500" >
<br>
</p>

For impersonation, the objective is to add the minimum amount of noise 'r' in the input image 'x' to convert the class lable to the target label 'c_t', as given below:
<p align="center">
<img src="/images/class5/impersonation_obj.png" width="450" >
<br>
</p>

For dodging, the objective is to add minimum amount of noise 'r' in the input image 'x' to deviate from the correct class label 'c_x'.
<p align="center">
<img src="/images/class5/dodging_obj.png" width="500" >
<br>
</p>

The noises are then carefully added to the 3D printed glasses.

### Experiments

The paper launches successful dodging and impersonation attacks. Figure below shows an example of impersonation where a female celebrity (left) is classified as a male celebrity (right) by the algorithm when the glass frame is used (center).
<p align="center">
<img src="/images/class5/example1.png" width="500" >
<br> <b>Figure:</b> Successful impersonation using the glasses
</p>

Figure below shows an example of dodging the face detection algorithm with minor changes to the image that donot affect a normal human judgement. Left image is the original image and the center and right images are the perturbed images where the algorithm does not detect the faces.
<p align="center">
<img src="/images/class5/example2.png" width="500" >
<br> <b>Figure:</b> Dodging the face detection
</p>

Further results show 100% dodging success rate and very high impersonation success rate.

### Summary
While the authors perform white-box attacks on the state-of-the-art face detection and face recognition algorithms, they also discuss about successfully carrying out the black-box attacks.


## Conclusion
