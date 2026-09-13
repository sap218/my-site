+++
title = "BashionI"
date = "2026-09-13"
categories = ['python','r','dashboards','trends']
toc = true
draft = true
+++

## BashionI: the b/power of `PowerBI` via b/fashion data

I use `Python`, `R`, and `SQL` a lot in my work. Often `SQL` for data mining, `Python` for data processing, and `R` for statistics and plots.

I thought I would explore the b/powers of PowerBI with a b/fashion dataset I 'faked'.
Some things to note: I don't have premium features of PowerBI, and I won't be discussing any results and the data is fabricated.

### Repository

See here for the code for generating the data, the data, code for tables and plotting, and the PowerBI dashboard file.

[**>> GitHub repository**](https://github.com/sap218/BashionI "GitHub repository for BashionI")

### Data generation via Python

I started with the `Python` module `random` and incorporated the [`faker`](https://faker.readthedocs.io/en/master/) library to generate a fake dataset.

For example,

```
"First_name": fake.first_name()
dates = fake.date_between(start_date=date(2025, 1, 1), end_date=date(2025, 12, 31))
ages = max(18, min(65, round(random.gauss(28, 10)))) # this line makes the average age 28
"Quantity": random.choices([1, 2, 3, 4], weights=[85, 10, 4, 1])[0]
```

I iteratively ran through the generator for 10,000 rows, I added some weights to the data to make it a little more 'real', and each row pertains to a 'sale'.

### Data summarising via R 

I used [`tidyverse`](https://tidyverse.org/) library, which includes `dplyr` for data manipulation and `ggplot2` for plotting.

Although my data faker collects fake names, when I read in the data via `R`, I excluded anything 'identifying', e.g. 

`select(-First_name, -Last_name, -Email) %>%`

And I created some plots!

Then with [`gtsummary`](https://www.danieldsjoberg.com/gtsummary/index.html) library, which produces 'publication-ready' tables, I produced some tables.

### PowerBI

When opening PowerBI, my first step was to import the data. PowerBI has many ways to import data: excel, `SQL` server, `csv`, web, and more.

I used `Get data > Text/CSV` and PowerBI automatically detected my file was in `tsv` format. In `Python` and `R` this is fairly easy too, e.g. w/ `Pandas` you would use `sep="/t"`.

And then started with the aim to create a plot I had in `R`: a line plot of costs/revenue by month. 
One thing to mention here, PowerBI doesn't have a way to export plots, instead they focus on their pages and export these as a `pdf`.
So I had to export the file as a `pdf` and then convert that to `png` to extract individual pages.

| R | PowerBI |
| --- | --- |
| {{< figure src="/images/posts/bashion/costs_line_RG.png" width="100%" class="figure-plain" alt="table of" >}} | {{< figure src="/images/posts/bashion/costs_line_PB.png" width="92%" class="figure-plain" alt="table of" >}} |

As we can see, both are very similar. In `R` I code, in PowerBI I drag and drop. 
`R` lets me easily add another line to the plot (dashed & curves) but in PowerBI, when adding multiple lines for one feature, it would 'group' them so it couldn't distinguish, e.g. made both "Revenue" lines the same instead of one being line and another curve.

This may be linked with a limitation of PowerBI in that many features are locked behind organisation access, for example box plots.

Another plot I tried to recreate is a bar plot distribution of ratings across age groups:

| R | PowerBI |
| --- | --- |
| {{< figure src="/images/posts/bashion/stars_bar_RG.png" width="100%" class="figure-plain" alt="table of" >}} | {{< figure src="/images/posts/bashion/stars_bar_PB.png" width="92%" class="figure-plain" alt="table of" >}} |

Something I liked about PowerBI is that I could click a button and the y-axis flipped, which is what I had to do to make these plots similar.

In `R` you can edit the width of bars and text sizes, but it's a little easier in PowerBI with plotting features - in `R` I have to remind myself which parameter to use...

Something I think is better in `R` is that you could create a loop and within a minute could have multiple of these plots and instead of distributions of ratings, instead could be other values, like age and distribution of products.
But in PowerBI I would either have to drag and drop different features or copy this plot and change features.

#### Dashboards

After comparing `R` and PowerBI, I decided to try and create dashboards.

I personally think this is where `Python` and `R` dashboard creating is easier...
From my previous work, I have developed dashboards via `RShiny` and `Python Streamlit` - both are dynamic: they can be deployed via the web meaning for some organisations, you could deploy them internally.
[Fashun]({{< ref "fashun" >}}) includes Google Trends, [Whaile]({{< ref "whaile" >}}) is shore observations, and Bashion is commercial.
All three can connect to an `SQL` server, create plots, and filter data. 
PowerBI seems a little more manual, although is beneficial in some ways: click a plot type, drag-drop features, easy changing of fonts/text size. 
Many features are locked behind accounts, exporting limitations, and I haven't considered how it may be difficult sharing PowerBI exports within organisations, e.g. do you share the **`.pbix`**, or the `.pdf`?
With `R` and `Python` I think it would be easier to version control.
Even outside of dashboards, there is `Python Jupyter` and `R Markdown` formats, which although will contain code, may be more open to others how you filtered, or calculated some new fields.

{{< figure src="/images/posts/bashion/dashboard1.png" class="figure-plain" caption="**Figure**: A screenshot of the first dashboard page." >}}

In this dashboard, I created new "measurements": revenue, total sales (row count), total units, and more.
I included filtering options on the right-hand side then added a bar plot on the left, with a feature that lets you zoom in.

```
Revenue = SUMX(
    'fake_data',
    'fake_data'[Cost] * 'fake_data'[Quantity]
)
Total Sales = COUNTROWS(fake_data)
Total Units = SUM(fake_data[Quantity])
```

Although cool, I do think this is easily achievable with `RShiny` and `Py Streamlit`, in fact you can also include other dynamic parameters to change things, e.g. plot colours, choose with features you want plotted, etc.

{{< figure src="/images/posts/bashion/dashboard2.png" class="figure-plain" caption="**Figure**: A screenshot of the second dashboard page." >}}

Here I played around with more plots, I especially enjoyed the scatter plot: this shows - although my data is fabricated - that when the cost increases, the stars/ratings also increase.
**This would be a great plot in real data!**

{{< figure src="/images/posts/bashion/dashboard3.png" class="figure-plain" caption="**Figure**: A screenshot of the third and last dashboard page." >}}

Here I chose the same plot and changed the y-axis and included some legends.
One minor limitation with PowerBI, although it automatically includes the subtitle of the y-axis and legend, the legend itself isn't automatically labelled, so it may be confusing at first what the individual bars are.

I also included a table, I tried to figure out how to add statistics (like `R`) but I think this is another limitation, perhaps locked behind the org accounts.

### Conclusion

PowerBI is great, I think the little finicky things in `R` and `Python` plotting (fonts/sizes) are slightly easier as you don't need to remind yourself the functions.
BUT with coding, you could set global params for these and ensure all plots apply.

With `R` and `Python` you could deploy dashboards internally and version control them.
Though I think you could create nice `pdf` files with PowerBI.

I found it easier to use `R`, this is probably as I have more experience (almost 10 years).
The tables I create in `R` can automatically include statistical tests, you can set loops and variables so it could run the table across any features/columns.

[**>> GitHub repository**](https://github.com/sap218/BashionI "GitHub repository for BashionI")
