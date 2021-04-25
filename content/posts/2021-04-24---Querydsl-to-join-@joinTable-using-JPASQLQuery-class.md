---
title: "[Querydsl] JPASQLQuery 클래스를 사용하여 @joinTable join하기"
date: "2021-04-24"
template: "post"
draft: false
slug: "[Querydsl]-JPASQLQuery-클래스를-사용하여-@joinTable-join하기"
category: "Querydsl"
tags:
  - "#Querydsl"
  - "#JPA"
  - "#Spring Framework"
  - "#Java"
description: ""
---

아래와 같이 `summoner`, `match`, `summoner_match` 엔티티들이 존재한다고 가정합니다.  

```Java
@Getter
@NoArgsConstructor
@Entity
public class Summoner implements Serializable {
    @Id@Column(name="id",length=56)
    private String id;
    @Column(name="account_id", length=56)
    private String accountId;
    private int profileIconId;
    private long revisionDate;
    private String name;
    @Column(length=78)
    private String puuid;
    private long summonerLevel;

    @OneToMany
    @JoinTable(name="summoner_match",
            joinColumns = @JoinColumn(name="account_id"),
            inverseJoinColumns = @JoinColumn(name="game_id")
    )
    private List<Match> matches = new ArrayList<>();

    // ...이하 생략
}
```
```Java
@Getter
@NoArgsConstructor
@Entity
public class Match {
    @Id
    @Column(name="game_id")
    private long gameId;
    private int queueId;
    private String platformId;
    private int seasonId;
    private long gameCreation;
    private long gameDuration;

    // ...이하 생략
}
```

여기서 Querydsl을 사용하여 다음과 같은 조회 쿼리를 생성하고 싶습니다.  
```SQL
SELECT
    *
FROM 
    summoner s
LEFT OUTER JOIN
    summoner_match sm
on
   s.account_id= sm.account_id
LEFT OUTER JOIN
    match m
on
   sm.game_id = m.game_id 
```
JPAQueryFactory와 JPASQLQuery를 활용해보겠습니다.

### JPAQueryFactory클래스 사용

```Java
@Override
    public IntegrationInformationResponseDTO findSummonerIntegrationInformationByName() {
        QSummoner summoner = QSummoner.summoner;
        QMatch match = QMatch.match;

        Summoner result = queryFactory
                .selectFrom(summoner)
                .leftJoin(summoner.matches, match).fetchJoin()
                .fetchOne();

        return new IntegrationInformationResponseDTO(result);
    }
```
다음과 같이 문제없이 조회 쿼리를 생성할 수 있습니다.

### JPASQLQuery클래스 사용
JPASQLQuery클래스 사용하려면 문제가 발생합니다.  
그 이유는 `@joinTable`의 객체는 Q클래스가 만들어지지 않기 떄문입니다.  
[github](https://github.com/querydsl/querydsl/issues/2809)와 [stackoverflow](https://stackoverflow.com/questions/67217677/how-to-join-jointable-using-querydsl)에 질문을 해도 방법을 찾을 수 없었습니다.  
꽤 오랫동안 찾아보다가 `QSummoner()`생성자를 활용하여 `@JoinTable`의 Q클래스를 생성하는 방법을 찾았습니다.  
```Java
/**
 * QSummoner is a Querydsl query type for Summoner
 */
@Generated("com.querydsl.codegen.EntitySerializer")
public class QSummoner extends EntityPathBase<Summoner> {
    public QSummoner(String variable) {
        super(Summoner.class, forVariable(variable));
    }
}
```

아래와 같이 활용할 수 있습니다.
```Java
 @Override
    public void findSummonerIntegrationInformationByName() {
        QSummoner summoner = QSummoner.summoner;
        // summoner_match Qclass
        QSummoner summoner_match1 = new QSummoner("summoner_match");
        QMatch summoner_match2 = new QMatch("summoner_match");
        QMatch match = QMatch.match;

        jpasqlQuery.select(summoner, match, SQLExpressions.rowNumber().over())
                .from(summoner)
                .leftJoin(summoner_match1).on(summoner.accountId.eq(summoner_match1.accountId))
                .leftJoin(match).on(summoner_match2.gameId.eq(match.gameId))
                .fetch();
    }
```