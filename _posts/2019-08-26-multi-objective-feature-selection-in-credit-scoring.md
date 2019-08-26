---
layout: post
title:  "Multi-Objective Feature Selection in Credit Scoring"
date:   2019-08-26 09:32:35 +0200
categories: [R, Feature_Engineering, Genetic_Algorithm, XGBoost]
mathjax: true
---

Multi-Objective Feature Selection in Credit Scoring was the title of my Master thesis at Humboldt University of Berlin. The thesis describes a new feature selection approach based on a multi-objective genetic optimization algorithm. An implementation of the [Multi-Objective NSGA-III algorithm for feature selection](https://cran.r-project.org/web/packages/nsga3/index.html) in R programming language was presented as a part of master thesis and published at [CRAN](https://cran.r-project.org/). This article presents a brief overview of the proposed approach as well as key findings.

### Introduction
Credit scoring is aimed at reducing the risk of granting funds to applicants that are likely to default on their payments.

The main idea behind credit scoring is to build a score card, or a system of rules, which enables predicting the probability of an applicant paying back the loan. This system of rules for saying "yay or nay" can be built by finding patterns in credit histories, financial, social, transactional and other data of previous applicants.

With the rise of Big Data phenomenon, a lot of new credit scoring techniques were developed that base their prediction not only credit history, like in traditional approaches, but are also able to incorporate a wide range of alternative data. A clear downside of this tendency is that the models are supplied with big datasets, where some features are potentially not relevant and redundant.

Like in any sports, where proper diet is as important as training itself, we should aim at feeding our models only with clean and relevant data.

Business problems are multi-objective by their nature. As a bank, you want to maximize your profits from issuing credits, but being too risk averse can shrink your market share. Thus, market share optimization can be considered as a second objective. Besides, you need to take into account all the costs of acquiring data etc. 3rd objective - costs minimization and so on. It follows that the applied algorithm for subset selection should be able to take into account multiple objectives simultaneously.

### The Approach

The proposed algorithm uses a [Pareto-based approach](https://en.wikipedia.org/wiki/Pareto_efficiency) for optimizing multiple objectives simultaneously.

The optimization part is based on a [genetic algorithm](https://en.wikipedia.org/wiki/Genetic_algorithm).
The main idea is can be presented as follows:
1. Generate $ n $ random subsets. Together they form a so called *population*
2. Train the model on each of the subsets (*individuals*) in the population and evaluate respective performance.  
3. Rank the individuals based on their performance and take top *m*, $ m < n $
4. Perform crossover operation. Basically this step means that you randomly select a pair of subsets and "make babies", i.e. produce new subsets, that inherit included features from their parents. Usually an individual is represented as a binary string, its length equals the number of features in the dataset. 1 = feature is included. Thus the baby string is formed by inheriting 1 or 0 for each feature from its parents with equal probability.
5. Combine baby set with parents set to produce new *generation*
6. What we want to achieve by repeating steps 1 through 5 is *convergence*, meaning that we expect to discover some pattern in genes, where better performing subsets are more likely to include some certain features and leave out some others.
7. In the end, you obtain a set of best performing subsets.

I have written an adaptation in R of Non-Dominated Sorting Genetic Algorithm - III, presented by K. Deb in 2013. You can find all the details regarding the algorithm in the original [paper](https://www.egr.msu.edu/~kdeb/papers/k2012009.pdf).





$$ 2 + x^2 = 11 $$
