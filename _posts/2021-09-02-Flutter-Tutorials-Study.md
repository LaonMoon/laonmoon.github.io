---
title: "[Flutter] Get started > Write your first app"
author: LaonMoon
date: 2021-09-03 14:54:00 +/-TTTT
categories: [Study,Flutter]
tags: [Flutter]
---

### 정리가 잘 된 튜토리얼 문서
- [첫 Flutter 앱 작성 1부](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt1#0)
- [첫 Flutter 앱 작성 2부](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2#0)

### Write your first Flutter app. [part 1](https://flutter.dev/docs/get-started/codelab) 학습할 내용
- Created a Flutter app from the ground up. : Flutter 앱을 처음부터 만들었습니다.
- Written Dart code. : Dart 코드를 작성했습니다.
- Leveraged an external, third-party library. : 외부 서드파티 라이브러리를 활용했습니다.
- Used hot reload for a faster development cycle. : 빠른 개발 사이클을 위해 hot reload를 사용했습니다.
- Implemented a stateful widget. : Stateful 위젯을 적용했습니다.
- Created a lazily loaded, infinite scrolling list. : Lazy한 방식 무한 스크롤 목록을 만들었습니다.

### Write your first Flutter app. [part 2](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2#0) 학습할 내용
- iOS, Android, 웹에서 자연스럽게 보이는 Flutter 앱을 작성하는 방법
- 핫 리로드를 사용한 개발 주기 단축 방법
- 스테이트풀(Stateful) 위젯에 상호작용을 추가하는 방법
- 두 번째 화면을 만들어 화면으로 이동하는 방법
- 테마를 사용하여 앱의 모양을 변경하는 방법

### 첫 Flutter 앱 작성 1부

일단 환경설정은 [여기](https://flutter-ko.dev/docs/get-started/test-drive?tab=vscode)부터 시작한다. 저번에 했던 예제지만 기억이 나지 않아 다시 진행해보려 한다.

다시 설정부터 했다.
[공식 문서](https://flutter-ko.dev/docs/get-started/install/windows)는 이곳에서 봤고, 이전에 [정리해둔 글](https://laonmoon.tistory.com/32?category=913697) 덕에 문제없이 할 수 있었다. 정말 기록이 중요한 것 같다.

그리고 어찌어찌 실행을 시키고 [hello world 띄우기](https://flutter.dev/docs/get-started/codelab)를 하려고 F5를 눌렀는데... 애뮬레이터가 크롬으로 떴다. 이전에는 스마트폰 형태의 애뮬레이터가 떴는데 생소했다. 근데 나름 이것도 괜찮은듯...? 일단 진행해보기로 했다.

더 읽어보니 web으로도 컴파일이 가능하고, 그래서 일단 chrome device가 자동으로 시작되어 chrome에서 켜진다고 한다.
>Every Flutter app you create also compiles for the web. In your IDE under the devices pulldown, or at the command line using flutter devices, you should now see Chrome and Web server listed. The Chrome device automatically starts Chrome. The Web server starts a server that hosts the app so that you can load it from any browser. Use the Chrome device during development so that you can use DevTools, and the web server when you want to test on other browsers.

+) 아무래도 vs code의 아랫부분의 창을 이용해서 폰 모양을 띄울 수 있는 것 같다. 근데 저번에도 느꼈지만 앱보다는 폰 자체가 버벅거려서 오히려 지금은 크롬이 나을지도 모르겠다. 아무래도 다시 install을 하던가 해야 할 듯...? 일단 기다려 보자.

문서가 영어로 되어 있는데 읽히기는 하지만 역시나 익숙하지는 않다. 인터넷을 할 때는 쓱쓱 읽어나가는게 습관이 되었는지 읽는 속도가 느려지니 왠지 불편하다. -> 그러고보니 한글 문서가 있었다... **[한글 버전](https://flutter-ko.dev/docs/get-started/codelab)은 이곳, [영어 버전](https://flutter.dev/docs/get-started/codelab)은 이곳이다.**

* While viewing the pubspec.yaml file in Android Studio’s editor view, click **flutter pub get**.

문서 하단에 보니 디버깅 모드에서 버벅거리는 일이 있을 수 있다고 한다.

>So far you’ve been running your app in debug mode. Debug mode trades performance for useful developer features such as hot reload and step debugging. It’s not unexpected to see slow performance and janky animations in debug mode.

앱이 준비된다면 따로 테스트 해볼 수 있다. [Flutters build modes](https://flutter.dev/docs/testing/build-modes)를 참고하자. 그리고 
[App size](https://flutter.dev/docs/perf/app-size)에 대해서도 알아볼 수 있다. 이런 것들은 나중에 앱을 완성하고 돌아와서 다시 읽어보자.

>Once you are ready to analyze performance or release your app, you’ll want to use Flutter’s “profile” or “release” build modes. For more details, see Flutter’s build modes.

### 첫 Flutter 앱 작성 2부
[여기](https://codelabs.developers.google.com/codelabs/first-flutter-app-pt2#3)부터 시작하면 된다.

* Flutter의 반응형 스타일 프레임워크에서 setState()를 호출하면 State 객체의 build() 메서드 호출이 트리거되어 UI가 업데이트됩니다.
* 일부 위젯 속성은 단일 위젯(child)을 구성하고, 일부 속성(예: action)은 대괄호([])로 표시된 여러 위젯(children)을 구성합니다.
* 선택한 항목 일부를 즐겨찾기에 추가하고 앱 바에서 목록 아이콘을 탭합니다. 즐겨찾기가 포함된 새 경로가 표시됩니다. 탐색기가 앱 바에 '뒤로' 버튼을 추가합니다. Navigator.pop을 명시적으로 구현할 필요가 없었습니다. '뒤로' 버튼을 탭하면 홈 경로로 돌아갑니다.

#### 테마 변경
MyApp 클래스에서 색상을 변경한다.
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

+) 아직은 뭐가 뭔지 잘 감이 안오지만 공부를 더 하면 알게 되겠지? 일단 앱을 완성을 해보자. 자, 이제 시작이다!







