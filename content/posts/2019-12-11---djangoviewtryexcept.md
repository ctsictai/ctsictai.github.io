---
title: Django View part3
date: "2019-12-11T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/django-part10/"
category: "Python/View/Try/Exception/"
tags:
  - "Python/Django"

description: "Views Try-Except & django give exceptions"
socialImage: "/media/gutenberg.jpg"
---

# Exception Handling

- Exception : 정상적인 프로그램 흐름을 중단시키는 에러를 말합니다.
- Exception Handling : 정상적인 프로그램 흐름을 중단하고 주변의 컨텍스트 또는 코드 블록에서 계속하기위한 메커니즘

# Try / Except - Exception Handling Method

try 블록 수행 중 오류가 발생하면 except 블록이 수행된다. 하지만 try 블록에서 오류가 발생하지 않는다면 except 블록은 수행되지 않는다.

- try-except 코드 예시

```
try:
    ...
except [발생 오류[as 오류 메시지 변수]]:
    ...
```

- try finally
  finally절은 try문 수행 도중 예외 발생 여부에 상관없이 항상 수행된다. 보통 finally절은 사용한 리소스를 close해야 할 때에 많이 사용한다.

```
f = int(s)
try:
    # 무언가를 수행한다.
finally:
    f.close()
```

f변수는 int()함수인데 try문에 어떠한 로직을 실행하고 exception에 관계없이 finally 절에서 f.close()로 로직을 마무리 할 수 있다.

## 여러개의 오류 처리시

```
try:
    ...
except 발생 오류1:
   ...
except 발생 오류2:
   ...
```

> 실제 코드 예시

```
class SigninView(View):
    def post(self, request):
        data = json.loads(request.body)

        try:
            validate_email(data["email"])
            user_data      = User.objects.get(email=data["email"])
            user_password  = user_data.password.encode('utf-8')
            byted_password = data['password'].encode('utf-8')

            if bcrypt.checkpw(byted_password, user_password):
                payload = {
                        "user_id"       : user_data.id,
                        "user_is_maker" : user_data.is_maker,
                        "exp"           : WEDIZ_SECRET['exp_time'],
                        }
                jwt_encode = jwt.encode(payload, WEDIZ_SECRET['secret'], algorithm="HS256")
                token = jwt_encode.decode("utf-8")
                return JsonResponse({"VALID_TOKEN" : token}, status=200)

            else:
                return JsonResponse({"MESSAGE" : "INVALID_PASSWORD"}, status=401)
        except User.DoesNotExist:
            return JsonResponse({"MESSAGE" : "INVALID_USER"}, status=401)
        except KeyError:
            return JsonResponse({"MESSAGE" : "INVALID_INPUT"}, status=400)
```

- try - except에서 if문으로 한번 더 binary decision 로직을 구현하였다.
- signin 로직이다. 이메일과 패스워드를 비교하여 일치하는 지 여부를 판별한다.
- except에서는 유저가 존재하지 않는 경우
- jsonresponse에서 key가 잘못된 경우에 예외처리로 처리하였다.
- 예외가 2개 이상인 경우 except를 예외 처리 수 만큼 붙여주면 된다.
- jsonresponse는 프론트에 http 통신으로 보내는 응답값이므로 json형식에 맞는 body 설정과 status설정을 해줘야 한다. status는 http status code를 뜻한다.
  - except는 예외처리이므로 보통 http status 400번대 에러가 status로 가장 많이 발생한다.
  - 서비스 상에서는 발생할 수 있는 모든 예외처리를 하는 것이 맞다.
    그래서 500번대 에러도 예외처리로 처리하면 좋다

# 예외 발생 시키기

프로그래밍을 하다 보면 종종 오류를 일부러 발생시켜야 할 경우도 생긴다. 파이썬은 raise 명령어를 사용해 오류를 강제로 발생시킬 수 있다.

예를 들어 부모 밑에 자식을 무조건 생성하고 싶을 때에 예외를 일부러 발생시켜 자식을 같이 생성하라고 강제할 수 있다. 그 때 개략적으로 코드를 아래와 같이 구성한다.

```
class Parent:
    def child(self):
        raise NotImplementedError
```

그러면 실제로 자식없는 부모를 생성하면 어떻게 될까?

```
class Jang(Parent):
    pass

jand = Jang()
jang.child()

Traceback (most recent call last):
  File "...", line 33, in <module>
    jang.child()
  File "...", line 26, in fly
    raise NotImplementedError
NotImplementedError
```

- Parent 클래스에 속해있는 child 함수를 구현하지 않으면 raise를 통해 에러가 도출 되도록 설계되어 있기 때문이다. 에러를 발생시키지 않으려면 child 함수를 구현해야한다.
  - 메소드 오버라이딩을 해야한다(상속받는 함수 재구현)

```
class Jang(Parent):
    def child(self):
        print("자식입니다")

jand = Jang()
jang.child()

자식입니다
```

# 예외이름을 잘 모르는 경우

장고나 파이썬에서 제공하는 기본적인 예외들이 있다. 이 경우에는 이 예외를 import해서 가져다가 쓰면되지만 이 경우에 해당되지 않거나 초보자는 잘 모를 때가 있다. 그럴 때 쓸 수 있는 방법이다.

```
try:
    test = []
    print(test[0])

except Exception as ex:
    print('에러가 발생 했습니다', ex)

    에러가 발생 했습니다 list index out of range
```

- 최후의 수단으로 어쩔수 없을 때 쓰는 것이 좋다
- 왠만한 예외는 장고나 파이썬에서 제공하고 있으므로 import해서 쓰자

# 장고에서 제공하는 예외

장고의 예외는 django.core.exceptions에서 import 해야한다.  
많은 종류가 있지만 제가 생각하기에 사용빈도가 높거나 중요한 것 위주로 리뷰해본다.

```
from django.core.exceptions import ObjectDoesNotExist, .....
```

## 1. ObjectDoesNotExist

DoesNotExist 예외 클래스의 기본 클래스이다. ObjectDoesNotExist는 전체 모델에 적용할 수 있는 일반적인 예외처리이다.

## 2. DoesNotExist

ObjectDoesNotExist 자식 클래스로써 특정 클래스에 속성으로 사용되어 진다.  
**EX)**

```
except User.DoesNotExist:
    return JsonResponse({"MESSAGE" : "YOU_ARE_NOT_USER"}, status=401)
```

## 3. MultipleObjectsReturned

예외 명 그대로 한 개의 쿼리셋 오브젝트가 리턴되야 하는데 다수의 쿼리셋 오브젝트가 리턴될 때 발생되는 예외처리이다.

- 주로 쿼리셋 **get()** 함수를 사용할 때 발생하는 예외로서 get은 한 개의 쿼리셋 오브젝트를 리턴해야 하는데 2개 이상 리턴되는 경우 예외를 발생시킨다.

## 4. ImproperlyConfigured

장고가 어떻게 든 잘못 구성된 경우 예외가 발생 settings.py 부정확하거나 파싱 할 수 있습니다

## 5. ValidationError

ValidationError데이터 형식 모델 필드 유효성 검사를 실패 할 경우 예외가 발생합니다. 자세한 사항은 양식 및 필드 유효성 검증 , 모델 필드 유효성 검증을 참고해야 한다. 다음에 다뤄보도록 하겠다

## 6. TransactionManagementError

데이터 베이스 트랜잭션에 대한 모든 문제에 대해 발생하는 예외로서
`django.db.transaction` 에서 import해야한다.

## 7. 데이터베이스예외 중 IntegrityError

`django.db` 에서 import한다.  
외래 키 검사 실패, 중복 키 등과 같이 데이터베이스의 관계 무결성에 영향을주는 경우 예외가 발생합니다.

## 7-2. 데이터베이스예외 중 ProgrammingError

이 예외는 SQL에 구문 오류가 있거나 테이블을 찾을 수 없는 경우와 같이 프로그래밍 오류에서 발생합니다.

# Python에서 제공하는 예외들

## 1. Exception

가장 기본이 되는 예외처리 클래스  
모든 시스템 종료 외의 내장 예외는 이 클래스 파생됩니다. 모든 사용자 정의 예외도 이 클래스에서 파생되어야 한다.

## 2. AttributeError

어트리뷰트 참조나 대입이 실패할 때 발생(속성 참조 실패)  
보통 view 로직에서 발생한다.

## 3. ImportError

import 문이 모듈을 로드하는 데 문제가 있을 때 발생합니다.  
또한 from ... import 에서 임포트 하려는 이름을 찾을 수 없을 때도 발생합니다.

## 4. KeyError

매핑 (딕셔너리) 키가 기존 키 집합에서 발견되지 않을 때 발생한다.

## 5. SyntaxError

parser가 문법 오류를 만날 때 발생합니다. import 문에서, 내장 함수 exec() 나 eval() 호출에서, 초기 스크립트나 (대화형으로) 표준 입력을 읽을 때 발생할 수 있습니다.

세부 사항을 쉽게 확인할 수 있도록, 이 클래스의 인스턴스에는 filename, lineno, offset 및 text 어트리뷰트가 있습니다. 예외 인스턴스의 str()은 메시지만 돌려줍니다.

## 6. TypeError

연산이나 함수가 부적절한 형의 객체에 적용될 때 발생합니다. 연관된 값은 형 불일치에 대한 세부 정보를 제공하는 문자열입니다.

이 예외는 객체에 시도된 연산이 지원되지 않으며 그럴 의도도 없음을 나타내기 위해 사용자 코드가 발생시킬 수 있습니다. 만약 객체가 주어진 연산을 지원할 의사는 있지만, 아직 구현을 제공하지 않는 경우라면, NotImplementedError 를 발생시키는 것이 적합합니다.

잘못된 type의 인자를 전달하면 (가령 int 를 기대하는데 list를 전달하기), TypeError 를 일으켜야 합니다. 하지만 잘못된 값을 갖는 인자를 전달하면 (가령 범위를 넘어서는 숫자) ValueError 를 일으켜야 합니다.

## 7. ValueError

연산이나 함수가 올바른 형이지만 부적절한 값을 가진 인자를 받았고, 상황이 IndexError 처럼 더 구체적인 예외로 설명되지 않는 경우 발생합니다.
