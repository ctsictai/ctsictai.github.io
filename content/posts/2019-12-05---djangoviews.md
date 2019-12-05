---
title: Django URL & querystring
date: "2019-12-04T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/django-part9/"
category: "Python/Django/URL/Querystring/"
tags:
  - "Python/Django"

description: "django Querystrung URL"
socialImage: "/media/gutenberg.jpg"
---

# urls.py

웹 환경에서 url 주소를 설정하는 것.
프론트엔드와 접점이 일어나는 시작점

- 용어 정리
  - URI – Uniform Resource Indentifier – URL + URN(Uniform Resource Name)
  - URL – Uniform Resource Locator – 웹프로그래밍에선 URI = URL로 쓰임

1. url관리
   장고의 앱별로 별도 관리를 하고 최종 url은 프로젝트 디렉토리 내의 url에서 처리한다.

- 앱 별로 관리 함으로써 앱별로 url을 views.py와 연계되어 관리하기가 더 수월하다.

이 때 쓰이는 게 include()이다.

```
urlpatterns = [
    path('account/', include('account.urls')),
    path('statistics/', include('statistics.urls')),
]
```

include()를 씀으로 로직이 있는 앱에 있는 url에 실제 주소가 저장 되어 있는 것을 가져다가 쓸 수 있다.

### path()

- django 2.0에서 새롭게 추가된 함수입니다.
- URL경로 상에서 예를 들어 <username>과 같은 꺽쇠괄호가 들어 있는 URL을 인식하여 뷰 함수에 키워드 인자로 전달합니다.

- 인자 컨버터로 제한하기
  - 이 번에는 키워드 인자로 전달받는 항목의 타입을 지정할 수 있습니다.
  - <컨버터:전달할키워드인자명> 이러한 형태로 입력하면 입력받는 데이터를 제한 할 수 있습니다.
  - 컨버터의 종류와 역할은 다음과 같습니다.
    - str : 경로 구분자를 제외한 비어 있지 않은 문자열
    - path: 경로 구분자를 포함한 비어 있지 않은 문자열
    - int : 0 또는 임의의 양의 정수와 일치합니다.
    - slug : 문자 또는 숫자와 하이픈 및 밑줄 문자로 구성된 슬러그 문자열과 일치합니다. 예를 들어, POWER_PEARL_OP
    - uuid : 형식화 된 UUID를 제공합니다. 여러 URL이 동일한 페이지에 매핑되지 않도록 랜덤 숫자&문자를 제공한다.  
      예를 들어 075194d3-6885-417e-a8a8-6c931e272f00과 같습니다. UUID 인스턴스를 반환합니다.

```
urlpatterns = [
    path('<name>/<int:userId>', SigninView.as_view()),
    path('<slug:modify_detail>/<int:userId>/<int:year>/<int:month>', Profile.as_view())
]
```

# Querystring

- 하나의 path(라우터) 에서 경우에 따라 다른 결과를 보여주기 위해서는 쿼리스트링이 사용된다.
- 쿼리스트링은 어떤 애플리케이션에게 정보를 전달할 때 사용되는 URL에 약속되어 있는 국제적인 표준

![](https://wayhome25.github.io/assets/post-img/nodejs/querystring.png)

- url에서 ? 다음 부분
- 변수=값&변수=값&변수=값 ... 의 형식

> 예시

- http://example.com/over/there?name=ferret --> name=ferret
- http://en.wikipedia.org/w/index.php?title=Main_page&action=raw --> title=Main_page&action=raw
- http://example.com/rising/?page=123 --> pagination 123개 표현
