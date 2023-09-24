- https://docs.tuist.io/tutorial/get-started/
-
- ## 계기
	- Tuist는 기존 멋쟁이 사자처럼에서 진행하는 2개의 대규모 프로젝트를 진행한 적이 있다.
	  그 과정 중에서 내가 사실상 2번의 PM을 담당 하였는데 Git 관련한 처리 및 관리를 수행하느라 많은 시간을 들였다.
	  그 중 가장 많은 시간을 들이도록 만든 파일은 xcodeproj / xcworkspace 에 대한 설정이 추가되고, 기타 GoogleInfo.plist 파일이라던지, Info.plist 파일 등이 누락되면서 중복된 파일도 올라가게 되었다. 따라서 이 파일들이 새로 다운받거나, 폴더 생성, 같은 파일이여도 새로 다운로드 받는 경우에는 파일의 Hash 값이 다르거나 등 여러 문제들이 충돌을 일으켰다. 이 과정에서 실수로 병합하는 경우에는,, git reset 해서 다시 시도 하기 일쑤 였다.
	- 이를 해결하기 위해 여러 자료를 찾아보았는데 눈에 띄인 사항이 Tuist 였다.
- ## 정의
	- **Tuist** 는 Xcode 프로젝트와의 생성, 유지 관리 및 상호 작용을 용이하게 하는 것을 목표로 하는 커맨드 라인 인터페이스(CLI, 명령줄 도구, 터미널을 통해 사용자와 컴퓨터가 상호 작용하는 방식)입니다. 바이너리로 배포되므로 종속성을 관리하기 위해 다른 도구에 의존하지 않고도 쉽게 설치하고 사용할 수 있습니다. 라고 소개 되어 있다.
- ## 원리
	- 간단히 말해서 실제로 tuist를 통해서 설정과 가장 큰 차이점으로 기대 되는 사항으로는
	  (1) **.xcodeproj**을 올리지 않게 되다보니 깃에서 충돌이 생기지 않는 사항
	  (2) 이 과정에 있어서 클린 코드 / 모듈 확장 및 수정이 편리하다.
	  (3) 모듈 플러그인, 기존 소스 설정을 가져오는 부분이 편리하다.
	  (4) 모듈 간 그래프로 (tuist graph 기능을 활용) 종속에 대한 처리를 표현이 쉽다.
-
- ## 설치
	- 1. curl -Ls https://install.tuist.io | bash
	- 2. 
	  ``mkdir Tuist
	  cd Tuist
	  tuist init --platform ios --template swiftui ``
	  (폴더 생성 -> 이동 -> tuist 생성 (옵션은 iOS)) + template swiftUI로 설정이 가능함
	- 3. 템플릿 설정 - [[stencil]]
	- https://cheonsong.tistory.com/17
	-
-
- ## 삭제
	- curl -Ls https://raw.githubusercontent.com/tuist/tuist/main/script/uninstall | bash
-
-
- https://leeari95.tistory.com/74
-