---
description: Guidelines for different types of fields, their naming, attributes, and style
---

# Fields

### Order of parameters

* hidden: yes|no
* UI-related descriptive parameters
  * label
  * group\_label
  * group\_item\_label
  * description
* type
* type-related parameters
  * timeframes
  * intervals
  * tiers
* sql
* sql-related parameters
  * sql\_distinct_\__key
  * sql\_start
  * sql\_end
  * sql\_latitude
  * sql\_longitude
* value\_format_\__name | value\_format

### One field as both Dimension and Measure

Measure is an aggregation, whereas Dimension is categorical data. Sometimes, we need the same column, especially if it has numerical value, to be available as both types.

In this case you can create a dimension and use it in the measure using the naming convention

```
dimension: revenue {
  type: number
  sql: ${TABLE}.revenue ;;
}

measure: total_revenue {
  type:sum
  sql: ${revenue_dim} ;;
}
```



