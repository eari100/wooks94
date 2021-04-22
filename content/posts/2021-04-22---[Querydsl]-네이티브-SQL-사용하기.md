---
title: "[Querydsl] 네이티브 SQL 사용하기"
date: "2021-04-22"
template: "post"
draft: false
slug: "[Querydsl]-네이티브-SQL-사용하기"
category: "Querydsl"
tags:
  - "#Querydsl"
  - "#Spring Framework"
  - "#Java"
description: ""
---

Spring Framework에서 Querydsl을 사용할때 네이티브 SQL을 사용하기 위한 세팅을 해보겠습니다.

### gradle 추가

```groovy
implementation group: 'com.querydsl', name: 'querydsl-sql', version: '4.4.0'
```

### JPASQLQuery

JPASQLQuery 클래스를 사용하면 JPA의 네이티브 SQL을 Querydsl에서 사용할 수 있습니다.  
Spring 프로젝트의 어디서든지 사용하기 위해 `@bean`으로 등록합니다.
![ex-01.png](/media/posts/2021-04-22---[Querydsl]-use-native-sql/ex-01.png)
원하는 SQLTemplates에 맞춰 코드를 작성하면 됩니다.  
저는 H2 데이터베이스를 사용했습니다.

### SQLExpressions

SQLExpressions를 사용하여 native SQL을 작성하면 됩니다.
![ex-02.png](/media/posts/2021-04-22---[Querydsl]-use-native-sql/ex-02.png)
match라는 도메인에 `row_number() over ()`라는 sql을 사용하여 순번을 매기도록 합니다.

### 테스트 코드
![ex-03.png](/media/posts/2021-04-22---[Querydsl]-use-native-sql/ex-03.png)
20건의 match 기록을 저장 후 조회해봅니다.  

```bash
Hibernate: 
    select
        match.game_id as game_id1_0_0_,
        match.game_creation as game_cre2_0_0_,
        match.game_duration as game_dur3_0_0_,
        match.platform_id as platform4_0_0_,
        match.queue_id as queue_id5_0_0_,
        match.season_id as season_i6_0_0_,
        row_number() over () as col_2 -- native SQL 실행
    from
        match
2021-04-22 23:23:00.943 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5110647401]
2021-04-22 23:23:00.945 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.945 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.946 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.946 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [420]
2021-04-22 23:23:00.946 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.947 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [1]
2021-04-22 23:23:00.947 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5110663852]
2021-04-22 23:23:00.947 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.947 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [2]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5110712716]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [420]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [3]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5110948806]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [420]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [4]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5111318269]
2021-04-22 23:23:00.948 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [420]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [5]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5111560760]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [6]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5111855885]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.949 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [7]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5111953625]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [8]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5112801333]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [9]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5112862930]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.950 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [10]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5113009318]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [420]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [11]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5113040393]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [12]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5113093150]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [420]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.951 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [13]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5113123898]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [14]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5113140714]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [15]
2021-04-22 23:23:00.952 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5113381568]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [16]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5113679956]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [420]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [17]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5114509070]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [18]
2021-04-22 23:23:00.953 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5114612211]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [19]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_id1_0_0_] : [BIGINT]) - [5115327873]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_cre2_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([game_dur3_0_0_] : [BIGINT]) - [0]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([platform4_0_0_] : [VARCHAR]) - [KR]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([queue_id5_0_0_] : [INTEGER]) - [450]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([season_i6_0_0_] : [INTEGER]) - [13]
2021-04-22 23:23:00.954 TRACE 14944 --- [           main] o.h.type.descriptor.sql.BasicExtractor   : extracted value ([col_2] : [NUMERIC]) - [20]
```
문제 없이 실행됩니다.

##### reference
[querydsl 공식 문서](http://www.querydsl.com/static/querydsl/latest/reference/html/ch02.html)  
[querydsl 공식 문서 번역](http://www.querydsl.com/static/querydsl/4.0.1/reference/ko-KR/html_single/#d0e393)

