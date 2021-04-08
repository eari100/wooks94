---
title: "[Querydsl] 프로젝션이 둘 이상을 조회할때는 Tuple을 사용해야 합니다"
date: "2021-04-09"
template: "post"
draft: false
slug: "Querydsl-프로젝션이-둘-이상을-조회할때는-Tuple을-사용해야-합니다"
category: "Querydsl"
tags:
  - "#Querydsl"
  - "#Spring Framework"
  - "#Java"
description: ""
---

Querydsl에서 조회 시 프로젝션이 둘 이상이라면 Tuple을 사용하면 됩니다.  

```Java
@Override
public List<SummonerIntegrationInformationResponseDTO> findSummonerIntegrationInformationByName(String summonerName) {
    QSummoner summoner = QSummoner.summoner;
    QMatch match = QMatch.match;

    // summoner, match 컬럼 데이터들을 List<Tuple>형태에 담아줍니다.
    List<Tuple> matchsInfo = queryFactory
            .select(summoner, match)
            .from(summoner)
            .innerJoin(summoner.matches, match)
            .where(summoner.name.eq(summonerName))
            .offset(0)
            .limit(20)
            .fetch();

    return matchsInfo.stream()
            // Tuple의 값을 꺼낼때는 'get(프로젝션명)'을 해줍니다.
            .map(m -> new SummonerIntegrationInformationResponseDTO(m.get(summoner), m.get(match)))
            .collect(Collectors.toList());
}
```