---
title: Token System
date: "2019-12-31T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/algorithm/sorting/1"
category: "Python, Sorting, Algorithm"
tags:
  - "Python, Sorting, Algorithm"

description: "Sorting Code Algorithm"
socialImage: "/media/image-2.jpg"
---

# 프로그래머스 정렬 1번 문제

배열 array의 i번째 숫자부터 j번째 숫자까지 자르고 정렬했을 때, k번째에 있는 수를 구하려 합니다.

예를 들어 array가 [1, 5, 2, 6, 3, 7, 4], i = 2, j = 5, k = 3이라면

array의 2번째부터 5번째까지 자르면 [5, 2, 6, 3]입니다.  
1에서 나온 배열을 정렬하면 [2, 3, 5, 6]입니다.  
2에서 나온 배열의 3번째 숫자는 5입니다.  
배열 array, [i, j, k]를 원소로 가진 2차원 배열 commands가 매개변수로 주어질 때, commands의 모든 원소에 대해 앞서 설명한 연산을 적용했을 때 나온 결과를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

### 제한사항

array의 길이는 1 이상 100 이하입니다.
array의 각 원소는 1 이상 100 이하입니다.
commands의 길이는 1 이상 50 이하입니다.
commands의 각 원소는 길이가 3입니다.

### 입출력 예

| array                 | commands                          | return    |
| --------------------- | --------------------------------- | --------- |
| [1, 5, 2, 6, 3, 7, 4] | [[2, 5, 3], [4, 4, 1], [1, 7, 3]] | [5, 6, 3] |

### 입출력 예 설명

1. [1, 5, 2, 6, 3, 7, 4]를 2번째부터 5번째까지 자른 후 정렬합니다. [2, 3, 5, 6]의 세 번째 숫자는 5입니다.
2. [1, 5, 2, 6, 3, 7, 4]를 4번째부터 4번째까지 자른 후 정렬합니다. [6]의 첫 번째 숫자는 6입니다.
3. [1, 5, 2, 6, 3, 7, 4]를 1번째부터 7번째까지 자릅니다. [1, 2, 3, 4, 5, 6, 7]의 세 번째 숫자는 3입니다.

## 기본 로직

1. 배열 array를 슬라이스([:]) 하고 정렬(sorted)을 한다
2. 정렬한 list에서 commands에 해당되는 인덱스의 값을 리턴한다.
3. 리턴한 값을 answer list에 추가한다(append)
4. commands의 길이가 최대 50까지 이므로 반복문이 필요하다. (나는 while문으로 접근하였다.)

## 문제 풀이

```
def solution(array, commands):
    answer = []
    i = 0
    while i <= len(commands)-1:
        ele = sorted(array[commands[i][0]-1:commands[i][1]])
        m = ele[commands[i][2]-1]
        answer.append(m)
        i += 1

    return answer
```

- while문으로 commands의 리스트 길이만큼 돌린다.
- 배열 array를 슬라이스 하고 정렬까지 완료
- 정렬한 list에서 command에 있는 인덱스값으로 요소값 리턴
- 리턴한 요소값을 answer list에 요소 추가

이렇게 풀었지만 더 간단히 푸는 방법이 있었다????

```
def solution(array, commands):
    return list(map(lambda x: sorted(array[x[0]-1:x[1]])[x[2]-1], commands))
```

- list에서 map과 람다함수를 이용해서 한 줄로 깔끔하게 풀어낸 모습이다.

- 람다함수의 x는 map의 인자중 map을 돌릴 리스트에 해당하는 commands가 된다.
- commands라는 list 요소를 lamda 함수 조건대로 변환해준다.
