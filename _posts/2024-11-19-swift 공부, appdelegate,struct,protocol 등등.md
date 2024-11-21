---
key:
title: 'whisky 코드를 파헤쳐서 swift를 공부해보자-1'
excerpt: 'swift 정리합니다.'
tags: [swift]
---

swift 앱에 대해 클론코딩을 하기 위해서 whisky라는 앱의 소스코드를 참고하여 작성합니다, 어떠한 이윤을 창출할 목적이 아닙니다.

일단 시작부분부터 볼거다.

```swift
@main
struct WhiskyApp: App {
    @State var showSetup: Bool = false
    @NSApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    @Environment(\.openURL) var openURL
    private let updaterController: SPUStandardUpdaterController

    init() {
        updaterController = SPUStandardUpdaterController(startingUpdater: true,
                                                         updaterDelegate: nil,
                                                         userDriverDelegate: nil)
    }
```

먼저 whiskyapp.swift 라는 앱의 시작점부터 살펴볼려고 한다.

@main은 이 구조체가 앱의 모든 시작점이라고 알리는듯하다,

그 후 strct가 나오는데, 이름은 whiskyapp, 여기서 App_은 App프로토콜을 따른다는 의미다, 여기서 프로토콜이란?

Swift에서 프로토콜은 일종의 설계도나 계약서 같은 것입니다

"이런 기능들은 반드시 구현해야 해요"라고 정의해놓은 것

```swift
struct WhiskyApp: App {  // App 프로토콜을 따르는 WhiskyApp
    var body: some Scene {  // 반드시 구현해야 하는 body
        WindowGroup {  // 앱의 창 그룹
            ContentView()  // 실제 보여질 내용
        }
    }
}
```

집을 지을 때 건축 설계도가 필요하듯이

앱을 만들 때는 App 프로토콜이라는 설계도를 따라 만듭니다

이 설계도는 "이 앱은 이런 기본 구조를 가져야 해요"라고 알려줍니다

즉 앱을 만들때, 만드는 사람에게 도움을 주기위해, 일종의 설계도에 따라 만들기 쉬우라고 주는거같다.

그렇다면 프로토콜중 app프로토콜은?

앱의 생명주기 관리 (시작, 종료 등)

앱의 기본 창과 화면 구성 정의

앱의 상태와 데이터 관리

시스템 이벤트 처리

이러한 것들이 특징인듯 하다.

```swift
@State var showSetup: Bool = false
```

@state 는 이 값이 변하면 화면을 다시 그려야 한다는 표시, 리엑트에서 state랑 비슷한듯 하다

showsetup이라는걸로 보아 설정창을 키고 끄는 로직인듯 하다, 즉 처음엔 false로 설정창이 꺼져있다, 저게 ture로 변하면 화면이 다시 그려지면서 설정창이 켜진다.

```swift
@NSApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
```

AppDelegate는

macOS 앱에서 필요한 이벤트 처리자를 연결

예를 들어 "앱이 시작될 때", "앱이 종료될 때" 같은 상황을 처리

macOS 앱에서는 AppDelegate라는 것이 앱의 생명주기와 이벤트를 관리합니다

SwiftUI는 새로운 프레임워크이고, 기존의 AppKit(macOS의 UI 프레임워크)과 연결이 필요합니다

@NSApplicationDelegateAdaptor는 이 두 세계를 연결해주는 다리 역할을 합니다

그래서 구문 분석하면 @NSApplicationDelegateAdaptor: 속성 래퍼(Property Wrapper)

(AppDelegate.self): 사용할 AppDelegate 클래스 지정

실제로 돌아가는걸 상상하면

```swift
class AppDelegate: NSObject, NSApplicationDelegate {
    // 앱이 시작될 때
    func applicationDidFinishLaunching(_ notification: Notification) {
        // 초기화 코드
    }
    
    // 앱이 종료될 때
    func applicationWillTerminate(_ notification: Notification) {
        // 정리 코드
    }
    
    // 다른 앱에서 파일을 열 때
    func application(_ application: NSApplication, open urls: [URL]) {
        // 파일 처리 코드
    }
}
```

이런 식으로 일일히 구현할 게 아니라 이런걸 사용하면 돠는 듯하다.

실제 사용 예시

앱이 백그라운드로 갈 때 특정 작업 수행

시스템 이벤트(예: 절전 모드) 감지

다른 앱과의 통신

파일 드래그 앤 드롭 처리

메뉴바 아이템 관리 같은걸 알아서 처리해주는 듯 하다.

```swift
@Environment(\.openURL) var openURL
```

\- 시스템의 URL 열기 기능을 사용하기 위한 변수

웹사이트나 다른 앱을 열 때 사용

그냥 앱에서 무슨 버튼 클릭하면 웹페이지 열거나 뭐 그런거에 쓰는듯?

```swift
private let updaterController: SPUStandardUpdaterController
```

앱 업데이트를 관리하는 컨트롤러

private은 "이 변수는 이 구조체 안에서만 사용할 거예요"라는 의미

let은 한 번 설정하면 바꿀 수 없는 상수를 의미

즉 이 구조체 안에서만 사용한다는 의미니까, 앱이 켜지는 순간에만 업데이트컨트롤러, 즉 업데이트 한다거나 하는게 켜진다는 듯하다.

```swift
init() {
    updaterController = SPUStandardUpdaterController(startingUpdater: true,
                                                     updaterDelegate: nil,
                                                     userDriverDelegate: nil)
}
```

init()은 구조체가 만들어질 때 실행되는 초기화 코드

여기서는 업데이트 컨트롤러를 설정

```swift
import SwiftUI
import Sparkle
import WhiskyKit
```

불러오기로 sparkle이라는걸 했는데

​	Sparkle 프레임워크

macOS 앱을 위한 자동 업데이트 프레임워크입니다

많은 macOS 앱들이 이 프레임워크를 사용해 업데이트를 관리합니다

앱스토어 외부에서 배포되는 앱들의 업데이트에 주로 사용됩니다

2. SPUStandardUpdaterController의 역할

새 버전 확인

업데이트 다운로드

설치 과정 관리

사용자에게 업데이트 알림

그래서?

```swift
SPUStandardUpdaterController(
    startingUpdater: true,     // 시작할 때 자동으로 업데이터 시작
    updaterDelegate: nil,      // 업데이트 과정을 커스터마이즈할 때 사용
    userDriverDelegate: nil    // 사용자 인터페이스 커스터마이즈할 때 사용
)
```

```swift
// 메뉴에서 업데이트 확인 버튼을 눌렀을 때
Button("Check for Updates...") {
    Task {
        await updateVM.checkUpdates()
    }
}
```

서버에서 업데이트 정보 확인

새 버전이 있으면 사용자에게 알림

사용자가 동의하면 업데이트 다운로드 및 설치 이렇게 돌아가는 듯 하다.

graph LR
    A[앱 시작] --> B[업데이트 확인]
    B --> C{새 버전 있음?}
    C -->|Yes| D[사용자에게 알림]
    D --> E[다운로드]
    E --> F[설치]
    C -->|No| G[종료]

이런식으로 돌아간다고 생각하면 될 듯하다.

