---
title: Decorator
date: "2020-01-07T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python/django/decorator"
category: "Python/Django/decorator/login_required"
tags:
  - "Python, Decorator, Login_required"

description: "decorator example"
socialImage: "/media/gutenberg.jpg"
---

# 데코레이터란?

함수(메서드)를 장식하는 개념으로 함수 앞뒤에 기능을 추가해서 손쉽게 함수를 활용할 수 있는 기법이다.

```
class Calc:
    @staticmethod    # 데코레이터
    def add(a, b):
        print(a + b)
```

## 그러면 왜 쓸까?

함수나 클래스의 메서드는 한 함수에 한 기능을 넣는 것이 좋다.  
(Unit test를 생각한다면 더더욱 함수 하나에 한 기능을 넣어야 한다.)

그런데 한 함수에 한 기능을 넣기에는 애매한 경우가 있다.  
혹은 한가지 기능에 간단한 로직을 첨가하는 것이 좀 더 좋아지는 경우  
한 기능을 여러 메서드나 함수에서 계속적으로 사용할 때 함수마다 기능을 넣는 것은 계속 같은 코드가 중복되는 문제가 발생한다.

이럴 때 사용하는 것이 데코레이터이다.

## 데코레이터 형태 설명

```
def trace(func):                             # 호출할 함수를 매개변수로 받음
    def wrapper():                           # 호출할 함수를 감싸는 함수
        print(func.__name__, '함수 시작')    # __name__으로 함수 이름 출력
        func()                               # 매개변수로 받은 함수를 호출
        print(func.__name__, '함수 끝')
    return wrapper                           # wrapper 함수 반환
    # 함수안에서 함수를 만들고 반환하는 클로저 개념

def hello():
    print('hello')
```

> 결과

```
trace_hello = trace(hello)    # 데코레이터에 호출할 함수를 넣음
trace_hello()                 # 반환된 함수를 호출

hello 함수 시작
hello
hello 함수 끝
```

## 데코레이터의 일반 형태

데코레이터 앞에 @를 붙여준다. 그리고 데코레이터를 적용할 함수 앞 줄에 꼭 입력한다.

```
@데코레이터
def 함수이름():
    코드
```

```
def trace(func):                             # 호출할 함수를 매개변수로 받음
    def wrapper():                           # 호출할 함수를 감싸는 함수
        print(func.__name__, '함수 시작')    # __name__으로 함수 이름 출력
        func()                               # 매개변수로 받은 함수를 호출
        print(func.__name__, '함수 끝')
    return wrapper                           # wrapper 함수 반환
    # 함수안에서 함수를 만들고 반환하는 클로저 개념

@trace               # @데코레이터
def hello():
    print('hello')
```

### ※ 데코레이터 여러개 지정하기

```
@데코레이터1
@데코레이터2
def 함수이름():
    코드
```

데코레이터가 위에서부터 차례대로 실행된다.

```

def decorator1(func):
    def wrapper():
        print('decorator1')
        func()
    return wrapper

def decorator2(func):
    def wrapper():
        print('decorator2')
        func()
    return wrapper

# 데코레이터를 여러 개 지정
@decorator1
@decorator2
def hello():
    print('hello')

hello()

decorator1
decorator2
hello
```
