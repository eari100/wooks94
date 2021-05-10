---
title: "[machbase JPA] hibernate example 나름대로 분석 해보기"
date: "2021-05-09"
template: "post"
draft: false
slug: "[machbase-JPA]-hibernate-example-나름대로-분석-해보기"
category: "machbase"
tags:
  - "#machbase"
  - "#JPA"
  - "#Spring Framework"
  - "#Java"
description: ""
---

짧은 견해로 쓴 글이기 때문에 사실과는 많이 다를 수 있습니다.  
개인적인 학습 의도로 작성되었습니다. 혹시 수정해야 될 부분이 있다면 의견 부탁드립니다.

### JDBC

![machbase-JPA-analysis-01.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-01.png)

com.machbase.jdbc는 maven repository에 존재하지 않기 때문에 machbase를 설치 시 포함되어 있는 machbase.jar을 참조하도록 설정했습니다.

![machbase-JPA-analysis-02.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-02.png)
*machbase.jar 경로 (Window)*

![machbase-JPA-analysis-03.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-03.png)
*machbase.jar 경로 (linux)*

### LogApplication.java

![machbase-JPA-analysis-04.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-04.png)

LogApplication는 CommandLineRunner를 구현했기 때문에 프로젝트를 빌드하면 run메서드를 실행하게 되어 있습니다.

JpaRepository 레파지토리 메서드를 사용하는 부분입니다.
![machbase-JPA-analysis-05.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-05.png)
*saveAll()*

![machbase-JPA-analysis-06.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-06.png)
*save()*

![machbase-JPA-analysis-07.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-07.png)
*findAll()*

![machbase-JPA-analysis-08.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-08.png)
*findById()*

JpaRepository에서 제공하지 않는 메서드도 역시 정의해서 사용가능 합니다.

![machbase-JPA-analysis-09.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-09.png)

![machbase-JPA-analysis-10.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-10.png)
*findByName()*

### MachbaseDialect.java

[org.hibernate.dialect](https://docs.jboss.org/hibernate/orm/5.2/javadocs/org/hibernate/dialect/package-summary.html)에서 살펴보니 hibernate에서는 MachbaseDialect를 제공하지 않는 것 같습니다.  
대신 프로젝트에 포함되어 있는 MachbaseDialect.java를 사용하고 있습니다.
`Oracle8iDialect` 과 흡사한 부분이 많습니다.

#### AbstractLimitHandler

limit절 처리를 하는 것 처럼 보입니다.

![machbase-JPA-analysis-11.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-11.png)

#### SQL_STATEMENT_TYPE_PATTERN

![machbase-JPA-analysis-12.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-12.png)

대소문자 구분 없이 정규표현식을 통해 SELECT, INSERT, DELETE 구문을 찾는 역할인 것 같습니다.

`Pattern.CASE_INSENSITIVE` : 대소문자 구분 하지 않음

#### register(Type)Mappings

문자형, 숫자형, 날짜 등 machbase의 컬럼의 타입을 지정해주었습니다.

![machbase-JPA-analysis-13.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-13.png)

![machbase-JPA-analysis-14.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-14.png)

#### registerFunctions

machbase에서 사용하는 함수를 지정해주었습니다.

![machbase-JPA-analysis-15.png](/media/posts/2021-05-09---machbase-JPA-analysis/machbase-JPA-analysis-15.png)