# Visualisations

### Q_uick Percent of Previous_

If you want to quickly know the **% of previous bar**, you can enable this setting in visualisation settings **`Show Percent of Previous`**It will create arrows overlaying bar chart. Just remember it's the % of the previous bar, not the difference

![](<../../.gitbook/assets/image (2) (1).png>)

### _Put it in a bin or a bucket_

Put in a bin ![:put\_litter\_in\_its\_place:](https://slack-imgs.com/?c=1\&o1=gu\&url=https%3A%2F%2Fa.slack-edge.com%2Fproduction-standard-emoji-assets%2F13.0%2Fgoogle-medium%2F1f6ae.png) or a bucket ![:bucket:](https://slack-imgs.com/?c=1\&o1=gu\&url=https%3A%2F%2Fa.slack-edge.com%2Fproduction-standard-emoji-assets%2F13.0%2Fgoogle-medium%2F1faa3.png) !Some dimensions are already provided for you in a way of **bins**/**tiers**/**buckets**, like **Outstanding Book Value Tier** or **Delinquency Tier**. We prefer to define them for you beforehand, so that the whole business uses **the same tiers**.But wait?! Sometimes one may want to have different buckets for a good reason. What do we do? Don't fret, we have a way of creating tiered dimension from the Explore:

1. Select your dimension - run your query
2. Click the gear ![:gear:](https://slack-imgs.com/?c=1\&o1=gu\&url=https%3A%2F%2Fa.slack-edge.com%2Fproduction-standard-emoji-assets%2F13.0%2Fgoogle-medium%2F2699-fe0f.png) icon in the header of your dimension and click **Bin**
3. Select the bin size (it's only for regularly sized bins), minimum and maximum value
4. Click `Get Field Info` if you want to see the **MIN** and **MAX** values for your dimension. Click Save
5. New dimension will show up in your data table
6. But wait, the data hasn't been grouped ![:shocked\_face\_with\_exploding\_head:](https://slack-imgs.com/?c=1\&o1=gu\&url=https%3A%2F%2Fa.slack-edge.com%2Fproduction-standard-emoji-assets%2F13.0%2Fgoogle-medium%2F1f92f.png) . That's because we have to remove the original dimension as it has lower granularity. Click the gear icon of the original dimension and remove it.
7. Re-run your query and enjoy your aggregated data by bins.

Now, you have buckets of new information! ![:boom:](https://slack-imgs.com/?c=1\&o1=gu\&url=https%3A%2F%2Fa.slack-edge.com%2Fproduction-standard-emoji-assets%2F13.0%2Fgoogle-medium%2F1f4a5.png)

![](<../../.gitbook/assets/image (3) (1).png>)![](<../../.gitbook/assets/image (1) (1).png>)![](<../../.gitbook/assets/image (6).png>)![](<../../.gitbook/assets/image (12).png>)![](<../../.gitbook/assets/image (14).png>)

### _Row Totals excluded from Conditional Formatting_

Good ole' Excel got us accustomed to conditional formatting rpetty badly. In Looker, you can also use them when visualising a table. Just go to Settings --> Formatting --> Enable Conditional Formatting.

One downside is if you select to show Row Totals in your Data Table, they will be taken into account for conditional formatting - it won't change the scale though but it will be visualised according to the sale of the raw data. In order to bypass this, create a table calculation that has the same result: `sum(pivot_row(${your_measure}))` and it will be excluded from conditional formatting.

![](<../../.gitbook/assets/image (5).png>)

![](<../../.gitbook/assets/image (9).png>)

As you can see, the Row Total is formatted but it doesn't change the scale. the highest point 2768 is still green. This becomes more of a visual preference that some people treat as a bug because it's all about visuals with conditional formatting.



### _Transposing List of Measures_

Imagine you only have a list of measures, it could be some financial metrics, average times or counts (but not for funnel) and you would like to simply show it as a two-column table.

In this example I will use Hubspot Deals data and their average time between some stages. I have only one row of measures like this:

![](<../../.gitbook/assets/image (2).png>)

Without a dimension, when using Transpose functionality, Looker will throw an error:

![](../../.gitbook/assets/image.png)

In order to go around it, we can create a Custom Dimension with empty string as a value:

![](<../../.gitbook/assets/image (8).png>)

Now, you can click Transpose in the settings of Table visualisation and a two-column table will be created. As you can see, our Custom Dimension is nowhere to be seen.

![](<../../.gitbook/assets/image (3).png>)

