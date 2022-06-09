``` sql
with events as (
  select 
    "First Event" as event_name, 
    cast(
      '2020-01-01 10:10:10' as datetime
    ) as date, 
    2 as expected_upcoming_event_days 
  union all 
    (
      select 
        "Fourth Event", 
        cast(
          '2020-01-07 10:10:10' as datetime
        ), 
        null
    ) 
  union all 
    (
      select 
        "Second Event", 
        cast(
          '2020-01-03 10:10:10' as datetime
        ), 
        3
    ) 
  union all 
    (
      select 
        "Third Event", 
        cast(
          '2020-01-06 10:10:10' as datetime
        ), 
        1
    )
) 
select 
  *, 
  datediff(
    lead(date, 1, date) over (
      order by 
        date
    ), 
    date
  ) as days_to_next_event, 
  datediff(
    date, 
    lag(date, 1, date) over (
      order by 
        date
    )
  ) as days_since_previous_event 
from 
  events 
order by 
  date;

```
