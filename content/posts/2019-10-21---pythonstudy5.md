---
title: Python Apprenticeship Study Part.5
date: "2019-10-21T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part5/"
category: "Python/JavaScript"
tags:
  - "Python/JavaScript"

description: "While/Function/Module&Package/Closure/Reduce"
socialImage: "/media/gutenberg.jpg"
---

# While문

while 구문은 특정 조건문이 True 일동안 코드블록을 반복 실행 합니다.

## While Else

파이썬의 while문은 else 문이 추가 될 수 있습니다.  
if 문의 else 문과 유사합니다. If 문의 else 문은 if 문의 조건문이 False이면 실행됩니다.  
While문의 else 문도 while의 조건문이 False 이면 실행됩니다.  
즉 while문이 종료되면 else 문이 실행된다는 뜻입니다.

![while-else](https://qph.fs.quoracdn.net/main-qimg-e45ba5aa18aeafb2de3789317bf8e662)

# 함수(Function)

- 함수의 개념

1. Input을 받아서
2. 어떠한 계산 혹은 로직을 실행하고
3. Output을 생성하는 것

파이썬 함수에서 Input을 parameter라고 하고 Output을 Return이라 한다

def 함수명(매개변수):

<수행할 문장1>  
 <수행할 문장2>  
 return 리턴할 값(optional)

## Parameter vs Argument

| Parameter                             | Argument                          |
| ------------------------------------- | --------------------------------- |
| 함수에 입력으로 전달된 값을 받는 변수 | 함수를 호출할 때 전달되는 입력 값 |

def add(a, b): # **a, b는 매개변수**

return a+b

print(add(3, 4)) # **3, 4는 인수**

## Input과 output 형태에 따른 함수의 형태

1. 일반적인 함수

- 입력값 – 파라미터든 인자든 여기서는 파라미터 (a, b)가 있다
- 출력값 – return result 가 존재함

def add(a, b):

    result = a + b
    return result

2. 입력값이 없는 함수

- 입력값 – () 이게 지정이 안되어 있는 함수!
- 출력값 – return ‘Hi’ 로 지정은 되어 있음

def say():

        return 'Hi'

_함수 호출하여 출력값 보이게 하려면?_  
 A = say() # 입력값을 안넣어야 – 앞에서 함수 정의 했던것과 동일하게  
 Print(a) => Hi

3. 출력값이 없는 함수

- 입력값 – a, b 파라미터
- 출력값 – return 이 없음

def add(a, b):

print("%d, %d의 합은 %d입니다." % (a, b, a+b))

### **주의**

- print는 리턴값이 아니고 상태를 출력해서 보여주는 명령어에 불과함 리턴값이 아님! 위의 함수를 리턴값을 찾으면 None 출력

- 리턴값이 없다는 얘기

4. 입, 출력값이 없는 함수

def say():

     print('Hi')

★ 함수를 사용할 단 한가지 방법 – 입출력값이 모두 없기 때문에

say() – 이렇게 하는 수 밖에! --> 함수이름()

Hi

## 입력값이 몇 개인지 정확히 모른다면?

1. Non-keyworded variable length of arguments

def add_many(*args): <-- *args – 입력값이 여러 개인 것을 알려주는 명령어 \*이것만인데 args도 세트로 같이 쓴다.

     result = 0
     for i in args:
     result = result + i
     return result

- 호출 시

  Add_many(1, 2, 3, 4) or Add_many(1, 2, 3, 4, 5 …..) or Add_many()

  - 모두 가능(list형태로 들어가서 순서가 매우 중요)

2.  Keyworded variable length of arguments

- Argument 수를 0부터 N까지 유동적으로 넘겨줄 수 있습니다.
- Keyword가 미리 정해져 있지 않기때문에 원하는 keyword를 유동적으로 사용할 수 있습니다.
- Keyworded variable length of arguments는 dictionary 형태로 지정됩니다.

def buy_A_car(\*\*kwargs):

    print(f"다음 사양의 자동차를 구입하십니다:")

    for option in kwargs:
        print(f"{option} : {kwargs[option]}")

- 호출 시

  `buy_A_car(seat="가죽", blackbox="최신", tint="yes")` - dict type 형태로 들어감 순서가 중요하지 않음

3. mixing args and kwargs  
   둘 다 쓰면 parameter에 있어서 굉장히 유동적인 함수가 가능함

def do_something(\*args, \*\*kwargs):

     ## some code here...
     ....

- 호출 시

  do_something(1, 2, 3, name="정우성", age=45)  
   do_something(1, 2, 3, 4, 5, "hello", {"주소" : "서울", "국가" : "한국"})  
   do_something(name="정우성", gender="남", height="187")  
   do_something(1)  
   do_something()

# Modules & package

변수나 함수 or 클래스 등을 모아 놓은 파일

- 하는 이유는 ?

  - 다른 파일에서 재사용이 가능
  - 코드를 나누어서 정리하고 유지보수가 좋게 하기 위해서

# 모듈&패키지 불러오기

From 모듈 이름 import 함수/변수/ 클래스, …… as 닉네임

- 닉네임 : 이 파일 안에서 모듈을 사용하기위한 임시이름

# 클로져(closure)

1. 중첩 함수가 부모 함수의 변수나 정보를 중첩 함수 내에서 사용한다
2. 부모 함수는 리턴값으로 중첩 함수를 리턴한다.
3. 부모 함수에서 리턴 했으므로 부모 함수의 변수는 직접적인 접근이 불가능 하지만 부모 함수가 리턴한 중첩 함수를 통해서 사용될수 있다.

> 그렇다면 closure는 언제 사용하는 것일까요?

     어떠한 정보를 기반으로 연산을 실행하고 싶지만 기반이 되는 정보는 접근을 제한하여 노출이 되거나 수정이 되지 못하게 하고 싶을때 사용합니다.

주로 factory 패턴을 구현할때 사용되는데요, factory는 공장이란 뜻이죠.
즉 뭔가를 생성해내는 패턴입니다. 주로 함수나 오브젝트를 생성해내는데 사용됩니다.  
Factory에서 뭔가를 생성해 내기 위해서는 설정값이 필요할것입니다.
그 설정값을 노출하지 않아서 수정이 불가능하게 하면서 해당 설정값을 기반으로한 연산을 할 수 있는 함수를 만들때 closure를 사용할 수 있습니다.

def generate_power(base_number):
def nth_power(power):
return base_number \*\* power

    return nth_power

> 호출 시

calculate_power_of_two = generate_power(2) – 부모 함수의 base_number 지정

calculate_power_of_two(7) - 부모함수의 base_number 2는 이미 지정 되어 있고

power parameter만 설정하여 대입하여 2 \*\* 7이 되었음

> 128

# Reduce 메서드 in Javascript

`reduce()` 메서드는 배열의 각 요소에 대해 주어진 리듀서(reducer) 함수를 실행하고, 하나의 결과값을 반환합니다.

리듀서 함수는 네 개의 인자를 가집니다.

1. 누산기accumulator (acc)
2. 현재 값 (cur)
3. 현재 인덱스 (idx)
4. 원본 배열 (src)

리듀서 함수의 반환 값은 누산기에 할당되고, 누산기는 순회 중 유지되므로 결국 최종 결과는 하나의 값이 됩니다.

## 구문

> `arr.reduce(callback[, initialValue])`

### parameter

- callback  
  배열의 각 요소에 대해 실행할 함수. 다음 네 가지 인수를 받습니다. - accumulator  
   누산기accmulator는 콜백의 반환값을 누적합니다. 콜백의 이전 반환값 또는, 콜백의 첫 번째 호출이면서 initialValue를 제공한 경우에는 initialValue의 값입니다. - currentValue  
   처리할 현재 요소. - currentIndex Optional  
   처리할 현재 요소의 인덱스. initialValue를 제공한 경우 0, 아니면 1부터 시작합니다. - array Optional  
   reduce()를 호출한 배열.
- initialValue Optional  
  callback의 최초 호출에서 첫 번째 인수에 제공하는 값. 초기값을 제공하지 않으면 배열의 첫 번째 요소를 사용합니다. 빈 배열에서 초기값 없이 reduce()를 호출하면 오류가 발생합니다.

> 콜백의 최초 호출 때 accumulator와 currentValue는 다음 두 가지 값 중 하나를 가질 수 있습니다.
>
> > - 만약 reduce() 함수 호출에서 initialValue를 제공한 경우, accumulator는 initialValue와 같고 currentValue는 배열의 첫 번째 값과 같습니다.
> > - initialValue를 제공하지 않았다면, accumulator는 배열의 첫 번째 값과 같고 currentValue는 두 번째와 같습니다.

배열이 비어있는데 initialValue도 제공하지 않으면 TypeError가 발생합니다.  
그래서 보통 initialValue를 주는 것이 안전합니다.

### reduce 작동방식

> [0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array) {return accumulator + currentValue;});

| callback   | accumulator | currentValue | currentIndex | array           | 반환 값 |
| :--------- | :---------: | :----------: | :----------: | :-------------- | :-----: |
| 1번째 호출 |      0      |      1       |      1       | [0, 1, 2, 3, 4] |    1    |
| 2번째 호출 |      1      |      2       |      2       | [0, 1, 2, 3, 4] |    3    |
| 3번째 호출 |      3      |      3       |      3       | [0, 1, 2, 3, 4] |    6    |
| 4번째 호출 |      6      |      4       |      4       | [0, 1, 2, 3, 4] |   10    |

reduce()가 반환하는 값으로는 마지막 콜백 호출의 반환값(10)을 사용합니다.

완전한 함수 대신에 화살표 함수를 제공할 수도 있습니다. 아래 코드는 위의 코드와 같은 결과를 반환합니다.

- [0, 1, 2, 3, 4].reduce( (prev, curr) => prev + curr );

> reduce()의 두 번째 인수로 초기값을 제공하는 경우, 결과는 다음과 같습니다:

[0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array)  
{
return accumulator + currentValue;
}, 10);

| callback   | accumulator | currentValue | currentIndex | array           | 반환 값 |
| :--------- | :---------: | :----------: | :----------: | :-------------- | :-----: |
| 1번째 호출 |     10      |      0       |      0       | [0, 1, 2, 3, 4] |   10    |
| 2번째 호출 |     10      |      1       |      1       | [0, 1, 2, 3, 4] |   11    |
| 3번째 호출 |     11      |      2       |      2       | [0, 1, 2, 3, 4] |   13    |
| 4번째 호출 |     13      |      3       |      3       | [0, 1, 2, 3, 4] |   16    |
| 5번째 호출 |     16      |      4       |      4       | [0, 1, 2, 3, 4] |   20    |

[자세한 내용은 여기서 확인하세요](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)
