---
title: "[Django] 5. 장고 실습 준비하기 : 05"
author: LaonMoon
date: 2021-08-21 13:01:00 +/-TTTT
categories: [Study, Django]
tags: [Django]
---

# **둘째 마당. 장고로 블로그 웹 사이트 만들기**

## 05 장고의 역할 이해하기

- 일반적인 웹 사이트 구조에 대한 개념
- 장고로 어떻게 구현할 수 있는지

### 05-1 웹 사이트의 작동 구조 이해하기

#### 프런트엔드와 백엔드란?

자동차에 비유해본다면...
- 프런트엔드 : 자동차의 외관과 실내 디자인
- 백엔드 : 엔진, 변속기, 브레이크, 에어컨 등 기계장치

**[프런트엔드로만 구성한 웹 사이트 살펴보기]**

주소를 입력해 검색하는 상황을 가정해보자. '~.com'이라는 주소를 갖는 서버는 프런트엔드만으로 구성된 웹 사이트를 서비스하고 있다. 그럼 웹 브라우저는 인터넷에 접속해서 '~.com'이라는 주소를 가진 서버 컴퓨터에 요청을 보낸다. 그 서버에서 제공하는 웹 사이트가 HTML로만 이루어진 단순한 구조이고 특별한 추가 요구 사항이 없으면 서버에서 index.html을 찾아서 보내준다. 웹 브라우저는 index.html을 받아 렌더링해서 화면에 보여준다.

index.html을 렌더링해서 펼친 웹 페이지에 `<a href="about_me.html">`라는 태그가 구현한 영역이 있을 때 그 영역을 사용자가 클릭하면 웹 브라우저는 다시 '~.com'이라는 주소를 가진 서버에 about_me.html을 요청하고, 서버는 about_me.html 파일을 찾아서 보내준다. 웹 브라우저는 받아온 html 파일을 렌더링해서 보여준다.

**[백엔드 기술이 필요한 이유]**

이런 웹 사이트는 단순하기 때문에 개발하기 쉽다는 장점이 있다. -> 그러나 웹 사이트 운영자가 일방적으로 정보를 제공할 수만 있을 뿐, 방문자의 행동을 저장하고 보여줄 수 없다는 한계가 있다. 게다가 웹 사이트 관리자도 새로운 내용을 추가하려면 html 파일을 일일이 수정해야 한다. 그래서 대부분의 웹 사이트는 데이터베이스를 활용한다.

`데이터 베이스를 활용하면` 사용자 계정, 사용자들의 게시글과 댓글, 조회수, 추천수 같은 정보를 수시로 저장할 수 있다. 그리고 html에 빈 칸이 있는 템플릿을 만들어 놓고, 필요한 정보를 데이터베이스에서 수시로 불러와서 보여줄 수도 있다. 이런 기능까지 고려한 웹 사이트를 만들려면 백엔드 기술까지 다룰 수 있어야 한다.

#### 웹 프레임 워크의 역할

여러 웹 사이트들 간의 공통점? -> 생성하고 `create`, 조회하고 `read`, 수정하고 `update`, 삭제하고 `delete` 이런 네 가지 기능(`CRUD`)이 필수로 들어간다. +) 웹 사이트 관리자가 편하게 관리할 수 있도록 `관리자 페이지`까지.

웹 프레임 워크는 이렇게 웹 개발에서 자주 사용하는 기능을 더 쉽게 개발할 수 있게 도와준다. 그래서 웹 프레임 워크를 이용하면 개발 시간을 단축하면서도 안정적인 웹 사이트를 구축할 수 있다.

### 05-2 장고의 작동 구조 이해하기

#### 장고로 만든 웹 사이트의 작동 구조

![how django works](/assets/img/Django/django_mechanism.png)

1. 먼저 클라이언트(웹 브라우저)는 일련의 과정을 거쳐 '~.com'이라는 이름의 서버를 찾아간다.
2. 프런트엔드 기술만으로 만든 웹 사이트가 index.html을 요청했던 것과는 달리, 우선 `urls.py`을 요청해 개발자가 써놓은 내용을 확인한다. urls.py에는 '~.com'이라는 URL로 접속했을 때는 index라는 함수를 실행시키자, '~.com/about_me/'로 접속했을 때는 about_me라는 함수를 실행시키자와 같은 내용이 기술되어 있다고 가정한다.
3. urls.py에서 언급하는 함수 또는 클래스는 `views.py`에서 정의한다. views.py의 index 함수의 예로는, 최근 게시글 5개를 index.html에 채워서 보여준다가 있을 수 있다. delete_post라는 함수가 있다면 게시글을 삭제한다와 같은 내용이 기술되어 있을 수도 있다.
4. 게시글(post)에 대한 내용은 `models.py`에서 정의한다. '게시글이 담아야 할 정보는 제목, 글, 내용, 작성자, 작성일이다'와 같은 식으로 정의한다. 장고에서 이렇게 자료의 형태를 정의한 클래스를 모델이라고 한다.
5. `models.py`에서 정의한 모델에 맞게 데이터베이스에서 필요한 자료를 가져온다. 예를 들어 views.py의 index 함수가 데이터베이스에서 최근 게시글 5개를 가져오는 기능을 한다면, 데이터베이스에서 최근 게시글 5개를 불러온다.
6. 마지막으로 데이터 베이스에서 가져온 자료를 템플릿(여기서는 `index.html`)의 빈 칸에 채워서 사용자의 웹 브라우저로 보낸다.

#### MTV 패턴이란?
장고로 만든 웹 사이트는 **모델 `model`**로 자료의 형태를 정의하고, **뷰 `view`**로 어떤 자료를 어떤 동작으로 보여줄지 정의하고, **템플릿 `templete`**으로 웹 페이지에서 출력할 모습을 정의한다. 이러한 작동 구조를 **MTV**패턴이라고 부른다.