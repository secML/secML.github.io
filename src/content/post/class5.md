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
