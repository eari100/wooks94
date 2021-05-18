---
title: "git commit 내역 삭제"
date: "2021-05-11"
template: "post"
draft: false
slug: "git-commit-내역-삭제"
category: "Git"
tags:
  - "#Git"
description: ""
---

github에 실수로 AWS RDS 계정 정보를 올려버려서 해당 파일의 커밋의 모든 기록들을 삭제하기로 했습니다.

```bash
$ git filter-branch --tree-filter 'rm -f src/main/resources/application.yml' HEAD
$ git push -f
```

커밋은 존재하지만 조회하면 git show로 조회해보면 아무 내용도 확인할 수 없습니다.

```bash
$ git show (your commit ID)
commit (your commit ID)
Author: Jaewook Choi <eari100@naver.com>
Date:   Wed May 5 22:45:50 2021 +0900

    feat : mysql 연동한 'dev-mysql' profile 추가


```

그러나 기존 commit ID 값도 변경이 되기 때문에 commit ID 이력을 관리하는 상황인 경우 주의가 필요합니다.

#### reference

[7.6 Git 도구 - 히스토리 단장하기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0)  
[Git 특정 파일에 대한 이력 삭제](https://www.whatwant.com/entry/Git-%ED%8A%B9%EC%A0%95-%ED%8C%8C%EC%9D%BC%EC%97%90-%EB%8C%80%ED%95%9C-%EC%9D%B4%EB%A0%A5-%EC%82%AD%EC%A0%9C)  
[git-filter-branch](https://itrepreneur.tistory.com/36)