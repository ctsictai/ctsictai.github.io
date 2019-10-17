---
title: Python Apprenticeship Study Part.2
date: "2019-10-17T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part2/"
category: "Python"
tags:
  - "Python"

description: "sequence obj -list/set/dictionary/tuple"
socialImage: "/media/gutenberg.jpg"
---

# LIST

> 다양한 데이터 타이들을 순서에 따라 저장할 수 잇는 데이터 타입이다.

- 대괄호[]안에 데이터들을 넣어 주면 된다.
- []만 변수로 선언하면 빈 list만 생성된다.
- ★ 리스트 타입의 개수를 구하기 위해서는 len함수를 이용하면 된다.
- 리스트 인덱스의 시작이 1이 아니라 0이다 -> [0] / [1]
- 리스트 역 인덱스의 시작은 -1이다 -> [-1] / [-2]
- 문자열 type로 적용 가능 + 숫자, boolean 값 등 모든 type의 값을 저장할 수 있으며 서로 다른 type의 값들을 저장하는것도 가능합니다.

## Adding and changing elements to list

1. append Added Element!!

   - 순차적으로 저장함 마지막 인덱스에 요소값 저장
   - List name.append(element)

2. "+"
   - 추가 요소가 1개 이상일 때
   - List_name = list_name + [element, element2]
   - new_list = list_name +list_name2
3. insert

   - 원하는 위치에 추가하는 메서드
   - List_name.insert(indexing number, element)
   - Indexing number란 저장할 원하는 위치를 지정해야 한다는 것!

4) element updating
   - List_name[idx] = element
   - Idx = 바꿀 위치 인덱싱
   - Element = 바꿀 요소값

# 튜플(tuple)

- 순서에 따라 저장하는데이터 타입
- 괄호 ()안에 데이터들을 넣어 주면 된다.
- 리스트와 튜플의 차이점
- 리스트 타입의 내용의 변경이 가능(mutable)
- 튜플의 경우 내용의 변경이 불가(immutable)
- 속도 면에서 튜플이 좀 더 빠르다
- 괄호를 사용하지 않아도 튜플을 만들 수 있습니다.
- 인덱싱은 리스트와 같다

> 언제 tuple 사용함?

일반적으로 2개에서 5개 사이의 요소들을 저장할 때, 특정 데이터를 즉각적으로 표현하고 싶을 때 - 2차원 수평면에 위치를 가리키는 벡터를 표현할 때 좋음

# Dictionary

- Dict type = {} – 빈 딕셔너리 선언
- Dict type = {‘aa’ : 2} => { key : value } 로 요소 추가
- 순서가 없는 key와 value 쌍으로 된 집합이다(순서가 필요 없음 unique한 key로 구별이 가능함)
-     Key와 value {}로 묶어주면 된다.
-     Update(키=값) 이름 그대로 딕셔너리에서 키의 값을 수정 및 추가
-     Pop(키) or pop(키, 기본값)은 특정 키-값 쌍을 삭제한 뒤 값을 반환
- Key가 딕셔너리 안에 있는지 판단하는 경우 in 연산자 활용
-     Dict은 안에 리스트 생성 가능/ key ; value 값 수정 및 추가 및 삭제 가능

## Create element in dictionary & updating element

`dictionary_name[new_key] = new_value`

## Read element in Dictionary

`dict_name[‘key_name’] = value 반환!`

- Key는 string이나 number가능 but 중복은 안됨 unique해야함!!
- 만약에 이미 있는 key값으로 요소를 집어 넣으면 updating이 된다.!

`dict1 = { 1 : "one", 1 : "two" }`  
`print(dict1)`  
`{ 1: "two" }`

## Delete element in dictionary

`del dict_name["key_name"]`

# SET

- 중복을 허용하지 않는 순서가 없는 자료형이다. (2가지 유니크한 특징이 있다)
- 순서가 없다는 것은 인덱싱을 통해 value를 뽑아 낼 수 없다는 얘기
- 중복이 허용이 안된다는 것은 요소들이 모두 unique한 값을 가진 다는 얘기
- 중복이 안된다? 활용 가능성이 매우 높지 – 중복 제거 필터로써
- Set 자료형에 인덱싱으로 값에 접근하려면 set를 list나 tuple로 변환 후 인덱싱해야 한다.

## Set 생성

- `S =set()` – 빈 set 자료형 생성
- `Sw = set(‘hello’)`
  > h e l o 자료 요소를 가진 set 자료형 생성 (l은 중복값이 있어서 하나만 요소값으로)

#### set()의 가장 많은 활용 법 – 교집합, 합집합, 차집합

1.  교집합 ( & or intersection())

    - `Set1 = set(1,2,3,4,5)`
    - `Set2 = set(3,4,5,6,7)`

      > Set1 & set2 => {3, 4, 5}  
      > Set1.intersection(set2) =>{3,4,5}

2.  합집합 ( | or union)

    - `Set1 = set(1,2,3,4,5)`
    - `Set2 = set(3,4,5,6,7)`

      > Set1 | set2 => {1,2,3,4,5,6,7}  
      > Set1.union(set2) = > {1,2,3,4,5,6,7}

3.  차집합 ( - or difference )  
     - `Set1 = set(1,2,3,4,5)` - `Set2 = set(3,4,5,6,7)`

    > 차집합은 순서가 매우 중요!!!

        >Set1 - set2 => {1,2}
        >Set1.difference(set2) = > {1,2}
        >Set2 – set1 => {6, 7}
        >Set2.difference(set1) => {6,7}

#### Set에 제공되는 기본 메서드

1. add (값 1개 추가)

   > s1 = set([1, 2, 3])  
   > s1.add(4)  
   > s1 {1, 2, 3, 4}

2. update ( 값 여러 개 추가)

   > s1 = set([1, 2, 3])  
   > s1.update([4, 5, 6])  
   > s1
   > {1, 2, 3, 4, 5, 6}

3. remove (특정값 제거)
   > s1 = set([1, 2, 3])  
   > s1.remove(2)  
   > s1
   > {1, 3}
