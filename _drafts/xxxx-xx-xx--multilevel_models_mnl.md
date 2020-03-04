---
layout: post
title: "Bayesian hierarchical models trump GLM"
cover: cover.jpg
date:   xxxx-xx-xx 
categories: posts
published: false
---

Modelling is rarely an easy task, as a consequence we all try to find shortcuts that simplify the building process. A common option is to take regression (or generalised linear models) as the solution to most problems; but even if regression is a quite powerful tool, it has some important limitations and pitfalls. For example, it isn’t trivial to model different subsets or groups of individuals with a few levels each, optimising accuracy whilst controlling for overfitting and complexity.

Two extreme approaches illustrate this subgroupping issue: the simplest one is to build a single model to explain the behaviour of all groups. These are known as _complete pooling models_; they are clearly unable to be accurate for all subgroups - that is the [regression towards the mean or reversion to mediocrity](https://en.wikipedia.org/wiki/regression_toward_the_mean) issue. The other extreme is building a separate model for each group, these are known as _no pooling models_, which are also suboptimal because each of them ignores the information from the others. This is particularly problematic when the sample size of some of the groups is small or if they interact.

A solution is to build a model that includes all the sub-models for each group and sub-group in a way that each sub-model uses some information from within the group and also from the other groups. The trick is to allow sub-groups to communicate in a way that the use of internal and external information is optimally weighted. These type of models have many names: hierarchical, multilevel, or mixed-effects and in some books they're also referred to as partial pooling models.

These models can be fit to data with traditional (frequentist) methods, however complexity builds up quickly, and it's easy to either end up with a major lack of transparency and/or overfitting. A nice and tidy solution is to use a Bayesian approach as explained [here](http://xcelab.net/rm/statistical-rethinking/) or [here](http://www.stat.columbia.edu/~gelman/arm/), which [allows a well structured model to control complexity](https://stats.stackexchange.com/questions/97233/is-multilevel-modelling-simpler-more-practical-or-more-convenient-using-bayesi). 

Not using a Bayesian approach becomes quickly burdensome and makes it difficult to avoid overfitting without strong regularisation methods which tend to make the model harder to build and interpret. In addition to coping well with complexity, Bayesian modelling is naturally prepared for iterative updating. It provides a structured way of taking new information in and propagating uncertainty. That is highly desirable as it is rare that models are static. In sight of new and more up-to-date information we're likely to be interested in improving the model.

Nevertheless, no approach is perfect, Bayesian methods most obvious downside is that estimating the coefficients requires a significant amount of computing power. Traditional methods that have analytical solutions to parameter estimation are a lot faster to build.

The issues that can be addressed by Bayesian hierarchical models (e.g.  periodic updates, regression to the mean and overfitting) will sound familiar to those who work with graduate earnings models in both DfE and UK Government Investments (UKGI). I think this methodology would allow us to address them.  I believe that providing accurate predictions for sub-groups of interest could be particularly beneficial to the ultimate goal of having one single model for all.

If you're not yet convinced, here's the killer benefit of these models: simulation with accurate uncertainty based on data is extremely easy to produce.
