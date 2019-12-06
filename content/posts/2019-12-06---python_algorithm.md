---
title: Binary Search algorithm by python
date: "2019-12-06T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/CodeAlgorithm/"
category: "Python/Algorithm/"
tags:
  - "Python/Algorithm/Data Structure"

description: "Search Methodology"
socialImage: "/media/gutenberg.jpg"
---

# Binary Search

target을 찾는 방법 중 하나이다.

가장 쉬운 방법은 list를 처음부터 끝까지 target 값을 찾는 것이다.

근데 이 방법은 딱 봐도 너무 무식하고 공수가 많이 든다는 것을 알 수 있다.

그러면 좀 더 효율적으로 할 수 있는 논리적 방법이 없을까??

많은 찾아야 할 값들 중에서 대강 스캔을 해본 뒤 정말 관계가 없어보이거나 필요없어 보이는 부분을 과감히 제외한다. 그리고 나머지 부분에서 내가원하는 target을 찾는 방법을 쉽게 떠올릴 수 있다.

일반적으로도 내가 해야되는 수많은 선택지 중에서 일부를 과감히 버리게 된다.

이걸 단순 논리적으로 접근한 것이 바로 이진탐색(Binary Search)이다.

## 1. 이진 탐색이란?

탐색할 리스트의 요소 개수를 절반으로 줄여가면서 탐색을 하는 방법을 말한다.

다만 이진 탐색(binary search)은 오름 차순으로 정렬된 배열에만 적용할 수 있다는 단점이 있습니다.

이진 탐색에서 중요한 요소는 low index와 high 혹은 max index와 반을 잘라 낼 수 있는 mid index이다.

크기가 n 인 리스트 data에서 target 이라는 특정 요소를 찾아낸다고 가정했을 때, 이진 탐색의 절차는 다음과 같다.

1. low =0, high = n−1 로 초기화 합니다.

2. mid 는 (log + high) 를 2 로 나눈 몫으로 결정합니다.

3. data[mid] 와 target 이 서로 같으면 목적을 달성했으므로 탐색을 종료합니다.

4. 만약 target < data[mid] 이면 high = mid-1 로 업데이트 한 후, 2번으로 돌아갑니다. 만약 target > data[mid] 라면 low = mid+1 로 업데이트 한 후, 2번으로 돌아갑니다.

4번을 자세히 보면 2번으로 계속 되돌아 가는 걸 볼 수 있다, --> 재귀함수를 써야되는 근거가 된다.

- 시간복잡도로 이진 탐색을 보면 기존의 일괄탐색은 O(n)인 반면

- 이진탐색은 그의 반으로 줄어든다. O(logn)

## 2. 재귀함수를 이용한 이진 탐색 알고리즘

위에서 보았듯이 low index와 high index (mid index는 low와 high로 구할 수 있음), 찾을 대상인 list 그리고 찾을 목표 target이 필요한 인자임을 알 수 있다

```
def search(nums, target, low, high):
    if low > high:
        return False

    mid = (low + high) // 2

    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        low = mid + 1
    else:
        high = mid - 1
    return search(nums, target, low, high)

```

- 리턴을 search로 해서 계속 재귀하는 함수이고 재귀함수에는 종료조건이 있어야 하는데 그게 바로 첫행에 있는 low>high이다.
- low > high는 반으로 나누고 나누다가 결국 나눌 리스트 값이 존재하지 않는 경우이다. 이런경우 찾는 값이 없는 것이므로 False를 리턴하였다.
