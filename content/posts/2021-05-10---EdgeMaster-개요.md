---
title: "EdgeMaster 개요"
date: "2021-05-10"
template: "post"
draft: false
slug: "EdgeMaster-개요"
category: "EdgeMaster"
tags:
  - "#EdgeMaster"
  - "#machbase"
description: ""
---

### cloud 컴퓨팅 모델의 한계

IoT 장치의 데이터의 증가로 인해 cloud 컴퓨팅은 다음과 같은 단점이 나타났습니다.

+ 처리 속도 저하
+ 데이터 양에 따른 비용 증가
+ 보안 문제

### edge computing의 등장

이를 해결 하기 위해 edge 컴퓨팅이 탄생하였습니다.  
edge 컴퓨팅은 클라우드와 게이트웨이 사이에 edge 노드라는 스토리지를 두어 응답 시간을 개선하고 대역폭을 절약을 합니다.  
또한 클라우드가 장애가 발생해도 서비스가 지속되는 효과를 가지고 있습니다.

![EdgeMaster-intro-01.png](/media/posts/2021-05-10---EdgeMaster-개요/EdgeMaster-intro-01.png)
*Empowered Edge의 등장*

2020년이 되어서는 한 단계 나아가서 게이트웨이 단까지 데이터를 저장하는 모델인 **Empowered Edge**라는 개념이 나오게 되었습니다.  
edge 컴퓨팅은 cloud에서 edge로 edge에서 게이트웨이로 점점 더 아래로 스토리지를 구축하는 양상으로 진화하고 있습니다.

그와 동시에 edge 개수 증가에 따른 관리 및 데이터 통합 문제가 발생되고 있습니다.  
+ 수많은 edge를 어떻게 설치, 관리, 모니터링할 것인가?
+ edge 데이터를 어떻게 중앙 서버로 재전송, 통합할 것인가?

### EdgeMaster 

이와 같은 문제들을 준비하기 위해 마크베이스에서는 'EdgeMaster'라는 솔루션을 개발하고 있습니다.  

![EdgeMaster-intro-02.png](/media/posts/2021-05-10---EdgeMaster-개요/EdgeMaster-intro-02.png)
위 사진은 EdgeMaster의 모델입니다.  
EdgeMaster은 edge단 데이터 게더링, 원하는 데이터를 특정 서버로 자동으로 전송 가능(전송 중 하드웨어에 장애가 발생되어도 데이터 소실없이 네트위크 복구 후에 재전송 가능)이 가능합니다.

#### EdgeMaster의 주요 기능

![EdgeMaster-intro-03.png](/media/posts/2021-05-10---EdgeMaster-개요/EdgeMaster-intro-03.png)

+ Connectivity Management : Cloud, Fog 위치에서 edge설치 및 게더링에 필요한 동작들을 원격 설치 가능
+ Device Management : Edge 단의 수집 데이터 조회, ERROR 감지, 디바이스에서 게더링된 데이터를 원하는 곳으로 전달
+ Data Management : Edge, Fog단의 데이터를 사용자에게 전송, 데이터 분석 차트 제공

종합해서 말하면 edge 컴퓨팅의 모델의 데이터 통제를 할 수 있다는 것입니다.

### Use case

![EdgeMaster-intro-05.png](/media/posts/2021-05-10---EdgeMaster-개요/EdgeMaster-intro-05.png)

Iot 공장 연수원인 '스마트공장 배움터' 에서의 사용 예시입니다.  
Edge 단에 Machbase Fog의 정보들을 클라우드 허브로 복제하면 그곳의 Machbse Fog에 저장되는 구조입니다.  
그리고 모니터링할 수 있는 EdgeMaster Server에서 확인할 수 있습니다.

### edge 관리 화면 예시

![EdgeMaster-intro-04.png](/media/posts/2021-05-10---EdgeMaster-개요/EdgeMaster-intro-04.png)

Edge들의 사용율, 데이터 게더링, 데이터 전송이 가능하도록 해주는 기능을 가지고 있습니다.

##### reference

[위키백과 - 에지 컴퓨팅](https://ko.wikipedia.org/wiki/%EC%97%90%EC%A7%80_%EC%BB%B4%ED%93%A8%ED%8C%85)  
[[Behind IoT] 엣지 컴퓨팅(edge computing)의 등장](https://www.youtube.com/watch?v=uI0XkIR3reM)  
[[About us] 새로운 솔루션 출시 "엣지 마스터"](https://www.youtube.com/watch?v=fMD_wRoxHWs)  
[Machbase 공식 홈페이지 - 왜 In-Memory 솔루션은 Edge Computing을 위한 기술이 아닌가?](https://www.machbase.com/kor/resource/blog/65)  
[Machbase 공식 홈페이지 - EdgeMaster](https://www.machbase.com/kor/solution/edgeMaster)  