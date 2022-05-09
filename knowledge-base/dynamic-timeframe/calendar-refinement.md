# Calendar Refinement

This refinement adds a parameter on unquoted type to your calendar. It tells the dimension, which field from the calendar table to select.&#x20;

```
view: +calendar {
  parameter: calendar_granularity {
    label: "Granularity"
    description: "Unquoted - selects calendar column dynamically"
    type: unquoted
    allowed_value: {
      label: "Day"
      value: "date"
    }
    allowed_value: {
      label: "Week"
      value: "first_day_of_week"
    }
    allowed_value: {
      label: "Month"
      value: "first_day_of_month"
    }
    allowed_value: {
      label: "Quarter"
      value: "first_day_of_quarter"
    }
    allowed_value: {
      label: "Year"
      value: "first_day_of_year"
    }
    default_value: "date"
  }

  dimension: timeframe {
    type: date
    allow_fill: no
    label_from_parameter: calendar_granularity
    sql: ${TABLE}.{% raw %}
{% parameter calendar_granularity %}
{% endraw %} ;;
    convert_tz: no
  }
}

```
