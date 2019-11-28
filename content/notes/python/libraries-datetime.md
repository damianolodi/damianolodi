---
title: Time and Datetime
linktitle: Datetime
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Packages
    weight: 2

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

```py
from datetime import datetime as dt
from datetime import date
import time
```

## The `datetime` Submodule

- `dt_obj = dt.datetime(yyyy, mm, dd, hh, mm, ss, ususus)` &rarr; create datetime object. Don't use leading zeros.
    - `dt_obj.date()`, `dt_obj.time()`, `dt_obj.year`, `...` &rarr; acess onyl part of the object.
- `dt.datetime.today()` &rarr; return **current local date and time** with a timezone specified as _None._
- `dt.datetime.now()` &rarr; return **current local date and time** in standard format. _It has a parameter to specify the time zone._
    - `dt.datetime.now().time()` &rarr; return only the date.
- `dt_obj.strftime('%Y-%m-%d %H:%M:%S.%f')` &rarr; format the datetime object and return a string.
- `dt.datetime.strptime('date time', 'format')` &rarr; convert a string into a datetime object following the provided format.

---
## The `date` Submodule

- `dt.date(yyyy, mm, dd)` &rarr; **create date**. Don't use leading zeros.
- `tday = dt.date.today()` &rarr; get **current global date:**
    - `tday.year`, `tday.month`, `tday.day` &rarr; get only part of the date;
    - `tday.isoweekday()` &rarr; get day of the week (monday is 1, sunday is 7);
    - `tday.weekday()` &rarr; get day of the week (monday is 0, sunday is 6).

---
## The `time` Submodule

- `t = dt.time(hh, mm, ss, ususus)` &rarr; create a time object.
    - `t.hour`, `t.minutes`, `t.seconds`, `...` &rarr; acess only part of the object.

---
## The `timedelta` Submodule

Timedeltas objects can be added/subtracted to dates, or they can be obtained subtracting two datetimes.

- `td = dt.timedelta(days=7)` &rarr; create a timedelta object long 7 days
- `td.days` &rarr; return only the "days" of the timedelta object
- `td.total_seconds()` &rarr; convert timedelta object in seconds

---

## The `pytz` Module

```py
import datetime as dt
import pytz
```
{{% alert note %}}
It is recommended to use this when working with timezones 
{{% /alert %}}

- `dt_obj = dt.datetime(yyyy, mm, dd, hh, mm, ss, tzinfo=pytz.UTC)` &rarr; create a _timezone-aware_ datetime object.
- `dt_utc = dt.datetime.now(tz=pytz.UTC)` &rarr; get current _timezone-aware_ date and time.
- `dt_utc.astimezione(pytz.timezone('timezone/name'))` &rarr; convert to a different timezone. To see al timezone names see the doc or print the *pytz.all_timezones* list.

Convert "naive" datetimes into _timezone-aware_ ones using:
```py
    my_dt = dt.datetime.now() # 'naive' datetime object
    my_timezone = pytz.timezone('timezone/name')
    dt_with_timezone = my_timezone.localize(my_dt)
```

---
## The `dateutil` Module

This module expands the `datetime` module.

To check if a string represent a date use:

```py
    from dateutil.parser import parse

    try:
        parse(string, fuzzy=False)
    except ValueError:              # it is not a date
        pass
```

---
## The `time` Package

- `time.perf_counter()` &rarr; return value (in seconds) of a performance counter (calcualted from the highest clock). Used to measure performance. The single value is useless, it is required to make difference between consecutive results ([doc](https://docs.python.org/3/library/time.html#time.perf_counter)).

---
## Resources

- [Package documentation](https://docs.python.org/3/library/datetime.html)
- [`dateutil` docs](https://dateutil.readthedocs.io/en/stable/)

