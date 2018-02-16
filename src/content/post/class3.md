+++
date = "15 Feb 2018"
draft = false
title = "Class 3: Privacy in Machine Learning"
author = "Team Gibbon"
slug = "class3"
+++

introduction
## Towards Evaluating the Robustness of Neural Networks
> Carlini, Nicholas, and David Wagner. _Towards evaluating the robustness of neural networks_. IEEE Symposium on Security and Privacy ("Oakland") 2017. [[PDF](https://arxiv.org/pdf/1608.04644.pdf)]

[Carlini et al.](https://arxiv.org/pdf/1608.04644.pdf) proposed an optimization based attack to break the distillation defense mechanism developed by Papernot et al and other undistilled networks. This poses severe security problems to machine learning classifiers as the current defense strategy becomes vulnerable to this new type of strong white-box attacks. The attack is based on optimization approach, where the problem is formulated as 

---
$$ \text{minimize}~\mathcal{D}(x,x+\delta) + cf(x+\delta)\\\\ \text{such that:}~x+ \delta \in [0,1]^{n} $$
---

where \\(f(x+\delta) \leq 0\\) iff the classification result of \\((x+\delta)\\) is in class \\(t\\) (i.e. \\(C(x+\delta) = t)\\). \\(c\\) is a coefficient controling the relative importance of misclassification and norm minimization. Intuitive understanding of the optimization problem above is, we want a smaller perturbation (unobservable by human) by minimizing the norm \\(\|\|\delta\|\|\\) and causing misclassification by minimizing the function \\(f(x+\delta)\\). The constraint of \\(x+\delta \in [0,1]^{n}\\) is because this attack is conducted in image domain and has to generate valid images whose pixel value is in \\([0,1]\\) range. In order to further avoid the constraint, the author apllies \\(tanh\\) function to change the variables. Specifically, \\(\delta_i = \frac{1}{2}(tanh(w_i)+1)-x_i \\) and we directly optimize the unconstrained variable \\(w_i \\) instead of \\(\delta_i\\). [ADAM](https://arxiv.org/pdf/1412.6980.pdf) is deployed to solve the above unconstrained optimization problem. A special note is the author implemented different \\(p\\)-norms \\((p = 0, 2, \infty)\\) under the optimization framework described above. 


The evaluation part is compared to other existing white-box attack methods. Specifically, [Fast Gradient Sign (FGS)](https://arxiv.org/pdf/1412.6572.pdf), [Iterative Fast Gradient Sign (IFGS)](https://arxiv.org/pdf/1607.02533.pdf) methods are suitable for \\(L_\infty \\)-norm, [Deepfool](https://arxiv.org/pdf/1511.04599.pdf) method is suitable for \\(L_2\\)-norm attack, [JSMA](https://arxiv.org/pdf/1511.07528.pdf) method is suitable for \\(L_{0}\\)-norm attack. The target model for the listed attacks are [distillation based network](https://arxiv.org/pdf/1511.04508.pdf) (defensitive strategy proposed for defending against FGS and IFGS methods) and normally trained neural networks. The final evaluation results demonstrate that the attack proposed in this work is far stronger than existing attack method in that it achieves highest attack success rate (this method achieves \\(100\%\\) success rate while other methods don't) and lowest perturbation magnitude. Some sample images generated from this attack are shown below. This attack is also now broadly accepted as a standard baseline for evaluating newly proposed defense mechanisms.

<p align="center">
<img src="/images/class3_img1.png" width="600" >
</p>

   <div class="caption">
Source: [_Towards evaluating the robustness of neural networks_]((https://arxiv.org/pdf/1608.04644.pdf)) [1]
   </div>
   
## Section title

> Authors. _Title_. IEEE Symposium on Security and Privacy ("Oakland") 2017. [[PDF](https://www.cs.cornell.edu/~shmat/shmat_oak17.pdf)]

[Shokri et al.](https://www.cs.cornell.edu/~shmat/shmat_oak17.pdf) attempt to ...

## Conclusion

...

— Team Nematode: \\
Austin Chen, Ethan Lowman, Aditi Narvekar, Jin, Suya

## References

[[1]](https://arxiv.org/pdf/1608.04644.pdf) Carlini N, Wagner D. Towards evaluating the robustness of neural networks. InSecurity and Privacy (SP), 2017 IEEE Symposium on 2017 May 22 (pp. 39-57). IEEE.\

[[2]](https://arxiv.org/pdf/1412.6980.pdf) Kingma DP, Ba J. Adam: A method for stochastic optimization. arXiv preprint arXiv:1412.6980. 2014 Dec 22.

[[3]](https://arxiv.org/pdf/1412.6572.pdf) Goodfellow IJ, Shlens J, Szegedy C. Explaining and harnessing adversarial examples. arXiv preprint arXiv:1412.6572. 2014 Dec 20.

[[4]](https://arxiv.org/pdf/1607.02533.pdf) Kurakin A, Goodfellow I, Bengio S. Adversarial examples in the physical world. arXiv preprint arXiv:1607.02533. 2016 Jul 8.

[[5]](https://arxiv.org/pdf/1511.04599.pdf) Moosavi Dezfooli SM, Fawzi A, Frossard P. Deepfool: a simple and accurate method to fool deep neural networks. InProceedings of 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR) 2016 (No. EPFL-CONF-218057).

[[6]](https://arxiv.org/pdf/1511.07528.pdf) Papernot N, McDaniel P, Jha S, Fredrikson M, Celik ZB, Swami A. The limitations of deep learning in adversarial settings. InSecurity and Privacy (EuroS&P), 2016 IEEE European Symposium on 2016 Mar 21 (pp. 372-387). IEEE.

[[7]](https://arxiv.org/pdf/1511.04508.pdf) Papernot N, McDaniel P, Wu X, Jha S, Swami A. Distillation as a defense to adversarial perturbations against deep neural networks. InSecurity and Privacy (SP), 2016 IEEE Symposium on 2016 May 22 (pp. 582-597). IEEE.
