---
title: Python Apprenticeship Study Part.4
date: "2019-10-20T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part4/"
category: "Python"
tags:
  - "Python"

description: "if문 part2/for문/iterator"
socialImage: "/media/gutenberg.jpg"
---

# If문 조건

if문에 정수, bool값, 문자열, 실수 다 들어감

## 조건문에서 False로 취급되는 것들

- None
- False
- 0인 숫자들 : 0, 0.0, 0j
- 비어 있는 문자열, 리스트, 튜플. 딕셔너리, 세트 : "", [], (), {}, set()
- 클래스 인스턴스의 `__bool__()`, `__len__()`메서드가 0또는 False를 반환할 때

위에서 나열한 것들을 제외한 모든 요소들은 True로 취급한다.

## 조건식을 여러개 지정하기

조건식이 여러 개일 때는 논리 연산자(==, <, >, < <, ….)를 활용한다는 점도 기억하자.

조건식에 조건을 여러 개 붙이려면 조건에 따라 and나 or를 붙인다.

# Nest If문

파이썬의 특징인 indentation으로 구분 되는 if문으로써 if문 안에 if문이 실행되는 이중 if문이다.

간격이 더 들어 갈수록 중첩이 더 된 것이다.

**Nest if문에서 root에 있는 if문의 return이 최종 def의 return값이 된다.**

# for문

List의 요소를 한번에 한개씩 가지고 for 구문 안에 있는 코드를 실행하게 됩니다.

for element in list:

     do_something_with_element

For 문은 list 뿐만이 아니라 tuple, set 등 다른 자료구조와도 사용할 수 있습니다.

## range함수와 함께 사용하는 for문

for 변수 in range(횟수):

     반복할 코드

### 시작하는 숫자와 끝나는 숫자 지정하기

range에 횟수만 지정하면 숫자가 0부터 시작하지만, 다음과 같이 시작하는 숫자와 끝나는 숫자를 지정해서 반복할 수도 있습니다.

- for 변수 in range(시작, 끝):

### 증가폭 사용하기

range는 증가폭을 지정해서 해당 값만큼 숫자를 증가시킬 수 있죠? 이번에는 0부터 9까지의 숫자 중에서 짝수만 출력해보겠습니다.

- for 변수 in range(시작, 끝, 증가폭):

### 숫자를 감소시키기

for와 range는 숫자가 증가하면서 반복했습니다. 그럼 숫자를 감소시킬 수는 없을까요?

for i in range(10, 0): # range(10, 0)은 동작하지 않음

    print('Hello, world!', i)

range(10, 0)과 같이 시작하는 숫자를 큰 숫자로 지정하고 끝나는 숫자를 작은 숫자로 지정하면 숫자가 감소할 것 같은데, 실행을 해보면 아무것도 출력되지 않습니다. 왜냐하면 range는 숫자가 증가하는 기본 값이 양수 1이기 때문입니다.

증가폭을 음수로 지정해서 반복해봅니다.
증가폭을 음수로 지정하는 방법 말고도 reversed를 사용하면 숫자의 순서를 반대로 뒤집을 수 있습니다.

### 입력한 횟수대로 반복하기

이번에는 입력한 횟수대로 반복을 해보겠습니다. 다음 내용을 IDLE의 소스 코드 편집 창에 입력하세요.

for_range_input.py

count = int(input('반복할 횟수를 입력하세요: '))

for i in range(count):

     print('Hello, world!', i)

소스 코드를 실행하면 '반복할 횟수를 입력하세요: '가 출력됩니다. 여기서 3을 입력하고 엔터 키를 누르세요.

- 실행 결과

반복할 횟수를 입력하세요: 3 (입력)  
Hello, world! 0  
Hello, world! 1  
Hello, world! 2  
3을 입력했으므로 'Hello, world!'가 3번 출력됩니다

### 시퀀스 이용한 for문

먼저 input으로 입력 값을 받아서 count 변수에 저장합니다(이때 반드시 int를 사용하여 input에서 나온 문자열을 정수로 변환해줍니다).  
 그리고 반복문에서는 for i in range(count):와 같이 range에 count를 넣어주면 입력받은 숫자만큼 반복됩니다.

다음과 같이 for에 range 대신 리스트를 넣으면 리스트의 요소를 꺼내면서 반복합니다.

> a = [10, 20, 30, 40, 50]

> for i in a:

      print(i)

10
20
30
40
50  
물론 튜플도 마찬가지로 튜플의 요소를 꺼내면서 반복합니다

- 문자열도 시퀀스 객체라고 했죠?  
  for에 문자열을 지정하면 문자를 하나씩 꺼내면서 반복합니다.

> for letter in 'Python':

     print(letter, end=' ')

P y t h o n

문자열 'Python'의 문자가 하나씩 분리되어 출력되었습니다. 여기서는 print에 end=' '을 지정했으므로 줄바꿈이 되지 않고, 각 문자가 공백으로 띄워져서 출력됩니다.

그럼 문자열 'Python'을 뒤집어서 문자를 출력할 수는 없을까요? 이때는 앞에서 배운 reversed를 활용하면 됩니다.

- reversed(시퀀스객체)

  - for letter in reversed('Python'):

        print(letter, end=' ')

n o h t y P

문자열 'Python'에서 문자 n부터 P까지 출력되었습니다. reversed는 시퀀스 객체를 넣으면 시퀀스 객체를 뒤집어 줍니다(원본 객체 자체는 바뀌지 않으며 뒤집어서 꺼내줌).

# Iterator -break (for / while 가능)

앞서 보았듯이 for 구문에서는 *리스트가 가지고 있는 요소의 수 만큼 for 구문에 속해있는 코드를 실행*합니다.
이걸 **iteration** 이라고 합니다.  
만일 리스트가 5개의 요소를 가지고 있으면 5 iterations 이라고 합니다.
즉 5번 반복한다는 뜻이죠.  
 그래서 for loops를 한국어로 for 반복구문 이라고 하기도 합니다.

그런데 가끔은 중간에 도중하차(?) 하고 싶을때가 있습니다. 굳이 끝까지 for 구문을 진행할 필요 없이 중간에서 멈추고 싶을때는 **break** 문을 사용하면 됩니다.

> break - (for /while 가능)
> For 구문에서 break 문이 실행되면 다음 iteration으로 넘어가지 않고 for 구문에서 빠져 나오게 됩니다.

> Continue - (for / while 가능)

만일 break처럼 for 구문에서 완전히 빠져 나오고 싶지는 않지만 다음 요소, 즉 다음 interation으로 넘어가고 싶을때는 continue 문을 사용하면 됩니다.

![break-continue](https://snscrawler.files.wordpress.com/2017/06/131.png)
