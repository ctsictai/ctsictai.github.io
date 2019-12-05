---
title: SqlAlchemy
date: "2019-12-05T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/sqlalchemy/"
category: "Python/Sqlalchemy/"
tags:
  - "Python/Sqlalchemy"

description: "Sqlalchemy"
socialImage: "/media/gutenberg.jpg"
---

# SqlAlchemy의 철학

SQL 데이터베이스는 크기와 성능이 중요해질수록 개체 컬렉션과 유사하게 동작합니다. 객체 컬렉션은 테이블과 행처럼 동작하지 않으므로 더 추상화가 중요합니다. SQLAlchemy는 이러한 두 가지 원칙을 모두 수용하려고합니다.

SQLAlchemy는 데이터베이스를 테이블 집합이 아닌 관계형 대수 엔진으로 간주합니다. 행은 테이블뿐만 아니라 조인 및 다른 select 문에서도 선택할 수 있습니다. 이들 유닛 중 어느 것이 더 큰 구조로 구성 될 수있다. SQLAlchemy의 표현식 언어는이 개념을 핵심으로합니다.

SQLAlchemy는 데이터 매퍼 패턴을 제공하는 선택적 구성 요소 인 ORM (Object-Relational Mapper)으로 가장 유명합니다. 여기서 클래스는 개방형 다중 방식으로 데이터베이스에 매핑 될 수 있으므로 개체 모델 및 데이터베이스 스키마를 처음부터 깨끗하게 분리 된 방식.

이러한 문제에 대한 SQLAlchemy의 전반적인 접근법은 소위 칭찬 지향적 접근법에 기반한 다른 대부분의 SQL / ORM 도구와 완전히 다릅니다. 자동화 벽 뒤에 SQL 및 객체 관계형 세부 정보를 숨기는 대신 일련의 구성 가능하고 투명한 도구 내에서 모든 프로세스가 완전히 노출됩니다. 라이브러리는 중복 작업을 자동화하는 작업을 수행하는 반면 개발자는 데이터베이스 구성 방법 및 SQL 작성 방법을 제어합니다.

[SQLALCHEMY 공홈 참조](https://www.sqlalchemy.org/)

# 1. 설치

<code> pip install sqlalchemy</code>

# 2. DB 연결

DB에 연결을 하기 위한 추가 프로그램 설치가 필요하다

## 2-1. DBAPI 설치

DBAPI는 동일한 API를 사용하여 다양한 데이터베이스에서 작동하도록 권장하는 표준입니다.

MySQL : PyMySQL, MySQL-Connector, CyMySQL, MySQL-Python (default)

<code> pip install pymysql</code>

## 2-2. Create engine

> ### databases.py - settings file

```
from sqlalchemy import create_engine
engine = create_engine('mysql+pymysql://username:password@localhost/db명')
```

엔진을 통해 mysql db 서버와 연결하는 기초 공사를 하는 중이다.
(아직 연결 안됨)

## 2-3. 진짜 연결

```
from sqlalchemy import create_engine
engine = create_engine('mysql+pymysql://username:password@localhost/db명')
engine.connect()

print(engine)
```

> `Engine('mysql+pymysql://username:password@localhost/db명')`
> 이 화면이 뜨면 정상 연결이 되었다.

# 3. 매핑 선언

ORM에서는 처음에 데이터베이스 테이블을 생성하고 사용할 수 있도록 설정한 뒤 다음 직접 정의한 클래스에 맵핑을 해야한다.(안 그러면 ORM 명령어가 DB단에 들어가지 않는다)

sqlalchemy에서는 두가지가 동시에 이뤄지는데 Declarative 란걸 이용해 클래스를 생성하고 실제 디비 테이블에 연결을 한다.

```
from sqlalchemy.ext.declarative import declarative_base
Base = declarative_base()
```

# 4. 실제 모델 테이블 형성

> ### models.py에서

```
from sqlalchemy import Column, Integer, String

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)
    fullname = Column(String)
    password = Column(String)

    def __init__(self, name, fullname, password):
        self.name = name
        self.fullname = fullname
        self.password = password

    def __repr__(self):
        return "<User('%s', '%s', '%s')>" % (self.name, self.fullname, self.password)
```

- DB의 필드를 만들려면 직접 컬럼 클래스를 import 해야 한다.
- 필드의 타입을 지정하려면 타입에 해당하는 클래스들을 import해야 한다.
  (ex) 숫자 타입 이라면 Integer | 문자타입이라면 String etc on..

- 위 User 클래스는 **tablename**에서 정의한 테이블에 네임에 맵핑된다.
- primary key인 id와 name, fullname, password 컬럼을 가진다.
- **repr**은 해당 클래스에 대해 string 값으로 대표되는 값을 표현해준다.

> Declarative system으로 만들어진 이 클래스는 table metadata를 가지게 되는데 이게 사용자정의 클래스와 테이블을 연결해주는 구실을 한다.

# 5. 세션 이용

ORM은 데이터베이스를 session을 이용해 다룰 수 있는데 처음 앱을 작성할 때 create_engine()과 같은 레벨에서 Session 클래스를 factory 패턴으로 생성할 수 있다.

```
Base.metadata.create_all(engine)
Session = sessionmaker()
Session.configure(bind=engine)
session = Session()
```

1행에서 engine을 통해 메타데이터에 접근함
2행에서 다른 트랜잭션을 위한 것들은 sessionmaker()에서 호출될 때 정의하는 것(자세한 사항은 추후 업뎃)
3행에서 세션에 엔진을 묶어 엔진 실행시 세션클래스에서 새 객체를 만들게 한다
4행에서 Session 클래스 편하게 쓰기 위해 인스턴스화
