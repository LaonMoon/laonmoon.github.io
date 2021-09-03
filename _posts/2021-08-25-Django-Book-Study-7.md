---
title: "[Django] 7. 장고 프로젝트에서 앱 개발하기: 07"
author: LaonMoon
date: 2021-08-25 23:44:00 +/-TTTT
categories: [Study, Django]
tags: [Django]
---

## 07 장고 프로젝트에서 앱 개발하기

장고 프로젝트는 보통 1개 이상의 앱을 가진다. 여기서 앱은 '블로깅 기능', '단일 페이지 보여주기 기능'과 같이 특정 기능을 수행하는 단위 모듈을 말한다.(안드로이드 애플리케이션을 '앱'이라고 부르는 것과는 다름.)

### 07-1 블로그 앱과 페이지 앱 만들기

모든 장고 프로젝트는 1개 이상의 앱으로 구성된다. 여기서 **앱**은 특정한 기능을 수행하는 단위 모듈로 생각하면 된다.(e.g. 블로그와 갤러리, 방명록의 3가지 기능을 갖는 웹사이트를 만들 때는 일반적으로 3개의 앱을 만들어 개발하고 관리한다.) 

앱의 개수는 상황에 따라 개발자 스스로 선택해야 할 문제다. 이 프로젝트에서는 2개의 앱을 만든다.

- 블로그 기능을 위한 blog 앱
- 대문 페이지와 자기소개 페이지를 보여주기 위한 single_pages 앱

#### **blog 앱과 single_pages 앱 만들기**

**가상환경 실행 확인 후 blog 앱 만들기**

blog 앱을 만들기 위해 `python manage.py startapp blog`를 입력한다. 그러면 현재 위치에 blog 앱 폴더가 생성된다. 마찬가지로 `python manage.py startapp single_pages`를 입력한다.

위 과정을 마치면, 파이참에서 2개의 폴더가 생성되어 있고, 각 폴더에는 파일이 생성된다. 이처럼 앱은 각각 독립된 파일들로 구성되어 독립된 기능을 한다. 앞으로 이 파일들을 수정해 웹 사이트를 만들어 갈 것이다.

**깨끗한 상태의 migration 폴더를 커밋한 이유는?**

'아무 작업도 하지 않았는데 왜 커밋을 할까?' 

그 이유는 모델이 생성, 수정될 때마다 blog 앱(또는 single_pages 앱)의 migrations 폴더에 자동으로 새로운 마이그레이션 파일이 생성되기 때문이다.

모델이 생성, 수정되면 앱의 데이터베이스 구조가 변경된다. 그러면 장고는 변화 내용을 migrations 폴더에 별도의 파일로 남겨 기록을 한다. 그런데 이 파일은 로컬 작업에 필요한 것으로, 서버 작업에는 필요하지 않다. 오히려 서버 작업의 혼란만 가중시킨다. 또한 이후 실습에서는 migrations 폴더의 내용을 깃에서 관리하지 않도록 설정할 것이다.

### 07-2 데이터베이스 개념 이해하기

블로그에 필요한 여러 정보를 효율적으로 저장하고 관리하기 위해 데이터베이스를 사용한다.

### 07-3 모델 만들기

장고의 장점 중 하나는 모델을 이용해 장고 웹 프레임워크 안에서 데이터베이스를 관리할 수 있다는 것이다. **모델**은 데이터를 저장하기 위한 하나의 단위라고 보면 된다.

일반적으로 데이터베이스를 다루려면 SQL 등의 언어를 배워야 하는데 웹 개발 입분자에게는 진입장벽으로 작용할 수 있다. 그래서 장고의 모델을 활용하면 CRUD 기능을 쉽게 구현할 수 있을 뿐 아니라 관리자 페이지, 입력 폼 등도 쉽게 만들 수 있다.

#### **블로그의 글을 위한 모델 만들기**

**[1] Post 모델 만들기**

블로그의 핵심인 포스트의 형태를 정의하는 Post 모델. 포스트에는 제목`title`, 내용`content`, 작성일`created_at`, 작성자 정보`author`가 필요하다.

blog/model.py를 열어 다음과 같이 입력한다.

```python
# Create your models here.
class Post(models.Model):
    title = models.CharField(max_length=30)
    content = models.TextField()
    
    created_at = models.DateTimeField()
    #author : 추후 작성예정
```

`Post 모델`은 models 모듈의 Model 클래스를 확장해서 만든 파이썬 클래스이다. 

- `title 필드`는 CharField 클래스로 만들고, 최대 길이가 30이 되도록 설정했다. CharField는 문자(char)를 담는 필드이다.
- `content 필드`는 문자열의 길이 제한이 없는 TextField()를 사용해 만들었다.
- `created_at 필드`는 DateTimeField로 만든다. DateTimeField는 월, 일, 시, 분, 초까지 기록할 수 있게 해주는 필드를 만들 때 사용한다.
- author 필드는 나중에 모델에서 외래키를 구현할 때 다룰 것이다.

[2] 데이터베이스에 Post 모델 반영하기

아직 Post 모델은 파이썬 클래스로만 존재한다. 이를 데이터베이스에 반영해야 실제 테이블이 생성된다. 터미널에 `python manage.py makemigrations`를 입력하자. 
그러면 'No changes detected'라는 메세지가 뜬다. models.py를 수정했는데 왜 아무런 변화를 감지하지 못했을까? 

왜냐하면 프로젝트 폴더(do_it_django_prj) 안에 있는 settings.py 파일에 현재 우리의 **'blog' 앱을 등록하지 않았기 때문**이다.

**[3] settings.py에 blog 앱 등록하기**

`settings.py`에는 **INSTALLED_APPS**라는 리스트가 있다. 여기에 'blog', 'single_pages' 앱을 추가한다.

```python
INSTALLED_APPS = [
    .
    .
    .
    'blog',
    'single_pages'
]
```

**[4] 데이터베이스에 Post 모델 반영하기**

그리고 다시 `python manage.py makemigrations`를 입력한다. 

```
Migrations for 'blog':
  blog\migrations\0001_initial.py
    - Create model Post
```
그러면 blog/migrations 폴더에 0001_initial.py파일이 생성된다. 그리고 아직 **데이터베이스에 모델이 적용되지 않은 상태**이기 때문에 실제 데이터베이스에 모델을 적용하려면 `python manage.py migrate` 명령을 실행해야 함.

```
Operations to perform:
  Apply all migrations: admin, auth, blog, contenttypes, sessions
Running migrations:
  Applying blog.0001_initial... OK
```

이 때 0001_initial.py 파일은 최초 makemigrations 명령 실행 시 생기는 파일이다. 그런데 이 파일은 버전 관리의 대상이 아님. -> `.gitignore`을 이용해 깃 관리 대상에서 제외하자.

**[5] .gitignore 수정하기**
```
# Pycharm
.idea/
migrations/
```
이렇게 하면 깃은 migrations 폴더의 내용을 관리하지 않는다.

**왜 migrations 폴더를 .gitignore에 등록했나?**

블로그 개발 중 models.py 수정할 일 多. 그리고 최종 결과물만 서버에 적용한다. 그런데 모델 수정 내용을 일일이 기록하다 보면 로컬 컴퓨터의 데이터베이스와 서버의 데이터베이스가 일치하지 않아 문제가 생길 수 있다. 그래서 migrations 폴더를 .gitignore에 등록하여 깃으로 버전 관리를 하지 않는 것.

#### **관리자 페이지에서 첫 포스트 작성하기**
`python manage.py runserver` 입력해서 서버 실행 -> `127.0.0.1:8000/admin/`들어가서 관리자 페이지 열기

**[1] admin.py에 Post 모델 추가하기**
blog/admin.py 파일에 추가하여 관리자 페이지에 Post 모델 등록.

```
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

**[2] 새로운 포스트 생성하기**

Posts 옆의 add를 눌러 글을 작성하기.

**=> models.py에 Post 모델을 정의하고, admin.py에 코드를 두 줄 추가하여 관리자 페이지에 새로운 포스트를 작성할 수 있는 기능이 추가되었다.**

#### **포스트 개선하기**

**[1]`__str__()` 함수로 포스트 제목과 번호 보여주기**

이대로면 포스트 목록에서 포스트의 제목이 보이지 않는다. -> **Post 모델에 `__str__()` 함수를 선언하자**

blog/models.py에서 추가한다.
```python
    class Post(models.Model):
    .
    .
    def __str__(self):
        return f'[{self.pk}]{self.title}'
```
장고의 모델을 만들면 기본적으로 **pk 필드**가 만들어진다.(pk는 각 레코드에 대한 고유값) 포스트를 처음 생성하면 자동으로 pk 값으로 1이 부여되고, 두 번째 포스트는 자동으로 2가 부여되는 방식이다. 

이 값을 이용해서 포스트의 제목과 번호를 문자열로 표현한다. 저장한 후 새로고침 하면 대괄호 안에 포스트 번호({self.pk})가 나오고 그 뒤에 포스트 제목 ({self.title})이 나온다.

**[2] 특정 지역 기준으로 작성 시각 설정하기**
do_it_django_prj/settings.py를 열고 수정한다.
```
TIME_ZONE = 'Asia/Seoul'
.
.
USE_TZ = False
```

**[3] 자동으로 작성 시각과 수정 시각 저장하기**
자동으로 작성 시간이 저장되고, 수정 시각도 저장되도록 하려면?

수정 시각을 저장할 updated_at 필드를 DateTimeField로 하나 더 만들면 된다. DateTimeField는 auto_now와 auto_now_add라는 설정이 있어서 처음 레코드가 생성된 시점, 마지막으로 저장된 시점을 자동으로 저장할 수 있다.

blog/models.py를 수정한다.
```python
class Post(models.Model):
    .
    .
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
```
created_at 필드에 auto_now_add=True로 설정해서 처음 레코드가 생성될 때 현재 시각이 자동으로 저장되게 한다. 그런 다음 updated_at 필드를 만들고 auto_now=Ture로 설정해서 다시 저장할 때마다 그 시각이 저장되도록 한다.

모델을 수정했으니 이를 `makemigrations`로 장고에게 알려주고, `migrate`로 데이터베이스에 반영해야 한다. 그리고 서버를 다시 실행한다.
```
python manage.py makemigrations
python manage.py migrate
python manage.py runserver
```

+my) 사실 궁금하다. makemigrations와 migrate를 나눠서 진행하는 이유는 뭘까? 더 알아보자. 일단 [여기](https://tibetsandfox.tistory.com/24) 참고!

#### **장고 셸 `Django shell` 사용하기**

`python manage.py shell`을 입력하면 장고 셸이 실행된다.

```python
>>> from blog.models import Post
>>> p = Post.objects.last()
```
blog 앱의 models.py에서 Post 모델을 import 한다. 그리고 Post.objects.last() 명령어로 데이터 베이스에서 Post 모델의 레코드 중 마지막 레코드를 가져와 p에 저장한다.(*레코드: 가로 방향으로 읽는 데이터)

```python
>>> p.title
'세 번째 글입니다'
>>> p.created_at
datetime.datetime(2021, 8, 25, 23, 37, 55, 147510)
>>> p.updated_at
datetime.datetime(2021, 8, 25, 23, 37, 55, 147510)
>>> exit()
```
최근 포스트 제목, 작성 시각, 수정 시각을 확인할 수 있다. 빠져나오는 건 `exit()`이다.