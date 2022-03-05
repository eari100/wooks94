---
title: "[Java] LocalDate"
date: "2022-03-05"
template: "post"
draft: false
slug: "[Java]-LocalDate"
category: "Java"
tags:
  - "#Java"
description: ""
---

## 객체 생성

`of`, `parse`

```Java
LocalDate of = LocalDate.of(2022, 3, 5);

// String to LocalDate
String date = "2022-03-05";
// DateTimeFormatter.ISO_DATE는 "yyy-mm-dd"를 상수로 선언
LocalDate parse = LocalDate.parse(date, DateTimeFormatter.ISO_DATE);
```

## 날짜 사이의 간격

`Period`, `between`

```Java
LocalDate startDateTime = LocalDate.of(2020, 12, 18); 
LocalDate endDateTime = LocalDate.of(2022, 12, 20); 
Period period = Period.between(startDateTime, endDateTime); 
log.debug("Years : {}", period.getYears());  // Years : 2
log.debug("Months : {}", period.getMonths()); // Months : 0
log.debug("Days : {}", period.getDays()); // Days : 2
```

## 해당 달의 첫일, 마지막일

```Java
LocalDate date = LocalDate.of(2022, 3, 5);
date.withDayOfMonth(1); // 2022-03-01
date.lengthOfMonth(); // 31
```

## 날짜 더하기/빼기

```Java
LocalDate date = LocalDate.of(2022, 3, 5);
date.plusMonths(1);
date.plusDays(1);
date.minusMonths(1);
date.minusDays(1);
```

##### reference

[Java - String을 파싱하여 LocalDate로 변환하는 방법](https://codechacha.com/ko/java-examples-how-to-convert-string-to-localdate/)  
[[Java] LocalDate, LocalDateTime 날짜 차이 계산하기](https://cornswrold.tistory.com/489)  
[[Java] API - LocalDate withDayOfMonth() 월의 첫날과 마지막 날 가져오기](https://blog.naver.com/PostView.nhn?blogId=dktmrorl&logNo=222319652865)