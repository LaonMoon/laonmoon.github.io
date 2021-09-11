---
title: "[tutorApp] 크롤링, DB, 서버, 앱... 그리고 firebase?"
author: LaonMoon
date: 2021-09-06 20:11:00 +/-TTTT
categories: [Development,tutoring App]
tags: [tutoring App]
---

### [생각의 흐름 정리]
1. 나름대로 크롤링 코드는 완성했다.
2. 이 코드를 클라우드 서버에 올릴 것인가? 올린다면 aws lambda?
3. 크롤링한 db를 쌓는다면 바로 firebase를 이용할 수 있는가? 아니면 django를 거쳐야 하는가?

-> 일단 flutter와 db연결. 이것만 하면 일단 완성인 것 같고.

그 뒤에 firebase와 aws를 연결하면 될 것 같다.

그리고 flutter와 django가 같은 형식의 db를 사용하는 것 같다.(sqlite) 그래서 장고로 크롤링 해오는 데이터를 flutter에 띄우는 형식이면 좋을 것 같다.

그래서 지금 생각하는 순서는

1. 크롤링 데이터 장고에 받아오기,
2. 데이터 flutter에 리스트로 띄우기 이다.

**+)210906 밤 10시 추가 : [이 분](https://softwaree.tistory.com/74)과 [이 분](https://beomi.github.io/beomi.github.io_old/python/2017/02/28/HowToMakeWebCrawler-Save-with-Django.html)의 도움을 받아 크롤링 데이터를 장고로 넘기는데 성공했다!**

사실 중간에 그대로 따라했음에도 막히는 부분이 있어서 잠시 답답했는데, 알고보니 함수를 잘 설정해놓고, 넘기는 값을 안 구현했던 것! 그래서 당연히 db로 추가되지 않았고, add_new_items(parse_blog()) 이 부분을 추가하고 나서야 드디어 db로 연결을 시킬 수 있었다. 일단 남은 건 

1. 장고에서 flutter로 읽어오기
2. 다시 flutter Listview 만들기!
+) 장고와 firebase 연결? 그럼 flutter와 firebase 연결?

일단은 db에서 flutter로 연결되는 과정을 완성하고, 그 db를 쌓는 위치를 aws로 옮기든 할 생각이다. 

음... 내 생각으로는 django의 데이터 형식이 sqlite이니까 flutter와도 바로 연결할 수 있지 않을까? flutter에서 데이터를 어디서 어떻게 읽어오는지 파악하면 실마리를 찾을 수 있을 것 같다.

+) 210907 밤 12시 반 추가 : sqlite에 잘 저장된거 맞나? 왜 DB Browser for sqlite로 보는데 못 찾겠지...?

### **[자료 정리]**
**[Django&크롤링]**
1. [나만의 웹 크롤러 만들기(4): Django로 크롤링한 데이터 저장하기](https://beomi.github.io/2017/03/01/HowToMakeWebCrawler-Save-with-Django/) + [이 글](https://beomi.github.io/beomi.github.io_old/python/2017/02/28/HowToMakeWebCrawler-Save-with-Django.html)은 똑같은 건데 설명이 조금 다름.
2. [파이썬으로 웹페이지 크롤링을 해보자. (1) Django 프로젝트 만들기](https://softwaree.tistory.com/74)

**[Django&DB]**
1. [django.6 가상환경 내에서 data 집어넣기](https://codermun-log.tistory.com/132)
2. [1. Django 모델 (Model)](http://pythonstudy.xyz/python/article/308-Django-%EB%AA%A8%EB%8D%B8-Model)

**[Django 자료]**
1. [Django-Tube를 만들어봅시다!](https://djangogirlsseoul.gitbooks.io/django-tube/content/)
2. [장고 (Django) [Django] 프로젝트 구조 설정 (앱, 템플릿, Static 파일 등)](https://it-eldorado.tistory.com/59)
3. [주니어 개발자의 Django ORM 수난기](https://daeguowl.tistory.com/171)
4. [점프 투 장고](https://wikidocs.net/70650)

**[AWS&크롤링]**
1. [AWS Lambda로 크롤링 서버를 만들어보자!](https://medium.com/@kyh980909/aws-lambda%EB%A1%9C-%ED%81%AC%EB%A1%A4%EB%A7%81-%EC%84%9C%EB%B2%84%EB%A5%BC-%EB%A7%8C%EB%93%A4%EC%96%B4%EB%B3%B4%EC%9E%90-4c249580150a)
2. [aws 웹 크롤링 알림 봇](https://steemit.com/kr/@sifnax/python-6-aws)
3. [Ruby on Jets : AWS Lambda에서 Selenium 크롤링](https://kbs4674.tistory.com/108)
4. [내맘대로 뉴스 요약 큐레이션 (2) - AWS Lambda 로 크롤링 자동화하기](https://inahjeon.github.io/devlog/side%20project/2020/05/24/news2.html)
5. [AWS Lambda로 크롤링 데이터 Firebase에 데이터 쌓는 법 (Lambda 설정)- 1편](https://firework-ham.tistory.com/112?category=868006)
6. [ETF 크롤링 데이터를 구글 클라우드 데이터 베이스(Firestore)에 업로드하기](https://izy.codes/etf-crawling-google-firestore/)

**[텔레그렘 api 이용]**
1. [[Python] 파이썬 웹크롤링봇 만들기 -5- Telegram API를 통한 새글 알리미 만들기](https://steemit.com/kr/@sifnax/python-5-telegram-api)
2. [웹페이지 업데이트를 알려주는 Telegram 봇](https://beomi.github.io/gb-crawling/posts/2017-04-20-HowToMakeWebCrawler-Notice-with-Telegram.html)

**[AWS 관련]**
1. [모바일 앱 서비스를 위한 AWS 서버 비용](https://cholol.tistory.com/516)

**[Django&AWS]**
1. [서버개발자가 되는법 [1] - 서버 개발환경 셋팅, AWS EC2만들고 Django 프로젝트 실행해보기](https://cholol.tistory.com/484)

**[Linux Crontab]**
1. [리눅스 크론탭(Linux Crontab) 사용법](https://jdm.kr/blog/2)

**[웹/앱 제작 -> 배포까지]**
1. [Fast api로 머신러닝 기반 웹사이트 만들고 배포하기](https://aimb.tistory.com/212])
2. [Projects/[2021][개인] 코드악보 공유APP](https://chinpa.tistory.com/category/Projects/%5B2021%5D%5B%EA%B0%9C%EC%9D%B8%5D%20%EC%BD%94%EB%93%9C%EC%95%85%EB%B3%B4%20%EA%B3%B5%EC%9C%A0APP)

**[Flutter&DB연결]**
1. [Flutter로 ToDo앱 클라우드DB로 연결하기 (feat. Firebase)](https://kyungsnim.net/86)
2. [[FLUTTER/Firebase]  Firestore 연동 데이터 추가 및 관리 코드](https://seizemymoment.tistory.com/9)
3. [Projects/[2021][개인] 코드악보 공유APP 9. 플러터로 프로젝트 이전 (2) - 플러터(flutter) 와 DB 연결하여](https://chinpa.tistory.com/82)
4. [Flutter - 10. TodoList App (3) Database를 연동해보자 (SQLite, sqflite](https://doitddo.tistory.com/121)
5. [Flutter Database (SQLite) 사용하기 (1)](https://dalgonakit.tistory.com/116)

**[Flutter&Firebase]**
1. [[Flutter] Flutter 앱에 Firebase 연동법](https://velog.io/@epdlqlem14/Flutter-Flutter-%EC%95%B1%EC%97%90-Firebase-%EC%97%B0%EB%8F%99%EB%B2%95)
2. [Flutter and Firebase - 플러터와 파이어베이스 - firebase에서 특정 문서 가져 오기](https://www.python2.net/questions-793316.htm)

**[Flutter&푸시알림]**
1. [Flutter - Node js - FCM으로 서버에서 푸시 보내보기](https://padro.tistory.com/192)

**[DB 관련]**
1. [카카오톡 학식봇 만들기-json 파일이란?](https://tre2man.tistory.com/159)

**[Flutter&Django]**
1. [(플러터와 장고로 1시간만에 퀴즈 앱/서버 만들기 [무작정 풀스택]](https://www.inflearn.com/course/%ED%94%8C%EB%9F%AC%ED%84%B0-%EC%9E%A5%EA%B3%A0-%ED%80%B4%EC%A6%88%EC%95%B1-%EC%84%9C%EB%B2%84-%ED%92%80%EC%8A%A4%ED%83%9D)

**[그 외]**
1. [REST API 제대로 알고 사용하기](https://meetup.toast.com/posts/92)

**[Flutter 화면 구성]**
1. [Flutter 강좌 - 탭 사용법 | Work with tabs](https://here4you.tistory.com/125)
2. [Flutter 갤러리 - 예제들!](https://gallery.flutter.dev/#/)