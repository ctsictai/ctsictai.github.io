---
title: Python Apprenticeship Study Part.9
date: "2019-11-02T23:50:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part10/"
category: "Python"
tags:
  - "Python/List Comprehension"

description: "List Comprehension"
socialImage: "/media/gutenberg.jpg"
---

# List Comprehension

## 기본 형식

> **new_list = [expression(i) for i in old_list if filter(i)]**

- exp - 표현식(만들려는 형태) 여기서 선언되는 변수가 for문 / if문에서 사용되야 의미가 있다.
- for문 - 표현식에 있는 변수명을 반복문을 돌림
- if문 - 안들어가도 됨 - 다만 표현식 반복문에 조건이 필요한경우

소위 브라켓이라고 하는 빈 리스트값 매겨 놓고 리스트안에서 포문 즉 반복문이 돌아간다. 그런데 newlist를 oldlist를 근거로 만드려는데 똑같이 만들려면 의미가 없고 여기에 filtering을 하여 newlist를 만든다.

이 때 filter에 따라 수많은 newlist가 생성 된다.

```
[x.lower() for x in ["A","B","C"]]
['a', 'b', 'c']
```

코드 해석

- x.lower() - x 라는 영문자열을 소문자로 바꿔라
- for x in [‘A’, ‘B’, ‘C’] - x를 [] list 요소 숫자만큼 돌려라
- 즉, 합치면 리스트 [‘A’, ‘B’, ‘C’]내의 요소x를 lower 소문자로 모두 바꿔라

=================================================

```
string = "Hello 12345 World"
numbers = [x for x in string if x.isdigit()]
print(numbers)
 ['1', '2', '3', '4', '5']
```

코드해석

- x는 표현식으로 의미는 없고
- for x in string - 여기서의 x가 표현식의 x와 같아야 한다. 안그러면 표현식 x는 undefined 된 에러변수가 된다.
- if x.isdigit() - 여기서 조건문이 들어가 isdigit 숫자만 출력한다.

## List comprehension 이걸 언제 쓸까??

- 다중 list나 다중 dict 등등 - 다중 배열(시퀀스 자료형) 구조일 경우 요소 접근에 대한 고민이 필요할 때(matrix 구조에 적용 가능)

- nested for문을 쓸 경우에 - 그게 보통 위의 경우가 많다.

### EX1) 3중 배열 요소 값 접근

```
matrix = [[[1,2,3], [4,5,6], [7,8,9]], [['q','w','e'], ['r', 't', 'y']]]

# 괄호 1번 벗기기
inner_matrix = [row for row in matrix]
print("inner_matrix :", inner_matrix)

# 괄호 2번 벗기기
inner_matrix_list = [row_element for row in matrix for row_element in row]
print("inner_matrix_list : ", inner_matrix_list)

# 괄호 3번 벗기기
inner_matrix_list_element = [element for inner_matrix in matrix
                                   for inner_matrix_list in inner_matrix
                                   for element in inner_matrix_list]

print("inner_matrix_list_element : ", inner_matrix_list_element)
```

matrix = [ [ [1,2,3], [4,5,6], [7,8,9]], [ ['q','w','e'], ['r', 't', 'y']]]  
matrix 내에 또 다른 매트릭스(inner_matrix)가 2개 있는 걸로 이해할 수 있다.  
즉, inner_matrix_1 = [[1,2,3], [4,5,6], [7,8,9]]  
inner_matrix_2 = [['q','w','e'], ['r','t','y']] 가 있는 형태이다.

inner_matrix_1과 inner_matrix_2의 괄호를 또 한번 벗기면  
inner_matrix_list_1 = [1,2,3], [4,5,6], [7,8,9]  
inner_matrix_list_2 = ['q','w','e'], ['r', 't', 'y'] 를 만나게 된다.

inner_matrix_list_1과 inner_matrix_list_2의 괄호를 또 한번 벗기게 되면 드디어 리스트를 구성하는 원소(element)를 만날 수 있다.  
inner_matrix_list_element_1 = 1,2,3,4,5,6,7,8,9  
inner_matrix_list_element_2 = 'q', 'w', 'e', 'r', 't', 'y'
