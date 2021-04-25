---
title: "[JPA] @JoinColumn의 referencedColumnName을 꼭 명시하자"
date: "2021-04-25"
template: "post"
draft: false
slug: "[JPA]-@JoinColumn의-referencedColumnName을-꼭-명시하자"
category: "JPA"
tags:
  - "#JPA"
  - "#Spring Framework"
  - "#Java"
description: ""
---

아래 처럼 non-primary key와 primary key를 @oneToMany로 연관관계를 맺은 적이 있습니다.

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

`summoner` 테이블과 `match` 테이블에 데이터를 삽입해주고 `summoner_match` 테이블에도 연관관계 데이터가 삽입되는지 테스트 코드를 작성해보았습니다.

```Java
    @Test
    public void summoner_match데이터_삽입() {
        String accountId = "yy15F-qXoM8a1kqFL8iJ0xMUTF6e6ZZlWKPdlrvgZIcr";
        int profileIconId = 11;
        long revisionDate = 1609294136000L;
        String name = "거세짱123";
        String id = "qOshc-BI3WAaQuvgpPI7GY7w0ZfjTt2WJHX_46zdQVqotlI";
        String puuid = "blugvIvgoZB2GPmLQryiiVl_61CnLNNf50b_UGKkCqilTFa42mL_ZEfSEUJTICP_X-n6xuMjMg65YQ";
        long summonerLevel = 293;

        Summoner summoner = Summoner.builder()
                .accountId(accountId)
                .profileIconId(profileIconId)
                .revisionDate(revisionDate)
                .name(name)
                .id(id)
                .puuid(puuid)
                .summonerLevel(summonerLevel)
                .build();

        long[] gameIds = {5115327873L, 5114612211L, 5114509070L, 5113679956L, 5113381568L,
                5113123898L, 5113140714L, 5113009318L, 5113093150L, 5113040393L,
                5112862930L, 5112801333L, 5111953625L, 5111855885L, 5111560760L,
                5111318269L, 5110948806L, 5110712716L, 5110647401L, 5110663852L};

        Arrays.sort(gameIds);
        int[] queueIds = {420, 450, 420, 420, 420, 450, 450, 450, 450, 450, 420, 450, 420, 450, 450, 450, 420, 450, 450, 450};
        String platformId = "KR";
        int seasonId = 13;

        for(int i=0;i<20;i++) {
            Match match = matchRepository.save(Match.builder()
                    .gameId(gameIds[i])
                    .queueId(queueIds[i])
                    .platformId(platformId)
                    .seasonId(seasonId)
                    .build()
            );
            // 연관관계 추가
            summoner.getMatches().add(match);
        }
        // summoner_match 데이터 삽입
        summonerRepository.save(summoner);

        SummonerIntegrationInformationResponseDTO result = summonerRepository.findSummonerIntegrationInformationByName(name);

            assertThat(result.getProfileIconId()).isEqualTo(profileIconId);
            assertThat(result.getName()).isEqualTo(name);
            assertThat(result.getSummonerLevel()).isEqualTo(summonerLevel);
            assertThat(result.getId()).isEqualTo(id);
            for(int i=0;i<20;i++)
                assertThat(result.getMatches().get(i).getGameId()).isEqualTo(gameIds[i]);
    }
```
```bash
Hibernate: 
    insert 
    into
        summoner_match
        (account_id, game_id) 
    values
        (?, ?)
2021-04-25 15:00:43.892 TRACE 32108 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [1] as [VARCHAR] - [qOshc-BI3WAaQuvgpPI7GY7w0ZfjTt2WJHX_46zdQVqotlI] // error : summoner의 pk 컬럼이 삽입된다.
2021-04-25 15:00:43.893 TRACE 32108 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [2] as [BIGINT] - [5110647401]
.
.
.
```
그러나 의도한 바와는 다르게 `summoner_match.account_id`에 `summoner.id`가 참조되어 삽입되었습니다.  
해결하기 위해서는 `@JoinColumn`에 `referencedColumnName`를 작성해주어야 합니다.  
`referencedColumnName`은 `name`의 매핑하는 쪽의 테이블 컬럼이 무엇인가 선언해줍니다.  

```Java
 @OneToMany
    @JoinTable(name="summoner_match",
            joinColumns = @JoinColumn(name="account_id", referencedColumnName="account_id"),
            inverseJoinColumns = @JoinColumn(name="game_id", referencedColumnName="game_id")
    )
    private List<Match> matches = new ArrayList<>();
```
`referencedColumnName`를 작성했습니다. 테스트 코드를 다시 빌드해봅시다.

```bash
Hibernate: 
    insert 
    into
        summoner_match
        (account_id, game_id) 
    values
        (?, ?)
2021-04-25 15:08:14.373 TRACE 15340 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [1] as [VARCHAR] - [yy15F-qXoM8a1kqFL8iJ0xMUTF6e6ZZlWKPdlrvgZIcr] // correct value
2021-04-25 15:08:14.374 TRACE 15340 --- [           main] o.h.type.descriptor.sql.BasicBinder      : binding parameter [2] as [BIGINT] - [5110647401]
.
.
.
```
의도한대로 `summoner_match.account_id`에 `summoner.account_id`가 참조되어 삽입되었습니다.  
`@JoinColumn`에서 Primary key로 매핑을 해준다면 별 문제가 되지 않겠지만 non primary key라면 꼭 `referencedColumnName`를 명시해주어야 될 것 같습니다.