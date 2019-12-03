---
title: Django View part2
date: "2019-12-03T23:30:03.284Z"
template: "post"
draft: false
slug: "/posts/django-part8/"
category: "Python/Django/View/QuerySet"
tags:
  - "Python/Django"

description: "django QuerySet CRUD View logic"
socialImage: "/media/gutenberg.jpg"
---

# QuerySet이란?

- SQL을 생성해주는 인터페이스이다.
- Django ORM에 의해 queryset을 통해 별도로 SQL문을 작성할 필요없이 DB로 부터 데이터를 가져오고 추가, 수정, 삭제가 가능하다
- LAZY한 특성을 가지고 있다. 미리 db에 접근해서 값을 불러오는 게 아니라 출력 등과 같이 필요한 순간에 sql로 매핑되어서 db에 접근하는 방식이다.
- queryset으로 반환되는것은 values를 제외하고는 object로 반환된다
- 여기서 object란 db의 table의 row라고 보면 된다.
- row란 pk(보통 장고에서 자동부여한다) 1행에 있는 모든 데이터들이 다 반환 된다. 이게 object의 실체 dict type이 아니기 때문에 반환이 필요함
- 'key’(입력한 필드명으로) : value(comment.필드명- 실제 레코드값이 반환됨)

```
for item in items:
	context = { key1 : value1
                key2 : value2
  }
```

# CRUD

Create Read Update Delete의 약자로서 웹 서비스의 기본 기능이다. 가장 중요한 것은 create와 read이며 update는 create의 하위호환 delete는 왠만하면 안하는 것이 정석이다.

# READ(데이터 조회)

## models.objects.Filter()

## And 조건

```
queryset = 모델클래스명.objects.all()
queryset = queryset.filter(조건필드1=조건값1, 조건필드2=조건값2)
queryset = queryset.filter(조건필드3=조건값3)
```

```
for model_instance in queryset:
    print(model_instance)
```

- 화면에 출력할 때 DB에 쿼리 (lazy)
  필터는 queryset object가 list 형태로 다수가 나오므로 for문으로 하나씩 하나씩 list의 요소를 print하는 방식이다.

> filter And 조건 예시

```
queryset1 = User.objects.filter(phone_number__icontains='1', age__endswith='3')
queryset2 = User.objects.filter(phone_number__icontains='1').filter(age__endswith='3')
```

- icontains는 대문자 무시하고 포함하는 문자를 뜻한다.
  ![filter 조건](https://wedizprofile.s3.ap-northeast-2.amazonaws.com/filter.png)

## exclude(제외 조건)

```
User.objects.all().exclude(name__icontains='cts')
User.objects.filter(name__icontains='we').exclude(phone_number__endswith='3')
```

## Or 조건

1. 쿼리셋 쿼리셋 두개 or 조건으로 할수도 있음

```
모델클래스명.objects.all().filter(first__startwith=”E”) | 모델클래스명.objects.all().filter(last__endwith=”t”)
```

2. Complex lookups with Q objects : or 조건을 사용하기 위해서는 Q 객체 import가 필요하다.

```
from django.db.models import Q

모델클래스명.objects.all().filter(Q(조건필드1=조건값1) | Q(조건필드2=조건값2)) # or 조건
모델클래스명.objects.all().filter(Q(조건필드1=조건값1) & Q(조건필드2=조건값2)) # and 조건
```

- 모델 클래스의 오브젝트 갯수확인  
   <code>User.objects.all().count()</code>

> filter를 통한 검색 구현 예시

```
HTTP GET Method
Stat_champions.objects.filter(date__lt=date.today()).exists():
               stat_list = [{
                   "id"                : stat['id'],
                   "rank"              : stat['rank'],
                   "winRate"           : stat['win_rates'],
                   "playCount"         : stat['player_numbers'],
                   "averageScore"      : stat['kda'],
                   "csScore"           : stat['cs_average'],
                   "goldScore"         : stat['gold_average'],
                   "date"              : stat['date'],
                   "championImgSrc"    : [{
                       "champion_img_src" : champions_id['champion_img_src']
                       } for champions_id in Champions.objects.filter(id=stat['champions_id']).values()],
                   "championName"      : [{
                       "champion_name" : champions_id['champion_name']
                       } for champions_id in Champions.objects.filter(id=stat["champions_id"]).values()],
                   } for stat in Stat_champions.objects.filter(date=Stat_champions.objects.order_by('date').last().date).values()
                   ]

```

- list comprehension을 이용하였다.
- date**lt=date.today() - date 필드의 **lt(less than) date.today()(오늘 날짜 datetime object)라는 뜻
  - date field의 날짜 < 오늘날짜인 경우
- id=stat['champions_id'] - id가 stat element의 champion_id와 일치하는 경우
- date=Stat_champions.objects.order_by('date').last().date - 날짜가 stat_champions에서 date로 order_by 정렬하고 그 중에서 last는 가장 끝 row의 date인 경우를 뜻한다.

## ordering

queryset의 기본 정렬은 모델 클래스 내부의 Meta.ordering 설정을 따른다.

```
class User(models.Model):
  ....
  class Meta:
    ordering = ['-id']
    # id 필드 기준 내림차순 정렬, 미지정시 임의 정렬
```

모델 Meta.ordering 을 무시하고 직접 정렬조건 지정도 가능하다.

```
queryset = queryset.order_by('field1') # 지정 필드 오름차순 요청
queryset = queryset.order_by('-field1') # 지정 필드 내림차순 요청
queryset = queryset.order_by('field2', 'field3') # 1차기준, 2차기준
```

## slicing(범위 조건)

```
queryset = queryset[:10] # 현재 queryset에서 처음10개만 가져오는 조건을 추가한 queryset
queryset = queryset[10:20] # 현재 queryset에서 처음10번째부터 20번째까지를 가져오는 조건을 추가한 queryset

# 리스트 슬라이싱과 거의 유사하나, 역순 슬라이싱은 지원하지 않음
queryset = queryset[-10:] # AssertionError 예외 발생

# 이때는 먼저 특정 필드 기준으로 내림차순 정렬을 먼저 수행한 뒤, 슬라이싱
queryset = queryset.order_by('-id')[:10]
```

## GET

지정 조건에 맞는 DB data를 fetch하는 개념
해당 조건에 해당되는 데이터가 1개임을 기대

- 0개 : 모델클래스명.DoesNotExist 예외 발생
- 1개 : 정상처리
- 2개 : 모델클래스명.MultipleObjectsReturned 예외 발생

```
model_instance = queryset.get(id=1)
model_instance = queryset.get(name='winfor')
```

### first(), last()

위에서도 살짝 나왔지만

- 지정 조건 내에서 첫번째/마지막 데이터 Row를 Fetch한다.
- 지정 조건에 맞는 데이터 Row가 없더라도, DoesNotExist 예외가 발생하지 않고, None을 반환하는 것이 특징이다.

# CREATE

- 추가시에 필수필드 (필드 정의 시에, blank=True, null=True 혹은 디폴트값이 지정되지 않은 필드) 를 모두 지정해야한다. **IntegrityError** 발생
- python shell에서 해당 모델의 상세 필드옵션을 확인할 수 있다.

> create 예시

```
class User(models.Model):
   email            = models.CharField(max_length=255, unique=True)
   user_name        = models.CharField(max_length=150, null=True, blank=True)
   password         = models.CharField(max_length=300, null=True, blank=True)
   social           = models.ForeignKey(SocialPlatform, on_delete=models.CASCADE, null=True, related_name='user_social', default=1)
   social_login_id  = models.CharField(max_length=100, null=True)
   profile_photo    = models.URLField(max_length=500, null=True, blank=True)
   company          = models.CharField(max_length=100, null=True, blank=True)
   company_position = models.CharField(max_length=50, null=True, blank=True)
   university       = models.CharField(max_length=50, null=True, blank=True)
   major            = models.CharField(max_length=50, null=True, blank=True)
   main_address     = models.CharField(max_length=50, null=True, blank=True)
   sub_address      = models.CharField(max_length=50, null=True, blank=True)
   introduction     = models.CharField(max_length=1200, null=True, blank=True)

   class Meta:
       db_table = 'users'
```

1. Model Instance의 save함수를 통해 저장

```
def post(self, request):
    data = json.loads(request.body)
    login_user = User.object.get(id=data['id'])

    login_user.company          = data['company']
    login_user.company_position = data['company_position']
    login_user.university       = data['university']
    login_user.major            = data['major']
    login_user.main_address     = data['main_address']
    login_user.sub_address      = data['sub_address']
    login_user.introduction     = data['introduction']
    login_user.save()
```

2. Model manager의 create 함수를 통해 저장

```
def post(self, request):
    data = json.loads(request.body)
    login_user = User.object.get(id=data['id'])
    User.objects.create(
      login_user.company          = data['company']
      login_user.company_position = data['company_position']
      login_user.university       = data['university']
      login_user.major            = data['major']
      login_user.main_address     = data['main_address']
      login_user.sub_address      = data['sub_address']
      login_user.introduction     = data['introduction']
    )
```

# UPDATE(수정)

1. Model Instance 속성을 변경하고, save 함수를 통해 저장(create와 같으므로 위의 코드를 참고한다)

2. update 함수에 업데이트할 속성값을 지정하여 일괄 수정

```
update_data = User.object.all()
update_data.update(introduction='preview') # 일괄 update
```

# Delete

> 주의 사항 : 데이터를 지우는 것은 언제나 신중해야 한다. 그래서 보통 data를 delete하기 보다는 data를 GET이나 POST를 하지 못하도록 정지 관리를 하게 된다.

1. Model Instance의 delete 함수를 호출하여 삭제
   user_instance = User.object.get(id=23)
   user_instance.delete()

2. QuerySet의 delete 함수를 호출하여, 관련 데이터를 삭제

```
user = User.object.all()
user.delete() # 일괄 data delete
```
