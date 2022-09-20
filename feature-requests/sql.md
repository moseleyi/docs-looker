# SQL

Window functions in SQL

Not being able to use window functions is very limiting. It usually shows up when we have a count per id but want to also use aggregation of a higher granularity.

Right now, we would have to pre-aggregate it, which goes against the Looker's principle of aggregating measure on the fly. Changing week to month, or a quarter, should work automatically.

&#x20;Now the only ways is to use complicated hacks in Table Calculations:

{% embed url="https://community.looker.com/explores-36/using-table-calculation-functions-and-operators-to-create-window-functions-20622" %}

Keeping many pre-aggregated values in the raw data or joining to a specific timeframe-based aggregation tables.

Two ways this could be achieved:

```
measure: total_value_by_country {
  type: sum
  sql: ${total_value_dim} ;;
  partition: ${country_id}
}
```
