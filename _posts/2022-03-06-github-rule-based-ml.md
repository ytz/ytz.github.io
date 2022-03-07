---
title:  "How GitHub uses ML to scale a rule-based system"
date:   2022-03-06
tags: 
    - GitHub
---

GitHub recently released a [blog post](https://github.blog/2022-02-17-leveraging-machine-learning-find-security-vulnerabilities/) on how they use machine learning to improve their existing code scanning capability, to alert developers to potential security vulnerabilities in their code. Essentially it is using a rule-based engine (CodeQL analysis engine) + ML to boost performance.

This rule-based engine + ML is common combo in the industry. Why? 2 reasons:

1. Target labels are readily available in the rule-based system
2. ML automates the pain point of manually maintaining rules in the rule-based system.

I have worked on this type of problem before (fraud detection), so it is interesting to see how other organization frame this problem, and overcome some challenges associated with this rule-based + ML combo.

## What is a rule-based engine?

A rule-based engine is commonly used in the area of risk (e.g. fraud detection). Basiclally, the engine contains rules that are hand-crafted by domain experts. The rules will then be checked against a stream of data, and raise an alert to the user to take action when the rules are triggered.

Rule-based engine is popular because it is simple and intuitive. However, it has some drawbacks:

1. It can be time-consuming to curate these rules, especially when the risk environment is constantly changing. To ensure it continues to remain effective, one has to constantly add new rules, as well as to modify existing rules to better fit current risk environment. The cost of maintaining a large repository of rules is high.
2. Rule-based engine can produce many false positive alerts. Most rules are essentially a bunch of `if-else` statements. They are easy to create and understand, but may not be precise enough. Combine with a poorly maintained repository of rules, users may end up with a backlog of alerts (mostly false positives) to clear everyday.

## Using ML to improve on rule-based engine

The hope of ML is that it will be able to learn from existing rules curated by domain experts, and generalize them to new risk environment automatically.

So, what are some lessons I took away from the blog post?

## 1. Evaluating performance of ML model

How do we know whether our ML model can detect vulnerabilies that were missed out by current rule-based engine? I thought what GitHub did was quite clever:

* Train ML model based on an older version of the rule-based engine
* Test whether ML model can detect vulnerabilities caught by new version rule-based engine (but missed out by old version)

Of course, the devil is in the details. The final result depends heavily the type of rules which excluded from the test set. Also, can we say for certain that the ML model can also capture vulnerabilites caught by the older version?

Perhaps a more vigorious method is to have multiple test set, each consisting of a different type of vulnerability, and aggregate the results on all test sets.

## 2. Using NLP & deep learning for code snippet

> Once we’ve extracted a rich set of potentially interesting features for each example, we tokenize and sub-tokenize them as is commonly done in NLP applications, with some modifications to capture characteristics specific to code syntax. We generate a vocabulary from the training data and feed lists of indices into the vocabulary into a fairly simple deep learning classifier, with a few layers of feature-by-feature processing followed by concatenation across features and a few layers of combined processing. The output is the probability that the current sample is a vulnerability for each query type.

Treating code as string is a new concept to me. The big advantage is being able to let the nerual network do auto feature engineering. However, I'm not sure whether the results can be interpretable, which is normally a requirement in the field of risk.

## Conclusion

Overall, it is very cool to see how tech companies utilizies their massive datasets, scope out ML problem statements, and evaluate their models.
