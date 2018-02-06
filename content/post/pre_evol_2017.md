+++
date = "2017-05-19"
draft = false
highlight = true
math = false
tags = ["academic"]
title = "Introduction to my Evolution2017 talk"

[header]
  caption = ""
  image = ""

+++

I will be presenting the analysis and results from the third study from my Ph.D at [Evolution2017 in Portland, Oregon](http://www.evolutionmeetings.org/evolution-2017---portland-oregon.html). In preparation for and to better contextualize my talk, I have decided to write an extended abstract. My talk is titled "The changing functional composition of the North American species pool: modeling species origination-extinction as a function of functional group and environmental context" and I'm the sole author on the study though all of the data analyzed is available through the [Paleobiological Database](https://paleobiodb.org/). 

___

A species pools change over time as new species enter the system through speciation or immigration and species leave through extinction and extirpation. Changes in the functional composition of a regional species pool are changes that occur across all local communities drawn from that species pool. While a species' presence in a local community is due to the availability of the necessary biotic-biotic or biotic-abiotic interactions that enable coexistence, a species' presence in a regional species pool just requires that at least one local community has that set of necessary interactions. I am investigating how the interaction of multiple macroecological and macroevolutionary processes acting at multiple levels of organization can lead to specific changes to the functional diversity of the species pool. 

Here, I analyze the diversity history of North American mammals functional groups for most of the Cenozoic (the last 65 million years). The goal of this analysis is to understand when, and possibly for what reasons, mammal functional groups are enriched or depleted relative to their average diversity. Additionally, I frame the historical processes affecting mammal diversity in terms of both their means of interacting with the biotic and abiotic environment (i.e. functional group) as well as their regional and global environmental context (e.g. dominant floral groups, temperature). 

I analyzed the mammal fossil occurrence data using a Bayesian hidden Markov model based off of the Jolly-Seber capture-mark-recapture model. I then modeled the three probability parameters (i.e. sampling, speciation, extinction) as the result three separate hierarchical logistic regressions. The code implementing this model was written using the Stan probabilistic programming language. 

I find that changes to mammal diversity are driven more by changes to speciation rate rather than changes to extinction rate. I also find that the only functional groups which increase in standing diversity over time are digitigrade and unguligrade herbivores, though herbivores in general increase in relative diversity compared to other ecotypes. This increase in diversity is at the possible expense of the arboreal functional groups which became increasingly rare and in many cases disappear entirely from the species pool by the Recent. Additionally, I find that global temperature is only associated with the origination of some mammal ecotypes but, in almost all cases, does not affect the extinction of mammal ecotypes. 

___

Now I just need to revise the dissertation chapter into an actual publication!
