---
layout: post
title: "A tool for your bag of tricks: Hierarchical Bayesian models"
cover: cover.jpg
date:   xxxx-xx-xx 
categories: posts
published: false
---

Modelling continues to be a very artistic activity: a manual process is required to build models with all the relevant inputs structured optimally for accurate forecasting and/or intelligible relation with outputs. This is rarely a trivial process; there are a lot of subtleties in statistical theory that make generic solutions unfit to solve most problems. Modellers of all kinds spend a lot of time trying to find powerful and flexible tools to support the model construction process.

A common option is to take regression as a solution to all problems. Regression is a quite powerful tool but has some important limitations. For example, to model different groups with a few levels each, reducing uncertainty whilst handling overfitting tends to be challenging.

Two extreme approaches illustrate the situation: the simplest one is to build a single model to explain the behaviour of all subgroups. This is known as a _complete pooling model_ and it is suboptimal if the groups make any difference at all - that is the [regression towards the mean or reversion to mediocrity](https://en.wikipedia.org/wiki/regression_toward_the_mean) issue. The other extreme is building a different model for each subgroup, known as _no pooling models_, which are also suboptimal because each of them ignores the information from the others. This is particularly problematic when the sample size of some of the subgroups is small and when they are correlated.

A solution is to build a model which includes all the sub-models for each subgroup in a way that each group's sub-model uses some information from within the group and also from the other groups. The trick is how sub-groups _communicate_ in a way that the use of internal and external information is optimally weighted. These have many names: hierarchical, multilevel, or mixed-effects models and in some books they're also referred to as _partial pooling models_.

These models can be fit with traditional methods, however they can become very complex quickly, and it's easy to either end up with a major lack of transparency and/or overfitting. A nice and tidy solution is to use a Bayesian approach as explained [here](http://xcelab.net/rm/statistical-rethinking/) or [here](http://www.stat.columbia.edu/~gelman/arm/), which [allows a well structured model to control complexity](https://stats.stackexchange.com/questions/97233/is-multilevel-modelling-simpler-more-practical-or-more-convenient-using-bayesi). It's worth noting that these models include as a particular case any of the Generalised Linear Models.

Not using a Bayesian approach becomes quickly burdensome as the number of interactions in the model grows. Possible interactions increase geometrically as the number of levels within a group go up and exponentially as the number of groups or factors grow. That makes it difficult to avoid overfitting without strong regularisation methods which tend to make the model harder to interpret. 

In addition to coping well with complexity, Bayesian modelling is naturally prepared for iterative updating. They provide a structured way of taking new information in, and propagating uncertainty in a controlled manner. That is highly desirable as it is rare that models are static. In sight of new and more up-to-date information we're likely to be interested in improving the model. Bayesian models .

Nevertheless, no approach is perfect, Bayesian methods most obvious downside is that estimating the coefficients requires a significant amount of computing power. Traditional methods that have analytical solutions to parameter estimation are a lot faster to build.

The issues that can be addressed by Bayesian hierarchical models (e.g. iterative modelling, regression to the mean and overfitting) are challenges faced by graduate earnings models in both DfE and UKGI. I think this methodology would allow us to address these issues and, more importantly, provide accurate predictions for subgroups of interest. And the benefits do not stop here, the Bayesian approach would allow periodically adding new information with relative ease.

Go bitches!!

---

First, I need a title.
Second, the structure:
1- intro. you have 2 paragraphs, so it needs to be short. What is the problem
we are trying to solve? Modelling is difficult, so amateurs like us need all
the help and we fall for pitfalls e.g. regression to the mean.
2- Why does this matter? And what we normally do that doesn't fix the problem -
regression
3- The solution - Why bayesian, why hierarchical, etc?
4- What happens when we use that solution and what doesn't.
5- Why this is useful to us

