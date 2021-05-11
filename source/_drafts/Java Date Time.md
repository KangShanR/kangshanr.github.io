---
title: Java Date Time
tags: [Date, LocalDateTime, Instant]
date: 2021-05-11 21:59:00
---

- Java 1.8 不再使用 TimeZone/Calander/Date 等日期时间类，转而使用 LocalDateTime/ZoneId（ZoneOffset/ZoneRegion）/ZonedDateTime 来替代。
    - 带时区时间 ZonedDateTime 可以直接使用方法切换时区: 时刻保持相同 .withZoneSameInstant(ZoneId)，时间保持一致 .withZoneSameLocal(ZoneId)
