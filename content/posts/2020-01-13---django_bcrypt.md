---
title: Bcrypt
date: "2020-01-13T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/django/bcrypt"
category: "Django/Bcrypt"
tags:
  - "Django, Bcrypt"

description: "Bcrypt encryption of security information"
socialImage: "/media/gutenberg.jpg"
---

# Bcrypt란?

Blowfish를 기반으로 만들어진 "단방향 암호화 해싱함수"로 1999년 USENIX에서 발표됐다. Rainbow table 공격을 막기 위해서 salt를 사용하며, 암호검사 요청이 반복될 수록 cost를 늘림으로써, 무차별 대입 공격(brute-force search)을 막을 수 있다. cost는 반복횟수로 2^n 이다.

- Rainbow table attack이란?
  미리 해쉬값들을 계산해 놓은 테이블을 Rainbow table이라고 한다. 이것으로 암호화된 해쉬값을 풀어내는 것

## Bcrypt 암호화 적용 구조

![](https://docs.google.com/drawings/d/e/2PACX-1vQNx1HRk87QQFoenXuRby493jI2Eg6gXIlTN-sxLFfurB-5FI5I5lutbrO-OPqWdnln2PBWXJv2X2Kl/pub?w=860&h=168)

- $ : bcrypt의 오리지날 버전은 $2a$ : bcrypt의 오리지날 버전은 $2$다. 오리지날 버전은 non-ASCII 문자나 Null 문자를 처리하는 방법을 정의하지 않았다. $2a\$는 널 문자를 포함하며, UTF-8로 인코딩 할 것을 정의한 버전이다. (encoding='utf-8')
- 10\$ : Cost의 크기는 2^10이다. Iteration count를 1024만큼 돌리겠다는 얘기다. (bcrypt.gensalt(round=10))
- N9qo8uLOickgx2ZMRZoMye : 랜덤하게 만든 salt
- ljZAgcFl7p92ldGxad68LJZdL17lhWy : 패스워드. Salt와 패스워드를 묶어서 해시해버렸기 때문에, 유추가 불가능하다.

![](http://d2.naver.com/content/images/2015/06/helloworld-318732-2.png)

# Django에서 사용하기

```
import bcrypt # 임포트한다(그 전에 라이브러리 설치는 필수)
```

## Main process(signup)

bytes(user_data, encoding='utf-8') → bcrypt.hashpw(data, bcrypt.gensalt()) -> .decode('utf-8')

- bytes(암호화할 패스워드, encoding='utf-8'한글포함한 대부분의 문자 포함)화 한다
- bcrypt.hashpw(암호화할 패스워드, 솔트 추가) - 암호화 함수
- bcrypt.gensalt() -보안강화를 위한 salt 추가!
- .decode('utf-8') - type을 'str'으로 변환

# Signin Process

password 값 도착 → bcrypt.hashpw(받은password, bcrypt.gensalt()) → DB password encoding→ checkpw(받은 패스워드 인코딩한 값, DB에 저장된 password 인코딩한 값) 비교

- 프론트에서 입력한 password를 요청 받음
- 받은 password 암호화
- DB password 불러와서 encoding한다. (type 맞춰주기 위해서)
- bcrypt.checkpw(받은 패스워드 인코딩한 값, DB에 저장된 password 인코딩한 값) 비교 함수 적용
- 일치하면 로그인 성공
- 일치하지 않으면 로그인 실패

## 추가로 쓰이는 인자

- bcrypt.gensalt(round=4) : round는 cost 인자로서 해쉬 함수를 반복할 횟수를 결정한다. 이 때 반복 횟수는 2^n이 된다.

## 예시

```
# bcrypt 임포트한다
import bcrypt
password = b"super secret password"
# 패스워드 해싱
hashed = bcrypt.hashpw(password, bcrypt.gensalt(14))
# DB 저장 패스워드 가져오기
user_data      = User.objects.get(email=data["email"])
user_password  = user_data.password.encode('utf-8')
# 프론트에서 받은 패스워드 hashing
byted_password = bcrypt.hashpw(data['password'], bcrypt.gensalt(14))
# 패스워드 체크
if bcrypt.checkpw(byted_password, user_password):
    print("It Matches!")
else:
    print("It Does not Match :(")
```
