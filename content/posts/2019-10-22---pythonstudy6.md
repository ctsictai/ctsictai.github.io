---
title: Python Apprenticeship Study Part.6
date: "2019-10-22T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part6/"
category: "Python/"
tags:
  - "Python/"

description: "Class"
socialImage: "/media/gutenberg.jpg"
---

# 클래스

## 개요

- 클래스는 객체의 구조와 행동을 정의합니다.
- 객체의 클래스는 초기화를 통해 제어합니다.
- 클래스는 복잡한 문제를 다루기 쉽도록 만듭니다.

### 객체와 인스턴스 차이

클래스로 만든 객체를 인스턴스라고도 한다.  
인스턴스라는 말은 특정 객체가 어떤 클래스의 객체인지를 관계 위주로 설명할 때 사용한다.

## 클래스 변수(속성)

클래스 정의에서 메서드 밖에 존재하는 변수를 클래스 변수(class variable)라 하는데, 이는 해당 **클래스를 사용하는 모두에게 공용**으로 사용되는 변수이다. 클래스 변수는 클래스 내외부에서 **"클래스명.변수명"** 으로 엑세스 할 수 있다.

## 인스턴스 변수(속성)

하나의 클래스로부터 여러 객체 인스턴스를 생성해서 사용할 수 있다. 클래스 변수가 하나의 클래스에 하나만 존재하는 반면, 인스턴스 변수는 각 객체 인스턴스마다 별도로 존재한다.

클래스 정의에서 메서드 안에서 사용되면서 **"self.변수명"**처럼 사용되는 변수를 인스턴스 변수(instance variable)라 하는데, 이는 각 객체별로 서로 다른 값을 갖는 변수이다.

인스턴스 변수는 클래스 내부에서는 self.width 과 같이 "self." 을 사용하여 엑세스하고, 클래스 밖에서는 **"객체변수.인스턴스변수"**와 같이 엑세스 한다.

> 만약 특정 변수명이나 메서드를 private으로 만들어야 한다면 두개의 밑줄(\_\_)을 이름 앞에 붙이면 된다.

### 파이썬의 attribute 찾는 과정

1. 파이썬에서 한 객체의 attribute를 읽을 경우에는 먼저 그 객체에서 attribute를 찾아보고,
2. 없으면 그 객체의 소속 클래스에서 찾고,
3. 다시 없으며 상위 Base 클래스에서 찾고,
4. 그래도 없으면 에러를 발생시킨다.

   - **클래스 변수를 엑세스할 때는 클래스명을 사용하는 것이 좋다.**

## 메서드(Method)

클래스 내에 생성된 함수들을 말한다.

Class Col:
  
 def col_mtd(\*kwargs):
수행할 문장 ....

a = Col()  
a.setdata(4,2) 인스턴스를 실행하면

![메서드와 매개변수](https://wikidocs.net/images/page/12392/setdata.png)

인스턴스 메서드(instance method), 클래스 메서드(class method), 정적 메서드(static method)가 있다.

가장 흔히 쓰이는 인스턴스 메서드는 인스턴스 변수에 엑세스할 수 있도록 메서드의 첫번째 파라미터에 항상 객체 자신을 의미하는 "self"라는 파라미터를 갖는다.

인스턴스 메서드는 여러 파라미터를 가질 수 있지만, 첫번째 파라미터는 항상 self 를 갖는다.

### Initializer (초기자)

클래스로부터 새 객체를 생성할 때마다 실행되는 특별한 메서드로 **init**() 이라는 메서드가 있는데, 이를 흔히 클래스 Initializer 라 부른다

Initializer는 클래스로부터 객체를 만들 때, 인스턴스 변수를 초기화하거나 객체의 초기상태를 만들기 위한 문장들을 실행하는 곳이다

입력 파라미터들을 각각 *self.param1*와 *self.param2*라는 인스턴스변수에 할당하여 객체 내에서 계속 사용할 수 있도록 준비한다.

self는 어떠한 실체를 가르키는 단어입니다.

ex) Car class에서 "self" 는 Car class의 객체인 hyundai나 bmw를 가르키는 것이다.

self는 class의 실체(instance)인 객체(object)를 가르킵니다

> 그리고 클래스를 실체화 할때 파이썬이 해당 객체(self)를 자동으로 **init** 함수에 넘겨줍니다.

- **init** 메소드는 클래스가 실체화 될때 자동으로 호출이 된다.
- **init** 메소드의 self 파라미터는 클래스가 실체화된 객체를 넘겨주어야 하며, 파이썬이 자동으로 넘겨준다.
- **init** 메소드의 self 파라미터는 항상 정의되어야 있어야 하며 맨 처음 파라미터로 정의 되어야 한다 (그래야 파이썬이 알아서 넘겨줄 수 있으므로)

> ☞ Class Method - @classmethod

클래스에서 **init** 말고도 다른 메소드를 원하는 대로 추가할 수 있습니다.

메서드 안에서 클래스 인스턴스 만들 수도 있습니다.
Method와 attribute(속성)의 차이는 명사와 동사의 차이라고 생각하시면 됩니다.

속성은 해당 객체의 이름 등의 정해진 성질인 반면에 메소드는 move, eat 등 객체가 행할 수 있는 어떠한 action같은 느낌이라고 생각할 수 있습니다.

> ☞ static Method - @staticmethod

정적메소드라 함은 클래스에서 직접 접근할 수 있는 메소드입니다.

staticmethod는 특별히 추가되는 인자가 없습니다 (self)
부모클래스의 클래스속성 값을 가져오고 class instance 속성은 가져오지 않습니다.

그래서 보통 정적 메서드는 인스턴스 속성, 인스턴스 메서드가 필요 없을 때 사용합니다.

정적 메서드는 인스턴스의 상태를 변화시키지 않는 메서드를 만들 때 사용합니다.

## 상속이란?

클래스에서 상속이란, 물려주는 클래스(Parent Class, Super class)의 내용(속성과 메소드)을 물려받는 클래스(Child class, sub class)가 가지게 되는 것입니다.

### 메서드 오버라이딩

1. 일반적인 메소드 오버라이딩
   메소드 오버라이딩은 부모 클래스의 메소드를 자식 클래스에서 재정의 하는 것입니다.

2. 부모 메소드 호출하기
   부모클래스의 메소드도 수행하고, 자식클래스의 메소드의 내용도 함께 출력하기를 원할 수 있습니다.  
   그럴때는 super() 라는 키워드를 사용하면 자식클래스 내에서 코드에서도 부모클래스를 호출할 수 있습니다.

3. 다중상속
   파이썬은 C++과 같이 다중상속이 가능합니다. 두 개 이상의 부모 클래스로부터 상속이 가능하다는 것.

- . mro() 메소드  
  mro() - 클래스를 작성하면 상속 관계를 확인할 수 있는 메소드

> <객체>.<메소드> - 이를 dot notation 이라고 합니다.

### 클래스 상속 사용하기

여기서 기능을 물려주는 클래스를 기반 클래스(base class), 상속을 받아 새롭게 만드는 클래스를 파생 클래스(derived class)라고 합니다.

보통 기반 클래스는 부모 클래스(parent class), 슈퍼 클래스(superclass)라고 부르고, 파생 클래스는 자식 클래스(child class), 서브 클래스(subclass)라고도 부릅니다.

![기반-파생 클래스](https://dojang.io/pluginfile.php/13905/mod_page/content/2/036001.png)

클래스 상속은 다음과 같이 클래스를 만들 때 ( )(괄호)를 붙이고 안에 기반 클래스 이름을 넣습니다.

class 기반클래스이름:  
  
 실행할 코드

class 파생클래스이름(기반클래스이름):
  
 실행할 코드

ex) 기반 클래스 - 파생클래스 예제

class Person:

    def greeting(self):
        print('안녕하세요.')

class Student(Person):

    def study(self):
        print('공부하기')

- james = Student()
- james.greeting() # 안녕하세요.: 기반 클래스 Person의 메서드 호출
- james.study() # 공부하기: 파생 클래스 Student에 추가한 study 메서드

Student 클래스를 만들 때 class Student(Person):과 같이 ( )(괄호) 안에 기반 클래스인 Person 클래스를 넣었습니다. 이렇게 하면 Person 클래스의 기능을 물려받은 Student 클래스가 됩니다.

Student 클래스에는 greeting 메서드가 없지만 Person 클래스를 상속받았으므로 greeting 메서드를 호출할 수 있습니다.

![클래스상속](https://dojang.io/pluginfile.php/13905/mod_page/content/2/036003.png)

### 상속 관계 확인하기

> issubclass(파생클래스, 기반클래스)

기반 클래스의 파생 클래스가 맞으면 True, 아니면 False를 반환합니다.

### 기반 클래스의 속성 사용하기

기반 클래스에 들어있는 인스턴스 속성을 사용해보겠습니다.

다음과 같이 Person 클래스에 hello 속성이 있고, Person 클래스를 상속받아 Student 클래스를 만듭니다. 그다음에 Student로 인스턴스를 만들고 hello 속성에 접근해봅니다.

class Person:

    def __init__(self):
        print('Person __init__')
        self.hello = '안녕하세요.'

class Student(Person):

    def __init__(self):
        print('Student __init__')
        self.school = '파이썬 코딩 도장'

- james = Student()
- print(james.school)
- print(james.hello) # 기반 클래스의 속성을 출력하려고 하면 에러가 발생함

### super()로 기반 클래스 초기화 하기

이때는 super()를 사용해서 기반 클래스의 **init** 메서드를 호출해줍니다. 다음과 같이 super() 뒤에 .(점)을 붙여서 메서드를 호출하는 방식입니다.

> super().메서드()

class Person:

    def __init__(self):
        print('Person __init__')
        self.hello = '안녕하세요.'

class Student(Person):

    def __init__(self):
        print('Student __init__')
        super().__init__()
        # super()로 기반 클래스의 __init__ 메서드 호출
        self.school = '파이썬 코딩 도장'

- james = Student()
- print(james.school)
- print(james.hello)

실행을 해보면 기반 클래스 Person의 속성인 hello가 잘 출력됩니다.

super().**init**()와 같이 기반 클래스 Person의 **init** 메서드를 호출해주면 기반 클래스가 초기화되어서 속성이 만들어집니다. 실행 결과를 보면 'Student **init**'과 'Person **init**'이 모두 출력되었습니다.

기반 클래스 Person의 속성 hello를 찾는 과정을 그림으로 나타내면 다음과 같은 모양이 됩니다.

![기반클래스 속성찾기 로직](https://dojang.io/pluginfile.php/13907/mod_page/content/3/036004.png)

### 기반 클래스를 초기화하지 않아도 되는 경우

만약 파생 클래스에서 **init** 메서드를 생략한다면 기반 클래스의 **init**이 자동으로 호출되므로 super()는 사용하지 않아도 됩니다.
