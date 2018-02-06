+++
date = "2017-06-02T19:59:21-05:00"
draft = false
highlight = true
math = false
tags = ["academic"]
title = "Getting into Bayesian data analysis"

[header]
  caption = ""
  image = ""

+++

None of my degrees are in statistics and I would hesitate to truly call myself a "statistician," but all my work involves tons of statistical modeling. I learned how to do statistical analysis, and Bayesian analysis in particular, through a hodge podge of means: biology classes, blogs, a 5-week training workshop, and reading multiple textbooks. I began my journey into Bayes knowing the basics of linear regression, covariance/correlation, model selection/AIC, and multivariate data analysis methods like PCA and NMDS; and I'd only heard of MCMC and Bayes solely in the context of phylogenetic inference. 

My interest in Bayes was started after programming in R for a couple years. I had just read a "teach yourself survival analysis" book and knew I wanted to write parametric survival models. I'd heard of BUGS/JAGS and was interested using something similar for designing my own models from the ground up. Stan had just come out at the start of my Ph.D., so I decided to get in on the ground floor. I knew I needed this kind of tool to really do the kind of analysis I craved; I just didn't know what exactly that meant. It was time to buckle down and dig into some textbooks.

There are two books that form the center of my statistical education: [Bayesian Data Analysis](https://www.amazon.com/Bayesian-Analysis-Chapman-Statistical-Science/dp/1439840954), and [ARM](https://www.amazon.com/Analysis-Regression-Multilevel-Hierarchical-Models/dp/052168689X/). The former is more "higher-level" and a great deal more complicated than the latter and I recommend reading ARM before BDA; though I ended up reading the textbooks in reverse. A year or two after those two books, I read through [Statistical Rethinking](https://www.amazon.com/Statistical-Rethinking-Bayesian-Examples-Chapman/dp/1482253445), which would probably be the one I'd pick to serve as the main textbook for any class on Bayesian modeling I would teach.

From these core books it is possible to read an incredible amount of statistics literature, and intuit the similarities between different "types" of models (e.g. GLMMs, survival analysis, logistic regression, Markov models). I had tried reading books like Bayesian Survival Analysis and I couldn't penetrate the math; for some reason the notation never made any sense to me. The way models are presented in BDA, ARM, and Statistical Rethinking is extremely clear and unifies all analyses in terms of probability and not subfield specific or assumed notation; it gets the math wankry out of data analysis. Now, when I approach a text I just concentrate on the PDFs and quickly jot out the model along the side.

For teaching and communication resources, the way models are presented in [Doing Bayesian Data Analysis](https://www.amazon.com/Doing-Bayesian-Data-Analysis-Second/dp/0124058884/) has proved invaluable to explaining how Bayesian models "work;" it is also way less abstract and complicated than [plate notation](https://en.wikipedia.org/wiki/Plate_notation). I've not actually read that textbook but I read through the section on drawing models. [Rasmus Baathhas produced some templates for drawing your own "Krushke diagrams."](http://www.sumsar.net/blog/2013/10/diy-kruschke-style-diagrams/) I've used this resource in talks before to great success.
