---
title: "[Flutter] 튜토리얼 클론 코딩"
author: LaonMoon
date: 2021-09-04 20:54:00 +/-TTTT
categories: [Study,Flutter]
tags: [Flutter]
---

## 공식 튜토리얼 클론 코딩
한 번 쭉 따라 완성을 해 봤으니 일단 코드를 작성해보는 연습을 해보려 한다. 결국 아무 생각 없이 따라하기만 하면 그건 정말 복사 붙여넣기 연습이 될테니까.

물론 내가 코드를 외워서 다 따라한다고 해도 끝이 나는 게 아님을 기억하기. 그 의미도 잘 알아야 한다.

### **1부 ver1. 일단 구글링과 함께 만들어보기+답 점검도 하며 피드백**
- ### **1. Hello World 띄우기**
  
  우선 어느정도 외워서 구성을 익혔다. 

```dart
import 'package:flutter/material.dart';

// 기본 메서드에 화살표(=>) 표기법을 사용합니다. 한 줄 함수 또는 메서드에 화살표 표기법을 사용합니다.
void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: "This is App.",
      home: Scaffold(
          appBar: AppBar(
            title: const Text("This is App Bar."),
          ),
          body: const Center(
            child: const Text("Hello World!"),
          )
      ),
    );
  }
}
  ```
  * **Error 부분은 1) MaterialApp 대신 Material 을 사용했던 것, 2) MaterialApp의 끝에 ;를 붙이지 않은 것, 3) body 안의 child를 빠뜨린 것, 4) Center()를 깜박한 것이었다.**

- 이 예에서는 머티리얼 앱을 만듭니다. [머티리얼](https://material.io/design/)은 모바일과 웹의 표준 시각 디자인 언어입니다. Flutter는 다양한 머티리얼 위젯을 제공합니다.
- 앱은 StatelessWidget을 확장하여 **앱 자체를 위젯으로** 만듭니다. Flutter에서는 정렬, 패딩, 레이아웃을 포함한 거의 모든 항목이 위젯입니다.
- 머티리얼 라이브러리의 `Scaffold 위젯`은 기본 앱 바와 제목뿐만 아니라 홈 화면의 위젯 트리를 포함하는 본문 속성도 제공합니다. 위젯 하위 트리는 상당히 복잡할 수 있습니다.
- 위젯의 기본 작업은 다른 하위 수준 위젯의 측면에서 위젯을 표시하는 방법을 설명하는 `build 메서드`를 제공하는 것입니다.
- 이 예의 본문은 Text 하위 위젯을 포함하는 Center 위젯으로 구성되어 있습니다. `Center 위젯`은 위젯 하위 트리를 화면 중앙에 맞춥니다.

- ### **2. `english_words: ^4.0.0` 패키지 사용하여 단어 띄우기**
[english_words](https://pub.dev/packages/english_words)라는 오픈소스 패키지를 사용. 이 패키지에는 가장 많이 사용되는 영어 단어 수천 개와 몇 가지 유틸리티 함수가 있다.

[pub.dev](https://pub.dev/)에서 english_words 패키지뿐만 아니라 다수의 다른 오픈소스 패키지도 찾을 수 있다.

pubspec.yaml의 flutter에 추가 -> get packages를 누른다.(다운로드 모양)

```dart
import 'package:english_words/english_words.dart';
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final wordPair = WordPair.random();
    return MaterialApp(
        title: "This is WordPair App",
        home: Scaffold(
          appBar: AppBar(
            title: const Text("This is Appbar"),
          ),
          body: Center(
            child: Text(wordPair.asCamelCase),
          )
        )
    );
  }
}
```
* **Error 부분은 body와 home을 반대로 썼던 것이다.**
* `english_words`라는 패키지의 다른 활용도 궁금해서 찾아보았다. [여기](https://pub.dev/documentation/english_words/latest/english_words/english_words-library.html)를 참고하면 될듯!
  - 간단하게는 wordPair.뒤에 asCamelCase말고도 다양한 걸 사용할 수 있다. -> asSnakeCase, aslowerCase 등
* [이 글](https://chinpa.tistory.com/75)이 정리가 되어 있는 것 같아 가져왔다.
* *Q. nouns만 나오거나 adjective만 나오도록 하고 싶은데 어떻게 할지 모르겠다. 기본적인 구성은 형용사+명사 형태로 나온다고 한다.*

- ### **3. 무한 스크롤 ListView 만들기**
이 단계에서는 stateful Widget을 추가한다. 스테이트리스(Stateless) 위젯은 변경 불가능-> 즉, 속성을 변경할 수 없으며 모든 값이 최종이다.

스테이트풀(Stateful) 위젯은 위젯의 전체 기간 동안 변경될 수도 있는 상태를 유지합니다. 스테이트풀(Stateful) 위젯을 구현하려면 클래스가 두 개 이상 필요합니다. 첫 번째는 State 클래스의 인스턴스를 만드는 StatefulWidget입니다. StatefulWidget 객체 자체는 변경 불가능하며 삭제 후 다시 생성할 수 있지만, State 객체는 위젯의 전체 기간보다 오래 지속됩니다.

이 단계에서는 State 클래스인 _RandomWordsState를 만드는 스테이트풀(Stateful) 위젯 RandomWords를 추가합니다. 그런 다음 기존의 MyApp 스테이트리스(Stateless) 위젯 내의 하위 요소로 RandomWords를 사용합니다.



### **1부 ver2. 최대한 도움 안 받고 코드 작성하기**
- ### **Hello World 띄우기**


- ### **2. `english_words: ^4.0.0` 패키지 사용하여 단어 띄우기**



- ### **3. 무한 스크롤 ListView 만들기**

### **1부 ver3. 아무 도움 안 받고 작성**
- ### **Hello World 띄우기**


- ### **2. `english_words: ^4.0.0` 패키지 사용하여 단어 띄우기**



- ### **3. 무한 스크롤 ListView 만들기**