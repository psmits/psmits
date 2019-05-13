+++
title = "Online textbook on Analytical Paleobiology"
date = 2019-05-13T14:00:38-07:00
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Peter Smits"]

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ['academia']
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = ""
caption = ""
preview = true

+++


For the last few months I have been working on an [online textbook/short-course on analytical paleobiology](https://psmits.github.io/paleo_book/index.html). The goal of this project is to develop a series of tutorials and how-to guides that are suitable to teach from as individual 2-hour lessons. These lessons are meant to serve launching points for further learning, and not as fully comprehensive reference tools (suggested readings are listed at the top of each lesson). Throughout the text I emphasize critically thinking about your data and using domain knowledge to shape and drive our analyses. I demonstrate this by using real-world datasets to illustrate analytical approaches and tools. 

The first section covers importing and cleaning data from the Paleobiology Database using `tidyverse` tools such as `dplyr`. 

The second section introduce the core concepts behind Bayesian data analysis. This approach is emphasized throughout the text.

Most of the subsequence sections cover regression modeling, from the most basic linear regression to interaction terms to logistic regression. 

I've written a section on varying-intercept models and am developing more material on multilevel models, in particular application to analyzing paleontological time-series data. 

You can track more information about this text on the [project page]({{< ref "/project/analytical_paleobiology.md" >}}), or on the [text's github page](https://github.com/psmits/paleo_book). 
