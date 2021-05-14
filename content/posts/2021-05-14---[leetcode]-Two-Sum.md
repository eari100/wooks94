---
title: "[leetcode] Two Sum"
date: "2021-05-14"
template: "post"
draft: false
slug: "[leetcode]-Two-Sum"
category: "Coding Test"
tags:
  - "#Coding Test"
  - "#leetcode"
  - "#Python"
description: "Hash Table을 이용하여 시간복잡도를 줄입니다."
---

[문제 풀러 가기](https://leetcode.com/problems/two-sum/)

1. Brute-force search

```Python
def twoSum(self, nums: List[int], target: int) -> List[int]:
    
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]
```

Brute-force search를 통해 풀면 시간복잡도가 O(n^2)이 됩니다.
개선이 필요했습니다.

2. Hash Table

Hash Table은 조회 시간이 O(1)입니다.  
따라서 dictionary를 활용하기로 했습니다.

```Python
def twoSum(self, nums: List[int], target: int) -> List[int]:
    nums_dict = {}

    for i, num in enumerate(nums):
        if target-num in nums_dict :
            return [nums_dict[target-num], i]

        nums_dict[num] = i
```

더욱 효율적인 알고리즘을 만들기 위해 List를 순차적으로 탐색하는 동시에 dictionary에 List의 요소를 하나씩 추가합니다.  
target-num가 nums_dict에 존재한다면 답을 구할 수 있습니다.

#### reference

[[파이썬 알고리즘 인터뷰] 7번(#1) Two Sum](https://www.youtube.com/watch?v=zG-ecTqsO4U&list=LL&index=4)
[[자료구조 알고리즘] LeetCode 1. Two Sum 설명 & 자바로 구현](https://www.youtube.com/watch?v=FHphOv2mmIA&list=LL&index=9)