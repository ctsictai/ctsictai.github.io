---

title: Python Apprenticeship Study Part.9
date: "2019-10-22T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python-part9/"
category: "Python/"
tags:

- "Python/"

description: "Decorator"
socialImage: "/media/gutenberg.jpg"

---

# Decorator

**@decorator명** --> 데코레이터 선언

함수 앞뒤에 기능을 추가해서 손쉽게 함수를 활용할 수 있는 기법  
여러 함수에 동일한 기능을 @데코레이터 하나로 간편하게 추가할 수 있다.  
중첩 함수를 만드는 데 있어서 좀 더 편리한 기능이다.  
 중첩 함수를 만드는데 있어서 그 중첩 함수를 글로벌하게 데코레이터로써 쓸 수 있다.

- 대표적인 데코레이터

  1. @staticmethod / @classmethod 등 class에서 메소드에 선언할 때 많이 씀

  2. @auth 웹 페이지 상에서 signin 된 상태를 증명하기 위한 인가 증명에서 많이 쓰인다.

## 데코레이터 작성하기

> def datetime_decorator(func): # <--- datetime_decorator 는 데코레이터 이름, func 가 이 함수 안에 넣을 함수가 됨
>
> > def wrapper(): # <--- 호출할 함수를 감싸는 함수
> >
> > > print ('time ' + str(datetime.datetime.now())) # <--- 함수 앞에서 실행할 내용  
> > >  func() # <--- 함수(데코레이터 적용시킬)  
> > >  print (datetime.datetime.now()) # <--- 함수 뒤에서 실행할 내용
> > > return wrapper # <--- closure 함수로 만든다

기본 구조는 함수를 인자로 불러와서 그 함수를 함수안에서 사용한다는 개념이다.

- print ('time ' + str(datetime.datetime.now())) 부분은
  데코레이터 적용할 함수 전에 실행할 로직이 들어가는 부분

- print (datetime.datetime.now()) 부분은 데코레이터 적용할 함수의 로직이 끝난 뒤 실행할 로직이 들어가는 부분

실행할 함수 앞 뒤로 감싼 로직을 실행한다는 의미에서 wrapper라고 통상적으로 함수명을 많이 짓는다.

## 매개변수와 반환값이 있는 데코레이터

def trace(func): # 호출할 함수를 매개변수로 받음

> def wrapper(a, b): # 호출할 함수 add(a, b)의 매개변수와 똑같이 지정
>
> > r = func(a, b) # func에 매개변수 a, b를 넣어서 호출하고 반환값을 변수에 저장  
> >  print('{0}(a={1}, b={2}) -> {3}'.format(func.**name**, a, b, r)) # 매개변수와 반환값 출력  
> >  return r # func의 반환값을 반환

    return wrapper        # wrapper 함수 반환

@trace # @데코레이터  
def add(a, b): # 매개변수는 두 개

> return a + b # 매개변수 두 개를 더해서 반환

print(add(10, 20))

이렇게 되면 결과값은

add(a=10, b=20) -> 30  
30

이 데코레이터와 함수에서는 데코레이터 함수 내에서 함수를 호출해 변수에 저장 후에 변수를 return하는 방식으로 로직이 구성되었다.  
그렇기 때문에 print가 먼저 실행되고 add 함수의 return이 나중에 실행된 것이다.

- **순서와 함수 호출 여부를 잘 봐야 한다**

## 가변인자 매개변수와 반환값이 있는 데코레이터

위의 과정은 매개변수가 고정되어 있는 함수이다.  
매개변수(인수)가 고정되지 않은 함수는 어떻게 처리할까요? 이때는 wrapper 함수를 가변 인수 함수로 만들면 됩니다.

def trace(func): # 호출할 함수를 매개변수로 받음

> def wrapper(\*args, \*\*kwargs): # 가변 인수 함수로 만듦
>
> > r = func(\*args, \*\*kwargs) # func에 args, kwargs를 언패킹하여 넣어줌  
> >  print('{0}(args={1}, kwargs={2}) -> {3}'.format(func.**name**, args, kwargs, r)) # 매개변수와 반환값 출력  
> >  return r # func의 반환값을 반환

> return wrapper # wrapper 함수 반환

@trace # @데코레이터  
def get_max(\*args): # 위치 인수를 사용하는 가변 인수 함수

> return max(args)

@trace # @데코레이터  
def get_min(\*\*kwargs): # 키워드 인수를 사용하는 가변 인수 함수

> return min(kwargs.values())

결과값  
print(get_max(10, 20))  
print(get_min(x=10, y=20, z=30))

get_max(args=(10, 20), kwargs={}) -> 20  
20  
get_min(args=(), kwargs={'x': 10, 'y': 20, 'z': 30}) -> 10  
10

고정 인자 함수와의 차이점은 wrapper 함수의 인자가 가변인자로 바뀐다는 점이다.

## Method Decorator

클래스메서드나 정적메서드 데코레이터 사용하듯이 클래스의 메소드에 사용되어지는 데코레이터

def h1_tag(function):

> def func_wrapper(self, \*args, \*\*kwargs): # <--- self 를 무조건 첫 파라미터로 넣어야 메서드에 적용가능(클래스에 적용하기 위한)
>
> > return "{0}".format(function(self, \*args, \*\*kwargs)) # <--- function 함수에도 self 를 넣어야 함

> return func_wrapper

### 클래스 선언시 메서드에 데코레이터 적용하기

- @staticmethod / @classmethod와 비슷

class Person:

> def **init**(self, first_name, last_name):
>
> > self.first_name = first_name  
> >  self.last_name = last_name

> @h1_tag  
>  def get_name(self):
>
> > return self.first_name + ' ' + self.last_name – 리턴값 1개!- 위에서 첫번째 인자 {0}으로 인식되어서 들어감

데코레이터 적용 확인해보기  
davelee = Person('Lee', 'Dave')  
print(davelee.get_name())

- Lee Dave

## 파라미터를 가지고 있는 데코레이터

def name_decorator(char): - #char는 name_decorator에 적용될 파라미터 -파라미터 적용을 위한 함수

> def real_deco(func): - #실제 데코레이터 함수 적용 (func) 적용할 함수 인자로 불러
>
> > def wrapper(): - # 감쌀꺼 func이 greetings() – none param --> wrapper no param # 혹 적용할 함수에 인자가 있으면 같이 적용해주자
> >
> > > return func() + char # func에 데코에서 받은 인자 char를 더해줌(둘 다 str type)

> > return wrapper - 리턴 (wrapper 함수 자체를)

> return real_deco – 최종 리턴 – 이래야 함수 실행됨 (함수자체를 리턴해버리니까)

@name_decorator("정우성") – 데코에 파라미터 char가 적용된 모습
def greeting():

> return "Hello, "

결과값  
print(greeting())  
Hello, 정우성
