+++
date = "30 Mar 2018"
draft = true
title = "Class 9: Adversarial Malware Detection"
author = "Team Nematode"
slug = "class9"
+++


### Introduction

### Malware History - Machine Learning Origins

### PDF Malware Classifiers

### Hidost: A Static Machine-Learning-Based Detector of Malicious Files

### Machine Learning on Malware is Hard

### State of the Art

Anti-malware defenders today attempt to passively analyze files of a specific type across the web and flag potential malware. These models are often trained offline. The counterparts to these software work much more actively to produce mis-classified malware. These attackers work against black box or often black-world (no response) classifiers.

Malware behavior preservation is difficult due to the nature of code; most changes risk breaking the code entirely. This means that random mutation processes need to be heavily limited to preserve the behavior of the code.

Mutation processes are often either trivial or not done at all. Very few papers allow mutations and verify the process afterward, as it requires dynamic analysis in a sandbox.

Malware development in search of evasive samples is done using genetic algorithms mostly using greedy searches. EvadeML is one such development process to create evasive PDF malwares targeting Hidost and PDFrate. The software has a 100% success rate within 20 generations and works by mutating the raw PDFs.

There's a slight disconnect between security researchers and pure ML researchers, particularly those that are doing research into deep learning. The deep learning architectures that are being researched for malware deterction are multi-layer perception on preselected features and convolutional networks, but there are very few recurrent models. 

Detection softwares are working to identify dynamic features more specifically in malware, as any static features of the code aren't being triggered at runtime. This shift minimizes the attackers' ability to mutate their code while maintaining functionality. This does, however, enable 'time-bomb' style malware, where certain malicious features are static until a certain datetime. 

### Future of the Field

### Conclusion

We've explored the current state of adversarial malware detection including Hidost. From simple to metamorphic viruses, malware has evolved into increasingly sophisticated threats. The field of adversarial malware detection has been evolving as well, as static and dynamic methods are developed that are increasingly effective at detecting malicious code. As the cutting edge of malware detection moves forward, we will see new fronts open up in the contest between malicious files and the algorithms that detect them.

### Sources
> Liang Tong, Bo Li, Chen Hajaj, Chaowei Xiao, Yevgeniy Vorobeychik. 2017. Hardening Classifiers against Evasion: the Good, the Bad, and the Ugly.  arXiv:1708.08327. Retrieved from https://arxiv.org/abs/1708.08327v2 [[PDF]] (https://arxiv.org/abs/1708.08327v2)

> R. Sommer and V. Paxson, "Outside the Closed World: On Using Machine Learning for Network Intrusion Detection," 2010 IEEE Symposium on Security and Privacy, Oakland, CA, USA, 2010, pp. 305-316.
https://doi.org/10.1109/SP.2010.25 [[PDF]] (http://ieeexplore.ieee.org/document/5504793/)
