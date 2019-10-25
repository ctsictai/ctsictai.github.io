---
title: Python Apprenticeship Study Part.7
date: "2019-10-22T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part7/"
category: "Python/"
tags:
  - "Python/"

description: "Scope"
socialImage: "/media/gutenberg.jpg"
---

# SCOPE

프로그래밍 언어에서 Scope는 어떠한 객체가 유효한 범위를 뜻한다.
해당 범위를 벗어난다면 객체는 사용할 수 없다.

Python에서 scope은 항상 객체가 선언된 지점에서 위로는 상위 객체 까지, 아래로는 모든 하위 객체들과 그 안에까지가 범위 입니다.

![SCOPE](https://wellsr.com/python/assets/images/2018-09-07-scopes-diagram.png)

## Local Scope

Local scope을 가지고 있는 변수나 함수 혹은 객체는 이름 그대로 특정 범위에서만 유효합니다.  
주로 함수 안에서 선언된 변수나 함수가 local scope을 가지고 있습니다. 그리고 이러한 변수들은 해당 함수 안에서만 유효합니다.

> def func():
>
> > a = 4

    print(a) --> local scope --> 정상 출력

print(a) --> 함수를 벗어남(유효범위 밖) --> 에러 메세지 출력

## Enclosing Scope

Nested function가 있을 때 적용되는 scope.  
부모 함수에서 선언된 변수는 중첩함수 안에서도 유효한 범위를 가지고 있습니다.

> def func():
>
> > a = 4

    print(a) --> local scope --> 정상 출력

> > > def inner():
> > >
> > > > b = 1  
> > > > print(a+b) --> a는 enclosing b는 local scope --> 정상 출력
> > > > print(b) --> b는 inner함수에서만 작동하므로 --> 에러

## Global Scope

Global scope은 함수 안에서 선언된것이 아닌 함수 밖에서 선언된 변수나 함수를 이야기 합니다.  
변수나 함수는 선언된 지점과 동일한 level의 지역, 그리고 더 안쪽의 지역들까지 범위가 유효합니다.

그리고 global scope을 가지고 있는 변수와 함수들은 선언된 지점이 해당 파일에서 가장 바깥쪽에서 선언되므로 해당 파일에서 선언된 지점 아래로는 다 유효한 범위를 가지고 있습니다. 그래서 "Global" scope 이라고 하는 것입니다.

![global vs local](https://t1.daumcdn.net/cfile/tistory/9990474B5B4607D014)

## Built-in Scope

Built-in scope은 scope중 가장 광범위한 scope입니다.  
파이썬안에 내장되어 있는, 파이썬이 제공하는 함수 또는 속성들이 built-in scope를 가지고 있습니다.

그리고 built-in scope는 따로 선언이 없이도 모든 파이썬 파일에서 유효한 범위를 가지고 있습니다.  
예를 들어, list등과 같은 자료구조의 element 총 개수를 리턴하는 len 함수가 바로 built-in scope를 가지고 있습니다.

## Shadowing

파이썬은 변수나 함수의 정의를 찾을때 다음 순서의 scope들 안에서 찾습니다.  
Local => Enclosing => Global => Built-in

그러므로 만일 동일한 이름의 변수들이 서로 다른 scope에서 선언이 되면 더 좁은 범위에 있는 변수(혹은 함수)가 더 넓은 범위에 있는 변수를 가리는 (shadowing)효과가 나타납니다.

- **변수 이름 중복이 안되도록 네이밍에 신경써야 한다.**
