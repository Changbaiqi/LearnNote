---
title: Java日期类使用技巧
date: 2023-05-19 01:54:48
author: 长白崎
categories:
  - ["数据结构与算法"]
  - ["Java"]
tags:
  - "算法"
  - "Java"
  - "日期使用技巧"
---



# Java日期类使用技巧

---

## 说明：

> Java特有的日期类的使用

## Java代码模板：

> ```java
> import java.time.Duration;
> import java.time.LocalDate;
> import java.time.LocalDateTime;
> import java.time.temporal.ChronoUnit;
> 
> 
> public class DateJudge {
>     public static void main(String[] args) {
> 
>         LocalDateTime nowDateTime = LocalDateTime.of(2022, 12, 6, 12, 36,25);
>         // 2.1 int getYear()：获取年份字段。
>         System.out.println(nowDateTime.getYear()); //2022
> 
>         // 2.2 Month getMonth()/ int getMonthValue()：获取时间的月份。
>         System.out.println(nowDateTime.getMonth()); // DECEMBER
>         System.out.println(nowDateTime.getMonthValue()); //12
> 
>         // 2.3 int getDayOfMonth()：获取日期在当月中的天数。
>         System.out.println(nowDateTime.getDayOfMonth()); //6
> 
>         // 2.4 DayOfWeek getDayOfWeek()：获取日期属于当前周的第几天
>         System.out.println(nowDateTime.getDayOfWeek()); //TUESDAY
>         System.out.println(nowDateTime.getDayOfWeek().getValue()); //2
> 
>         // 2.5  int getDayOfYear()：获取日期时间在当前年的天数。
>         System.out.println(nowDateTime.getDayOfYear()); //340
> 
>         //字符串转LocalDateTime
>         LocalDateTime localDateTime = LocalDateTime.parse("2021-01-28 13:12:11", DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
> 
> 
>         // 3.1 int getHour()：获取当前时间的小时数。
>         System.out.println(nowDateTime.getHour()); //12
>         // 3.2 int getMinute()：获取当前时间的分钟。
>         System.out.println(nowDateTime.getMinute()); //36
>         // 3.3  int getSecond()：获取当前时间秒数。
>         System.out.println(nowDateTime.getSecond()); //25
> 
>         LocalDateTime tempTime = LocalDateTime.of(2021, 7, 6, 11, 36,25);
>         System.out.println(tempTime); //2021-07-06T11:36:25
>         System.out.println("x = 1年后的日期时间"+tempTime.plusYears(1)); //x = 1年后的日期时间2022-07-06T11:36:25
>         System.out.println("x = 1月后的日期时间"+tempTime.plusMonths(1)); //x = 1月后的日期时间2021-08-06T11:36:25
>         System.out.println("x = 1周后的日期时间"+tempTime.plusWeeks(1)); //x = 1周后的日期时间2021-07-13T11:36:25
>         System.out.println("x = 1天后的日期时间"+tempTime.plusDays(1)); //x = 1天后的日期时间2021-07-07T11:36:25
>         System.out.println("x = 1小时后的日期时间"+tempTime.plusHours(1)); //x = 1小时后的日期时间2021-07-06T12:36:25
>         System.out.println("x = 1分钟后的日期时间"+tempTime.plusMinutes(1)); //x = 1分钟后的日期时间2021-07-06T11:37:25
> 
> 
>         System.out.println("x = 1年前的日期时间"+tempTime.minusYears(1)); //x = 1年前的日期时间2020-07-06T11:36:25
>         System.out.println("x = 1月前的日期时间"+tempTime.minusMonths(1)); //x = 1月前的日期时间2021-06-06T11:36:25
>         System.out.println("x = 1周前的日期时间"+tempTime.minusWeeks(1)); //x = 1周前的日期时间2021-06-29T11:36:25
>         System.out.println("x = 1天前的日期时间"+tempTime.minusDays(1)); //x = 1天前的日期时间2021-07-05T11:36:25
>         System.out.println("x = 1小时前的日期时间"+tempTime.minusHours(1)); //x = 1小时前的日期时间2021-07-06T10:36:25
>         System.out.println("x = 1分钟前的日期时间"+tempTime.minusMinutes(1)); //x = 1分钟前的日期时间2021-07-06T11:35:25
> 
> 
>         LocalDateTime tempTime1 = LocalDateTime.of(2021, 7, 6, 11, 36,25);
>         LocalDateTime tempTime2 = LocalDateTime.of(2021, 8, 6, 12, 36,25);
>         // 5.1 判断tempTime1是否在tempTime2之后，是否比其更晚
>         boolean after = tempTime1.isAfter(tempTime2); // false
>         // 5.2 判断tempTime1是否在tempTime2之前，是否比其更早
>         boolean before = tempTime1.isBefore(tempTime2); //true
>         // 5.3 判断tempTime1是否与tempTime2相等
>         boolean equal = tempTime1.isEqual(tempTime2); // false
> 
> 
>         LocalDateTime changeBeforeTime = LocalDateTime.of(2021, 7, 6, 11, 36,25);
>         System.out.println("修改年份后的日期" + changeBeforeTime.withYear(2022)); // 修改年份后的日期2022-07-06T11:36:25
>         System.out.println("修改月份后的日期" + changeBeforeTime.withMonth(11)); // 修改月份后的日期2021-11-06T11:36:25
>         System.out.println("修改当月天数后的日期" + changeBeforeTime.withDayOfMonth(11)); // 修改当月天数后的日期2021-07-11T11:36:25
>         System.out.println("修改当年天数后的日期" + changeBeforeTime.withDayOfYear(11)); // 修改当年天数后的日期2021-01-11T11:36:25
>         System.out.println("修改小时后的日期" + changeBeforeTime.withHour(10)); // 修改小时后的日期2021-07-06T10:36:25
>         System.out.println("修改分钟.秒后的日期" + changeBeforeTime.withMinute(10).withSecond(10)); //修改分钟.秒后的日期2021-07-06T11:10:10
> 
> 
> 
> 
> 
> 
>         LocalDateTime beginTime = LocalDateTime.of(2021, 7, 6, 11, 36,25);
>         LocalDateTime endTime = LocalDateTime.of(2021, 9, 8, 01, 56,25);
>         // 相差的年份数
>         long betweenYears = ChronoUnit.YEARS.between(beginTime, endTime);
>         System.out.println(betweenYears);  // 0
>         // 相差的月份数
>         long betweenMonths = ChronoUnit.MONTHS.between(beginTime, endTime);
>         System.out.println(betweenMonths);  // 2
>         // 相差的天数
>         long betweenDays = ChronoUnit.DAYS.between(beginTime, endTime);
>         System.out.println(betweenDays);  // 63
>         // 相差的小时数
>         long betweenHours = ChronoUnit.HOURS.between(beginTime, endTime);
>         System.out.println(betweenHours); //1526
>         // 相差的分钟数
>         long betweenMinutes = ChronoUnit.MINUTES.between(beginTime, endTime);
>         System.out.println(betweenMinutes);//91580
>         // 相差的秒钟数
>         long betweenMillis = ChronoUnit.MILLIS.between(beginTime, endTime);
>         System.out.println(betweenMillis);//91580
> 
> 
> 
> 
>         Duration between1 = Duration.between(beginTime, endTime);
>         System.out.println(between1.toDays()); // 63
>         System.out.println(between1.toHours()); //1526 (63天*24小时+(24+1-11)不是整天的小时数)
>         System.out.println(between1.toMinutes()); // 91580 (1526小时*60分钟+（56-25）分钟外相差的秒数)
>         System.out.println(between1.toMillis()); // 5494800000
> 
>     }
> 
> }
> ```