# Calendar Table

The main prerequisite is the one and mighty `calendar` table. Having a table that is retransformed every day and containing all the necessary details about every date is useful in many ways.&#x20;

Calendar table is only used in a JOIN. You join it to the date/timestamp/datetime on which you want to perform dynamic timeframes. Everything happens in calendar view and you don't have to add anything else to your source view.

:exclamation:This table has be transformed daily as it contains very useful columns called `*_offset` that change every day.&#x20;

### Snowflake

```sql
WITH calendar_base AS 
  SELECT
      -- Day
      date,
      DATEDIFF(DAY, CURRENT_DATE(), date)                                 AS day_offset,
      DAYOFWEEKISO(date) < 6                                              AS is_weekday,

      -- Day + Week
      DAYOFWEEK(date)                                                     AS day_of_week,
      DAYOFWEEKISO(date)                                                  AS iso_day_of_week,
      DATE_TRUNC(WEEK, date)                                              AS first_day_of_week,
      DATEADD(DAY, 6, DATE_TRUNC(WEEK, date))                             AS last_day_of_week,
      date = first_day_of_week                                            AS is_start_of_week,
      date = last_day_of_week                                             AS is_end_of_week,

      -- Day + Month
      DAYOFMONTH(date)                                                    AS day_of_month,
      COUNT(date) OVER(PARTITION BY DATE_TRUNC(MONTH, date))              AS days_in_month,
      DATE_TRUNC(MONTH, date)                                             AS first_day_of_month,
      LAST_DAY(date)                                                      AS last_day_of_month,
      FLOOR((EXTRACT(DAY FROM date) + 6) / 7)                             AS day_of_week_in_month,
      date = first_day_of_month                                           AS is_start_of_month,
      date = last_day_of_month                                            AS is_end_of_month,

      -- Day + Quarter
      DATE_TRUNC(QUARTER, date)                                           AS first_day_of_quarter,
      LAST_DAY(date, QUARTER)                                             AS last_day_of_quarter,
      RANK() OVER(PARTITION BY DATE_TRUNC(QUARTER, date) ORDER BY date)   AS day_of_quarter,
      COUNT(date) OVER(PARTITION BY DATE_TRUNC(QUARTER, date))            AS days_in_quarter,
      date = first_day_in_quarter                                         AS is_start_of_quarter,
      date = last_day_of_quarter                                          AS is_end_of_quarter,

      -- Day + Year
      DAYOFYEAR(date)                                                     AS day_of_year,
      DATE_TRUNC(YEAR, date)                                              AS first_day_of_year,
      LAST_DAY(date, YEAR)                                                AS last_day_of_year,
      COUNT(date) OVER(PARTITION BY DATE_TRUNC(YEAR, date))               AS days_in_year,
      date = first_day_of_year                                            AS is_start_of_year,
      date = last_day_of_year                                             AS is_end_of_year,

      -- Week
      WEEK(date)                                                          AS week,
      WEEKISO(date)                                                       AS iso_week,
      DATEDIFF(WEEK, CURRENT_DATE(), date)                                AS week_offset,

      -- Month
      MONTH(date)                                                         AS month,
      DATEDIFF(MONTH, CURRENT_DATE(), date)                               AS month_offset,
      TO_VARCHAR(date, 'YYYY-MM')                                         AS month_year,

      -- Month + Quarter
      DENSE_RANK() OVER(PARTITION BY DATE_TRUNC(QUARTER, date) ORDER BY DATE_TRUNC(MONTH, date)) AS month_in_quarter,

      -- Quarter
      EXTRACT(QUARTER FROM date)                                          AS quarter,
      DATEDIFF(QUARTER, CURRENT_DATE(), date)                             AS quarter_offset,
      TO_VARCHAR(DATE_TRUNC(QUARTER, date), 'YYYY-MM')                    AS quarter_ym,

      -- Year
      EXTRACT(YEAR from date)                                             AS year,
      DATEDIFF(YEAR, CURRENT_DATE(), date)                                AS year_offset,
      MOD(EXTRACT(YEAR FROM date), 4) = 0                                 AS is_leap_year
  FROM (
    SELECT
      DATEADD(DAY, '+' || SEQ4(), '2020-01-01'::DATE)::DATE AS date
    FROM
      TABLE (
        GENERATOR(ROWCOUNT => 3652)
      )
  )
),

calendar AS (
  SELECT
    *,
    IFF(is_weekday, (COUNT(IFF(is_weekday, date, NULL)) OVER(PARTITION BY first_day_of_month ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)), NULL) AS workday_in_month,
    IFF(NOT is_weekday, (COUNT(IFF(is_weekday, NULL, date)) OVER(PARTITION BY first_day_of_month ORDER BY date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)), NULL) AS weekend_day_in_month
  FROM calendar_base
)

SELECT
  date,
  day_offset,
  day_of_week,
  iso_day_of_week,
  first_day_of_week,
  last_day_of_week,
  day_of_month,
  days_in_month,
  first_day_of_month,
  last_day_of_month,
  day_of_week_in_month,
  first_day_of_quarter,
  last_day_of_quarter,
  day_of_quarter,
  days_in_quarter,
  day_of_year,
  first_day_of_year,
  last_day_of_year,
  days_in_year,
  week,
  iso_week,
  week_offset,
  month,
  month_offset,
  month_year,
  month_in_quarter,
  quarter,
  quarter_offset,
  quarter_ym,
  year,
  year_offset,
  is_leap_year,
  workday_in_month,
  weekend_day_in_month,
  MAX(workday_in_month) OVER(PARTITION BY first_day_of_month) AS working_days_in_month,
  MAX(weekend_day_in_month) OVER(PARTITION BY first_day_of_month) AS weekend_days_in_month
FROM calendar
;
```
