# Dynamic Timeframe

Standard dynamic timeframe solution described here: [https://help.looker.com/hc/en-us/articles/360001288468-Dynamic-Timeframes-for-Dimension-Groups](https://help.looker.com/hc/en-us/articles/360001288468-Dynamic-Timeframes-for-Dimension-Groups) has its drawbacks. Because the resulting dimension is `string`, it means it won't get an automatic formatting from Looker, i.e. `January` instead of `2022-01-01`

However, there is a way that allows us to use dynamic timeframe and granularity, and also adds missing features:

1. You can filter using `timeframe` field rather than the field you're joining to
2. Dimension is returned as `date`, which means its recognised as time series
3. Application of the code can be simplified so that you don't need to copy it everywhere

### Steps

High-level steps are as follows:

1. Expose your calendar table as view `calendar.view.lkml`
2. Create a refinement of calendar view called `parameters.view.lkml`
3. Join the calendar to your view
4. Change view\_label of the calendar join to your main view's name
5. Enjoy

### Things to remember:

1. In order to be able to use Timeframe with other values like Day of Week or Week of Year, the code would have to be altered.
