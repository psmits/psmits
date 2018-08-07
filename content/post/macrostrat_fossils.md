+++
title = "Inspecting fossil diveristy in Macrostrat units"
date = 2018-02-12
draft = false

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["academic", "macrostrat", "data visualization"]
categories = []

# Featured image
# Place your image in the `static/img/` folder and reference its filename below, e.g. `image = "example.jpg"`.
# Use `caption` to display an image caption.
#   Markdown linking is allowed, e.g. `caption = "[Image credit](http://example.org)"`.
# Set `preview` to `false` to disable the thumbnail in listings.
[header]
image = ""
caption = "[Macrostrat](https://macrostrat.org)"
preview = false

+++


In this post I'm going to dive deeper in to [Macrostrat](https://macrostrat.org) and start looking at genus diversity of geological units; this is the first follow-up on my [previous post]({{< relref "post/macrostrat.md" >}}). Like before, the code used to generate all the figures etc. is available [here](https://github.com/psmits/psmits/blob/master/static/code/macro_fossils.r).

Our initial data call is exactly the same as my previous post; I'm looking for geological units with Permian sediments: https://macrostrat.org/api/v2/units?interval_name=Permian&response=long&format=csv. Unfortunately this API call does not return useful information about what fossils are found in the geologic unit, only how many. Fortunately, this is pretty easy to overcome.

To get the fossil information for each geological unit, I need make another API call. With a tiny bit of code the unit id's are transformed into a comma seperated string that can be used to form the appropriate url

```
strat <- read.csv('https://macrostrat.org/api/v2/units?interval_name=Permian&response=long&format=csv', 
                  stringsAsFactors = FALSE)
furl <- paste0('https://macrostrat.org/api/v2/fossils?unit_id=',
              paste0(strat$unit_id, collapse = ','),
                '&response=long&format=csv')
fossil <- read.csv(furl, stringsAsFactors = FALSE)
```

This new macrostrat information has a lot of useful information about the fossils associated with each of the geological units. The key bit of information I'm going to be using here is the Paleobiology Database collection ids. These unique ids correspond to the nebulous unit of "collection" which may refer to specimens collected together or specimens that are mentioned in the same paper. In any case, these collections are associated with geological units and are our key to getting more taxonomic information.

Using the unique list of collection ids, I make an API call to the PBDB. First, I make a matrix with the unique combinations of collections and geological units. Second, I make the API call given a unique string of collection ids. Finally, I match the collection id associated with each fossil back to the Macrostrat geological unit id. This should be all the information necessary to make a bunch of useful and interesting visualizations.

```
cypher <- unique(cbind(fossil$cltn_id, fossil$unit_id))

furl <- paste0('https://paleobiodb.org/data1.2/occs/list.txt?coll_id=', 
               paste0(fossil$cltn_id, collapse = ','), 
               '&show=full')
taxon <- read.csv(furl, stringsAsFactors = FALSE)

taxon$unit_id <- cypher[match(taxon$collection_no, cypher[, 1]), 2]
```

Now that all the data is in memory, we can begin exploring the nature of that data.

Macrostrat units have lots of data associated with them, including duration and areal extent. So, my initial explorations are checking out unit duration vs unit diversity and unit area vs unit diversity. 

![log duration vs diversity](/img/unit_div_logage.png)

![log area vs diversity](/img/unit_div_logarea.png)

So, there doesn't appear to be a linear relationship between either unit duration or area and unit diversity; perhaps any model of this data should strongly consider possible non-linear effects of covariates on unit diversity. However, there are a lot of units with 0 fossil observed which may be obscuring an actual pattern. Let's try again but this time only with the units that bear fossils (super easy to do in ggplot).

![log duration vs diversity, no 0s](/img/unit_div_age_gr0.png)

![log area vs diversity, no 0s](/img/unit_div_area_gr0.png)

That appears to add something to the discussion, and the possibility of a non-linear between these covariates and unit diversity appears definitely worth considering. One new concern is that the measure of diversity is based on ALL genera identified to that unit; this includes everything from bivalves to brachiopods to crinoids and beyond. Perhaps in a future analysis I'll break down these explorations by taxonomic group (stay tuned).

Finally, I have two more plots before I sign off on this showcase: unit diversity over time. Plotting each geological unit at its midpoint we can get an idea if average unit diversity has any systematic change over time. Additionally, we can see if there are time periods with scrappy records. I've made the plot both with and without units bearing 0 fossils.

![time vs diversity](/img/unit_div_time.png)

![time vs diversity, no 0s](/img/unit_div_time_gr0.png)

Stay tuned for more explorations of Macrostrat. If you're interested in looking at the code used to generate these plots, check [here](https://github.com/psmits/psmits/blob/master/static/code/macro_fossils.r).


