---
title: java8 LocalDate, LocalTime, LocalDateTime, etc... Tutorial
layout: single
categories: blog
tags: [study, java8, LocalDate, LocalDateTime, LocalTime]
comments: true
share: true
excerpt: Time class using tutorial (LocalDate, LocalTime, LocalDateTime)
---
## LocalDate

### 1. How to get today's date

```java
    LocalDate today = LocalDate.now();
    System.out.println("Today is " + today);

    ---
    result:
    Today is 2017-02-23
```

### 2. How to get seperated date info

```java
    LocalDate date = LocalDate.of(2017, 02, 23);
    int year = date.getYear();
    Month month = date.getMonth();
    int day = date.getDayOfMonth();
    DayOfWeek dow = date.getDayOfWeek();
    int len = date.lengthOfMonth();
    boolean leaf = date.isLeapYear;

    System.out.println("Date is "+ date);
    System.out.println("Year is "+ year);
    System.out.println("Month is "+ month);
    System.out.println("Day is "+ day);
    System.out.println("DayOfWeek is "+ dow);
    System.out.println("Len is "+ len);
    System.out.println("Leaf is "+ leaf);

    ---
    result:
    Date is 2017-02-23
    Year is 2017
    Month is FEBRUARY
    Day is 23
    DayOfWeek is THURSDAY
    Len is 28
    Leaf is false

```


## LocalTime

### 1. How to get current time

```java
    LocalTime currentTime = LocalTime.now();
    System.out.println("now time is " + currentTime);

    ---
    result:
    now time is 01:21:47.905
```

### 2. How to get seperated time info

```java
    LocalTime time = LocalTime.now();
    System.out.println("now time is " + time);

    int hour = time.getHour();
    int minute = time.getMinute();
    int second = time.getSecond();
    System.out.println("Hour is "+ hour);
    System.out.println("Minute is "+ minute);
    System.out.println("Second is "+ second);

    ---
    result:
    now time is 01:25:40.520
    Hour is 1
    Minute is 25
    Second is 40
```



## LocalDateTime

## Transfer formatting
[Predefined Formatters](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)

### 1. From LocalDate to String

```java
    LocalDate date = LocalDate.now();
    System.out.println(date.format(DateTimeFormatter.BASIC_ISO_DATE));
    System.out.println(date.format(DateTimeFormatter.ofPattern("yyyy-MM-dd")));

    ---
    result:    
    20170223
    2017-02-23
```

### 2. From String to LocalDate

```java
    System.out.println(LocalDate.parse("2017-02-23"));
    System.out.println(LocalDate.parse("20170223", DateTimeFormatter.BASIC_ISO_DATE));

    ---
    result:
    2017-02-23
    2017-02-23
```

### 3. From LocalDate to java.sql.Date

```java
    System.out.println(Date.valueOf(LocalDate.of(2017, 2, 23)));

    ---
    result:
    2017-02-23
```

### 4. From LocalDate to java.util.Date

```java
    Date date = Date.from(LocalDate.of(2017, 02, 23)
        .atStartOfDay(ZoneId.systemDefault())
        .toInstant()));
    System.out.println(date);

    ---
    result:
    Thu Feb 23 00:00:00 KST 2017    
```


### 5. From LocalDate to Timestamp

```java
    LocalDate localDate = LocalDate.now();
    Timestamp timestamp = Timestamp.valueOf(localDate.atStartOfDay());
    System.out.println("Timestamp: " + timestamp);

    ---
    result:
    Timestamp: 2017-02-23 00:00:00.0
```



### 6. From LocalDateTime to String

```java
    LocalDateTime localDateTime = LocalDateTime.now();
    System.out.println(localDateTime.format(DateTimeFormatter.BASIC_ISO_DATE));
    System.out.println(localDateTime.format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss")));

    ---
    result:
    20170223
    2017-02-23 02:22:34
```

### 7. From String to LocalDateTime

```java
    LocalDateTime localDateTime1 = LocalDateTime.parse("2017-02-23T10:15:30");
    LocalDateTime localDateTime2 = LocalDateTime.parse("2017-02-23 10:15:30",
                        DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));

    System.out.println(localDateTime1);
    System.out.println(localDateTime2);

    ---
    result:
    2017-02-23T10:15:30
    2017-02-23T10:15:30
```

### 8. From LocalDateTime to java.util.Date

```java
    Date date = Date.from(LocalDateTime.now()
                        .atZone(ZoneId.systemDefault())
                        .toInstant());
    System.out.println(dateFromDatetime);

    ---
    result:
    Fri Feb 24 02:36:43 KST 2017
```

### 9. From LocalDateTime to java.sql.Timestamp

```java
    LocalDateTime localDateTime = LocalDateTime.now();
    Timestamp timestamp = Timestamp.valueOf(localDateTime);
    System.out.println("Timestamp: " + timestamp);

    ---
    result:
    Timestamp: 2017-02-23 02:18:50.244
```

---
[reference: Datetime](https://docs.oracle.com/javase/tutorial/datetime/iso/overview.html)


[reference: DateTimeFormatter](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html)





