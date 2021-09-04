---
title: "[Flutter] 튜토리얼 클론 코딩 1부"
author: LaonMoon
date: 2021-09-05 01:11:00 +/-TTTT
categories: [Study,Flutter]
tags: [Flutter]
---

## 공식 튜토리얼 클론 코딩
한 번 쭉 따라 완성을 해 봤으니 일단 코드를 작성해보는 연습을 해보려 한다. 결국 아무 생각 없이 따라하기만 하면 그건 정말 복사 붙여넣기 연습이 될테니까.

물론 내가 코드를 외워서 다 따라한다고 해도 끝이 나는 게 아님을 기억하기. 그 의미도 잘 알아야 한다.

### **1부 ver1. 일단 구글링과 함께 만들어보기+답 점검도 하며 피드백**
원래 처음은 구글링과 함께 코드를 작성하려 했지만, 현재 작성하는 코드가 튜토리얼인 특성상 정리된 글들이 너무 많아서 이 과정은 생략하기로 했다.
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

여기부턴 [이 곳](https://flutter.dev/docs/get-started/codelab)을 다시 참고했다. 설명은 이곳이 더 친절하게 나와있는 것 같다.

Stateless widgets are `immutable`, meaning that their properties can’t change—all values are final.
> Stateless 위젯은 `변경 불가능`-> 즉, 속성을 변경할 수 없으며 모든 값이 최종이다.

Stateful widgets maintain state that might change during the lifetime of the widget. stateful widget을 구현하기 위해서는 적어도 **two classes**가 필요하다:
1) a `StatefulWidget class` that creates an instance of a State class.
2) a `State class`.

The StatefulWidget class is, itself, immutable and can be thrown away and regenerated, but the State class persists over the lifetime of the widget.
> Stateful 위젯은 위젯의 전체 기간 동안 변경될 수도 있는 상태를 유지합니다. Stateful 위젯을 구현하려면 클래스가 두 개 이상 필요합니다. 첫 번째는 State 클래스의 인스턴스를 만드는 StatefulWidget입니다. StatefulWidget 객체 자체는 변경 불가능하며 삭제 후 다시 생성할 수 있지만, State 객체는 위젯의 전체 기간보다 오래 지속됩니다.

이 단계에서는 `RandomWords`라는 stateful widget을 추가한다. RandomWords, which creates its State class, `_RandomWordsState`. You’ll then use RandomWords as a child inside the existing MyApp stateless widget.
> 이 단계에서는 State 클래스인 _RandomWordsState를 만드는 Stateful 위젯 RandomWords를 추가합니다. 그런 다음 기존의 MyApp Stateless 위젯 내의 하위 요소로 RandomWords를 사용합니다.

- typing **`stful`**. The editor asks if you want to create a `Stateful widget`. The boilerplate code for two classes appears, and the cursor is positioned for you to enter the name of your stateful widget.

Enter RandomWords as the name of your widget. The RandomWords widget does little else beside creating its State class.
> 아래 코드에서 볼 수 있듯이 RandomWords 위젯은 State 클래스를 만드는 것 외에는 다른 작업을 거의 하지 않습니다.

Once you’ve entered RandomWords as the name of the stateful widget, the IDE automatically updates the accompanying State class, naming it _RandomWordsState. By default, the name of the State class is prefixed with an underbar. Prefixing an identifier with an underscore enforces privacy in the Dart language and is a recommended best practice for State objects.
> 스테이트풀(Stateful) 위젯 이름으로 RandomWords를 입력한 후에는 IDE에서 동반 State 클래스가 자동으로 업데이트되어 이름이 _RandomWordsState로 지정됩니다. 기본적으로 State 클래스의 이름에는 앞에 밑줄이 붙습니다. 식별자 앞에 밑줄을 사용하면 Dart 언어에서 개인 정보 보호가 적용되며 State 객체에 관해서는 이 표기를 사용하는 것이 좋습니다.

The IDE also automatically updates the state class to extend State<RandomWords>, indicating that you’re using a generic State class specialized for use with RandomWords. Most of the app’s logic resides here—it maintains the state for the RandomWords widget. This class saves the list of generated word pairs, which grows infinitely as the user scrolls.
> 또한 IDE는 상태 클래스를 자동으로 업데이트하여 State<RandomWords>를 확장합니다. 이는 RandomWords 용도로 특화된 일반적인 State 클래스를 사용하고 있음을 나타냅니다. 이 클래스에는 앱의 로직 대부분이 보관되며⁠ RandomWords 위젯의 상태가 유지됩니다. 이 클래스는 생성된 단어 쌍의 목록을 저장합니다. 단어 쌍은 사용자가 스크롤함에 따라 무제한으로 증가합니다.

  - ### **3-1. Stateful Widget 실습**

```dart
import 'package:flutter/material.dart';
import 'package:english_words/english_words.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
        title: 'use stateful widget',
        home: Scaffold(
            appBar: AppBar(title: const Text('This AppBar')),
            body: Center(
              child: RandomWords(),
            )));
  }
}

class RandomWords extends StatefulWidget {
  const RandomWords({Key? key}) : super(key: key);

  @override
  _RandomWordsState createState() => _RandomWordsState();
}

class _RandomWordsState extends State<RandomWords> {
  @override
  Widget build(BuildContext context) {
    final twoWords = WordPair.random();
    return Text(twoWords.asCamelCase);
  }
}
```
* **Error 부분은 statefulWidget의 return 값을 까먹은 것이다.**

앞의 코드와 달라진 점은 크게 없다. 추가적으로 `stful`을 입력하면 statefulWidget을 만들기 위한 틀이 자동으로 뜨고, RandomWords라는 이름을 입력하면 동시에 적용된다.

그리고 기존의 statelessWidget에 있던 `final twoWords = WordPair.random()`을 statefulWidget의 build로 옮겨 오고, 그 안에 마찬가지로 `return Text(twoWords.asCamelCase)`를 해주었다. 그리고 statelessWidget의 body의 Center의 child 값을 `Text(twoWords.asCamelCase)`에서 `RandomWords()`로 바꾸어 주었다.

아무래도 단어 목록 리스트를 만들고 추가되는 형식을 사용하기 위해 statefulWidget으로 단어 생성과 관련된 요소들을 옮겨왔고, 이를 statelessWidget과 연결시킨 것 같다.

방금 그러면 이왕 다 옮겨 올거면 굳이 statelessWidget을 남겨놓을 필요가 있나?라는 생각이 들었었는데, 현재는 MyApp을 따로 제시해줄 class가 필요한 것 같다는 생각이 들었다.

그리고 `final twoWords = WordPair.random();`를 statelessWidget의 build에 옮겨봤는데 작동하지 않았다. 

*Q. Undefined name이라고 뜨는데, 꼭 같은 위치에 적어야 하는 이유가 뭘까?*

 - ### **3-2. 무한 스크롤 ListView 만들기**
이 단계에서는 `_RandomWordsState`를 확장하여 단어 쌍 목록을 생성하고 표시합니다. 사용자가 스크롤하면 목록(ListView 위젯에 표시됨)이 무제한으로 증가합니다. ListView의 builder 팩토리 생성자를 사용하면 필요할 때 목록 보기를 지연 빌드할 수 있습니다.

- 추천 단어 쌍을 저장하는 `_suggestions 목록`을 추가합니다.
- 또한 글꼴 크기를 키우기 위한 `_biggerFont 변수`를 추가합니다.

```dart
class _RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];                 // NEW
  final _biggerFont = const TextStyle(fontSize: 18); // NEW
  ...
}
```
일단은 이렇다는데 이번에는 final이 붙은 코드가 왜 build안으로 들어가지 않아도 되는걸까? build 안에 들어간다는건 build가 될 때마다 달라지는 값이기 때문인가?

```dart
class _RandomWordsState extends State<RandomWords> {
  final _biggerFont = const TextStyle(fontSize: 18);
  @override
  Widget build(BuildContext context) {
    final twoWords = WordPair.random();
    return Text(twoWords.asCamelCase, style: _biggerFont);
  }
}
```
_biggerFont를 먼저 적용해보았다. 글씨가 성공적으로 커졌다. 

Next, you’ll add a `_buildSuggestions() function` to the `_RandomWordsState class`. This method builds the ListView that displays the suggested word pairing.
>미리 적기는 했지만 `_RandomWordsState 클래스`에 `**_buildSuggestions() 함수**`를 추가할 것이다. 이 메서드는 추천 단어 쌍을 표시하는 ListView를 빌드한다.

The `ListView class` provides a builder property, itemBuilder, that’s a factory builder and callback function specified as an anonymous function.
>ListView 클래스는 익명 함수로 지정된 팩토리 빌더 및 콜백 함수인 빌더 속성 itemBuilder를 제공합니다.

Two parameters are passed to the function—the BuildContext, and the row iterator, i. The iterator begins at 0 and increments each time the function is called. It increments twice for every suggested word pairing: once for the ListTile, and once for the Divider. This model allows the suggested list to continue growing as the user scrolls.
> BuildContext와 행 반복자 i 등 두 매개변수가 함수에 전달됩니다. 이 반복자는 0부터 시작되며 함수가 호출될 때마다 추천 단어 쌍마다 한 번씩 증분됩니다. 이 모델을 사용하면 사용자가 스크롤함에 따라 추천 단어 목록이 계속 증가할 수 있습니다.

- **_buildSuggestions 함수**

_RandomWordsState 클래스에 추가한다. 일단 이 함수 같은 경우에는 dart에 익숙해지면 이해가 쉬울 것 같아 넘어가려 한다.
```dart
  Widget _buildSuggestions() {
    return ListView.builder(
      padding: const EdgeInsets.all(16),
      itemBuilder: (BuildContext _context, int i) {
        if (i.isOdd) {
          return Divider();
        }
        final int index = i ~/ 2;
        if (index >= _suggestions.length) {
          _suggestions.addAll(generateWordPairs().take(10));
        }
        return _buildRow(_suggestions[index]);
      }
    );
  }
```

`_buildSuggestions 함수`는 단어 쌍별로 한 번씩 `_buildRow`를 호출한다.(return 값 참고) 이 함수는 각각의 새 쌍을 ListTile로 표시하여 2부에서 더 유용한 행을 만드는 데 도움이 된다.

마찬가지로 `_buildRow 함수`를 `_RandomWordsState`에 추가한다.
```dart
  Widget _buildRow(WordPair pair) {
    return ListTile(
      title: Text(
        pair.asPascalCase,
        style: _biggerFont,
      ),
    );
  }
```

**ListTile에 대한 설명**은 [이 곳](https://api.flutter.dev/flutter/material/ListTile-class.html)에서 친절한 동영상과 함께 이해를 할 수 있다. [여기](https://here4you.tistory.com/137)는 ListTile을 이용해서 Listview를 만드는 예제가 있다.

flutter에서 함수를 Widget으로 정의를 하는 것 같다. 그리고 Widget build는 조금 성격이 다른 것 같기도 하고.

- _RandomWordsState.의 build 메서드를 업데이트합니다.
```dart
@override
  Widget build(BuildContext context) {
    //final wordPair = WordPair.random(); // Delete these...
    //return Text(wordPair.asPascalCase); // ... two lines.

    return Scaffold (                     // Add from here...
      appBar: AppBar(
        title: Text('Startup Name Generator'),
      ),
      body: _buildSuggestions(),
    );                                      // ... to here.
  }
```
단어 생성 라이브러리를 직접 호출하지 않고 `_buildSuggestions()`를 사용하도록 변경합니다. (Scaffold는 기본 머티리얼 디자인 시각적 레이아웃을 구현합니다.) stateless widget의 appbar를 지우지 않은 상태로 작성하면 appbar가 2개가 생긴다.

```dart
class _RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];
  final _biggerFont = const TextStyle(fontSize: 18);
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      // Add from here...
      body: _buildSuggestions(),
    );
  }
```
Scaffold의 appBar를 stateful widget 안으로 굳이 넣어야 하나?라는 생각이 들어서 appbar는 안 건드리고 이대로만 작성했는데도 잘 되는 것 같다. 

### **1부 ver2. 최대한 도움 안 받고 코드 작성하기**

- ### **Hello World 띄우기**
이 정도는 이제 별 무리 없이 작성한다. 다만 자세한 내용들은 더 알아보아야 한다.
  - import 'package/flutter/material.dart';의 의미
  - 반드시 void main() => runApp(`클래스 이름`);의 형식으로만 가능한지?
  - statefulwidget만을 사용할 수 있는지?
  - build, materialApp, Scaffold의 의미
  - @override의 의미

- ### **2. `english_words: ^4.0.0` 패키지 사용하여 단어 띄우기**
이것도 영어단어가 나오도록 고치는 건 간단한데, 한 번 씩 ; 붙이는 걸 까먹어서 오류가 나곤한다. 주의하기!

- ### **3. 무한 스크롤 ListView 만들기**
이 부분은 함수들의 경우에는 코드를 참고해야 할 것 같기는 한데, 할 수 있는데 까지는 작성해보려 한다.

- stateful Widget 만들기 [O]
- 무한 스크롤 ListView 만들기 : 이 부분은 일단 예제의 코드를 참고하였다.
- 함수 적용 [X] : 하나의 class에 여러개의 widget이 들어간다.
- widget 작성 후 최종 정리 - _RandomWordsState의 return 값 수정하기 [O]
```dart
Widget build(BuildContext context) {
    return Scaffold(
      body: _buildSuggestions(),
    );
  }
```
사실 이 부분만 수정하면 된다. 드디어 1부 완성이다!

+) 그나저나 처음에 그냥 복사 붙여넣기하며 구현만 했을 때는 감도 안왔는데, 이렇게 하나하나 작성하며 코드를 외우고 따라가다 보니 어느정도 느낌은 오는 것 같다.