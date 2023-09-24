- https://developer.apple.com/documentation/swiftui/app/
- https://developer.apple.com/documentation/swiftui/windowgroup
-
- ## WindowGroup
	- Use a WindowGroup as a container for a view hierarchy presented by your app. The hierarchy that
	  you declare as the group’s content serves as a template for each window that the app creates from
	  that group
	- 앱에서 제공하는 뷰 계층의 컨테이너로 WindowGroup을 사용하세요.
	- 그룹의 콘텐츠로 선언하는 계층 구조는 앱이 해당 그룹에서 만드는 각 창의 템플릿 역할을 합니다.
	- SwiftUI가 기본적으로 제공하는 primitive scene type 중의 하나
	- iPadOS나 macOS에서는 여러 Window를 띄울 수 있다 - 각 Scene은 개별 Window가 된다.
	- macOS에서는 Safari 브라우저 처럼 관련 창을 하나의 탭 창으로 모을 수도 있다. - 각 Scene은 개별 Tab이 된다.
	- iOS, watchOS, tvOS에서는 한 번에 하나의 전체화면 창을 띄우는 것을 선호한다.
	- Every window created from the group maintains independent state. For example, for each new
	  window created from the group the system allocates new storage for any State or StateObject
	  variables instantiated by the scene’s view hierarchy.
	- 그룹에서 생성된 모든 창은 독립적인 상태를 유지합니다.
	- 예를 들어, 그룹에서 생성된 각 새 창에 대해 시스템은 장면의 뷰 계층에 의해 인스턴스화된 상태 또는 StateObject 변
	  수에 대해 새 저장소를 할당합니다.
-
- ((64e2b067-0470-485a-a4b7-df99f6c56be7))
-