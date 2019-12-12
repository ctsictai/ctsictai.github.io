---
title: OAuth, Social Signin
date: "2019-12-12T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/django/socialsignin"
category: "Python/Django/OAuth"
tags:
  - "Python/Django/OAuth/SocialSignin"

description: "Auth/Social_login"
socialImage: "/media/gutenberg.jpg"
---

# OAuth2.0

주요 특징 및 개선점

1. 간단해 졌다.  
   OAuth 1.0a에서는 https 가 필수가 아니었기 때문에 API를 호출할 때 signature를 생성해서 호출해야 했다. 때문에 OAuth 1.0a API를 테스트 하려면 curl등을 사용하기 힘들고 별도의 API 콘솔등을 사용해서 테스트 해야 했다. OAuth 2.0의 Bearer 토큰 인증 방식을 쓰면 더 이상 signature 가 필요 없기 때문에 API를 테스트하거나 예제를 만들 때 간단하게 curl등 직관적인 방법을 사용해서 문서화하고 개발 할 수 있게 되었다.

2. 더 많은 인증 방법을 지원  
   OAuth 1.0a는 한가지 인증 방식을 제공한다. HMAC을 이용한 암호화 인증 방식이다. 하지만 OAuth 2.0은 시나리오별로 여러가지 인증 방식을 제공하기 때문에 웹브라우저, 모바일 등의 다양한 시나리오에 대응할 수 있게 해준다.

3. 대형 서비스로의 확장성 지원  
   커다란 서비스를 만들기 위해서는 인증 서버를 분리할 수 있어야 하고 또한 인증 서버를 다중화 할 수 있어야 한다. OAuth 2.0에서는 실제 API 서비스를 하는 서버와 인증 역할을 하는 authorization server의 역할을 명확히 구분함으로서 인증서버의 분리와 다중화 등에 대한 고려가 되어있다.

## 다양한 인증방식

### 1. Authorization Code Grant

웹 서버에서 API를 호출하는 등의 시나리오에서 Confidential Client가 사용하는 방식이다. 서버사이드 코드가 필요한 인증 방식이며 인증 과정에서 client_secret 이 필요하다.
로그인시에 페이지 URL에 response_type=code 라고 넘긴다.

### 2. Implicit Grant

token과 scope에 대한 스펙 등은 다르지만 OAuth 1.0a과 가장 비슷한 인증방식이다. Public Client인 브라우저 기반의 어플리케이션(Javascript application)이나 모바일 어플리케이션에서 이 방식을 사용하면 된다. Client 증명서를 사용할 필요가 없으며 실제로 OAuth 2.0에서 가장 많이 사용되는 방식이다.
로그인시에 페이지 URL에 response_type=token 라고 넘긴다.

## 다양한 토큰 지원

OAuth 2.0은 기본적으로 Bearer 토큰, 즉 암호화하지 않은 그냥 토큰을 주고받는 것으로 인증을 한다. 기본적으로 HTTPS 를 사용하기 때문에 토큰을 안전하게 주고받는 것은 HTTPS의 암호화에 의존한다. 또한 복잡한 signature 등을 생성할 필요가 없기 때문에 curl이 API를 호출 할 때 간단하게 Header 에 아래와 같이 한 줄을 같이 보내므로서 API를 테스트해볼 수 있다.
Authorization: Bearer

## Refresh token

클라이언트가 같은 access token을 오래 사용하면 결국은 해킹에 노출될 위험이 높아진다. 그래서 OAuth 2.0에서는 refresh token 이라는 개념을 도입했다. 즉, 인증 토큰(access token)의 만료기간을 가능한 짧게 하고 만료가 되면 refresh token으로 access token을 새로 갱신하는 방법이다. 토큰의 상태를 관리해야 해서 개발이 복잡해 지는 단점이 있다.

## API 권한 제어 (scope)

OAuth 2.0은 써드파티 어플리케이션의 권한을 설정하기 위한 기능이다. scope의 이름이 스펙에 정의되어있지는 않으며 여러 개의 권한을 요청할 때에는 콤마등을 사용해서 로그인 시에 scope를 넘겨주게 된다.

# 소셜로그인

유저가 별도의 회원가입 없이 유저가 이용하고 있는 소셜 웹사이트(kakao, google 등)의 로그인정보를 사용하여 웹사이트에 로그인계정을 얻게되어 회원으로 접근할 수 있도록 하는 방법.

## 장점

1. 해외 소셜 로그인 업체를 이용하면 외국인들을 대상으로 하는 회원 서비스가 쉬워진다.
2. 서버에 개인정보를 최소한으로 저장하여 DB 저장 공간을 아낄 수 있다.
3. 많은 계정과 비밀번호를 일일히 기억할 필요 없이 소셜로그인으로 모두 통합되어 관리가 쉽다.

## 단점

1. 보안 해킹 이슈가 있다. 하나로 통합되어 가입/로그인 절차를 진행하므로 해킹에 취약하다
2. 서버입장에서는 소셜 로그인 회원들을 관리하기가 까다로워졌다.(개인정보 취득이나 회원의 판별여부)
3. 여러개의 소셜 로그인을 제공하는 경우 다중 소셜 로그인을 허용할 것인지 말 것인지 까다로운 문제에 봉착한다.

장점은 유저입장에서의 장점이 많고 단점은 서버단에서의 관리적 입장에서의 단점이 많다. 그래서 유저 입장에서 본 결과 대부분의 사이트에서 소셜 로그인 기능을 제공하고 있는 것이다.

# 소셜로그인 전체 프로세스 로직

![페이스북 로그인 로직](https://nachwon.github.io/img/facebook_login/flow.png)

# 백엔드 로직순서

1. 프론트엔드에서 유저의 소셜로그인에 필요한 access_token을 발급받아 백엔드 서버로 전달한다
2. 받은 access_token을 가지고 해당 소셜 플랫폼 api로 필요한 회원정보를 요청
3. 소셜 플랫폼에서 여러가지 개인 보호 정책등으로 걸러진 유저의 회원정보를 json 형식으로 받음
4. 받은 개인정보를 유저를 식별할 수 있는 정보(id값(소셜플랫폼에서 제공하는))와 기타 추가적인 정보값을 DB에 있는지 확인하고 없으면 저장
5. Signin 로직 구현 --> backend server에서 자체적으로 발급하는 토큰을 전달하면된다.(DB에 저장하던 말던 상관없이 위의 절차가 완료되면 백서버에서 발급하는 토큰을 전달한다.)

## 소셜로그인을 쉽게 관리하기 위한 Tip

1. 소셜 플랫폼회사코드를 따로 테이블을 만들어서 관리한다.

- 이 때 유저테이블과의 관계는 1:N으로 소셜플랫폼하나에 여러 유저가 있을 수 있고 유저는 하나의 플랫폼을 가지므로!

```
class SocialPlatform(models.Model):
    platform_name = models.CharField(max_length=20, default=0)

    class Meta:
        db_table = "social_platform"

class User(models.Model):
    ....
    social          = models.ForeignKey(SocialPlatform, on_delete=models.CASCADE, max_length=20, blank=True, default=1)
    social_login_id = models.CharField(max_length=50, blank=True)
```

- 소셜로그인을 회원을 구분 지을 수 있는 것은 소셜플랫폼 회사에서 제공하는 'id'이다.
- 'id'가 예를들어 facebook id와 google id에서 겹칠 수 있는 문제가 발생한다. 그래서 소셜 플랫폼 제공하는 회사 코드도 만들어서 회사코드와 id로 소셜로그인 유저를 구분할 수 있게 된다.
- 소셜로그인의 가장 큰 핵심은 소셜로그인한 회원을 서비스 페이지에서 어떻게 구별할 것인가의 문제이다
- 실제 예제로 카카오를 예로 들어 설명한다.(나머지 소셜로그인도 비슷하게 진행된다.)

### 실제 소셜 로그인 예제(카카오)

```
class KakaoSigninView(View):
   def post(self, request):
       kakao_token  = request.headers["Authorization"]
       # request의 header에 bearer(암호화 되지 않는 토큰)형식으로 토큰을 프론트에서 받는다.
       if not kakao_token:
           return JsonResponse({"MESSAGE" : "INVALID_KAKAO_TOKEN"}, status=400)

       headers      = ({'Authorization' : f"Bearer {kakao_token}"})
       url          = "https://kapi.kakao.com/v1/user/me"
       response     = requests.post(url, headers=headers, timeout=2) # header에서 프론트에서 받는 토큰과 정보를 요청할 api url주소를 통해 requests.post로 요청함

       user_data    = response.json()
       try:
           if User.objects.filter(social_login_id=user_data['id']).exists(): # 소셜id는 소셜플랫폼회사에서 회원을 식별하는 주식별자이다. 이게 있다면 이 소셜플랫폼으로 회원가입하고 로그인을 적어도 1번 이상 했다는 얘기 --> 로그인 로직 구현
               user = User.objects.get(social_login_id=user_data['id'])
               payload = {
                   "user_id"       : user.id,
                   "kakao_id"      : user.social_login_id,
                   "user_is_maker" : user.is_maker,
                   "exp"           : WEDIZ_SECRET['exp_time']
                   }
               jwt_encode = jwt.encode(payload, WEDIZ_SECRET['secret'], algorithm="HS256") # 카카오에서 받은 토큰으로 사용자를 인증하고 홈페이지에서 사용할 엑세스토큰을 자체 발급한다.
               return JsonResponse({"VALID_TOKEN" : jwt_encode.decode('utf-8')} , status=200)
           else: # 한 번도 이 소셜플랫폼으로 로그인 한적이 없는 경우 회원가입 로직을 구현한 후 곧바로 로그인 까지 구현함
               signup_user = User.objects.create(
                   email     = user_data['kakao_account']['email'],
                   social    = SocialPlatform.objects.get(id=2).id,#카카오 id가 2
                   social_login_id = user_data['id']
                   )
               payload = {
                   "user_id"       : signup_user.id,
                   "kakao_id"      : signup_user.social_login_id,
                   "user_is_maker" : signup_user.is_maker,
                   "exp"           : WEDIZ_SECRET['exp_time']
                   } # 로그인 로직
               jwt_encode = jwt.encode(payload, WEDIZ_SECRET['secret'], algorithm="HS256")
               return JsonResponse({"VALID_TOKEN" : jwt_encode.decode('utf-8')}, status=200)
       except ValueError:
           return JsonResponse({"MESSAGE" : "INVALID_EMAIL"}, status=401)
```

- request의 header에 bearer(암호화 되지 않는 토큰)형식으로 토큰을 프론트에서 받는다.
- header에 'authorization' 키에 value를 프론트에 받는 토큰을 실어서 회원정보를 요청할 api url주소를 통해서 requests.post방식으로 카카오 api 서버에 요청함
- 소셜id는 소셜플랫폼회사에서 회원을 식별하는 주식별자이다. 이게 있다면 이 소셜플랫폼으로 회원가입하고 로그인을 적어도 1번 이상 했다는 얘기 --> 로그인 로직 구현
- 카카오에서 받은 토큰으로 사용자를 인증하고 홈페이지에서 사용할 엑세스토큰을 자체 발급한다.

> 카카오 response key / value

```
{
  "id":123456789,
  "properties":{
     "nickname":"홍길동카톡",
     "thumbnail_image":"http://xxx.kakao.co.kr/.../aaa.jpg",
     "profile_image":"http://xxx.kakao.co.kr/.../bbb.jpg",
     "custom_field1":"23",
     "custom_field2":"여"
     ...
  },
  "kakao_account": {
    "profile_needs_agreement": false,
    "profile": {
      "nickname": "홍길동",
      "thumbnail_image_url": "http://yyy.kakao.com/.../img_110x110.jpg",
      "profile_image_url": "http://yyy.kakao.com/dn/.../img_640x640.jpg"
    },
    "email_needs_agreement":false,
    "is_email_valid": true,
    "is_email_verified": true,
    "email": "xxxxxxx@xxxxx.com"
    "age_range_needs_agreement":false,
    "age_range":"20~29",
    "birthday_needs_agreement":false,
    "birthday":"1130",
    "gender_needs_agreement":false,
    "gender":"female"
  }
}
```

- "is_email_valid": true - 카카오의 이메일이 유효한것인지 알려주는 값(유효하지 않을 수도 있다.) --> 이걸 담보하기 위해서 이메일 검증 방법을 쓰기도 한다. 그건 다음에 살펴보도록 한다.
- "is_email_verified": true - 카카오에서 인증된 이메일을 판별하는 인자
- email
  - 사용자 카카오계정의 이메일
  - 카카오계정 이메일은 변경될 수 있습니다.
  - 이메일 개인정보 제공동의를 하지 않은 사용자의 이메일은 제공되지 않습니다. email_needs_agreement=true인데 email값이 내려오지 않는다면, 사용자의 정보 제공 동의가 필요한 경우입니다.

![카카오 로그인 이메일 인증](https://wedizstoryimage.s3.ap-northeast-2.amazonaws.com/kakao_email.png)

- 나머지 필요한 정보가 있으면 response 값을 잘 보고 해당 키값을 통해 가져오면 된다.
