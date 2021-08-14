---
title: "[Django] 4. 부트스트랩 알아보기(2) : 04-2"
author: LaonMoon
date: 2021-08-14 21:02:00 +/-TTTT
categories: [Study, Django]
tags: [Django]
---

### **04-2 부트스트랩으로 웹 사이트 모양 만들기**

#### 블로그 페이지 모양 구현하기
**[1] css와 내비게이션 바 가져오기**

blog_list.html을 수정한다.
```html
	<link href="./bootstrap4/css/bootstrap.min.css" rel="stylesheet" type="text/css">
</head>
    <body>
		<nav class="navbar navbar-expand-lg navbar-light bg-light">
			<div class="container">
				...
			 <div>
		</nav>
```

**[2] 페이지 내용이 들어갈 부분 구상하기**

화면 크기가 medium일 때 8:4, large일 때 9:3이 되도록 설정한다. 이를 위해 container를 class로 갖는 `div`를 만들고 위 아래 여백을 주기 위해 my-3으로 지정한다.

```html
<div class="container my-3">
	<div class="row">
		<div class="col-md-8 col-lg-9">
			<h1>Blog</h1>
		</div>
		<div class="col-md-4 col-lg-3">
			<h3>Search</h3>
			<h3>Categories</h3>
		</div>
	</div>
</div>
```

**[3] Start Bootstrap에서 템플릿 샘플 찾기**

[부트스트랩 공식 홈페이지](https://getbootstrap.com/docs/4.5/examples/) > Examples 페이지에서 웹 페이지 샘플을 다양하게 제공하고 있다. 그리고 [Start Bootstrap](https://startbootstrap.com/templates/blog-news) > Templates > Blog로 들어가면 블로그 웹 사이트 템플릿 예제로 Blog Post와 Blog Home이 나온다.(Blog Post)

#### 카드에 구성 요소 담기

1. Blog Home 예제의 카드 구성 살펴보기
2. Blog Home 예제의 소스 코드 살펴보기

- **ctrl+U**를 누르면 웹페이지의 소스코드를 바로 볼 수 있다. 
- placeholder.com을 사용해 임시 이미지를 넣을 수도 있다. 원하는 폭과 높이를 지정하면 그 크기에 맞는 그림을 보내준다. 이는 웹 개발을 할 때 유용하게 사용할 수 있다.

3. 검색 창 카드와 카테고리 카드 살펴보기

gird는 전체 화면만 12분할 하는 것이 아니라 요소 안에서도 12분할하여 화면을 구성할 수 있다.

#### 페이지 이동 버튼과 푸터 추가하기
1. Blog Home 예제에서 pagination 소스 코드 가져오기

블로그 포스트를 한 페이지에 나타내는 것은 비효율적이다. 그러므로 페이지당 보여주는 포스트 갯수를 설정하고 다른 페이지로 넘어갈 수 있게 하는 것이 필요함.
+) pagination은 페이지 매김 같은 의미인 것 같다. 페이지를 이동할 수 있는 버튼.

blog_list.html에 pagination에 해당하는 부분의 코드를 복사한다.
```html
</div>
<nav aria-label="Pagination">
    <hr class="my-0" />
        <ul class="pagination justify-content-center my-4">
             ...
        </ul>
</nav>
</div>
<div class="col-md-4 col-lg-3">
```

2. Blog Home 예제에서 footer 소스 코드 가져오기

자바스크립트 코드 바로 위에 추가해준다.
```html
<!-- Footer-->
        <footer class="py-5 bg-dark">
            <div class="container"><p class="m-0 text-center text-white">Copyright &copy; Your Website 2021</p></div>
        </footer>
```

#### 모달로 로그인 창 만들기

모달 `modal`은 웹 브라우저 위에 팝업 형태로 나오는 요소이다.

[Bootstrap 공식 웹사이트](https://getbootstrap.com/docs/4.5/components/modal/) > Components > Modal로 들어간 후 > Live demo의 Launch demo modal 버튼을 클릭하면 모달을 볼 수 있다.

1. 모달 예제의 소스 코드 살펴보기

소스 코드는 **모달을 나타내기 위한 버튼 `Button trigger modal`**과 **모달을 구현한 부분 `Modal`**으로 나눌 수 있다.

2. 모달 소스 코드 추가하기

blog_list.html에 적용한다.
```html
<ul class="navbar-nav ml-auto">
	<li class="nav-item">
		<a class="nav-link" href='#' data-toggle"modal" data-target="#loginModal">Log In</a>
	</li>
</ul>

</nav>
<!-- Modal -->
<div class="modal fade" id="loginModal" ... aria-labelledby="loginModalLabel" ...>
  	...
    <h5 class="modal-title" id="loginModalLabel">Modal title</h5>
        ...
      </div>
    </div>
  </div>
</div>
```
#### 버튼 추가하기
1. 부트스트랩에서 제공하는 버튼 예시 살펴보기

[부트스트랩 공식 사이트](https://getbootstrap.com/docs/4.5/components/buttons/) > Documentaton > Components > Buttons

[기본 버튼]

기본적으로 `<button type="button" class="btn">`을 사용하면 부트스트랩의 버튼 모양을 사용할 수 있다. 즉, 색상이나 크기 등을 class에 추가로 설정하면 버튼의 모양을 변화시킬 수 있다.

[테두리가 있는 버튼]

Outline buttons 항목에서 테두리가 있는 버튼 예시 찾을 수 있다.

[`<a>`태그로 만든 버튼]
Button tags 항목을 보면 HTML 태그로 버튼을 만드는 예시가 나와있다. `<a>태그`의 class를 `<button>` 태그를 사용할 때와 동일하게 설정하고 role="button"을 추가하면 된다.

2. 버튼 3개 만들기

구글 아이디 로그인(Log in with Google), 이메일 로그인(Log in with E-mail), 이메일 회원가입(Sign up with E-mail)로 총 3개의 버튼을 만들 것이다.

```html
<div class="modal-body">
    <div class="row">
		<div class="col-md-6">
			<button type="button" class="btn btn-outline-dark">Log in with Google</button>
			<button type="button" class="btn btn-outline-dark">Log in with Email</button>
		</div>
		<div class="col-md-6">
			<button type="button" class="btn btn-outline-dark">Sign up with E-mail</button>
		</div>
	</div>
</div>
```

3. 버튼 크기와 위치 조정하기

class에 btn-block과 brn-sm을 추가한다.

```html
<button type="button" class="btn btn-outline-dark btn-block btn-sm">Log in with Google</button>
<button type="button" class="btn btn-outline-dark btn-block btn-sm">Log in with Email</button>
...
<button type="button" class="btn btn-outline-dark btn-block btn-sm">Sign up with E-mail</button>
```

#### 버튼에 아이콘 추가하기

1. Font awesome 무료 계정 만들기

아이콘을 추가하고 싶을 때 -> [Font awesome](https://fontawesome.com/)이라는 아이콘 툴킷 서비스 이용하기.

2. 아이콘 추가하기
```html
<h5 class="modal-title" id="loginModalLabel"><i class="fas fa-sign-in-alt"></i>Log In</h5>

<button type="button" class="btn btn-outline-dark btn-block btn-sm"><i class="fab fa-google"></i> Log in with Google</button>
<button type="button" class="btn btn-outline-dark btn-block btn-sm"><i class="far fa-envelope"></i>Log in with Email</button>
...
<button type="button" class="btn btn-outline-dark btn-block btn-sm"><i class="far fa-envelope"></i>Sign up with E-mail</button>
```
복사해온 코드 뒤에 `&nbsp&nbsp`을 넣으면 여백이 생긴다.
```html
<h5 class="modal-title"... </i>&nbsp&nbsp Log In</h5>

<button type="button" ... </i>&nbsp&nbsp Log in with Google</button>
<button type="button" ... </i>&nbsp&nbsp Log in with Email</button>
...
<button type="button" ... </i>&nbsp&nbsp Sign up with E-mail</button>
```