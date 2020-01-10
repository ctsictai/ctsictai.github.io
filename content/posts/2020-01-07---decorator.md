---
title: Decorator
date: "2020-01-08T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python/django/decorator"
category: "Decorator"
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

## 가변 인수를 가지는 데코레이터

매개변수(인수)가 고정되지 않은 함수를 처리할 때 wrapper 함수를 가변 인수 함수로 만들면 됩니다.

```
def trace(func):                     # 호출할 함수를 매개변수로 받음
    def wrapper(*args, **kwargs):    # 가변 인수 함수로 만듦
        res = func(*args, **kwargs)  # func에 args, kwargs를 언패킹하여 넣어줌
        print('{0}(args={1}, kwargs={2}) -> {3}'.format(func.__name__, args, kwargs, r))
                                     # 매개변수와 반환값 출력
        return res                   # func의 반환값을 반환
    return wrapper                   # wrapper 함수 반환

@trace                   # @데코레이터
def get_max(*args):      # 위치 인수를 사용하는 가변 인수 함수
    return max(args)

print(get_max(10, 20))
> get_max(args=(10, 20), kwargs={}) -> 20
> 20
```

## 매개변수(인자)가 존재하는 데코레이터

데코레이터는 값을 지정해서 동작을 바꿀 수 있습니다

```
def is_multiple(x):              # 데코레이터가 사용할 매개변수를 지정
    def real_decorator(func):    # 호출할 함수를 매개변수로 받음
        def wrapper(a, b):       # 호출할 함수의 매개변수와 똑같이 지정
```

일반적인 데코레이터를 만들 때 함수 안에 함수를 하나만 만들었습니다. 하지만 매개변수가 있는 데코레이터를 만들 때는 함수를 하나 더 만들어야 합니다.

먼저 is_multiple 함수를 만들고 데코레이터가 사용할 매개변수 x를 지정합니다.(매개변수를 지정할 함수) 그리고 is_multiple 함수 안에서 실제 데코레이터 역할을 하는 real_decorator(일반적인 데코레이터 함수)를 만듭니다. 즉, 이 함수에서 호출할 함수를 매개변수로 받습니다. 그다음에 real_decorator 함수 안에서 wrapper 함수를 만들어주면 됩니다.

```
def is_multiple(x):              # 데코레이터가 사용할 매개변수를 지정
    def real_decorator(func):    # 호출할 함수를 매개변수로 받음
        def wrapper(a, b):       # 호출할 함수의 매개변수와 똑같이 지정
            res = func(a, b)     # func를 호출하고 반환값을 변수에 저장
            if res % x == 0:     # func의 반환값이 x의 배수인지 확인
                print('{0}의 반환값은 {1}의 배수입니다.'.format(func.__name__, x))
            else:
                print('{0}의 반환값은 {1}의 배수가 아닙니다.'.format(func.__name__, x))
            return res           # func의 반환값을 반환
        return wrapper           # wrapper 함수 반환
    return real_decorator        # real_decorator 함수 반환
```

여기서는 real_decorator, wrapper 함수를 두 개 만들었으므로 함수를 만든 뒤에 return으로 두 함수를 반환해줍니다.

데코레이터를 사용할 때는 데코레이터에 ( )(괄호)를 붙인 뒤 인수를 넣어주면 됩니다.

```
@데코레이터(인수)
def 함수이름():
    코드
```

```
@is_multiple(3)     # @데코레이터(인수)
def add(a, b):
    return a + b
print(add(10, 20))
> add의 반환값은 3의 배수입니다.
> 30
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

# 실제 데코레이터 생성 예시

로그인 시 JWT 토큰을 발급하고 특정 페이지를 조회하거나 페이지를 게시할 때 로그인 유저인지 확인하기 위한 데코레이터

```

def login_decorator(func):
    def wrapper(self, request, *args, **kwargs):

        if "Authorization" not in request.headers:
            return JsonResponse({"ERROR_CODE":"INVALID_LOGIN"}, status=401)             # 웹페이지에서 기존에 발급받았던 토큰을 request.header에 실어서 보낸다. 이 때 아무것도 없다면 로그인이 안되있다고 판단하고 에러를 리턴한다.

        encode_token = request.headers["Authorization"]                                 # 프론트에서 보낸 토큰

        try:
            data = jwt.decode(encode_token, SECRET['secret'], algorithm='HS256')
            user = User.objects.get(id = data["user_id"])                               # 토큰값에 있는 user_id와 user DB table에 있는 user_id와 비교
            request.user = user                                                         # 프론트엔드에게 받은 request.user에 3번의 자료 저장 =>프론트엔드에게 전달해주기 전 준비과정
        except jwt.DecodeError:                                                         # 토큰값이 decode가 안될 시에 에러 리턴
            return JsonResponse({"ERROR_CODE" : "INVALID_TOKEN"}, status = 401)

        except User.DoesNotExist:                                                       # user가 존재하지 않는 경우 에러 리턴
            return JsonResponse({"ERROR_CODE" : "UNKNOWN_USER"}, status = 401)

        return func(self, request, *args, **kwargs)                                     # 4번에 저장된 request를 데코레이터 리턴
=> 데코레이터종료 및 프론트엔드에게 해당 유저의 정보를 리턴

    return wrapper
```

### 실제 데코레이터 적용 예시

```
@login_decorator                                     # 데코레이터 함수 적용
def get(self, request):                              # 로그인 데코레이터 적용시킬 함수
    user = request.user                              # request.user가 로그인 데코레이터에서 받은 변수값
    data = [{
        "user_name" : user.user_name,
        "user_email" : user.email
    }]
    return JsonResponse({"data": data}, status=200)
```
