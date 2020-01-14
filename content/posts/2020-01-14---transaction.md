---
title: transaction
date: "2020-01-14T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/django/transaction"
category: "Transaction"
tags:
  - "Transaction"

description: "Transaction"
socialImage: "/media/gutenberg.jpg"
---

# 트랜잭션이란?

트랜잭션(Transaction)은 데이터베이스의 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위 또는 한꺼번에 모두 수행되어야 할 일련의 연산들을 의미한다.

트랜잭션의 특징

1. 트랜잭션은 데이터베이스 시스템에서 병행 제어 및 회복 작업 시 처리되는 작업의 논리적 단위이다.

2. 사용자가 시스템에 대한 서비스 요구 시 시스템이 응답하기 위한 상태 변환 과정의 작업단위이다.

3. 하나의 트랜잭션은 Commit되거나 Rollback된다.

## ACID(Atomicity, Consistency, Isolation, Durability)

트랜잭션에 있어서 중요한 4가지 특징이다. 꼭 알아둬야 되는 개념으로 한 번 정리하고 넘어간다.

1. **원자성(Atomicity)**은 트랜잭션과 관련된 작업들이 부분적으로 실행되다가 중단되지 않는 것을 보장하는 능력이다.  
   예를 들어, 자금 이체는 성공할 수도 실패할 수도 있지만 보내는 쪽에서 돈을 빼 오는 작업만 성공하고 받는 쪽에 돈을 넣는 작업을 실패해서는 안된다. 원자성은 이와 같이 중간 단계까지 실행되고 실패하는 일이 없도록 하는 것이다.
2. **일관성(Consistency)**은 트랜잭션이 실행을 성공적으로 완료하면 언제나 일관성 있는 데이터베이스 상태로 유지하는 것을 의미한다.  
   무결성 제약이 모든 계좌는 잔고가 있어야 한다면 이를 위반하는 트랜잭션은 중단된다.
3. **고립성(Isolation)**은 트랜잭션을 수행 시 다른 트랜잭션의 연산 작업이 끼어들지 못하도록 보장하는 것을 의미한다.  
   이것은 트랜잭션 밖에 있는 어떤 연산도 중간 단계의 데이터를 볼 수 없음을 의미한다. 은행 관리자는 이체 작업을 하는 도중에 쿼리를 실행하더라도 특정 계좌간 이체하는 양 쪽을 볼 수 없다.  
   공식적으로 고립성은 트랜잭션 실행내역은 연속적이어야 함을 의미한다. 성능관련 이유로 인해 이 특성은 가장 유연성 있는 제약 조건이다.
4. **지속성(Durability)**은 성공적으로 수행된 트랜잭션은 영원히 반영되어야 함을 의미한다.  
   시스템 문제, DB 일관성 체크 등을 하더라도 유지되어야 함을 의미한다. 전형적으로 모든 트랜잭션은 로그로 남고 시스템 장애 발생 전 상태로 되돌릴 수 있다.  
   트랜잭션은 로그에 모든 것이 저장된 후에만 commit 상태로 간주될 수 있다.

# 트랜잭션 연산 및 상태

### Commit연산

- COMMIT은 변경한 데이터를 데이터베이스에 마지막으로 반영하고 현재의 트랜잭션을 종료하라는 명령어

- COMMIT을 하면 트랜잭션의 처리과정이 모두 반영되며 하나의 트랜잭션 과정이 끝남

- COMMIT하기 전 데이터가 저장됨

- DDL(CREATE, DROP, ALTER, RENAME, TRUNCATE)은 AutoCommit임

- 정상적인 종료시에도 COMMIT 작업을 수행함 (EXIT로 종료)

### Rollback연산

- ROLLBACK은 그 반대로 변경한 데이터를 변경(DML)을 취소하고 현재의 트랜잭션을 끝내라는 명령어

- 트랜잭션으로 인한 하나의 묶음처리가 시작되기 이전의 상태로 되돌려지는 것

- 이전 COMMIT한 곳까지만 복구가 됨

- 이전의 상태로 돌아가니 지금까지 수행했던 데이터베이스의 변경을 모두 무효화시킴

- 비정상적인 종료시 자동으로 ROLLBACK 작업을 수행함 (우측 상단 X버튼 클릭시, 정전, 컴퓨터가 Down시)

### 트랜잭션 상태도

![](https://t1.daumcdn.net/cfile/tistory/997656365AE1FCA40B)

# Transaction in Django

## 데코레이터(decorator)를 이용한 python + django 트랜잭션

django 에서 트랜잭션을 이용하는 가장 쉬운 방법으로 데코레이터를 이용하는 방법입니다. 데코레이터를 이용하게 되면, 메서드 안에는 코드를 삽입할 필요가 없습니다.  
**“@transaction.atomic”** 이라는 데코레이터를 붙여주기만 하면 끝입니다. django 에서 기본적으로 제공해주는 데코레이터이므로, 따로 모듈을 설치해줄 필요도 없습니다.  
가장 간단하게 atomic(원자성)한 트랜잭션을 처리하기 위한 손쉬운 방법이죠

```
from django.db import transaction  # 장고에서 제공하는 transaction

@transaction.atomic                 # transaction decorator
def transaction_test1(arg1, arg2):
    # start transaction
    a.save()

    b.save()
    # end transaction
```

## with 명령어를 이용한 트랜잭션

메서드 전체가 아닌 메서드의 일부분만 트랜잭션으로 묶어줄 필요가 있을 때 사용합니다. 트랜잭션으로 묶일 부분을 직접 지정해줘야 하는 불편함(?)이 있지만, 데코레이터와 마찬가지로 비교적 간단하게 처리가 가능합니다.

```
from django.db import transaction

def transaction_test2(arg1, arg2):

    a.save()    # 항상 save 처리됨, 예외가 발생할 경우 에러 발생

    with transaction.atomic(): # 메서드에 일부분만 트랜잭션 처리
        # start transaction
        b.save()

        c.save()
        # end transaction
```

## savepoint 를 직접 지정해 주는 트랜잭션

위의 2가지 방법의 경우, 메서드 내에서(트랜잭션으로 묶여져있는) exception 이 발생하더라도 저절로 롤백이 되기 때문에 예외처리를 따로 해 줄 필요는 없습니다.  
하지만, 3 번의 경우에는 savepoint 및 cummit 지점을 직접 지정해 주기 때문에 **예외처리** 또한 별도로 처리되어야 합니다.

```
from django.db import transaction

def transaction_test3(arg1, arg2):

    a.save()

    sid = transaction.savepoint()
    # start transaction
    try:

        b.save()

        c.save()

        transaction.savepoint_commit(sid)
        # end transaction
    except Exception
        # 트랜잭션 내에서 에러 발생시 롤백처리
        transaction.savepoint_rollback(sid)
```

## DB 설정에서 트랜잭션 처리를 자동으로 설정하는 방법

django의 기본 프로젝트 디렉토리에 settings.py에 있는 DATABASE 설정에서 주요 문구를 추가하면된다.

```
# settings/base.py

DATABASE = {
  'default': {
    #...생략
    'ATOMIC_REQUESTS': True,
  }
}
```

- 'ATOMIC_REQUESTS': False, 라면 디폴트값이 트랜잭션 처리를 하지 않는다. 그래서 데이터베이스 변화가 있는 API를 설계할 때 일일히 지정해줘야 한다.
