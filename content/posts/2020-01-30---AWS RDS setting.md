---
title: AWS RDS Setting
date: "2020-01-30123:30:03.284Z"
template: "post"
draft: false
slug: "/posts/AWS/RDS"
category: "RDS"
tags:
  - "RDS"

description: "AWS RDS settings"
socialImage: "/media/image-1.jpg"
---

# RDS란?

AWS RDS는 인프라 및 데이터베이스 업데이트를 관리해주는 것 뿐만 아니라 까다로운 관계형 데이터베이스의 설치, 운영 그리고 관리를 지원하는 서비스입니다.

Amazon RDS는 현재 MySQL, Oracle, SQL Server, PostgreSQL, MariaDB, Aurora(MySQL과 호환)을 비롯한 총 6가지 데이터베이스 엔진을 지원하고 있습니다

# RDS in MySQL 인스턴스 생성

1. 먼저 MySQL 설정 파일을 만들어야 한다. 직접 안만들어도 default 설정 파일이 있지만 utf-8 인코딩 설정을 해줘야 한국어가 저장 가능하다. 먼저 "Parameters groups" 페이지로 가서 "Create parameter group" 옵션을 선택하자.

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/create+parameter+group.png)

2. "Parameter group" 설정 파일을 생성하자. Group name과 Description은 자유롭게 정하면 된다. "Paramter group family"는 생성하는 데이터 베이스와 버전에 맞게 지정해야 한다. 우리의 경우 mysql5.7 데이터베이스를 생성할 예정이니 "mysql5.7"로 하자.

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/create+parameter+group+setting+page.png)

3. 방금 생성한 설정 파일을 수정 하자.

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/create+parameter+group+setting+edit.png)

4. 내가 설정할 parameters는 다음과 같다

- chracter_set_client --> utf8mb4로 변경
- chracter_set_connection --> utf8mb4로 변경
- chracter_set_database --> utf8mb4로 변경
- chracter_set_results --> utf8mb4로 변경
- chracter_set_server --> utf8mb4로 변경
- collation_connection --> utf8mb4_general_ci로 변경
- collation_server --> utf8mb4_unicode_ci로 변경

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/create+parameter+group+setting+editing+ex.png)

위의 파라미터를 다 설정 하였으면 "Preview changes"를 눌러서 수정사항 리뷰를 하자. 아래와 같이 나와야 한다.

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/create+parameter+group+setting+edit+final+state.png)

모든게 정확하면 "Save Changes"를 클릭

5. "Create database" 클릭 후 MySQL을엔진으로 선택하자.

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/RDS+1-2.DB%EC%9C%A0%ED%98%95+%EC%84%A0%ED%83%9D.png)

6. 데이터 생성방식 선택하자 - 표준 생성으로 선택하고 넘어가면 된다.
   ![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/RDS+1-1.DB+%EC%83%9D%EC%84%B1%EB%B0%A9%EC%8B%9D+%EC%84%A0%ED%83%9D+.png)

7. 과금방식 선택
   ![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/RDS+2.%EA%B3%BC%EA%B8%88%EC%84%A0%ED%83%9D.png)

8) DB 세부사항 설정을 하자. 거의 대부분 default 값 그대로 두면 된다. 맨 아래 settings 섹션에서 master username과 비밀번호 그리고 데이테베이스 이름만 설정하면 된다.

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/RDS+3.%EC%8B%9D%EB%B3%84%EC%9E%90+%EC%9E%90%EA%B2%A9%EC%A6%9D%EB%AA%85.png)

9. 인스턴스 클래스 및 스토리지 유형 & 용량 설정  
   (1) 인스턴스 클래스 설정
   ![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/RDS+4.%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4+%ED%81%AC%EA%B8%B0.png)
   (2) 스토리지 유형 및 용량 설정
   ![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/RDS+5.%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80+%EC%9C%A0%ED%98%95%EB%B0%8F+%EC%9A%A9%EB%9F%89.png)

10. VPC 설정

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/RDS+6.VPC.png)

11. VPC 추가 연결 설정
    ![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/RDS+7.%EC%B6%94%EA%B0%80%EC%97%B0%EA%B2%B0%EC%84%A4%EC%A0%95.png)

그리고 Database options 세션에 "DB parameter group"을 방금 생성한 파라미터 설정 파일로 변경해주자. 나머지는 default 값 그대로 하면 된다. 끝나면 "Create database" 버튼을 눌루면 마무리 된다.

![](<https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/DB+options+parameter+group+(2).png>)

12. 이제 "Instances" 페이지에 가서 방금 생성한 데이터 베이스 대쉬보드 페이지로 가자. 참고로 데이터 베이스가 사용 준비되기 까지 몇분 이상 소요 될 수 있다.

13. Security group 설정을 변경하여 어디서든 접속이 가능하게 하자. 원래는 데이터베이스를 이렇게 어디서든 접속 가능하게 열어놓으면 안된다. 해킹의 위험이 있다.

![](https://wedizfundmakerimage.s3.ap-northeast-2.amazonaws.com/security+group.png)

14. 생성된 데이터베이스에 접속한후 데이터베이스와 테이블들을 생성하자.

```
mysql -h database endpoint 주소 입력 -u root -p
```

로그인 해서 local db 생성하듯이 하면 된다.
