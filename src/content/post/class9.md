+++
date = "30 Mar 2018"
draft = true
title = "Class 9: Adversarial Malware Detection"
author = "Team Nematode"
slug = "class9"
+++
## Evolution of the Malware Arms Race

> Babak Bashari Rad, Maslin Masrom, and Suhaimi Ibrahim. _Evolution of Computer Virus Concealment and Anti-Virus Techniques: A Short Survey_. University Technology Malaysia. 6 April 2011. [[PDF](https://arxiv.org/ftp/arxiv/papers/1104/1104.1070.pdf)]

Since the first appearances of early malware, advances in both combating and creating viruses have analogously mirrored the general patterns of the medical battle against evolving biological virus outbreaks: that is, as anti-virus software continually develops innovative techniques for detecting existing viruses, virus writers seek out new methods to cheat those detection systems. To understand the present state of the field and the role of machine learning in combating malware, let's look at a brief history of how both the attacks and defenses have evolved in the malware domain.

#### Encrypted Viruses

At its most basic, a virus is simply a program that is designed to [alter the way a computer operates and spread from one computer to another](https://us.norton.com/internetsecurity-malware-what-is-a-computer-virus.html). However, to be truly effective, a virus must also conceal itself so as to replicate and infect another computer. Thus, the most primitive approach to covering the operation of the virus code, first appearing in 1988, uses encryption to change the virus body binary codes with encryption algorithms in order to make it more difficult to analyze and detect.

Encrypted viruses are typically made up of two parts: the encrypted body of the virus itself, and a small (unencrypted) segment of decryption code. When the infected program runs, the decryption loop executes, which first decrypts the encrypted virus code, and then moves the program control to the body of the virus code.

![](/images/class9/structureofencryptedvirus.png "Structure of an encrypted virus")
   <div class="caption">
Source: [_Camouflage in Malware: From Encryption to Metamorphism_]((http://paper.ijcsns.org/07_book/201208/20120813.pdf)) [2]
   </div>

#### -Morphic Viruses

Oligomorphic, polymorphic, metamorphic
Static analysis fails
Emulation, build profiles
Mimicry attacks
	Intrusion Detection Systems:
		signature-based (bytes, sequence of system calls)
		anomaly detection (database of normal behaviour)
	Mimicry
		malicious under the guise of the model

## Machine Learning in Malware

> Konrad Rieck, Thorsten Holz, Carsten Willems, Patrick Düssel, Pavel Laskov. _Learning and Classification of Malware Behavior_. 15th USENIX Security Symposium. 23 April 2008. [[PDF](https://pdfs.semanticscholar.org/e701/d190583135b841252886742bf15736015f3c.pdf)]

Attacks do not share static code
	Malware varies code
	Mimicry varies system calls
But they do share behaviour
Learn this behaviour

## PDF Malware Classifiers

## Hidost: A Static Machine-Learning-Based Detector of Malicious Files

### Machine Learning on Malware is Hard
While machine learning algorithms have been successfully applied in various vision and natural language related tasks, they have not been fully successful in malware detection. The reasons are mainly three-fold:

#### High Cost of Error
In the case of malware detection, when measuring the performance of a classifier, both false positive rate and false negative rate are important factors. While the false positive rate means a benign software is classified as a malware, in the false negative case a malware is classified as benign. Both the events are undesirable for malware detection. Since, false positive requires excessive spending of a human analyst's time which could be expensive and time consuming. On the other hand, false negative means a malware is left undetected, which can have serious security consequences. A typical machine learning model trained for the malware detection task tends to have non-zero false positive rates and false negative rates.

#### Semantic Gap
There is a disconnect between the prediction result of a machine learning model and the action to be taken based on the result. For instance, if a model predicts a malware with 65% confidence, what action should be taken? Should the target software be removed without intimation to the end user? Should the user be alerted for a manual inspection? In such a scenario, it is hard to interpret the results and take a meaningful action.

#### Difficulty with Evaluation
One of the major difficulty in the evaluation of malware detection tool is the availability of datasets. Most of the public datasets like Contagio or Drebin are small or outdated, whereas the malwares keep updating aggressively. It is difficult to create new rich malware datasets due to data privacy concerns and the labelling of malwares in the wild. This poses problem for training and evaluating the machine learning models.

Another difficulty is the evasiveness of the malwares. Malwares have become dynamic enough to evade the malware classifiers. Given a white-box access to the classifier, malware can perform adversarial training like gradient-based method to evade detection. Even with black-box access, the malware can perform mimicry attack by appending features of benign samples. However, this may or may not break the malware's functionality and hence it is hard to generate useful samples. One variant is to use [reverse-mimicry attack](https://dl.acm.org/citation.cfm?id=2484327) by embedding malicious features into benign file to generate malicious samples. Recent literature has also shown evasion by [genetic mutation](http://people.cs.vt.edu/~gangwang/class/cs6604/papers/ndss16.pdf) and [hill climbing method](https://acmccs.github.io/papers/p119-dangA.pdf), where the malware can dynamically adapt to evade detection
## State of the Art

Anti-malware defenders today attempt to passively analyze files of a specific type across the web and flag potential malware. These models are often trained offline. The counterparts to these software work much more actively to produce mis-classified malware. These attackers work against black box or often black-world (no response) classifiers.

Malware behavior preservation is difficult due to the nature of code; most changes risk breaking the code entirely. This means that random mutation processes need to be heavily limited to preserve the behavior of the code.

Mutation processes are often either trivial or not done at all. Very few papers allow mutations and verify the process afterward, as it requires dynamic analysis in a sandbox.

Malware development in search of evasive samples is done using genetic algorithms mostly using greedy searches. EvadeML is one such development process to create evasive PDF malwares targeting Hidost and PDFrate. The software has a 100% success rate within 20 generations and works by mutating the raw PDFs.

There's a slight disconnect between security researchers and pure ML researchers, particularly those that are doing research into deep learning. The deep learning architectures that are being researched for malware deterction are multi-layer perception on preselected features and convolutional networks, but there are very few recurrent models. 

Detection softwares are working to identify dynamic features more specifically in malware, as any static features of the code aren't being triggered at runtime. This shift minimizes the attackers' ability to mutate their code while maintaining functionality. This does, however, enable 'time-bomb' style malware, where certain malicious features are static until a certain datetime. 

## Future of the Field

### Conclusion

We've explored the current state of adversarial malware detection including Hidost. From simple to metamorphic viruses, malware has evolved into increasingly sophisticated threats. The field of adversarial malware detection has been evolving as well, as methods are developed that are increasingly effective at detecting malicious code. As the cutting edge of malware detection moves forward, we will see new fronts open up in the contest between malicious files and the algorithms that detect them.
— Team Nematode: \\
Bargav Jayaraman, Guy "Jack" Verrier, Joshua Holtzman, Max Naylor, Nan Yang, Tanmoy Sen

## References

[[1]](https://arxiv.org/ftp/arxiv/papers/1104/1104.1070.pdf) Babak Bashari Rad, Maslin Masrom, and Suhaimi Ibrahim, "Evolution of Computer Virus Concealment and Anti-Virus Techniques: A Short Survey." IJCSI International Journal of Computer Science Issues, Vol. 8, Issue 1. January 2011

[[2]](https://arxiv.org/ftp/arxiv/papers/1104/1104.1070.pdf) Babak Bashari Rad, Maslin Masrom, and Suhaimi Ibrahim, "Camouflage in Malware: from Encryption to Metamorphism." IJCSNS International Journal of Computer Science and Network Security, VOL.12 No.8. August 2012.

[[3]](https://pdfs.semanticscholar.org/e701/d190583135b841252886742bf15736015f3c.pdf) Konrad Rieck, Thorsten Holz, Carsten Willems, Patrick Düssel, Pavel Laskov, "Learning and Classification of Malware Behavior." 15th USENIX Security Symposium. 23 April 2008.

[[4]](https://arxiv.org/abs/1708.08327v2) Liang Tong, Bo Li, Chen Hajaj, Chaowei Xiao, Yevgeniy Vorobeychik, "Hardening Classifiers against Evasion: the Good, the Bad, and the Ugly." arXiv:1708.08327. August 2017.

[[5]](http://ieeexplore.ieee.org/document/5504793/) R. Sommer and V. Paxson, "Outside the Closed World: On Using Machine Learning for Network Intrusion Detection." 2010 IEEE Symposium on Security and Privacy, Oakland, CA, USA, 2010, pp. 305-316. May 2010.