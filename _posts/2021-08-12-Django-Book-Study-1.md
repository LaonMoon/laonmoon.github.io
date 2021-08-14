---
title: "[Django] 1. Django 알아보기(+HTML 실습) : 03-1까지"
author: LaonMoon
date: 2021-08-12 08:34:00 +/-TTTT
categories: [Study, Django]
tags: [Django]
---

### **01-2 장고 웹 프레임워크 알아보기**

#### 웹 사이트의 작동
방문자가 웹사이트의 서버에 요청을 보내면(요청 `request`) 서버는 필요한 정보를 방문자에게 보내준다(응답 `response`).

요청에는 URL입력, 회원가입, 댓글 삭제 등 -> 서버에 있는 정보를 가져오기, 서버에 내용을 저장하거나 삭제.

#### 웹 프레임워크란?
웹사이트 구현 필요한 여러 복잡한 기능 쉽게 만들 수 있게 도와주는 도구. 이미 개발되어 있는 기능을 가져와서 개발 중인 웹사이트에 쉽게 추가할 수 있음. 성능과 보안 측면에서도 충분히 검증된 방법으로 개발할 수 있다.

#### why Django?
1. python 기반
2. 웹 서버나 데이터베이스를 따로 설치할 필요X
3. 관리자 페이지, 보안 기능을 제공함.

#### Django vs Flask
사실 이전에 플라스크를 공부해보려다가 중간에 유야무야했었는데, Django가 끝나면 Flask도 공부할 것이다. 둘 다 파이썬 웹 프레임워크지만, 장고보다 플라스크가 자유도가 높고 프레임워크에 덜 의존적으로 개발할 수 있다는 장점을 가지고 있다.

### **03-1 HTML 살펴보기**
웹페이지는 대부분 HTML. CSS, 자바스크립트로 구성되어 있다.

#### 01단계 -  HTML의 가장 기초적인 상태
```html
<!DOCTYPE html>
<html>
    <head>

    </head>
    <body>

    </body>
</html>
```

#### 02단계 - index.html 채우기
```html
<!DOCTYPE html>
<html>
    <head>
		<title>문라온의 홈페이지</title>
    </head>
    <body>
		<nav>
			<a href="./index.html">Home</a>
			<a href="./blog_list.html">Blog</a>
			<a href="./about_me.html">About Me</a>
		</nav>
		
		<h1>첫 번째 크기 헤드라인</h1>
		<h2>두 번째 크기 헤드라인</h2>
		<h3>세 번째 크기 헤드라인</h3>
		<h4>네 번째 크기 헤드라인</h4>
		<h5>다섯 번째 크기 헤드라인</h5>
		<p>문단은 p로 쓰세요.</p>
		<a href="https://www.google.com">Go to google</a>
		<hr/>
		<img src="./images/stay_funky.jpg" width="600px">
    </body>
</html>
```
* nav : 웹 사이트의 주요 페이지를 안내하는 내비게이션 역할을 만들 때
* hr/ : 수평선을 그어주는 태그
* src : 이미지 소스 위치

#### 03단계 - about_me.html 채우기
```html
<!DOCTYPE html>
<html>
    <head>
		<title>About me</title>
    </head>
    <body>
		<nav>
			<a href="./index.html">Home</a>
			<a href="./blog_list.html">Blog</a>
			<a href="./about_me.html">About Me</a>
		</nav>
		
		<h1>About me</h1>
		<h2>개발자 문라온입니다.</h2>
		
		<p>하고 싶은 일을 하며 살고 싶습니다.</p>
		<a href="index.html">Go back First Page</a>
		<img src="./images/boy.jpg" width="600px">
    </body>
</html>
```

#### 04단계 - 스타일 적용하기
일단 다음과 같은 코드를 작성한다.
```html
<!DOCTYPE html>
<html>
    <head>
		<title>About me</title>
    </head>
    <body>
		<nav style="background-color: darkgreen; font-size: 150%; text-align:center">
			<a href="./index.html" style="color:white">Home</a>
			<a href="./blog_list.html" style="color:white">Blog</a>
			<a href="./about_me.html" style="color:white">About Me</a>
		</nav>
		
		<h1>About me</h1>
		<h2>개발자 문라온입니다.</h2>
		
		<p>하고 싶은 일을 하며 살고 있습니다.</p>
		<a href="index.html">Go back First Page</a>
		<img src="./images/boy.jpg" height="600px">
    </body>
</html>
```

이 때, css 부분을 따로 빼내어 `head 태그 안에 style 태그를 삽입`해서 작성할 수도 있다.

```html
<!DOCTYPE html>
<html>
    <head>
		<title>About me</title>
		<style>
		nav{
		background-color: darkgreen; font-size: 150%; text-align:center
		}
		nav a{
		color:white
		}
		</style>
    </head>
    <body>
		<nav>
			<a href="./index.html">Home</a>
			<a href="./blog_list.html">Blog</a>
			<a href="./about_me.html">About Me</a>
		</nav>
		
		<h1>About me</h1>
		<h2>개발자 문라온입니다.</h2>
		
		<p>하고 싶은 일을 하며 살고 있습니다.</p>
		<a href="index.html">Go back First Page</a>
		<img src="./images/boy.jpg" width="600px">
    </body>
</html>
```
#### 05단계 - blog_list.html 만들기
```html
<!DOCTYPE html>
<html>
    <head>
		<title>About me</title>
		<style>
		nav{
		background-color: darkgreen; font-size: 150%; text-align:center
		}
		nav a{
		color:gold
		}
		</style>
    </head>
    <body>
		<nav>
			<a href="./index.html">Home</a>
			<a href="./blog_list.html">Blog</a>
			<a href="./about_me.html">About Me</a>
		</nav>
		
		<h1>Blog</h1>
		<p>Empty.</p>
    </body>
</html>
```

