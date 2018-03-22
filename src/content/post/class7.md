+++
date = "20 Mar 2018"
draft = false
title = "Class 7: Biases in ML & Discriminatory Advertising"
author = "Team Gibbon"
slug = "class7"
+++

## Motivation
...
##
## Investigating Ad Transparency Mechanisms in Social Media: A Case Study of Facebook’s Explanations
> Andreou, Athanasios, et al. "Investigating ad transparency mechanisms in social media: A case study of Facebook’s explanations." NDSS, 2018.[[PDF]](http://wp.internetsociety.org/ndss/wp-content/uploads/sites/25/2018/02/ndss2018_10-1_Andreou_paper.pdf)

### What is ad transparency mechanisms in social media？ 

Transparency mechanisms are solutions for many privacy complaints from users and policy makers. Users have little understanding of what data the advertising platforms have about them and why they are shown particular ads. The transparency mechanisms in Facebook are "Why am I seeing this?" botton that provides  users with an explanation of why they were shown a particular ad (ad explanations), and an Ad Preferences Page that provides users with a list of attributes Facebook has inferred about them and how (data explanations). In Twitter, there are similar transparency mechanisms. You can also see "Why am I seeing this?" botton that provides  users with an explanation of why they were shown a particular ad (ad explanations) in Twitter.

### What did this paper do?

This paper mainly did an investigation and analysis of ad explanations, or why users see a certain ad and an investigation of data explanations, or how data is inferred about a user, which is strongly related to the ad transparency mechanisms in Facebook. This paper first introduced how ad platform in facebook works and then evaluate the two transparency mechanisms we introduced before with some properties.

### The Ad platform in Facebook

There are three main processes in facebook ad platform: a) the data inference process; b) the audience selection process; c) the user-ad matching process.

(a) The data inference process is the process that allows the advertising platform to learn the users’ attributes. It has three parts: (1) the raw user data (the inputs), containing the information the
advertising platform collects about a user either online or offline; (2) the data inference algorithm (the mapping function between inputs and outputs), covering the algorithm the advertising platform uses to translate input user data to targeting attributes;(3) the resulting targeting attributes (the outputs) of each user that advertisers can specify to select different groups of users.

![](/images/class7/a.png)

(b) The audience selection process is the interface that allows advertisers to express who should receive their ads. Advertisers create audiences by specifying the set of targeting attributes the audience needs to satisfy. Later, to launch an ad campaign, advertisers also need to specify a bid price and an optimization criterion.
![](/images/class7/b.png)

(c) The user-ad matching process takes place whenever someone is eligible to see an ad. It examines all the ad campaigns placed by different advertisers in a particular time interval, their bids, and runs an auction to determine which ads are selected.

![](/images/class7/c.png)


### Ad Explanations and the experiments on this transparency mechanism

![](/images/class7/WhySeeingThis.png)

As you can see in the picture, There're attritubutes and potentional attritubutes here. 
This paper used 5 different properties to evaluate the performance of Ad explanations:</br>
1. Correctness: Every attribute listed was used by the advertiser</br>
2. Personalization: The attributes listed are unique to the individual</br>
3. Completeness: If all relevant attributes are included in the explanation</br>
4. Consistency: Users with the same attributes see the same explanations</br>
5. Determinism: A user would see the same explanation for ads based on the same target attributes</br>

The experiment this paper did to evaluate the ad explanations is using Chrome browser extension to record ads and explanations. The experiment had 35 users' data across 5 months. This experiment also did to evaluate the data explanation. This paper made simple statistics for the explanation(See the following figure). And then, it shows the results of different properties on this experiment.

![](/images/class7/stat.png)

### Data Explanations and the experiments on this transparency mechanism

The data explanations is applied in "Your interests" part as shown in the following picture. 

![](/images/class7/like.png)

The properties here are not the same as those used in the Ad explanation part. There are 3 new properties.
1. Specificity: A data explanation is precise if it shows the precise activities that were used to infer an attribute about a user.
2. Snapshot completeness: A data explanation is snapshot complete if the explanation shows all the inferred attributes about the user that Facebook makes available.
3. Temporal completeness: a temporally complete explanation is one where the platform shows all inferred attributes over a specified period of time.

The results of different properties on this experiment are showed in the following picture.


![](/images/class7/DataRes.png)
### Conclusion
While the Ad Preferences Page does bring some transparency to the different attributes users can be targeted with,
the provided explanations are incomplete and often vague. Facebook does not provide information about data-brokerprovided attributes in its data explanations or in its ad explanations.


## Potential for Discrimination in Online Targeted Advertising

> Till Speicher, Muhammad Ali, Giridhari Venkatadri, Filipe Nunes Ribeiro, George Arvanitakis, Fabr&iacute;cio Benevenuto, Krishna P. Gummadi, Patrick Loiseau, Alan Mislove. _Potential for Discrimination in Online Targeted Advertising_. Proceedings of the 1st Conference on Fairness, Accountability and Transparency, PMLR 81:5-19, 2018. [[PDF]](http://proceedings.mlr.press/v81/speicher18a/speicher18a.pdf)

Recently, online targeted advertising platforms like Facebook have received intense criticism for allowing advertisers to discriminate against users belonging to protected groups. 

Facebook, in particular, has been handed a civil rights lawsuit for allowing advertisers to target ads using an attribute called "ethnic affinity." Facebook has clarified that "ethnic affinity" does not represent ethnicity, but rather represents a user’s affinity for content related to different ethnic communities. Facebook has agreed to rename the attribute to "multicultural affinity" and to disallow using this attribute to target ads related to housing, employment, and financial services. However, Facebook offers many different ways to describe a set of targeted users, so it’s not adequate to disallow targeting on certain attributes. In this paper, the authors develop a framework for quantifying ad discrimination and show the potential for discriminatory advertising using the three different targeting methods on Facebooks advertising platform: personally identifiable information (PII)-based targeting, attribute-based targeting, and look-alike audience targeting.

### Quantifying Ad Discrimination

The authors identify three potential approaches to quantifying discrimination.

**Based on advertiser’s intent:** The authors reject this approach since it is hard to measure and it does not capture unintentionally discriminatory ads.

**Based on ad targeting process:** This category includes existing anti-discrimination measures, like disallowing use of sensitive attributes when defining a target population. The authors reject this approach since it breaks down when there exist several methods of targeting a population.

**Based on targeted audience (outcomes):** This approach takes into account only which users are targeted, not how they are targeted. The authors use this approach to quantify ad discrimination since outcome-based analyses generalize independent of targeting methods.

The authors formalize outcome-based discrimination as follows:

Let \\(\mathbf{D} = (u\_i)\_{i=1,\ldots,n}\\) be a database of user records \\(u_i\\).<br />
Let \\(u_i \in \mathbb{B}^m\\) be a vector of \\(m\\) boolean attributes.<br />
Let \\(s \in \{1, \ldots, m\}\\) be the sensitive attribute we are considering.<br />
Let \\(u_s\\) be the value of sensitive attribute \\(s\\) for user \\(u\\).<br />
Let \\(\mathbf{S} = \{u \in \mathbf{D} | u_s = 1\}\\) be the set of all users having sensitive attribute \\(s\\).

The authors define a metric for how discriminatory an advertiser’s targeting is, inspired by the disparate impact measure used for recruiting candidates from a pool.

Let \\(\mathbf{TA}\\) (target audience) be the set of users selected by the targeting process.<br />
Let \\(\mathbf{RA}\\) (relevant audience) be the set of all users in the database \\(\mathbf{D}\\) who would find the ad useful and interesting.<br />
Define the representation ratio measure to capture how much more likely a user is to be targeted when having the sensitive attribute than if the user did not have the attribute:

$$\text{rep\_ratio}_s(\mathbf{TA}, \mathbf{RA}) = \dfrac{|\mathbf{TA} \cap \mathbf{RA}_s|/|\mathbf{RA}_s|}{|\mathbf{TA} \cap \mathbf{RA}\_\{\neg s\}|/|\mathbf{RA}\_\{\neg s\}|}$$

where \\(\mathbf{RA}\_s = \{u \in \mathbf{RA} | u\_s = 1 \}\\) is the subset of the relevant audience with the sensitive attribute and \\(\mathbf{RA}\_{\neg s} = \{u \in \mathbf{RA} | u\_s = 0\}\\) is the complementary subset of the relevant audience without the sensitive attribute

Define the disparity in targeting measure to capture both over- and under-representation of a sensitive attribute in a target audience:

$$\text{disparity}\_s(\mathbf{TA}, \mathbf{RA}) = \max\left(\text{rep\_ratio}\_s(\mathbf{TA}, \mathbf{RA}), \dfrac{1}{\text{rep\_ratio}\_s(\mathbf{TA}, \mathbf{RA})}\right)$$

Disparity must be computed based on the relevant audience \\(\mathbf{RA}\\) because \\(\mathbf{RA}\\) may have a different distribution of the sensitive attribute than the whole database \\(\mathbf{D}\\). The authors assume that sensitive attributes considered have the same distributions in the relevant audience as the global population, and therefore high disparity in targeting is evidence of discrimination. Following the "80%" disparate impact rule, a reasonable disparity threshold for a group to be over- or under-represented may be \\(\max(0.8, 1/0.8) = 1.25\\). 

The recall of an ad quantifies how many of the relevant users with the sensitive attribute the ad targets or excludes:

$$\text{recall}(\mathbf{TA}, \mathbf{RA}') = \dfrac{|\mathbf{TA} \cap \mathbf{RA}'|}{|\mathbf{RA}'|}$$

where \\(\mathbf{RA}'\\) is one of \\(\mathbf{RA}\_s\\) or \\(\mathbf{RA}\_{\neg s}\\) depending on whether we’re considering the inclusion or exclusion of \\(\mathbf{S}\\).

### PII-Based Targeting

PII-based targeting on the Facebook advertising platform allows advertisers to select a target audience using unique identifiers, like phone numbers, email addresses, and combinations of name with other attributes (e.g. birthday or zip code). The authors show that public data sources, such as voter records and criminal history records, contain sufficient PII to construct a discriminatory target audience for a sensitive attribute without explicitly targeting that attribute.

The authors constructed datasets to show that they could implicitly target gender, race, and age using North Carolina voter records. Each of these attributes is listed in voting records, and the remaining fields together uniquely identify the voter (i.e. last name, first name, city, state, zip code, phone number, and country). The authors uploaded datasets targeting values of each attribute and recorded Facebook’s estimated audience size.

![](/images/class7/table1.jpg)

The Voter Records column shows the distribution of attribute values in the voter records data set. For a given attribute, the Facebook Users column shows how many of the 10k people in the dataset constructed for that attribute are actually targetable on Facebook (as reported by the Facebook advertising platform). The final column shows the portion of the targetable users who actually match the targeted attribute, found by restricting the target audience using Facebook’s records of the sensitive attribute. High targetable percentages values show that the voter records overlap significantly with the voter records data set. High validation percentages show that the auxiliary PII was highly accurate at describing particular users with the targeted attribute. Note that there are some low validation percentages, which the authors attribute to Facebook’s inaccurate or incomplete records of some data (for example, they do not know race, only "multicultural affinity"). 

### Attribute-Based Targeting

Attribute-based targeting allows advertisers to select a target audience by specifying that targeted users should have some attribute or combination of attributes. The authors group these attributes into two categories: curated attributes and free-form attributes. Curated attributes are well-defined binary attributes spanning demographics, behaviors, and interests — Facebook tracks a list of over 1,100 of these. Free-form attributes describe users inferred interest in entities such as websites and apps as well as topics such as food preferences or niche interests. The authors estimate that there are at least hundreds of thousands of free-form attributes.

The authors demonstrate that many curated attributes are correlated with sensitive attributes like race, and can therefore be used for discriminatory audience creation. The following table shows experimental results obtained by uploading sets of voter records filtered to contain only a single race and measuring Facebook’s reported size of the subaudiences for each curated attribute. The figures in parentheses are the recall and representation ratio for a population from North Carolina. The top three most inclusive and exclusive attributes per ethnicity are listed. Note the high representation ratios for the "Most inclusive" column and the low representation ratios for the "Most exclusive" column.

![](/images/class7/table2.jpg)

The authors similarly demonstrated that free-form attributes could used in a discriminatory manner. For example, targeting a vulnerable audience could be made possible by targeting the free-form attributes "Addicted," "REHAB," "AA," or "Support group." The authors also showed how Facebook’s attribute suggestions feature could be used to discover new highly-discriminatory free-form attributes. For example, starting a search with "Fox" (37% conservative audience on Facebook) and following a chain of suggestions leads to "The Sean Hannity Show" (95% conservative audience on Facebook).

### Look-Alike Audience Targeting

Look-alike audience targeting allows advertisers to generate a new target audience that looks similar to an existing set of users (the fans of one of their Facebook pages or an uploaded PII data set). The authors show that this feature can be used to scale a biased audience to a much larger population. Experimental results suggest that Facebook attempts to determine the attributes that distinguish the base target audience from the general population and propagates these biases to the look-alike audience. The authors show that this bias propagation can amplify both intentionally created and unintentionally overlooked biases in source audiences.

## Algorithmic Transparency via Quantitative Input Influence

> Anupam Datta, Shayak Sen, Yair Zick. _Algorithmic Transparency via Quantitative Input Influence: Theory and Experiments with Learning Systems_. 2016 IEEE Symposium on Security and Privacy (SP), 2016. [[PDF]](https://www.andrew.cmu.edu/user/danupam/datta-sen-zick-oakland16.pdf)

Machine learning systems are increasingly being used to make important societal decisions, in sectors including healthcare, education, and insurance.
For instance, an ML model may help a bank decide if a client is eligible for a loan, and both parties may to know critical details about how the model works.
A rejected client will likely want to know why they were rejected: would they have been accepted if their income was higher?
The answer would be especially important if their reported income was lower than their actual income;
more generally, the client can ensure that their input data contained no errors.

Conversely, the model's user may want to ensure that the model does not discriminate based on sensitive inputs, such as the legally-restricted features of race and gender.
Simply ignoring those features may not be sufficient to prevent discrimination; e.g., ZIP code can be used as a proxy for race.
This paper proposes a method to solve these problems by making the model's behavior more transparent: a quantitative measure of the effect of a particular feature (or set of features) on the model's decision for an individual.
The paper offers several approaches suited for various circumstances, but they all fall under the umbrella of "quantitative input inflence", or QII.

### Unary QII

The simplest quantitative measure presented is unary QII, which measures the influence of one attribute on a quantity of interest \\(Q_\mathcal{A}\\) for some subset of the sample space \\(X\\).
Formally, unary QII is determined as

![](/images/class7/unaryQII.png)

where the first term is the actual expected value of \\(Q_\mathcal{A}\\) for this subset, and the second term is the expected value if the feature \\(i\\) were randomized.

For example, consider the rejected bank client from above.
If they restrict \\(X\\) to only contain their feature vector, and they set \\(Q_\mathcal{A}\\) to output the model's probability of rejection,
then unary QII tells how much any individual feature impacted his loan application.
If the unary QII for a feature is large, changing the value of that feature would likely increase their odds of being accepted;
conversely, changing the value of a feature with low unary QII would make little difference.

The paper presents a concrete example: Mr. X has been classified as a low-income individual, and he would like to know why.
Since only 2.1% of people with income above $10k are classified as low-income, Mr. X suspects racial bias.
In actuality, the transparency report shows that neither his race nor country of origin were significant;
rather, his marital status and education were far more influential in his classification.

![](/images/class7/mrxprofile.png)

![](/images/class7/mrxreport.png)

The sample space \\(X\\) can also be broadened to include an entire class of people.
For instance, suppose \\(X\\) is restricted to include people of just one gender, and \\(Q_\mathcal{A}\\) is set to output the model's probability of acceptance.
Here, unary QII would reveal the influence of a feature \\(i\\) on men and on women.
A disparity between the two measures may then indicate that the model is biased:
specifically, the feature \\(i\\) can be identified as a proxy variable, used by the model to distinguish between men and women (even if gender is omitted as an input feature).

![](/images/class7/unarygraph.png)

However, unary QII is often insufficent to explain a model's behavior on an individual or class of individuals.
This histogram shows the paper's results for their "adult" dataset:
for each individual, the feature that created the highest unary QII was found, and the unary QII value was plotted in the histogram.
Most individuals could not be explained by any particular feature, and most features had little influence by themselves.

### Set and Marginal QII

Thankfully, unary QII can easily be generalized to incorporate multiple features at once.
Set QII is defined as

![](/images/class7/setQII.png)

where \\(S\\) is a set of features (as opposed to a single feature, like \\(i\\) in unary QII).
The paper also defines marginal QII

![](/images/class7/marginalQII.png)

which measures the influence of a feature \\(i\\) after controlling for the features in \\(S\\).
These two quantitative measures have different use cases, but both are more general (and thus more useful) than unary QII.

Marginal QII can measure the influence of a single feature \\(i\\), like unary QII, but only for a specific choice of \\(S\\),
and the amount of influence can vary wildly depending on the choice of \\(S\\).
To account for this, the paper defines the _aggregate influence_ of \\(i\\), which measures the expected influence of \\(i\\) for random choices of \\(S\\).

### Conclusion

The above variants of QII can be used to provide transparency reports, offering insight into how an ML model makes decisions about an individual.
Malicious actors may seek to abuse such a system, carefully crafting their input vector to glean someone else's private information.
However, these QII measures are shown too have low sensitivity, so differential privacy can be added with small amounts of noise.

These QII measures are useful only if the input features have well-defined semantics.
This is not true in domains such as image or speech recognition, yet transparency is still desirable there.
The authors assert that designing transparency mechanisms in these domains is an important future goal.
Nevertheless, these QII measures are remarkably effective on real datasets, both for understanding individual outcomes and for finding biases in ML models.

## Semantics derived automatically from language corpora contain human-like biases

> Aylin Caliskan, Joanna J. Bryson, Arvind Narayanan. _Aylin Caliskan, Joanna J. Bryson, Arvind Narayanan_. Science Magazine, 2017. [[PDF]](http://science.sciencemag.org/content/sci/356/6334/183.full.pdf)

The focus of this paper is how machine learning can learn from the biases and stereotypes in humans. The main contributions of the authors are:

Using word embeddings to extract associations in text
Replicate human bias to reveal prejudice behavior in humans
Show that cultural stereotypes propagate to widely used AI today

### Uncovering Biases in ML

The authors began by replicating inoffensive biases using their original Word-Embedding Association Test (WEAT) method. Word embedding is a representation of words in vector space. WEAT is a test applied to words in AI which represents words as a 300 dimensional vector. The words are then paired by distances between the vectors. Using WEAT, they demonstrated that flowers have pleasant associations and insects have unpleasant associations. Or instruments are more pleasant than weapons. The word embeddings know the properties of flowers or weapons even though they have no experience with them!

After showing that WEAT works, they use this technique to show that machine learning absorbs stereotype biases. In a study by Bertrand and Mullainathan, 5,000 identical resumes were sent out to 1,300 job ads and varied only the names. The European American names were 50% more likely to be offered an opportunity to be interviewed. Based on this study, the authors used WEAT to test the pleasantness associations with the names from Bertrand’s work and found European American names were more pleasant than African American names.

They then turned to studying gender biases. Female names were associated with family as oppose to male names which were associated with career. They also showed woman/girl associated more with arts than math compared to men. These observations were then correlated with data in the labor force. This is show in the figure below:

![](/images/class7/gender_bias.PNG)

The authors then applied another method of their creation called Word-Embedding Factual
Association Test (WEFAT) to show that these embeddings correlate strongly with the occupations women have in the real world. They then used the GloVe method to find similarity between a pair of vectors. Similarity between vectors is related to the probability that the words co-occur with other words similar to each other. GloVe finds this by doing dimensionality reduction to amplify signal in co-occurring probabilities. 

Afterwards, they did a crawl of the internet and got 840 billion words, and each word had a 300 dimension vector derived from counts of other words that occur with it in a 10 word window. WEFAT allowed them to further examine how embeddings capture empirical information. Using this, they were able to predict properties from the given vector.

### Conclusion

So what does their work mean? Their results show there’s a way to reveal unknown implicit associations. They demonstrate that word embeddings encode not only stereotyped bias, but also other knowledge like that flowers are pleasant. These results also explain origins of prejudice in humans. It shows how group identity transmits through language before an institution explains why individuals make prejudiced decisions. There are implications for AI and ML because technology could be perpetuating cultural stereotypes. What if ML responsible for reviewing resumes absorbed cultural stereotypes? It’s important to keep this in mind and be cautious in the future.


—-- Team Gibbon: 
Austin Chen, Jin Ding, Ethan Lowman, Aditi Narvekar, Suya


#### References

[[1]](http://proceedings.mlr.press/v81/speicher18a/speicher18a.pdf) Till Speicher, Muhammad Ali, Giridhari Venkatadri, Filipe Nunes Ribeiro, George Arvanitakis, Fabr&iacute;cio Benevenuto, Krishna P. Gummadi, Patrick Loiseau, Alan Mislove. _Potential for Discrimination in Online Targeted Advertising_. Proceedings of the 1st Conference on Fairness, Accountability and Transparency, PMLR 81:5-19, 2018.

[[2]](https://www.andrew.cmu.edu/user/danupam/datta-sen-zick-oakland16.pdf) Anupam Datta, Shayak Sen, Yair Zick. _Algorithmic Transparency via Quantitative Input Influence: Theory and Experiments with Learning Systems_. 2016 IEEE Symposium on Security and Privacy (SP), 2016.

[[3]](http://science.sciencemag.org/content/sci/356/6334/183.full.pdf) Aylin Caliskan, Joanna J. Bryson, Arvind Narayanan. _Aylin Caliskan, Joanna J. Bryson, Arvind Narayanan_. Science Magazine, 2017.
