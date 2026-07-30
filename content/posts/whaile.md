+++
title = "Whaile"
date = "2026-07-30"
categories = ['python','dashboards','trends','machine learning']
toc = true
draft = true
+++

## Whaile: a `Python Streamlit` App of UK Shore Observations

[**>> Whaile App**](url "Python Streamlit application called Whaile")

**come back soon**

’ “ ” … w/


### Introduction

As a fan of data, I’ve found enjoyment over the years creating data dashboards using `R Shiny`. 
Most notably my [Fashun app](https://sap218.shinyapps.io/fashun_app/ "link to fashun app"): correlation plots of Google search trends that highlight particular relationships.
(you can read more about Fashun [here]({{< ref "fashun" >}}))

#### `Python and Streamlit`

I decided to explore the `Python` realm, there are different frameworks, e.g. [`Django`](https://www.djangoproject.com/ "link to django"), [`Flask`](https://flask.palletsprojects.com/en/stable/ "link to flask"), and [`Streamlit`](https://streamlit.io/ "link to streamlit"). 

I investigated all three and decided to explore `Streamlit` in some depth. 
From an initial glance, `Streamlit` is similar to `R Shiny` to build interactive data Apps.

### The Data

I started with the tough task of finding a data set: I wanted to curate the data myself but couldn't find the right fit. 
With my previous App, I mined Google search trends. For this App, I decided to show how APIs are useful! 

With the [Ocean Biodiversity Information System](https://obis.org/data/access/), I used the API tool to curate a large set of ocean Observations over the past few decades, covering a variety of species both common and non-common across UK shores.
This API has numerous endpoints, such as species, family, latitude, longitude, dates, shore distance, coordinate uncertainty.

**Important**: see here for the [Statement of Use](https://github.com/sap218/whaile#data "link to statement of use"). 

I gathered a list of 22 species that are common, uncommon, and rare sightings across UK shores; chose the country “United Kingdom”; and requested 10,000 observations each.

```
params = {
	"scientificname": species,
	"country": "United Kingdom",
	"size": 10000
}
```

This may seem like a LOT of data, but once we filter the license per observation to keep only CC-0 and CC by 4.0, etc. (all open licenses that allow for data use as long as I don’t share or use the data commercially) we lose a lot of observations - better safe than sorry!

With the data, I did minimal cleaning: I extracted any 4 digit number starting with 1 or 2 from dates for Year, filtered out rows that data was in a lower quartile range (many dates spanned historic records from the early 1900s), and small fixes (e.g. replacing underscores in values).

| Before | After |
| --- | --- |
| {{< figure src="/images/posts/whaile/02_hist_before.png" class="figure-plain" alt="histogram of dates before filtering" >}} | {{< figure src="/images/posts/whaile/02_hist_after.png" class="figure-plain" alt="histogram of dates after filtering" >}} |

Finally, I decided to do some Clustering with DBScan (Density-Based Spatial Clustering of Applications with Noise).
There are many clustering algorithms out there, and usually in research we can use multiple and analyse individual performances (or choose consensus clustering).
DBScan looks at shapes in data: how close observations are to another and chooses the number of clusters for us - it’s great for detecting outliers, detecting natural groups, and detecting unusual shapes, e.g. shorelines.

For the data curation API, cleaning, and clustering, see the [**Notebooks**](https://github.com/sap218/whaile/tree/main/data "link to data notebooks") to work through.
Here you can see the API endpoints I requested, the cleaning, and clustering.

### The App

With `Streamlit`, I started with some simple features: import the data, curate a list of categorical features, and plot a UK map of longitude and latitude coordinates.
(the UK map was sourced from [Wikipedia](https://commons.wikimedia.org/wiki/File:Uk_outline_map.png)

I started with a scatterplot: longitude and latitude data points and I noticed the plot was noisy, so I added a checkbox to default as outliers removed.
Then I decided to colour by cluster, then other categorical variables, and then I realised I created an Endpoint.

{{< figure src="/images/posts/whaile/App.PNG" class="figure-plain" caption="**Figure**: A screenshot of the Whaile App." >}}

#### The Endpoint

The main feature of the App is to colour the scatterplot by a variable of interest: our endpoint. 
The main purpose of the clustering was this to be our endpoint but I wanted users to choose their own and explore the data.

A minor limitation of the App is that although there is filtering available - outliers, dates, how common the species is on UK shores - the clustering is static based on the entire data.

Once an endpoint is chosen, the datapoints on the UK map scatterplot are coloured and a legend appears.
Underneath the scatterplot a few dropdowns appear: (these dropdowns depend of the endpoint and other features chosen)

1. A bar plot of the endpoint counts w/ a table
2. Count of the endpoint across clusters

Underneath the option to choose the endpoint, a checkbox appears to do some comparisons...

#### Comparisons & Statistics

Users can choose to compare the endpoint to a categorical variable and a numeric variable. More dropdowns appear:
- Bar plot is now stacked by the categorical variable
- Most common categorical variable across the endpoint
- Histogram of the numeric with a table of averages across the endpoint

Underneath the option to tick the comparisons box, another checkbox appears to do some statistics…

The statistics chooses the endpoint and compares the numeric variable against via an ANOVA plus the categorical variable against via a Chi-Squared.

Users can choose their own p-value cut-off as what is significant for them, and results will also present highlight if the p-value is significant when adjusting for Bonferroni correction.

#### Other Features

##### World map

`Streamlit` has built-in functions for plotting, most notably a WorldMap for scatterplots.
The functionality was a little limiting in terms of adding a legend, etc. so instead I provided a colour mapping table underneath.

##### Taxonomy

Another feature - which I really enjoyed - was the taxonomy sunburst - please do have a go at this!

| World Map | Taxonomy |
| --- | --- |
| {{< figure src="/images/posts/whaile/worldmap.PNG" width=100% class="figure-plain" alt="histogram of dates before filtering" >}} | {{< figure src="/images/posts/whaile/taxonomy.PNG" width=90% class="figure-plain" alt="histogram of dates after filtering" >}} |

### Some Investigations!

*come back soon*


{{< alert type="info" >}}
When I first started developing this App, I didn’t filter for UK or check licenses, so when post-review I started to consider these, a lot of datasets were dropped.
Furthermore, many values were limited (e.g. only Falses and no Trues).
Plots and statistical results don’t have much of an effect anymore but at least I had fun, shown some skills, and learnt a lesson to remember in future!
{{< /alert >}}

#### Implications

First we can see that the clustering has grouped based on geographic observations, which has a relationship with species.

**Cluster 3**: Scotland’s The Minch, White-beaked dolphin, shore distance average: 172.33, bathymetry average: 84.68.

+ The Minch has deeper waters near the coastline, reflected in the average bathymetry, and a higher density of fish so it may be expected to see White-beaked dolphins closer to the shore here as this is a preferred habitat.

**Cluster 2**: Wales’ Ceredigion Bay, Bottlenose dolphin, shore distance average: 397.09, bathymetry average: 4.43.

+ Although usually observed very close to the coastline, observations on Ceredigion bay can reflect travelling parallel to the coastline here and foraging food. Bottlenose dolphins commonly forage in shallow waters.

**Cluster 7**: The Cornish Coast, Common dolphin, shore distance average: 547.8, bathymetry average: -11.64.

+ Common dolphins are visitors and don’t usually have permanent resident populations around UK shores, moreover are offshore feeders, reflecting their high shore distance average.

**Important**: of course any results in this dashboard aren’t making any claims: more thorough investigations would be needed including more data, better data engineering, and clustering methodologies.

### Wrapping-up

To conclude, **`Whaile`** is a dashboard of UK shore observations: visualising clusters across shores, highlighting species groups, and investigating distances and bathymetry.
This was a fun project, showing a new skill using `Streamlit` with `Python` for data dashboards, data curation with APIs, and clustering.

The code is available on [GitHub](https://github.com/sap218/whaile "github link") which includes the data curation, engineering, and the code for the App.

### The App

[**>> Whaile App**](url "Python Streamlit application called Whaile")

{{< alert type="success" >}}
Please do have a little explore - let me know if you find anything interesting!
{{< /alert >}}
