---
title: Java Date Time
tags: [Date, LocalDateTime, Instant]
date: 2021-05-11 21:59:00
---

- Java 1.8 不再使用 TimeZone/Calander/Date 等日期时间类，转而使用 LocalDateTime/ZoneId（ZoneOffset/ZoneRegion）/ZonedDateTime/Instant 来替代。
    - 带时区时间 ZonedDateTime 可以直接使用方法切换时区: 时刻保持相同 .withZoneSameInstant(ZoneId)，时间保持一致 .withZoneSameLocal(ZoneId)
    - Instant 不包含时区概念。表示一个时刻， 这个时刻在不同的时区其时间不一样。
        - 由同一个 LocalDateTime.toInstant(ZoneOffset) 转化为 instant 表示不一样的时刻
            - 由同一个 Date 通过 toInstant() 转成的 instant ，通过 ZonedDateTime.ofInstant(Instant, ZoneId) 可以表示同一个时刻在不同时区的时间。而使用不同时区的 toEchoMilli() 方法转换出来的秒数只会是同一个值，因为其 instant 是恒定的。
