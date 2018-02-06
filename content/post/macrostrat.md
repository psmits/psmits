+++
title = "Downloading unit data from Macrostrat and looking at it"
date = 2018-02-07
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
caption = "[Macrostrat](https://macrostrat.org/)"
preview = false

+++

![macrostrat](/img/macrostrat_logo.png)

[Macrostrat](https://macrostrat.org/) is a geology resources created by [Shanan Peters](http://strata.geology.wisc.edu/), who currently maintains it and the [Paleobiology Database/PBDB](https://paleobiodb.org/) with a small group of developers. Macrostrat is a database of geologic units from North America representing almost all of time. One of the coolest aspect of this database is that geologic units are recorded regardless of if they bear fossils or not, which is very useful when you're trying to analyze the conditions under which fossils are both preserved and found. In addition to basic information like the temporal and geographic location and duration of each geologic units, there are many important pieces of metadata associated with each unit such as coordinates of the unit's top and bottom, thickness, if continuous sequence above/below, what PBDB collections are present in that unit, a lithological description of that unit, and a lot of other interesting information. For more information either visit the [macrostrat website](https://macrostrat.org) or check out this [new pre-print describing a lot of the more advanced/colorful features and tools assocaited with the project](https://eartharxiv.org/ynaxw).

While geologic units in Macrostrat can be summarized in different ways (e.g. column), I am going to focus on downloading and processing geologic units and then visualizing their temporal structure. All of this is going to be done using R and the code snippet to produce all the figures etc. will be available at the [end](https://gist.github.com/psmits/aaa912dd7c14bfa710d58b03cc8e0b8f).

Macrostrat data is served through a [no-frills API](https://macrostrat.org/api/v2) that allows for simple html calls to return CSV or JSON data for as many geologic units as desired. For this example, I want information all geologic units that have Permian sediments; to accomplish this, the simple html call is <https://macrostrat.org/api/v2/units?interval_name=Permian>. 

The data resulting from this simple call, however, is missing a lot of interesting metadata and is currently in json format. To receive the extra metadata and have the call returned in csv format, the html call is updated to <http://macrostrat.org/api/v2/units?interval_name=Permian&response=long&format=csv>. The response paramater has two values (short, long), while the format parameter controls the output being in a JSON variant or plan csv. While changing the output format from json to csv isn't strictly necessary, it simplifies a few downstream operations and makes my life easier.

Ok, so no we have a large data frame in memory; as of writing this entry, there are 1165 geologic units that have any part of their range in the Permian. From here we can extract useful information: "how long are the geologic units?"; "are fossil bearing units longer in duration than those that do not bear fossils?"; "is there a gap where no fossil bearing units are found?"; and so on. 

First things first, we should plot attempt to visualize our data. A basic way to visualize geologic units is to plot units as horizontal lines from their start point till end point, ordered based on start times. Macrostrat units have variables called b_age and t_age which are the bottom and to ages of the geologic unit, respectively. These values are based on the underlying continuous age model which attempts to sort the order of geologic units more finely than their initial age estimates; by relying on these age estimates we are assuming that the continuous time model is accurate; this might bother some people because scientists are so adverse to assumptions but happily use statistical tools, all of which are just statements of assumptions, but that's an argument for another time.

Anyway, here is a plot all the geologic units that have some element of their duration in the Permian:

![all units](/img/units_basic.png)

This simple visualization reveals some interesting features of our data: 1) there are some really long geologic units that begin well before the Permian and end well after the Permian, 2) many geologic units have similar "onset" times not necessarily similar end times. Because some of these units span a lot of time outside of the Permian, zooming in on just the Permian interval (298.9-251.9 Mya) might help more clearly visualize the data. Here is a that figure with horizontal lines indicating the beginning and end of the Permian.

![zoom in on units](/img/units_zoom.png)

This updated figure helps visualize how many of the geologic units range into or out of the Permian. It also helps us appreciate the change rate of geologic unit onset, with a rather continuous addition of units from 295-285 Mya transitioning into a possibly punctuated pattern of many units have contemporaneous onsets with only a handful of units beginning outside these moments of contemporaneous unit onset.

The next modification I want to make to this plot is indicating which units bear fossils or not. There are two Macrostrat geologic unit variables which answer this question: pbdb_collections, and pbdb_occurrences. These variables describe how many PBDB collections and how many fossil specimens in the PBDB are associated with that unit. Because I'm not currently interested in the actual identity of those fossils, just if they are present or not, this activity is pretty simple. There is a good chance I'll discuss linking Macrostrat and the PBDB in a future post, so stay tuned.

Using a simple **ifelse** statement, I can quickly get a factor describing if there are more than 0 collections associated with a unit. When that is mapped onto our plot, this pattern emerges: 

![color units by fossils](/img/units_color.png)

The first thing that pops out when looking at this figure is that there are a lot more geologic units that do not bear fossils than those that do, and that those to do bear fossils may have a shorter duration than those that do but it is hard to tell.

Given these interesting observations, lets plot a comparison of unit duration for fossil bearing and non-fossil bearing units. I'm going to plot duration on a log scale so that comparisons are in terms of magnitude, not absolute value.

![unit durations](/img/units_duration.png)

From visual inspection, it appears my hunch is probably wrong; fossil bearing and non-fossil bearing units have very similar distributions and I'd guess they have indistinguishable means.

Anyway, I'll probably be demonstrating more ways to poke around at Macrostrat data in the future. [Here's a gist of the code necessary to produce all of the above plots.](https://gist.github.com/psmits/aaa912dd7c14bfa710d58b03cc8e0b8f).


