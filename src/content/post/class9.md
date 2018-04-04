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

## Machine Learning on Malware is Hard

## State of the Art

Anti-malware defenders today attempt to passively analyze files of a specific type across the web and flag potential malware. These models are often trained offline. The counterparts to these software work much more actively to produce mis-classified malware. These attackers work against black box or often black-world (no response) classifiers.

Malware behavior preservation is difficult due to the nature of code; most changes risk breaking the code entirely. This means that random mutation processes need to be heavily limited to preserve the behavior of the code.

Mutation processes are often either trivial or not done at all. Very few papers allow mutations and verify the process afterward, as it requires dynamic analysis in a sandbox.

Malware development in search of evasive samples is done using genetic algorithms mostly using greedy searches. EvadeML is one such development process to create evasive PDF malwares targeting Hidost and PDFrate. The software has a 100% success rate within 20 generations and works by mutating the raw PDFs.

There's a slight disconnect between security researchers and pure ML researchers, particularly those that are doing research into deep learning. The deep learning architectures that are being researched for malware deterction are multi-layer perception on preselected features and convolutional networks, but there are very few recurrent models. 

Detection softwares are working to identify dynamic features more specifically in malware, as any static features of the code aren't being triggered at runtime. This shift minimizes the attackers' ability to mutate their code while maintaining functionality. This does, however, enable 'time-bomb' style malware, where certain malicious features are static until a certain datetime. 

## Future of the Field

## Conclusion

We've explored the current state of adversarial malware detection including Hidost. From simple to metamorphic viruses, malware has evolved into increasingly sophisticated threats. The field of adversarial malware detection has been evolving as well, as static and dynamic methods are developed that are increasingly effective at detecting malicious code. As the cutting edge of malware detection moves forward, we will see new fronts open up in the contest between malicious files and the algorithms that detect them.

— Team Nematode: \\
Bargav Jayaraman, Guy "Jack" Verrier, Joshua Holtzman, Max Naylor, Nan Yang, Tanmoy Sen

## References

[[1]](https://arxiv.org/ftp/arxiv/papers/1104/1104.1070.pdf) Babak Bashari Rad, Maslin Masrom, and Suhaimi Ibrahim, "Evolution of Computer Virus Concealment and Anti-Virus Techniques: A Short Survey." IJCSI International Journal of Computer Science Issues, Vol. 8, Issue 1. January 2011

[[2]](https://arxiv.org/ftp/arxiv/papers/1104/1104.1070.pdf) Babak Bashari Rad, Maslin Masrom, and Suhaimi Ibrahim, "Camouflage in Malware: from Encryption to Metamorphism." IJCSNS International Journal of Computer Science and Network Security, VOL.12 No.8. August 2012.

[[3]](https://pdfs.semanticscholar.org/e701/d190583135b841252886742bf15736015f3c.pdf) Konrad Rieck, Thorsten Holz, Carsten Willems, Patrick Düssel, Pavel Laskov, "Learning and Classification of Malware Behavior." 15th USENIX Security Symposium. 23 April 2008.

[[4]](https://arxiv.org/abs/1708.08327v2) Liang Tong, Bo Li, Chen Hajaj, Chaowei Xiao, Yevgeniy Vorobeychik, "Hardening Classifiers against Evasion: the Good, the Bad, and the Ugly." arXiv:1708.08327. August 2017.

[[5]](http://ieeexplore.ieee.org/document/5504793/) R. Sommer and V. Paxson, "Outside the Closed World: On Using Machine Learning for Network Intrusion Detection." 2010 IEEE Symposium on Security and Privacy, Oakland, CA, USA, 2010, pp. 305-316. May 2010.