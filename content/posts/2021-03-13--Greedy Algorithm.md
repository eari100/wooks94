---
title: "Greedy Algorithm"
date: "2021-03-13"
template: "post"
draft: false
slug: "Greedy-Algorithm"
category: "Coding Test"
tags:
  - "#Algorithm"
  - "#Coding Test"
  - "#python"
description: "매 선택에서 지금 이 순간 당장 최적인 답을 선택하여 적합한 결과를 도출하자"
---

### Greedy Algorithm이란?

[나무위키](https://namu.wiki/w/%EA%B7%B8%EB%A6%AC%EB%94%94%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
에서 Greedy Algorithm에 대해 다음과 같이 설명한다.

그리디 알고리즘(욕심쟁이 알고리즘, Greedy Algorithm)이란 "매 선택에서 지금 이 순간 당장 최적인 답을 선택하여 적합한 결과를 도출하자" 라는 모토를 가지는 알고리즘 설계 기법이다.

### 예제

아래의 예제와 코드는 [(이코테 2021 강의 몰아보기) 2. 그리디 & 구현](https://www.youtube.com/watch?v=2zjoKjt97vQ&list=PLRx0vPvlEmdAghTr5mXQxGpHjWqSz0dgC&index=3) 강의 내용을 정리하였다.

#### 거스름 돈

당신은 음식점의 계산을 도와주는 점원이다.
카운터에는 거스름돈으로 사용할 500원, 100원, 50원, 10원짜리 동전이 무한히 존재한다고 가정한다.
손님에게 거슬러 줘야 할 돈이 N원일 때 거슬러줘야 할 동전의 최소 개수를 구하라.
단, 거슬러 줘야 할 돈 N은 항상 10의 배수이다.

```python
# n : 손님이 지불한 금액
# array : 카운터에서 가지고 있는 동전 종류 배열
# count : 걸러주는 동전의 수
def solution(n, array):
  count = 0
 
  for coin in array:
    count += n // coin
    n %= coin
  return count

# solution(1260, [500, 100, 40, 10])  
```

#### 1이 될 때까지

어떠한 수 N이 1이 될 때 까지 다음의 두 과정 중 하나를 반복적으로 선택하여 수행하려고 한다.
단, 두번째 연산은 N이 K로 나누어떨어질 때만 선택할 수 있다.
1. N에서 1을 뺀다.
2. N을 K로 나눈다.

예를 들어 N이 17, K가 4라고 가정하자. 이때 1번의 과정을 한 번 수행하면 N은 16이 된다.
이후에 2번의 과정을 두 번 수행하면 N은 1이 된다. 결과적으로 이경우 전체과정을 실행한 횟수는 3이된다. 이는 N을 1로 만드는 최소 횟수이다.
N과 K가 주어질 때 N이 1이 될 때까지 1번 혹은 2번의 과정을 수행해야하는 최소 횟수를 구하는 프로그램을 작성하시오

입력조건
+ 첫째줄에 N(2 <= N < = 100000)과 K(2 <= K < = 100000)가 공백으로 구분되며 각각 자연수로 주어진다.
이때 입력으로 주어지는 N은 항상 K보다 크거나 같다.

출력조건
+ 첫째줄에 N이 1이 될 때까지 1번 혹은 2번의 과정을 수행해야 하는 횟수의 최솟값을 출력한다.

```python
def solution(N, K):
  answer = 0

  while N!=1:
    answer += 1
    if N % K==0:
      N =  N // K
    else:
      N -= 1
    print(N)

  return answer

# solution2(1000, 7) 
```

그러나 10,000,000,000 건 같이 매우 큰 수가 N이라면 아래와 같이 코드를 작성해주는 것이 좋다.

```python
def solution3(N, K):
  result = 0
  while True:
    # N이 K로 나누어 떨어지는 수가 될때까지 빼기
    target = (N//K)*K
    result += (N-target)
    N = target
    # N이 K보다 작을 때 (더 이상 나눌 수 없을 때) 반복문 탈출
    if N<K:
      break
    result += 1
    N //= K

  # 마지막으로 남은 수에 대하여 1씩 빼기
  result += (N-1)
  return result
  # solution2(1000, 7) 
```
반복문 마지막에 N //= K를 실행하여 기하급수적으로 N의 크기를 줄여주기 때문에 O(log n) 로그 시간 복잡도로 만들어줄 수 도 있기 때문이다.

#### 곱하기 혹은 더하기

각 자리가 숫자로만 이뤄진 문자열 S가 주어졌을 때, 각 숫자 사이에 곱셈기호 또는 덧셈기호를 넣어 결과적으로 만들어질 수 있는 가장 큰 수를 구하시오.

```python
def solution(S):
  result = int(S[0])
  
  for i in range(1, len(S)) :
    num = int(S[i])
    if num <= 1 or result <= 1 :
      result += num
    else :
      result *= num
    
  return result

# solution('02984')
```
#### 모험가 길드

모험가 길드장인 동빈이는 모험가 그룹을 안전하게 구성하고자 공포도가 X인 모험가는 반드시 X명 이상으로
구성한 모험가 그룹에 참여해야 여행을 떠날 수 있도록 규정했다.
N명의 모험가에 대한 정보가 주어졌을때, 여행을 떠날 수 있는 그룹 수의 최댓값을 구하라.

```python
def solution(data):
  data.sort()

  count = 0
  result = 0

  for i in data:
    count += 1
    if count >= i:
      result += 1
      count = 0

  return result

solution([2,3,1,2,2])
```