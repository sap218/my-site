+++
title = "BashionI"
date = "2026-09-13"
categories = ['python','r','dashboards','trends']
toc = true
+++

## BashionI: the power of `PowerBI` via fake fashion data

I use `Python`, `R`, and `SQL` a lot in my work. Often `SQL` for data mining, `Python` for data processing, and `R` for statistics and plots.

I thought I would explore the b/powers of PowerBI with a b/fashion dataset I 'faked'. # little bJoke

PowerBI is a Business Intelligence app. You can analyse and visualise data via dashboards and reports.

It is less coding and more drag/drop and clicking.
Some things to note: I don't have premium features of PowerBI, and I won't be discussing any results and the data is fabricated.

### Repository

The `Python` code I used to generate the data and plotting w/ `R` is stored in the GitHub repository.
Along with the data itself and the PowerBI file.
There are various plots and in the `README` includes any tutorials I followed.
Feel free to go explore and maybe even download the dashboard file...

[**>> GitHub repository**](https://github.com/sap218/BashionI "GitHub repository for BashionI")

### Data generation via Python

I started with the `Python` module `random` and incorporated the [`faker`](https://faker.readthedocs.io/en/master/) library to generate a fake dataset.
For example:

```
"First_name": fake.first_name()
dates = fake.date_between(start_date=date(2025, 1, 1), end_date=date(2025, 12, 31))
ages = max(18, min(65, round(random.gauss(28, 10)))) # this line makes the average age 28
"Quantity": random.choices([1, 2, 3, 4], weights=[85, 10, 4, 1])[0] # 85% chance of a person buying only 1 of an item
```

I iteratively ran through the generator for 10,000 rows, each row representing a 'sale'.
I added some weights to the data to make it a little more 'real' (encourage some results for some interesting plots).

### Data summarising via R 

I used [`tidyverse`](https://tidyverse.org/) library, which includes `dplyr` for data manipulation and `ggplot2` for plotting.

Although my data faker collects fake names, when I read in the data via `R`, I excluded anything 'identifying', e.g. 

`select(-First_name, -Last_name, -Email) %>%`

And I created some plots!

Then with [`gtsummary`](https://www.danieldsjoberg.com/gtsummary/index.html) library, I produced some tables, in what they decribe as 'publication-ready'.

Now I had something I could compare with my explorations of PowerBI.

### PowerBI

When opening PowerBI, my first step was to import the data.
PowerBI has many ways to import data: excel, `SQL` server, `csv`, web, and more.

I used `Get data > Text/CSV` and PowerBI automatically detected my file was in tab-separated `tsv` format.
In `Python` and `R` this is fairly easy too, e.g. `Pandas` you would use `sep="/t"`.

With the data imported, I decided I would try and recreate one of the plots I had already exported in `R`: a line plot of costs/revenue by month. 
One thing to mention here, PowerBI doesn't have a way to export plots, instead they focus on their pages and export these as a `pdf`.
So I had to export the file as a `pdf` and then convert that to `png` to extract individual pages.
For those who use PowerBI for dashboards and reports, this is probably great - but for individual plots it was a little inconvenient.

| R | PowerBI |
| --- | --- |
| {{< figure src="/images/posts/bashion/costs_line_RG.png" width="100%" class="figure-plain" alt="line plot of costs and revenue via r" >}} | {{< figure src="/images/posts/bashion/costs_line_PB.png" width="92%" class="figure-plain" alt="line plot of costs and revenue via powerbi" >}} |

As we can see, both are very similar. In `R` I coded, in PowerBI I drag and dropped.
`R` lets me easily add another line to the plot, it can be the same data point but one dashed and the other curved.
In PowerBI, it would 'group' them and couldn't distinguish, e.g. made the second "Revenue" instance the same style as the first.

This may be linked with the limitation of PowerBI features being locked behind organisation access, for example box plots.
Or perhaps highlights how `ggplot2` gives you a lot of plotting control, especially with more complex graphs.

Another plot I tried to recreate is a bar plot distribution of ratings across age groups:

| R | PowerBI |
| --- | --- |
| {{< figure src="/images/posts/bashion/stars_bar_RG.png" width="100%" class="figure-plain" alt="bar plot of ratings distribution across age groups in r" >}} | {{< figure src="/images/posts/bashion/stars_bar_PB.png" width="92%" class="figure-plain" alt="bar plot of ratings distribution across age groups in powerbi" >}} |

One thing I particularly liked about PowerBI is that I could click a button and the y-axis flipped, which is what I had to do to make these plots similar.
But again some features aren't as easy as a button click...

As you can see the bar widths are different, in `R` you can edit the width of bars and text sizes, but it's a little easier in PowerBI.
With plotting features in `R` I have to remind myself which parameter to use...but PowerBI some features it's a tick of a box.

Something I think is better in `R` is you can automate processes.
You could create a loop and within a minute would have multiple of the same plots with the same x-axis but different y-axis.
But in PowerBI I would either have to drag and drop different features, or copy the plot multiple times and manually change the y-axis.

#### Dashboards

After comparing `R` and PowerBI, I decided to try and create a dashboards.

I personally think this is where `Python` and `R` dashboard creating is easier...
From my previous work, I have developed dashboards via `RShiny` and `Python Streamlit` - both are dynamic: they can be deployed via the web meaning for some organisations, you could deploy them internally.
[Fashun]({{< ref "fashun" >}}) includes Google Trends, [Whaile]({{< ref "whaile" >}}) is shore observations, and Bashion is commercial.
All three are data dashboard-oriented but different datasets with the same underlying approach: curate data, wrangle the data, and then display the data.
PowerBI was a more manual, although is beneficial in some ways: click a plot type, drag-drop features, easy changing of fonts/text size.
There are limitations for more complex plots, many features are locked behind accounts, exporting difficulties for single plots, and I haven't considered how it may be difficult sharing PowerBI files within organisations, e.g. do you share the **`.pbix`**, or the `.pdf`? If it's supposed to be interactive, wouldn't a `pdf` file be limiting?
With `R` and `Python` I think it would be easier to version control, address bugs, and continiously improve with new requested features.
I think via coding, dashboards offers more flexibility and being able to reproduce/automate analyses.
Even outside of dashboards, there is `Python Jupyter` and `R Markdown` formats, which although will contain code, may be more open to others how you filtered, or calculated some new fields.

{{< figure src="/images/posts/bashion/dashboard1.png" class="figure-plain" caption="**Figure**: A screenshot of the first dashboard page." >}}

In this dashboard, I created several "measures": revenue, total sales (row count), total units, and more.
I included numerous filtering options on the right-hand side then a bar plot on the left, with a feature that lets you zoom in.

```
Revenue = SUMX(
    'fake_data',
    'fake_data'[Cost] * 'fake_data'[Quantity]
)
Total Sales = COUNTROWS(fake_data)
Total Units = SUM(fake_data[Quantity])
```

Although cool, I do think this is easily achievable with `RShiny` and `Py Streamlit`, in fact you can also include other dynamic parameters to change things, e.g. choosing which features you want plotted on demand, plot colours, splitting a plot by features, etc.

{{< figure src="/images/posts/bashion/dashboard2.png" class="figure-plain" caption="**Figure**: A screenshot of the second dashboard page." >}}

Here I played around with more visualisation types, I especially enjoyed the scatter plot: this shows that when the cost increases, the stars/ratings also increase. (remember: data is fabricated!)

**This would be a great plot with real data!**

{{< figure src="/images/posts/bashion/dashboard3.png" class="figure-plain" caption="**Figure**: A screenshot of the third and last dashboard page." >}}

Here I chose the same plot and changed the y-axis and included some legends.
One minor limitation with PowerBI: it automatically includes the subtitle of the y-axis and legend, the legend itself isn't automatically labelled, so it may be confusing at first what the individual bars are.

I also included a table, I tried to figure out how to add statistics (like `R`) but I think this is another limitation, again perhaps locked behind org access.
Nevertheless, with `gtsummary` it would be as easy as `add_p()`.

### Conclusion

Overall I think PowerBI is great, for reports and plots - I can understand it's popularity.
The drag-drop and clicking makes it very useful and provides the ability to work seamlessly.

With `Python` and `R` I think I find it more convenient and easier when considering more complex plots...although slightly frustrating having to remember the parameter controls for font sizes, adding titles, etc.
BUT with coding, you can set global params for these and ensure all plots apply.
Variables means you could automatically include these in plot titles, exporting names, etc.

Considering dashboard, again I think with `R` and `Python` you can deploy dashboards internally and version control them.
People could ask for new features, explore the dashboard themselves, you can fix bugs.
From my personal experience, dashboards have improved meetings as we can look at the data on demand, rather than asking if data exists then meeting the next week to confirm it does.

In terms of reporting, `R Markdown` and `Py Jupyter` are nice but I believe you could create nicer `pdf` reports with PowerBI - it would be especially useful for communication, e.g. science communication and summaries.

I am probably slightly more bias towards coding as I have almost 10 years of experience.
The code I produce are as automated as I can make them and reproducable.
I can easily deploy statistical tests via loops, and features aren't locked behind organisation access/accounts. (I do really appreciate that PowerBI is accessible though!)

[**>> GitHub repository**](https://github.com/sap218/BashionI "GitHub repository for BashionI")
