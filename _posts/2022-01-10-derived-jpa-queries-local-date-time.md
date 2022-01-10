---
layout: post
title: Fetch data usinbg derived queries and DateTime.now()
---


## DateTime.now()

- get date time now:
The bellow method returns a date time object in the given moment in the time zone of the system
(e.g. when the programm executes the method).

```java
DateTime.now();
```


The time zone is the time zoen of the system.

- get time in a specifid time zone:

```java
ZoneId asiaSingapore = ZoneId.of("Asia/Singapore");
DateTime.now(asiaSingapore);
```

## How to fetch data based on a given date time (using derived queries)

example:

```java
public class Item {

  @Embedded
  private Created created;

  // getters and setters...
}


@Embeddable
class Created {

    @Column(name = COLUMN_CREATED_AT)
    private Date at;

  }
```

query base on time:

```java

List<Item> findByCreatedAtAfter(Date time);

```

fetches all the items after the given time (this date is a java.util.Date instance).
