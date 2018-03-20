+++
date = "20 Mar 2018"
draft = false
title = "Class 7: Biases in ML & Discriminatory Advertising"
author = "Team Gibbon"
slug = "class7"
+++

## Motivation
...

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

—-- Team Gibbon: 
Austin Chen, Jin Ding, Ethan Lowman, Aditi Narvekar, Suya


#### References

[[1]](http://proceedings.mlr.press/v81/speicher18a/speicher18a.pdf) Till Speicher, Muhammad Ali, Giridhari Venkatadri, Filipe Nunes Ribeiro, George Arvanitakis, Fabr&iacute;cio Benevenuto, Krishna P. Gummadi, Patrick Loiseau, Alan Mislove. _Potential for Discrimination in Online Targeted Advertising_. Proceedings of the 1st Conference on Fairness, Accountability and Transparency, PMLR 81:5-19, 2018.
