---
title: Django View
date: "2019-12-01T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/django-part7/"
category: "Python/Django/View/CBV"
tags:
  - "Python/Django"

description: "django controller views.py description"
socialImage: "/media/gutenberg.jpg"
---

# 제너릭 뷰를 이용한 view point 개발 – CBV(Class Based View)

> 클래스형 뷰는 상속과 믹스인 기능을 이용하여 코드 재사용하고 뷰를 체계적으로 구성할 수 있다.

- 장점

1. GET, POST등의 HTTP 메소드에 따른 처리 기능을 코딩할 때 if함수를
   사용하지 않고 메소드명으로 구분할 수 있으므로 코드의 구조가 깔끔해짐.
2. 다중 상속과 같은 객체 지향 기술이 가능하므로, 클래스형 제너릭 뷰 및 믹스인 클래스 등을 사용할 수 있고, 이는 코드 재사용성(오버라이딩)이나 개발 생산성을 획기적으로 높여줌

- As_view() 진입 메소드  
  Urlconf(URLconfiguration) -장고에서 URL과 일치하는 뷰를 찾기 위한 패턴들의 집합 – urls하고 그걸 구현할 로직인 views하고 매개하는 것  
  클래스 인스턴스를 생성하고, 그 인스턴스의 dispatch()메소드 호출 -&gt; dispatch 메소드는 요청을 검사해서 GET, POST등의 어떤 HTTP메소드로 요청 되었는지 알아내고, 인스턴스 내에서 해당 이름을 갖는 메소드로 요청을 중계함 (http header method 판별하는 메서드 중요하다!!) 해당 메서드가 정의되어 있지 않으면, HttpResponseNotAllowed 예외를 발생

# FBV(Function Based View)

뷰를 클래스가 아닌 함수 단위에서 정의하는 것을 뜻한다.
CBV에서 가장 큰 차이는 As_view()가 없기 때문에 직접 request.method를 통해서 요청을 처리해야 한다.

# JsonResponse

응답형식을 json data type으로 한다.
Jsonresponse는 import가 필요하다

```
from django.http import JsonResponse
```

```
return JsonResponse({“wdw” : search_user_data}, safe=False, status=200)
```

- default content-type : application/json
- param data : data는 json type이여야 한다.(python의 dict type) - 실제로 response로 보낸다.
- param safe : dict type object가 serialized가 되는 조건을  
  default 값은 True 세팅 = non-dict면 typeerror return
- status = http status상태 코드 지정 하는것

모델 만들고 views 만들고 urls 연결해주고 jsonresponse를 통해 무언가를 응답 리턴값을 주면 그걸 곧바로 서버에 띄워 볼 수 있다.

# HTTPIE (서버테스트 할 수 있는 프로그램)

- Intergration test를 할 수 있는 프로그램.
- 인스톨
  `python get-pip.py`

http [flags][method] URL [ITEM [ITEM]]  
    •   flags : 실행시 전달할 옵션으로 – 로 시작(Ex: --json)  
    •   METHOD : HTTP 메소드로 생략시 GET.  
    •   URL: 연결할 url  
http --help 를 실행하면 각 플래그별 상세한 설명을 볼 수 있음.

# 실제 CBV 예시

회원가입하는 뷰 로직이다.

```
class SignupView(View):
	def post(self, request):
		user_data =json.loads(request.body)

		try:
			validate_email(user_data["email"])
			if User.objects.filter(email=user_data["email"]).exists():
				return JsonResponse({"MESSAGE" : "THIS_IS_EMAIL_ALREADY_EXIST"}, status=400)
			else:
				byted_password  = bytes(user_data["password"], encoding='utf-8')
				hashed_password = bcrypt.hashpw(byted_password, bcrypt.gensalt())
				decode_password = hashed_password.decode('utf-8')
				user = User.objects.create(
						email        = user_data["email"],
						user_name    = user_data["user_name"],
						password     = decode_password,
						is_agree     = user_data["is_agree"],
						promotion    = user_data["promotion"],
                        phone_number = user_data['phone_number'],
						is_maker     = False
						)
				default_interest = ProfileInterest.objects.create(
					education_kids       = False,
					fashion_beauty_goods = False,
					home_design_item     = False,
					concert_culture      = False,
					sport_mobility       = False,
					publishing           = False,
					animal               = False,
					tech_home_appliance  = False
				)
				UserGetInterest.objects.create(user=user, profile_interest = default_interest)
				return JsonResponse({"MESSAGE" : "SIGNUP_SUCCESS"}, status=200)

		except ValidationError:
			return JsonResponse({"MESSAGE" : "NOT_EMAIL_FORM"}, status =400)
		except KeyError:
			return JsonResponse({"MESSAGE" : "INVALID_PUT"}, status=400)
```

- try - exception으로 예외 처리로 에러 처리에 대한 로직을 구현함
- bcrypt라는 암호화 알고리즘을 사용해 password를 보호하기 위한 보안 장치를 사용하였다.
- DB에 필요한 데이터를 받아 저장하였다.
- 다 대 다 관계로 묶인 테이블의 default data 지정하고 Join table 생성한다.
