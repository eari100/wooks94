---
title: "[Ajax] dataType contentType"
date: "2021-12-11"
template: "post"
draft: false
slug: "[Ajax]-dataType-contentType"
category: "Spring Framework"
tags:
  - "#Spring Framework"
  - "#Java"
description: ""
---

```JS
$.ajax({
    dataType : "json",
    contentType: "application/json; charset=utf-8",
    data : JSON.stringify(data)
})
```

+ `dataType`: 서버에서 어떤 데이터 타입을 받을 것인지 의미 (서버 -> 클라이언트)  
json, html, text 등이 있습니다.  
dataType과 서버에서 내려오는 데이터 타입이 일치 하지 않으면 `statusText 가 "parsererror"`로 나타나는 에러가 나타나게 됩니다. 

+ `contentType`: 클라이언트에서 어떤 데이터 타입을 보낼 것인지 의미 (클라이언트 -> 서버)  
application/json; charset-utf-8을 주로 사용하며  
default는 application/x-www-form-urlencoded; charset=utf-8 입니다.

##### reference

[Ajax 요청에서 dataType 과 contentType은 뭐가 다른걸까?](https://velog.io/@cksdnr066/Ajax-%EC%9A%94%EC%B2%AD%EC%97%90%EC%84%9C-dataType-%EA%B3%BC-contentType%EC%9D%80-%EB%AD%90%EA%B0%80-%EB%8B%A4%EB%A5%B8%EA%B1%B8%EA%B9%8C)  
[ajax 에서의 parsererror 에러](https://blog.hajs.me/159)  