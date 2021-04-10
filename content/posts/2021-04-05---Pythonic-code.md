---
title: "Pythonic code"
date: "2021-04-05"
template: "post"
draft: false
slug: "Pythonic-code"
category: "Coding Test"
tags:
  - "#Python"
description: "같은 기능을 수행하는 코드 중 가장 Pythonic한 코드들"
---

[Pythonic code가 과연 효율적일까? - 안주은](https://www.youtube.com/watch?v=Txz7K6Zc-_M)의 내용을 요약하였습니다.

### List
`append()`, `extend()`보다는 `List comprehension`을 사용합시다.

```python
[i*i for i in range(10)]
```
`append()`, `extend()`를 for문안에 작성하면 n번 호출 되지만 `List comprehension`은 1번만 호출되므로 성능상 이점이 있습니다.

### Merging dictionary
`dictionary comprehension`보다는 `update()`, `dictionary kwargs`가 성능이 좋습니다.
둘 중 `dictionary kwargs`가 간결해보이니 `dictionary kwargs`가 더 pythonic합니다.

```python
def update_method(d_1, d_2):
    result = {}
    result.update(d_1)
    result.update(d_2)
    return result

update_method({'a': 1, 'b': 2},{'c': 3, 'd': 4}) # {'a': 3, 'b': 2, 'd': 4}

# pythonic
def dict_kwargs(d_1, d_2):
    result = {**d_1, **d_2}
    return result

dict_kwargs({'a': 1, 'b': 2},{'c': 3, 'd': 4})
```

### Finding an item in a collection
요소를 찾을 때는 `list`, `tuple`보다는 `set`이 자료구조상 가장 빠릅니다.  
해시 테이블을 만들어서 사용하기 때문입니다.

```python
# set
print(3 in {1,2,3,4,5,6,7,8,9,10}) # True
# list
print(3 in [1,2,3,4,5,6,7,8,9,10])
# tuple(pythonic)
print(3 in (1,2,3,4,5,6,7,8,9,10))
```

그러나 컬렉션의 크기가 작다면 `list`, `tuple`을 고려해야 합니다.  
해시 테이블을 만드는 데 비용이 들기 때문입니다.

### String formatting
데이터가 많아질수록 `%`, `formatted string` 보다는 `f string`이 성능상 좋습니다.

```python
print(['%d' % i for i in range(10)]) # ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
print(['{i}'.format(i=i) for i in range(10)])
# pythonic
print([f'{i}' for i in range(10)])
```