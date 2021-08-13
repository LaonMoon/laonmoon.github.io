---
title: "[Django] 3. 부트스트랩 알아보기 : 까지"
author: LaonMoon
date: 2021-08-13 14:53:00 +/-TTTT
categories: [Study, Django]
tags: [Django]
---

### 04-1 부트스트랩 알아보기
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

##### **CDN** 
Contents delivery network. 사람들이 자주 사용하는 css, js 파일 등을 모아둔 곳.

-> 사실 CDN은 깃헙 블로그를 만들다가 들어본 용어다. 지금 사용하고 있는 테마를 만든 분은 이미지를 cdn 어쩌고로 연결해서 avatar를 설정해둔 것 같았는데, 나는 asset 파일에 사진을 넣고 경로를 지정하고 싶었지만 그러지 못하고 깃헙에 올려두었기 때문에 그 방법이 궁금했던 차였다.(+ 혹시나 싶어 다시 경로로 수정해봤는데 잘 된다...?)

#### 네비게이션 바 수정하기
1. Components에서 [내비게이션 바 요소](https://getbootstrap.com/docs/4.5/components/navbar/) 고르기

부트스트랩 사이트 > Documentation > Components

