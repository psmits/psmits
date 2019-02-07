+++
date = "2019-01-07"
draft = false
external_link = ""
highlight = true
image_preview = ""
math = false
summary = "Teaching material on analytical paleobiology"
tags = ["academic"]
title = "Teaching analytical paleobiology"

[header]
  caption = ""
  image = ""

+++


In 2018 I helped teach a graduate-level class at UC-Berkeley on analytical paleobiology. Since then I have continued to produce short modules covering many different aspects of analytical paleobiology. All of these lessons have been written in [RMarkdown](https://rmarkdown.rstudio.com/) and emphasize the use of [`tidyverse` packages](https://www.tidyverse.org/). Additionally, I emphasize Bayesian data analysis as fundamental and use the [`brms` package](https://github.com/paul-buerkner/brms) for implementing [Stan-based](https://mc-stan.org/) models in R.

So far I have written lessons on 

1. Importing and cleaning data from the [Paleobiology Database](https://paleobiodb.org/#/) using `dplyr`, with an emphasis on reproducible analyses.
2. Introduction to the logic of Bayesian data analysis.
3. Introduction to linear regression with a single binary predictor, emphasizing the interpretation of parameters and checking model adequacy.
4. Graph theory and network analysis approaches in paleobiology, with an emphasis on analyzing biogeographic co-occurrence of species. This module was requested by the class and is thus out of order.
5. Continuing linear regression by introducing continuous predictors, methods for improving parameter interpretation, and further emphasis on checking model adequacy.
6. Linear regression with categorical predictors with more than two levels, interpreting models with multiple predictors (incl. continuous and discrete), interpreting interaction effects, and methods for visualizing multivariate regression. (In progress).

The initial 5 lessons are available [here](https://github.com/psmits/cal_paleostats). I have begun compiling these initial lessons, as well as new ones, in an online textbook using [bookdown](https://bookdown.org/home/).
