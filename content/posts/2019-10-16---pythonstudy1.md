---
title: Python Apprenticeship Study Part.1
date: "2019-10-16T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part1/"
category: "Python"
tags:
  - "Python"

description: "변수 선언/연산자/들여쓰기/if 조건문"
socialImage: "/media/gutenberg.jpg"
---

# 변수 할당

- name = "wsd"
- 변수 이름 = 변수 값

> javascript와 달리 var / let /const를 붙일 필요가 없다.

# 주요 연산자

- **"+"** 덧셈 _a + b_
- **"-"** 뺄셈 _a - b_
- **"\*"** 곱셈 _a \* b_
- **"/"** 나눗셈 _a / b_
- **"//"** 버림 나눗셈 _a // b_
- **"%"** 나머지 _a % b_
- "**" 거듭제곱 \_a ** b\_

- **"+="** 덧셈 후 할당 _a += b_
- **"-="** 뺄셈 후 할당 _a -= b_
- **"\*="** 곱셈 후 할당 _a \*= b_
- **/=** 나눗셈 후 할당 _a /= b_

## 사칙연산 규칙

일반적인 수학적 사칙연산의 규칙을 따른다.

# Literal string interpolation

사용하려면 다음의 문법을 지켜야 합니다

1. 먼저 따옴표 앞에 **"f"** 를 붙여야 합니다. f를 붙이면 파이썬은 f 다음에 오는 strin g 값을 literal string interpolation 이라고 인지하고, string 안에있는 변수들을 실제 값으로 치환 합니다.
2. 치환 하고 싶은 변수 (혹은 변수가 아니어도 됩니다. 예를 들어 함수 호출이 될 수도 있습니다) 를 **중괄호**{}를 사용해서 표시합니다.

<code>print( f"""wecol {date} at {inventor} live in {location}""")</code>

> print(f"""{} """)

# indention(들여 쓰기)

하지만 파이썬에서는 들여쓰기는 요구사항 입니다. 들여쓰기를 통해 코드의 종속성을 나타냅니다.
예를 들어. JavaScript나 자바등의 다른 언어들은 함수에 종속된 코드를 나타내기 위해서 중괄호 ({ }) 를 사용합니다. 하지만 파이썬에서는 중괄호를 사용하지 않고 들여쓰기를 사용해서 종속된 코드를 나타냅니다.

![들여쓰기](http://pythonstudy.xyz/images/basics/identation.png)

# If statement (조건문)

> <code> if name == "차은우":</code>
>
> > <code> print(f"Hello {name}") </code>

**==** 는 같다 Equal의 뜻이다.

## 주의

> if 구문과 연결되어 있는 코드들은 if 구문 보다 더 안쪽으로 간격이 들어와 있어야 합니다.

### 홀짝 구문하는 if문

나머지 연산자 %을 사용하면 된다.

<CODE> if a % 2 == 0:</code> 이면 짝수
<CODE> if a % 2 != 0:</code> 이면 홀수

- **!=** 는 같지 않다 not equal의 뜻이다.

- **>** 는 더 크다 greater than의 뜻이다.

- **<** 는 더 작다 less than의 뜻이다.

- **>=**는 같거나 크다 greater than or equal to

- **<=**는 같거나 작다 less than or equal to

# Elif and Else

## elif

- elif 는 if 구문을 보조 하는 역할 입니다. elif 는 else if 를 줄인뜻입니다.

1. elif 는 if 구문과 연결되어 사용되며 if 구문이 먼저 선행 되고 그 다음에 위치하게 됩니다.
2. elif 는 만일 if 구문이 False 이면 실행되며 if 구문이 False 일 경우 다른 condition을 테스트 한 후 실행됩니다.

> if car == "현대":
>
> > print("현대는 국산차")

> elif car == "기아":
>
> > print("기아는 국산차")

- elif는 if문이 실행 된 뒤 다음을 진행 하는 코드이다.

## else

- else는 if 구문 (그리고 elif 구문이 있다면 elif 구문) 의 condition이 False인 경우 default 로 실행됩니다.
- else 는 if 구문과 elif 구문이 먼저 선행되고 마지막에 위치하게 되며 if / elif 구문의 코드가 실행 되지 않으면 마지막으로 default로 실행됩니다.

![1](http://i.imgur.com/fqJOBUS.png)
