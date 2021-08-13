---
title: "[Django] 2. 웹 프런트엔드 기초 : 03-3까지"
author: LaonMoon
date: 2021-08-13 11:28:00 +/-TTTT
categories: [Study, Django]
tags: [Django]
---

### 03-2 CSS 살펴보기
웹페이지 디자인이 일관성 있게 만들려면 style 태그를 복사해온다? -> X. 웹페이지 수가 많을 때는 그러기 힘들다.

**CSS(Cascading style sheets)**를 사용!

#### CSS란?
웹 문서의 디자인을 구현하기 위한 언어.

HTML : 웹페이지의 틀. 내용을 채움.

CSS : 텍스트의 크기, 색, 이미지의 크기나 위치를 지정

#### CSS 적용하기
1. 새로 css 파일을 만든다.
```css
nav{
	background-color: darkgreen; font-size: 150%; text-align:center
	}
nav a{
	color:white
	}
```
2. html 파일에 CSS 링크를 단다.

```html
<link href="./practice.css" rel="stylesheet" type="text/css">
```
style 태그가 있던 곳의 코드를 바꾼다.

### 03-3 자바스크립트 살펴보기
사용자가 서버에 특정 페이지를 요청하면 서버는 html 파일과 css 파일을 넘겨주고, 사용자의 컴퓨터 혹은 스마트폰의 브라우저는 이 파일을 렌더링해서 화면에 보여주는 방식이었다. -> **정적인 페이지**를 보여줄 때 적합한 형태. 왜냐하면 화면 내용 중 아무리 작은 요소만 바꾸려고 해도 서버에 다시 요청해서 새 html 파일과 css 파일 전체를 다시 받아와야 하기 때문.

**동적인 부분**을 만들 때는(e.g. 실시간 덧셈 기능) **자바스크립트**를 사용!

#### 덧셈 기능 추가하기

1. 덧셈 기능 자바스크립트 함수 추가하기

`head 태그`안에 script 태그를 넣는다.
```html
<head>
		<title>문라온의 홈페이지</title>
		<link href="./practice.css" rel="stylesheet" type="text/css">
		
		<script type="text/javascript">
			function doSomething(){
				let a = document.getElementById('inputA').value;
				let b = document.getElementById('inputB').value;
				document.getElementById("valueA").innerHTML = a;
				document.getElementById("valueB").innerHTML = b;
				document.getElementById("valueC").innerHTML = Number(a) + Number(b);
			}
		</script>
    </head>
```
id가 inputA, inputB인 input 요소의 값을 가져와 변수 a,b에 저장함. 그리고 id가 valueA, valueB인 요소의 텍스트를 a,b 값으로 수정함. id가 valueC인 요소의 텍스트는 a와 b의 값을 숫자로 바꾼 다음 더한 결괏값으로 수정함.

2. 자바스크립트 함수에 활용할 HTML 태그 추가하기

doSomething() 함수에서 사용한 inputA와 inputB라는 id로 `input 태그`를 추가한다. input 태그에는 onkeyup="doSomething()"을 추가해 이 요소에서 키보드의 키가 눌렸다가 올라가면 doSomething() 함수가 실행되도록 설정한다. 결과값을 출력하는 문장은 doSomething() 함수가 값을 반영할 수 있도록 `span 태그`를 사용해 추가한다. 

`body 태그`안에 추가한다.
```html
<label for="inputA">a</label>
		<input id="inputA" value=1 onkeyup="doSomething()">
		<label for="inputB">b</label>
		<input id="inputB" value=2 onkeyup="doSomething()">
		
		<p><span id="valueA">1</span> + <span id="valueB">2</span> = <span id="valueC">3</span>입니다.<p>
```

#### 현재 시간 확인 기능 추가하기
1. whatTimeIsIt() 함수 추가하기

`script 태그`안에 whatTimeIsIt()이라는 함수를 추가하고, alert(new Date())를 추가해 현재 시간을 팝업으로 보여주는 기능을 만든다.
```html
function whatTimeIsIt() {
				alert(new Date());
			}
```
2. 버튼 추가하기
`body 태그`안에 추가하기.
```html
<button onclick="whatTimeIsIt()">현재시간</button>
		<hr />
```

#### js 파일을 여러 html 파일에 적용하기
마찬가지로 여러 파일에 적용하고 싶다면?
1. js 파일 만들기

만들었던 function을 각각의 js파일로 옮긴다.

2. html 파일에 js 링크 달기

`head 태그` 안에 넣어준다. 
```html
<script type="text/javascript" src="add_two_numbers.js"></script>
<script type="text/javascript" src="what_time_is_it.js"></script>
```
그리고 `body 태그`안에는 버튼을 추가해야 한다.

```html
<button onclick="whatTimeIsIt()">현재시간</button>
```