---
title: "[Django] 3. 부트스트랩 알아보기(1) : 04-1"
author: LaonMoon
date: 2021-08-13 14:53:00 +/-TTTT
categories: [Study, Django]
tags: [Django]
---

### **04-1 부트스트랩 알아보기**
`Bootstrap`은 웹 개발에 있어 보편적으로 널리 쓰이는 구성 요소들을 미리 디자인 해둔 툴킷 `toolkit`이다.

#### 부트스트랩의 CSS와 자바스크립트 적용하기

1. 부트스트랩에서 css, css.map 파일 가져오기

- 부트스트랩 [공식 사이트](https://getbootstrap.com/docs/4.5/getting-started/download/)에서 v4.5.X 버전을 선택하고 Compiled CSS and JS 아래의 다운로드를 클릭한다.

- 압축 파일 안의 css 폴더, 그 안에 있는 파일을 가져온다.
- bootstrap4/css 폴더 안에 파일을 복사하고, html 파일에는 `link 태그`를 수정한다.

```html
<link href="./bootstrap4/css/bootstrap.min.css" rel="stylesheet" type="text/css">
```

2. 자바스크립트 코드 추가하기

자바스크립트 코드는 부트스트랩 웹 사이트에서 직접 가져온다. jsDelivr 항목을 찾아 코드 복사하기.

```html
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js" integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN" crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.min.js" integrity="sha384-w1Q4orYjBQndcko6MimVbzY0tgp4pWB4lZ7lr30WKz0vr/aWKhXdBNmNb5D92v7s" crossorigin="anonymous"></script>
```
`/body 태그` 바로 위에 해당 코드를 넣어줌.

#### +) CDN 
Contents delivery network. 사람들이 자주 사용하는 css, js 파일 등을 모아둔 곳.

-> 사실 CDN은 깃헙 블로그를 만들다가 들어본 용어다. 지금 사용하고 있는 테마를 만든 분은 이미지를 cdn 어쩌고로 연결해서 avatar를 설정해둔 것 같았는데, 나는 asset 파일에 사진을 넣고 경로를 지정하고 싶었지만 그러지 못하고 깃헙에 올려두었기 때문에 그 방법이 궁금했던 차였다.(+ 혹시나 싶어 다시 경로로 수정해봤는데 잘 된다...?)

#### 네비게이션 바 수정하기
1. Components에서 [내비게이션 바 요소](https://getbootstrap.com/docs/4.5/components/navbar/) 고르기

부트스트랩 사이트 > Documentation > Components > Navbar의 코드를 copy해온다.

2. 내비게이션 바 수정하기

```html
<a class="navbar-brand" href="#">Navbar</a>
``` 
이 부분을 Navbar에서 Do It Django로 바꾼다.
```html
<a class="nav-link" href="#">Home <span class="sr-only">(current)</span></a>
<a class="nav-link" href="#">Link</a>
```
href= 다음 비어 있는 부분을 ./about_me.html로 바꾼다. 마찬가지로 href= 를 ./blog_list.html로 바꾼다.

#### container로 레이아웃 적용하기
부트스트랩의 container를 사용하면 페이지 구성을 위한 레이아웃을 간단히 만들 수 있다.

**[container 적용해 여백 만들기]**

```html
<div>
	<h1>About me</h1>
	<h2>개발자 문라온입니다.</h2>
		
	<p>하고 싶은 일을 하며 살고 있습니다.</p>
		
	<button onclick="whatTimeIsIt()">현재시간</button>
	<a href="index.html">Go back First Page</a>
	<img src="./images/boy.jpg" width="600px">
</div>
```
about_me.html에서 내용에 해당하는 부분을 `div 태그`로 감싸고 class는 container로 설정하면 -> 양쪽에 여백이 생기고 그 사이에 내용이 담긴다.

**[내비게이션 바 여백 만들기]**

이 때 두 가지 방법이 있다.

1. `nav 태그`의 class에 container 추가하기

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
```
여기에 container를 추가한다.
```html
<nav class="navbar navbar-expand-lg navbar-light bg-light container">
```
그러면 내비게이션 바 전체 양 옆에 여백이 생긴다.

2. `div 태그`를 만들고 class 값으로 container를 설정하기

`nav 태그`의 class에 container를 설정하지 말고, `nav 태그` 안쪽에 `div 태그`를 새로 만들고 class 값으로 container를 설정하면 된다.

```html
<nav class= ...>
	<div class="container">
		<a class=...>
	</div>
</nav>
```
그러면 내비게이션 바는 유지되면서 버튼만 여백 안으로 들어오게 된다.

#### grid로 웹 페이지 구성 관리하기
grid는 전에 flutter를 다룰 때 grid view라고 해서 들어본 적이 있다. 그 때의 grid view는 마치 표같이 생긴 화면을 보여줬는데, 이것도 마찬가지인 듯 하다. 

부트스트랩의 grid 기능을 사용하면 웹 페이지를 세로 12칸으로 나누어 관리할 수 있다. 이 기능을 사용하면 화면 크기를 다양하게 바꾸어도 그 크기에 맞게 웹 페이지 모양도 바뀌어 나타난다.

1. 사진 위치 바꾸기
화면 전체를 12칸으로 나눴을 때 오른쪽부터 3칸에 해당하는 열로 사진을 옮길 것이다.

div class="container"안에 div class="row"를 하나 만든다. 그리고 그 안에 `div 태그`를 또 만들고 class="col-9", class="col-3"으로 설정해 9칸짜리 열과 3칸짜리 열로 나눈다.

- 9칸짜리 열에 해당하는 왼쪽 부분 -> 텍스트로 된 내용 넣기
- 3칸짜리 열에 해당하는 오른쪽 부분 -> 사진을 넣기

```html
<div class="container">
	<div class="row">
		<div class="col-9">
			<h1>About Me</h1>
			...
		</div>
		<div class="col-3">
			<img src=...>
		</div>
	</div>
</div>
```
그리고 `img 태그`에서 height="300px"을 지우고 class="img-fluid w-100"으로 설정한다.(그러면 사진이 생각보다 작게 나온다.) 화면 크기가 달라져도 그 비율은 달라지지 않는 걸 알 수 있다.

2. 화면 크기에 따라 구성도 바뀌게 만들기
부트스트랩 공식 홈페이지 > Documentation > Layout > Grid > How it works를 찾자. 코드를 복사하고 class="row"인 div 요소만 붙여 넣는다.

##### +) 부트스트랩으로 배경색 쉽게 추가하기

부트스트랩 공식 웹사이트에서 웹사이트의 배경색과 글자색을 조화롭게 구성하는 걸 도와줌. [Documentation > Utilities > Colors]로 들어가기
```html
<div class="container">
  <div class="row">
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      One of three columns
    </div>
    <div class="col-sm">
      One of three columns
    </div>
  </div>
</div>
```
여기서 sm은 화면 크기가 small 일 때만 적용하는 기준이라는 의미이다. 화면 크기에 대한 기준은 부트스트랩 공식 문서에 표로 정리되어 있다.

3. 열마다 이미지 추가하기

```html
<div class="row">
	<div class ="col-12">
		<h2>Portfolio</h2>
	</div>
    <div class="col-sm col-lg-6 bg-info">
      <img src="./images/c.jpg" class="img-fluid">
    </div>
    <div class="col-sm col-lg-3 bg-secondary">
      <img src="./images/b.jpg" class="img-fluid">
    </div>
    <div class="col-sm col-lg-3 bg-warning">
      <img src="./images/a.jpg" class="img-fluid">
    </div>
</div>
```

#### spacing으로 간격 주기
간격을 조정하는 방법 -> 마진 `margin`과 패딩 `padding`이 있다. 마진은 내용 경계 바깥쪽으로 간격을 두는 방법이고, 패딩은 내용 경계 안쪽으로 간격을 두는 방법이다.

![margin and padding](/assets/img/Django/margin_and_padding.jfif)

1. 내비게이션 바와 내용 사이에 마진 넣기

부트스트랩에서는 요소의 class에 `m-숫자`를 추가해 마진을 줄 수 있다.`mt-숫자`를 추가하면 위에만 마진을 주고, `mb-숫자`는 아래에, `my-숫자`는 위 아래 모두에 마진을 준다.

```html
<div class="row my-3">
```
2. 내비게이션 바 오른쪽 정렬하기

spacing을 이용하면 구성 요소를 정렬시킬 수 있다. `ml-auto`나 `mr-auto`를 사용한다. ml-auto는 왼쪽 마진을 최대한 확보하라는 의미이고, mr-auto는 오른쪽 마진을 최대한 확보하라는 의미이다.

내비게이션 바에 Log In 버튼을 추가하고 이 버튼만 오른쪽 정렬하기-> **ml-auto을 추가**한다 or **첫번째 `ul 태그`의 class에 mr-auto를 추가**한다.

```html
</ul>
    <ul class="navbar-nav ml-auto">
	<li class="nav-item">
		<a class="nav-link" href='#'>Log In</a>
	</li>
	</ul>
  <div>
```
