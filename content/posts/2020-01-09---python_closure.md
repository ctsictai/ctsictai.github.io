---
title: Python Closure
date: "2020-01-09T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/python/closure"
category: "Python/Closure"
tags:
  - "Python, Closure"

description: "closure in python"
socialImage: "/media/gutenberg.jpg"
---

전에 봤던 데코레이터의 개념을 정확히 알기 위해서는 Closure를 알아야한다. Closure의 개념을 알기 위해서는 변수의 범위를 알아야한다.(SCOPE)
전에 정리를 한번했었는데 간단히 정리하고 넘어가도록 한다.

# 변수의 사용 범위 알아보기

Closure 개념을 알아보기 전에 변수의 사용범위에 대해 짚고 넘어간다.

```
x = 10       # 전역변수 func 함수 안에서도 유효하고 func 밖에서도 유효한 변수
def func():
  a = 4      # 지역변수 - func 함수안에서만 유효한 변수
  print(a)   # --> local scope --> 정상 출력

print(a)     # 에러남 --> why? : 지역변수 a를 전역범위에서 사용했기 때문에
print(x)     # 10 정상 출력
```

## 함수 안에서 전역변수 변경하기

```
x = 10       # 전역변수 func 함수 안에서도 유효하고 func 밖에서도 유효한 변수
def func():
  global x   # 글로벌 변수 선언
  x = 4      # 함수 내에서 뿐만 아니라 전역에서도 유효한 변수가됨

func()       # func 함수 호출 global x --> 4로 변경
print(x)
4            # func에서 선언된 global x 값으로 결과값
```

## 함수 안의 함수에서 지역변수 변경

```
def A():
    x = 10        # A의 지역 변수 x
    def B():
        x = 20    # x에 20 할당 -> 이건 B의 지역변수로 위의 x와 다르다

    B()
    print(x)      # A의 지역 변수 x 출력

A()
10               # A의 지역변수값 그대로 출력
```

x값을 일치 하기 위해서는 지역변수값을 변경 시켜야 한다.

```
def A():
    x = 10          # A의 지역 변수 x
    def B():
        nonlocal x  # 현재 함수의 바깥쪽에 있는 지역 변수 사용
        x = 20      # A의 지역 변수 x에 20 할당

    B()
    print(x)        # A의 지역 변수 x 출력

A()
20                  # B에서 바뀐 값 20 이 출력
```

- nonlocal이 지역변수를 찾는 순서
  가까운 함수부터 지역 변수를 찾고, 지역 변수가 없으면 계속 바깥쪽으로 나가서 찾습니다.(계층적)

실무에서는 이렇게 여러 단계로 함수를 만들 일은 거의 없다고 한다. 그리고 함수마다 이름이 같은 변수를 사용하기 보다는 변수 이름을 다르게 짓는 것이 좋습니다.

전역 변수는 코드가 복잡해졌을 때 변수의 값을 어디서 바꾸는지 알기가 힘듭니다. 따라서 전역 변수는 가급적이면 사용하지 않는 것을 권장합니다.

# Closure란?

어떤 함수 내부에서 정의된 함수는 클로저가 될 수 있으며, 클로저는 바깥 함수로부터 생성된 변수값을 변경 또는 저장할 수 있는 함수이다.

- 언제 활용??  
  지역 변수와 코드를 묶어서 사용하고 싶을 때 활용합니다. 또한, 클로저에 속한 지역 변수는 바깥에서 직접 접근할 수 없으므로 데이터를 숨기고 싶을 때 활용합니다.

```
def login_decorator(func):
    def wrapper(self, request, *args, **kwargs):   # Closure 부분

        if "Authorization" not in request.headers:
            return JsonResponse({"ERROR_CODE":"INVALID_LOGIN"}, status=401)

        encode_token = request.headers["Authorization"]  # request가 바깥 전역함수 login_decorator의 func에서 request 변수 중 headers에 있는 ["Authorization"]값
        try:
            data = jwt.decode(encode_token, SECRET['secret'], algorithm='HS256')
            user = User.objects.get(id = data["user_id"])     request.user = user       # request.user에 새로운 값 부여
        except jwt.DecodeError:
            return JsonResponse({"ERROR_CODE" : "INVALID_TOKEN"}, status = 401)

        except User.DoesNotExist:
            return JsonResponse({"ERROR_CODE" : "UNKNOWN_USER"}, status = 401)

        return func(self, request, *args, **kwargs) # 바뀐 변수값을 리턴
    return wrapper     # 함수 자체를 리턴
```

## lamda로 클로져 만들기

```
def calc():
    a = 3
    b = 5
    return lambda x: a * x + b    # 람다 표현식을 반환(함수 따로 안만들고)

c = calc()
print(c(1), c(2), c(3), c(4), c(5))
```
