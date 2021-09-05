---
title: "[Flutter] 튜토리얼 클론 코딩 2부"
author: LaonMoon
date: 2021-09-05 09:11:00 +/-TTTT
categories: [Study,Flutter]
tags: [Flutter]
---

### **2부 ver1. 일단 답 점검도 하며 피드백**
[여기 부터(4. 목록에 아이콘 추가)](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2/#3) 시작하면 된다.

- ### **1. 목록에 하트 아이콘 추가하기**
이 단계에서는 각 행에 하트 아이콘을 추가합니다. 

- `_RandomWordsState`에 `_saved Set`를 추가합니다. 이 Set에는 사용자가 즐겨찾기에 추가한 단어 쌍이 저장됩니다. 

- 올바르게 구현된 Set는 중복 항목을 허용하지 않으므로 List보다 Set를 사용합니다.

```dart
class _RandomWordsState extends State<RandomWords> {
  final _suggestions = <WordPair>[];
  final _saved = <WordPair>{};     // NEW
  final _biggerFont = TextStyle(fontSize: 18.0);
```
`<WordPair>` 뒤의 모양이 다르다. 아무래도 []는 list이고, {}는 set을 의미하는 듯 하다. 즉, _saved라는 이름을 갖는 wordpair set을 만든 것!

- `_buildRow 함수`에 `alreadySaved 확인`을 추가하여 단어 쌍이 이미 즐겨찾기에 추가되어 있지 않은지 확인합니다.

```dart
Widget _buildRow(WordPair pair) {
  final alreadySaved = _saved.contains(pair);  // NEW
  ...
}
```
pair를 _saved가 contain하고 있는지를 물어보는 것 같다. 리턴 값은 true or false의 형식일까?

[이 글](http://work6.mrds.kr/bbs/board.php?bo_table=flutter&wr_id=17&page=5)을 참고해보자. 그러면 앱을 만들 때 굳이 따로 조건문을 만들지 않아도 앱 자체에서도 처리를 할 수 있을 것으로 보인다.

-` _buildRow()`에서 `ListTile 객체`에 하트 모양의 아이콘을 추가하여 즐겨찾기를 사용 설정할 수도 있습니다. 다음 단계에서는 하트 아이콘을 사용해 상호작용하는 기능을 추가합니다.

```dart
Widget _buildRow(WordPair pair) {
  final alreadySaved = _saved.contains(pair);
  return ListTile(
    title: Text(
      pair.asPascalCase,
      style: _biggerFont,
    ),
    trailing: Icon(   // NEW from here...
      alreadySaved ? Icons.favorite : Icons.favorite_border,
      color: alreadySaved ? Colors.red : null,
    ),                // ... to here.
  );
}
```
이렇게 하면 빈 하트 아이콘이 생기고, alreadySaved?를 통해 이미 클릭이 되었는지를 판별하게 되는 것 같다. 하트를 누르면 _saved set에 저장이 될 듯하다.

```dart
Widget _buildRow(WordPair pair) {
    return ListTile(
      title: Text(
        pair.asPascalCase,
        style: _biggerFont,
      ),
      trailing: Icon(
        // NEW from here...
        Icons.favorite,
        color: Colors.red,
      ),
    );
  }
}
```
이런 식으로 _saved와 관련 없이 아이콘만 띄우도록 해보았는데, 이렇게 하면 하트 아이콘은 뜨지만 이미 빨간색이 채워져 있는 하트가 생성된다.

```dart
Icons.star,
color: Colors.yellow,
```
이런 식으로 바꾸면 노란 색이 채워진 별을 만들 수도 있다. Icon에 대한 추가적인 설명은 [이 곳](https://api.flutter.dev/flutter/widgets/Icon-class.html)에서 볼 수 있다.

```dart
trailing: Icon(   // NEW from here...
      alreadySaved ? Icons.favorite : Icons.favorite_border,
      color: alreadySaved ? Colors.red : null,
    ),  
```

위의 코드에서 '?'의 의미가 궁금해서 찾아보니, 객체가 null 인 경우 충돌하지 않고 null을 반환하며 해당 속성 또는 메서드가 호출되지 않는다고 한다.([참고](https://www.python2.net/questions-315320.htm))

Q. 그렇다면 alreadySaved? 라고 작성하는 것은 null일 경우를 대비해서 적어놓는 것이라 치면, 만일 null 값이 아니라면 영향을 미친다는 것 같은데 Icons.favorite : Icons.favorite_border의 경우에는 alreadySaved 값에 상관없이 있어야 하는 것이 아닌가?
```dart
Icons.favorite_border,
color: alreadySaved ? Colors.red : null,
```
그래서 이런 식으로 작성해도 되지 않을까?

-> 그런데 이게 클릭해서 색이 사라지고 이런 개념이 아니라 그냥 빈 하트와 색이 있는 하트를 왔다갔다 하는 거라면 빈 하트의 경우에도 alreadySaved와 연결되는 무언가가 있어야 할 것 같기는 하다.

하지만 똑같은 형식으로 alreadySaved ? 라고 되어 있는데 차이점을 두려면... 아하 Colors.red 뒤에 null 이라고 표시함으로서 서로 대조되는 상황을 갖도록 한 것 같다. 

그러면 alreadySaved에 있는 단어를 클릭했다면 빈 하트가 나와야 하고, 없는 단어를 클릭했다면 red color가 되어야 한다. 즉, alreadySaved 값이 null이라면 Colors.red : null이 적용이 되는 것 같다.

*Q. Icons.favorite : Icons.favorite_border 의 경우에는 왜 :로 작성을 했을까? 그냥 바로 Icons.favorite_border만 적용하는 건 안 맞는 건가?*

- ### **2. 상호작용 추가하기 - 하트 아이콘 탭 할 수 있도록**
그 다음은 [상호작용 추가하기](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2/#4)이다. 이 단계에서는 하트 아이콘을 탭할 수 있게 만들고 즐겨찾기를 저장한다.

- 사용자가 목록의 항목을 탭하여 즐겨찾기 상태를 전환하면 이 단어 쌍이 일련의 저장된 즐겨찾기에서 삭제되거나 추가됩니다.
- 이렇게 하려면 `_buildRow 함수`를 수정합니다. 단어가 이미 즐겨찾기에 추가되어 있는 경우 단어를 다시 탭하면 즐겨찾기에서 삭제됩니다. 타일을 탭하면 함수는 `setState()`를 호출하여 상태가 변경되었음을 프레임워크에 알립니다.

* setState(), 이것도 내가 만들고자 하는 앱에 꼭 필요한 요소 인것 같다. [이런 예시](https://ichi.pro/ko/peulleoteo-gandanhan-setstate-leul-sayonghayeo-jangbaguni-yeje-jagseong-181204910852562)도 있으니 참고하기! 

-> StatefulWidget은 state를 포함하며, setState 가 발생할때마다 다시 Build 과정이 일어난다. 때문에, 동적 화면을 쉽게 구현이 가능하다.([출처](https://pks2974.medium.com/flutter-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-9532e16aff57))

-> Flutter의 반응형 스타일 프레임워크에서 setState()를 호출하면 State 객체의 build() 메서드 호출이 트리거되어 UI가 업데이트됩니다.

* 우연히 찾긴 했는데 [이 글](https://medium.com/@dan_kim/%EB%B2%88%EC%97%AD-flutter-%EC%9C%84%EC%A0%AF-%EC%82%AC%EC%9A%A9%ED%95%B4%EB%B3%B4%EA%B8%B0-1a22231d25c6)도 플러터의 전반적인 그림을 이해하는데 유용해 보인다.

```dart
onTap: () {      // NEW lines from here...
      setState(() {
        if (alreadySaved) {
          _saved.remove(pair);
        } else {
          _saved.add(pair);
        }
      });
    },
```

- ### **3. 버튼 눌러 새 페이지로 이동**
[6. 새 화면으로 이동](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2/#5)을 참고하면 된다.

이 단계에서는 즐겨찾기를 표시하는 새 페이지(Flutter에서는 경로(route)라고 함)를 추가합니다. 홈 경로와 새로운 경로 간에 탐색하는 방법을 알아봅니다.

Flutter에서 **Navigator**는 **앱의 경로가 포함된 스택을 관리**합니다. 경로를 Navigator의 스택에 푸시하면 디스플레이가 이 경로로 업데이트됩니다. Navigator의 스택에서 경로를 표시하면 디스플레이가 이전 경로로 반환됩니다.

  - ### **3-1. appbar의 버튼 만들기**
`_RandomWordsState`의 `build 메서드`에서 `AppBar`에 목록 아이콘을 추가합니다. **사용자가 목록 아이콘을 클릭하면 저장된 즐겨찾기가 포함된 새 경로가 Navigator에 푸시되고 아이콘이 표시됩니다.**

```dart
actions: [
          IconButton(icon: Icon(Icons.list), onPressed: _pushSaved),
        ],
```
Build의 AppBar 안에 이 코드가 작성되어야 한다. 앱 바에 버튼을 만드는 것이기 때문에. 그리고 _pushSaved에 대해 정의를 해주어야 한다.

_RandomWordsState 클래스에 _pushSaved() 함수를 추가하라고 되어 있지만, 나는 AppBar를 옮기지 않은 상태이기 때문에 MyApp 클래스에 다음의 코드를 작성해 보겠다.

```dart
void _pushSaved() {
  }
```
이렇게 하면 앱 바의 오른쪽에 아이콘이 나타난다. 아직은 _pushSaved 함수가 비어 있기 때문에 아이콘을 탭해도 아직 아무런 동작도 일어나지 않는다.

  - ### **3-2. 새 페이지로 이동하기**

**경로를 빌드하여 Navigator의 스택에 푸시**합니다. 이 작업을 하면 화면이 변경되어 새 경로가 표시됩니다. 새 페이지의 콘텐츠는 익명 함수 내 **MaterialPageRoute의 builder 속성에 내장되어 있습니다.**

아래와 같이 Navigator.push를 호출하여 경로를 탐색기의 스택에 푸시합니다. IDE에서 유효하지 않은 코드를 경고하지만 다음 섹션에서 코드를 수정할 것입니다.

```dart
void _pushSaved() {
  Navigator.of(context).push(
  );
}
```
다음으로, **MaterialPageRoute 및 빌더를 추가**합니다. 지금은 ListTile 행을 생성하는 코드를 추가합니다. ListTile의 divideTiles() 메서드는 각 ListTile 사이에 가로 간격을 추가합니다. divided 변수는 편의 함수 toList()가 목록으로 변환한 최종 행을 보유합니다.

```dart
  void _pushSaved() {
    Navigator.of(context).push(
      MaterialPageRoute<void>(
        // NEW lines from here...
        builder: (BuildContext context) {
          final tiles = _saved.map(
            (WordPair pair) {
              return ListTile(
                title: Text(
                  pair.asPascalCase,
                  style: _biggerFont,
                ),
              );
            },
          );
          final divided = ListTile.divideTiles(
            context: context,
            tiles: tiles,
          ).toList();

          return Scaffold(
            appBar: AppBar(
              title: Text('Saved Suggestions'),
            ),
            body: ListView(children: divided),
          );
        }, // ...to here.
      ),
    );
  }
```
앱 바의 리스트 아이콘을 클릭한 후에 이동한 페이지에도 즐겨찾기 한 목록의 리스트를 보여주어야 하기 때문에 listTile을 설정하는 코드인 것 같다.

builder 속성은 SavedSuggestions라는 새 경로의 앱 바가 포함된 Scaffold를 반환합니다. 새 경로의 본문은 ListTiles 행을 포함하는 ListView로 구성됩니다. 각 행은 구분선으로 구분됩니다.

- ### **4. 테마를 사용하여 UI 변경(배경색 바꾸기)**
[7. 테마를 사용하여 UI 변경](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2/#6)의 단계이다.

이 단계에서는 앱 테마를 수정합니다. 테마는 앱의 디자인을 제어합니다. 실제 기기 또는 에뮬레이터에 따라 기본 테마를 사용하거나 브랜딩을 반영하도록 테마를 맞춤설정할 수 있습니다.

ThemeData 클래스를 구성하여 앱의 테마를 손쉽게 변경할 수 있습니다. 앱에서 기본 테마가 사용되지만 앱의 기본 색상을 흰색으로 변경하겠습니다.

MyApp 클래스에서 색상을 변경할 수 있다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Startup Name Generator',
      theme: ThemeData(          // Add the 3 lines from here...
        primaryColor: Colors.white,
      ),                         // ... to here.
      home: RandomWords(),
    );
  }
}
```

### **2부 ver2. 최대한 도움 안 받고 코드 작성하기**

- ### **1. 목록에 하트 아이콘 추가하기**
  
- ### **2. 상호작용 추가하기 - 하트 아이콘 탭 할 수 있도록**

- ### **3. 버튼 눌러 새 페이지로 이동**

  - ### **3-1. appbar의 버튼 만들기**

  - ### **3-2. 새 페이지로 이동하기**

- ### **4. 테마를 사용하여 UI 변경(배경색 바꾸기)**