# Joining to the view

This join connects to a historical table of `orders` entity

```
  join: calendar {
    view_label: "Orders"
    fields: [
      calendar_granularity,
      timeframe
    ]
    type: left_outer
    relationship: many_to_one
    sql_on: ${orders.history_date} = ${calendar.date} ;;
  }
```

It will expose one filter-field only and one dimension:

![](<../../.gitbook/assets/image (11).png>)

And by selecting them both, you can build your timeline:

![](<../../.gitbook/assets/image (13) (1).png>)

And the best thing is that we can select and filter on Timeframe and the same time, as well as the formatting in the visualisation is changed to fit the best time granularity, done automatically by the visualisation engine.
