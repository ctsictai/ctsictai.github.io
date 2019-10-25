---
title: Python Apprenticeship Study Part.8
date: "2019-10-22T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part8/"
category: "Python/"
tags:
  - "Python/"

description: "예외처리"
socialImage: "/media/gutenberg.jpg"
---

# 예외처리

# 1. TRY - EXCEPTION

try에 실행할 코드를 넣고 except에 예외가 발생했을 때 처리하는 코드를 넣습니다.

> try:
>
> > 실행할 코드

> except:
>
> > 예외가 발생했을 때 처리하는 코드

20을 정수값을 가진 x 로 나누는 코드를 예를 들면

다른 숫자는 상관없지만 x가 0인 경우는 연산이 되지 않는다.

이 때 처리를 어떻게 할 것인가

> try:
>
> > x = int(input('나눌 숫자를 입력하세요: '))

    y = 20 / x
    print(y)

> except: # 예외가 발생했을 때 실행됨
>
> > print('예외가 발생했습니다.')

x가 0인 경우에는 예외로 간주하여 except에 코딩된 print가 처리 된다.

## 그러면 특정 예외만 처리하고 싶은 경우에는 어떻게 해야 할까?

다음과 같이 정수 두 개를 입력받아서 하나는 리스트의 인덱스로 사용하고, 하나는 나누는 값으로 사용합니다.  
그리고 except를 두 개 사용하고 각각 ZeroDivisionError와 IndexError를 지정합니다.

y = [10, 20, 30]

> try:
>
> > index, x = map(int, input('인덱스와 나눌 숫자를 입력하세요: ').split())

    print(y[index] / x)

> except ZeroDivisionError: # 숫자를 0으로 나눠서 에러가 발생했을 때 실행됨
>
> > print('숫자를 0으로 나눌 수 없습니다.')  0으로 나눌 경우

> except IndexError: # 범위를 벗어난 인덱스에 접근하여 에러가 발생했을 때 실행됨
>
> > print('잘못된 인덱스입니다.')  실수 넣었을 때

주석에 쓰여져 있는 대로 숫자를 0으로 나누는 경우와 인덱스 범위를 넣었을 경우 두 가지 경우를 특정하여 예외처리를 할 수 있다.

## 예외의 에러 메시지 받아오기

except에서 as 뒤에 변수를 지정하면 발생한 예외의 에러 메시지를 받아올 수 있습니다.

앞에서 만든 코드의 except에 as e를 넣습니다. 보통 예외( exception)의 e를 따서 변수 이름을 e로 짓습니다

y = [10, 20, 30]

> try:
>
> > index, x = map(int, input('인덱스와 나눌 숫자를 입력하세요: ').split())

    print(y[index] / x)

> except ZeroDivisionError as e:
>
> > print('숫자를 0으로 나눌 수 없습니다.', e)

> except IndexError as e:
>
> > print('잘못된 인덱스입니다.', e)

**as e** 를 넣어서 달라지는 점은 무엇일까?

> 0으로 나누는 경우인 ZeroDivisionError를 발생해보면

    인덱스와 나눌 숫자를 입력하세요: 2 0 (입력)
    숫자를 0으로 나눌 수 없습니다. (division by zero) --> ()내용이 **as e**로 지정해서 나온 것이다

> 정수가 아닌 실수를 입력하여 IndexError를 발생시키면?

    인덱스와 나눌 숫자를 입력하세요: 3.1 5 (입력)
    잘못된 인덱스입니다. (list index out of range) --> 이게 e로 지정한 것

# Else와 Finally

이번에는 예외가 발생하지 않았을 때 코드를 실행하는 else를 사용해보겠습니다. 다음과 같이 else는 except 바로 다음에 와야 하며 except를 생략할 수 없습니다.

> try:
>
> > 실행할 코드

> except:
>
> > 예외가 발생했을 때 처리하는 코드

> else:
>
> > 예외가 발생하지 않았을 때 실행할 코드

## 예외와는 상관없이 항상 코드 실행하기(finally)

이번에는 예외 발생 여부와 상관없이 항상 코드를 실행하는 finally를 사용해보겠습니다.  
특히 finally는 except와 else를 생략할 수 있습니다. Finally는 코드가 끝나면 무조건 마지막에 실행되는 코드를 말한다.

> try:
>
> > 실행할 코드

> except:
>
> > 예외가 발생했을 때 처리하는 코드

> else:
>
> > 예외가 발생하지 않았을 때 실행할 코드

> finally:
>
> > 예외 발생여부가 상관없이 무조건 마지막에 실행되는 코드

![try-except-else-finally](https://t1.daumcdn.net/cfile/tistory/99611C4C5D4AE03E28)

## 예외 발생시키기

예외를 발생시킬 때는 raise에 예외를 지정하고 에러 메시지를 넣습니다(에러 메시지는 생략 할 수 있음).

- raise 예외('에러메시지')

1. raise의 처리 과정

> def three_multiple():
>
> > x = int(input('3의 배수를 입력하세요: '))

    if x % 3 != 0:                            # x가 3의 배수가 아니면
        raise Exception('3의 배수가 아닙니다.')    # 예외를 발생시킴
    print(x)                             # 현재 함수 안에는   except가 없으므로

            # 예외를 상위 코드 블록으로 넘김

> try:
>
> > three_multiple()

> except Exception as e: # 하위 코드 블록에서 예외가 발생해도 실행됨
>
> > print('예외가 발생했습니다.', e)

이 코드를 x에 3의 배수가 아닌 수를 넣고 실행을 해보면 결과가

> 예외가 발생했습니다. 3의 배수가 아닙니다.

이렇게 발생합니다. except가 출력되고 Exception에서 준 내용이 출력되는 형식입니다.

three_multiple 함수는 안에 try except가 없는 상태에서 raise로 예외를 발생시켰습니다.  
이렇게 되면 함수 바깥에 있는 except에서 예외가 처리됩니다. 즉, 예외가 발생하더라도 현재 코드 블록에서 처리해줄 except가 없다면 except가 나올 때까지 계속 상위 코드 블록으로 올라갑니다.

함수안의 local scope와 바깥의 global scope 구분을 잘해야 합니다.

## 예외 다시 발생 시키기

이번에는 try except에서 처리한 예외를 다시 발생시키는 방법입니다.  
 except 안에서 raise를 사용하면 현재 예외를 다시 발생시킵니다(re-raise).

> except:
>
> > raise ~~~

### assert로 예외 발생시키기(추가 내용)

예외를 발생시키는 방법 중에는 assert를 사용하는 방법도 있습니다.  
 assert는 지정된 조건식이 거짓일 때 AssertionError 예외를 발생시키며 조건식이 참이면 그냥 넘어갑니다. 보통 assert는 나와서는 안 되는 조건을 검사할 때 사용합니다.

x = int(input('3의 배수를 입력하세요: '))  
assert x % 3 == 0, '3의 배수가 아닙니다.' # 3의 배수가 아니면 예외 발생, 3의 배수이면 그냥 넘어감  
print(x)
