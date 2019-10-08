---
title: 09.30 ~ 10.04 한 주 정리 part.1
date: "2019-10-08T17:41:37.121Z"
template: "post"
draft: false
slug: "/posts/perfecting-the-art-of-perfection/"
category: "JavaScript"
tags:
  - "JavaScript"
  - "Web Developer"
description: "1. 변수

변수를 선언하는 명령어 - var / let / const - 최신버전에서는 var 사용은 지양한다.

- Let – 변수값 수정 가능

- Const – 변수값 수정 불가능

- [ ] 변수 선언 문법."
socialImage: "/media/image-2.jpg"
---

# 09.30 ~ 10.04 한 주 정리 part.1

## 1. 변수

변수를 선언하는 명령어 - var / let / const - 최신버전에서는 var 사용은 지양한다.

- Let – 변수값 수정 가능

- Const – 변수값 수정 불가능

- [] 변수 선언 문법

![](https://blogfiles.pstatic.net/MjAxOTEwMDRfMTIz/MDAxNTcwMTk4OTQ1NzQx.a4yWDmRikvIoOXDD6w84MQEZsHreb5R5xINSum38KJYg.dscq-Xo5r3CKotSm2oiZJHxT4IACet5L4_o_mciQVoog.PNG.civicofjuve/image.png?type=w1)

◎ 변수이름 짓기 규칙

- 대소문자 구문 - 변수이름, 함수이름, 연산자 모두 대소문자를 구분합니다. 따라서 myName과 MyName은 다른 변수입니다.

- 변수 이름을 정할 때, 첫 번째 문자는 반드시 글자나 밑줄(\_), 달러기호(\$)중 하나입니다.

- 두 번째 문자부터는 글자, 밑줄, 달러, 숫자 중에서 자유롭게 쓸 수 있습니다.

- 변수이름, 함수이름 등 camelCase(카멜케이스) 방식으로 쓸 것.

※ 카멜케이스(camelCase)란?

낙타 등처럼 울퉁불퉁하다는 소리입니다. 단어가 새로 시작할 때부터 대문자로 쓰면 됩니다.

- 변수 선언 ex)

> let name = “김개발”;

- 변수 값 수정

> Name = “김코딩”;

- [ ] List item

(주의할 점!) 변수 값 수정 시에는 let을 붙일 필요가 없다 why? 이미 선언된 변수명이기 때문에

변수명만 선언하고 변수값은 나중에 줘도 됨!

- let man ;

> man = “남”;

## 2. 함수(function)

- [ ] 텍스트 추가

![](https://blogfiles.pstatic.net/MjAxOTEwMDRfMjQg/MDAxNTcwMTk5MjY0OTQ0.-aIwwtQ2LF8WdknKCnADHT8DE94qZDiXp1V-bNdChLsg.-n75Xf-fl31jUw1FPvf_3YIdKIAys3xRC4Nn6jK63pcg.PNG.civicofjuve/image.png?type=w1)

![](https://blogfiles.pstatic.net/MjAxOTEwMDRfMTk0/MDAxNTcwMTk5MjY5OTE3.XvtmLWFKhgRfsBwixP71SPirBqMvXIX4HmjoPR3m3Jog.aDKIvV46VDz6wi_TSEww4Mgl9qQN2iyDPQhR5I6Inpwg.PNG.civicofjuve/image.png?type=w1)

1. function 키워드로 시작하여

2. 함수 이름을 지어주고

3. 함수를 알리는 괄호((): parentheses)를 열고 닫고

4. 함수의 시작을 알리는 중괄호({: curly bracket)을 열어줍니다.

5. 실행할 코드를 작성합니다. 함수의 body라고 부르기도 합니다. 이 부분에 들여쓰기가 되어있습니다. 함수 내부에 있는 코드라는 것을 알기 좋게 하려고 들여쓰기 하였습니다.

6. return(반환) 할 것이 있다면 작성합니다.

7. 중괄호(}: curly bracket)로 닫아줍니다.

8. 함수 호출(실제 js에서 실행되는 것)

### 새로운 함수 정의법 - Arrow Function

_ES6에서 새로 생긴 방법_

> const getName = (name) => {}

Const getName(함수명) = (name)인자 – 인자가 하나면 ( ) 생략가능 나머지는 안됨 => {}(로직) – return만 있는 경우 {return} 모두 생략 가능

> getName(); – 호출방식은 기존과 동일하다.

\*구버전 vs ES6 함수 선언 비교
| 구버전 | ES6 |
|--|-- |
| let getEmail = function(username){return `${username}@gmail.com`; | const getEmail = username => `${username}@gmail.com`; |

### 함수의 인자(argument) 대하여

어떤 함수를 호출했을 때 전달받는 Value

function multiply(arg1, arg2){ - multiply라는 함수는 인자를 2개 받는다.

var myNumber = arg1 \* arg2; - 받은 인자 2개를 사용하였다.

if (myNumber > 100) { 받은 인자 2개를 사용한 것에 조건문 추가

return "크다!"; - 출력되는 결과

} else{

return "작다!"; - 출력되는 결과 – 함수의 실행결과를 출력해주는 함수 안해주면

undefined가 출력이 된다.

}

}

## 3. 수학식 표현 방법

★ Num++ => num = num + 1 – 계산을 해서 그 결과를 리턴 까지 하는 명령어

★ Num-- => num = num - 1

※ 중요한 코딩 오류 중 하나 num++의 js 성질

`let num = 1;`

`let newNum = num++;`

- newNum은 num의 1이 할당되고 num++이게 실행되서 num은 2가 됨

newNum에도 2가 할당되고 싶으면

`let newNum = ++num;` - ++을 앞에다가 써야 됨

## 4. 데이터 타입

- Undefined - 정의 되지 않는 변수
- null - 없는 값
- boolean - True/False | 조건문에서 조건에 대한 결과 값으로 많이 쓰이는 타입
- 숫자 - number
- 문자열 - string으로 " " 나 ' ' 혹은 `` 둘러 쌓인 값
- 객체 - function을 제외한 모든 객체
