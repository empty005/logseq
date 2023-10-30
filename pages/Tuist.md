- https://docs.tuist.io/tutorial/get-started/
- 알아야 하는 이론과 [[Xcode]]에 대한 개념
	- https://baegteun.tistory.com/2
-
- ## 계기
	- Tuist는 기존 멋쟁이 사자처럼에서 진행하는 2개의 대규모 프로젝트를 진행한 적이 있다.
	  그 과정 중에서 내가 사실상 2번의 PM을 담당 하였는데 Git 관련한 처리 및 관리를 수행하느라 많은 시간을 들였다.
	  그 중 가장 많은 시간을 들이도록 만든 파일은 xcodeproj / xcworkspace 에 대한 설정이 추가되고, 기타 GoogleInfo.plist 파일이라던지, Info.plist 파일 등이 누락되면서 중복된 파일도 올라가게 되었다. 따라서 이 파일들이 새로 다운받거나, 폴더 생성, 같은 파일이여도 새로 다운로드 받는 경우에는 파일의 Hash 값이 다르거나 등 여러 문제들이 충돌을 일으켰다. 이 과정에서 실수로 병합하는 경우에는,, git reset 해서 다시 시도 하기 일쑤 였다.
	- 이를 해결하기 위해 여러 자료를 찾아보았는데 눈에 띄인 사항이 Tuist 였다.
-
- ## 한계
	- 실제로 Tuist를 적용하려고 하였으나 앱 내의 NaverNMap, GeoFire 의 기능을 활용하고 싶은 CocoaPod 파일을 설치해야 했습니다. Tuist는 swift Package Manager까지는 적용이 가능하도록 되어 있지만, Cocoapod을 설치 할 경우 지원을 하지 않다보니, 해당 기능을 적용하기에는 더 많은 조사와 시간이 들어가야 했습니다.
	- 그리고 Tuist 를 쓰는 장점으로는 모듈화와 프로젝트 관리 측면에서 Conflict이 일어나지 않는 부분 이였는데,
	  모듈화 적용을 구상하였지만 이에 대한 러닝 커브가 높아서 적용 시도를 포기하였습니다. 그부분이 가장 아쉬움으로 남습니다. 다음 프로젝트 시작이나 기간에는 꼭 적용을 목표로 하고 싶습니다.
-
-
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
	- https://baegteun.tistory.com/2
-
-
- ##  설정
	- curl -Ls https://install.tuist.io | bash
	  logseq.order-list-type:: number
	- mkdir Grew && cd Grew (폴더 생성 + 현 위치 변경)
	  logseq.order-list-type:: number
	- ``tuist init --platform ios --template swiftui`` 실행
	  logseq.order-list-type:: number
	- 이후에 프로젝트 파일 싹 다 삭제 한 이후에 Project+Templates 파일에 수정 (테스트는 포함하지 않아서 일단 주석 처리)
	  logseq.order-list-type:: number
	  collapsed:: true
		- ```swift
		  
		  import ProjectDescription
		  
		  public extension Project {
		      static func makeModule(
		          name: String,
		          platform: Platform = .iOS,
		          product: Product,
		          organizationName: String = "Grew",
		          packages: [Package] = [],
		          deploymentTarget: DeploymentTarget? = .iOS(targetVersion: "17.0", devices: [.iphone, .ipad]),
		          dependencies: [TargetDependency] = [],
		          sources: SourceFilesList = ["Sources/**"],
		          resources: ResourceFileElements? = nil,
		          infoPlist: InfoPlist = .default
		      ) -> Project {
		          let settings: Settings = .settings(
		              base: ["ENABLE_USER_SCRIPT_SANDBOXING":"YES"],
		              configurations: [
		                  .debug(name: .debug),
		                  .release(name: .release)
		              ], defaultSettings: .recommended)
		  
		          let appTarget = Target(
		              name: name,
		              platform: platform,
		              product: product,
		              bundleId: "\(organizationName).\(name)",
		              deploymentTarget: deploymentTarget,
		              infoPlist: infoPlist,
		              sources: sources,
		              resources: resources,
		              dependencies: dependencies
		          )
		  
		  //         let testTarget = Target(
		  //            name: "\(name)Tests",
		  //            platform: platform,
		  //            product: .unitTests,
		  //            bundleId: "\(organizationName).\(name)Tests",
		  //            deploymentTarget: deploymentTarget,
		  //            infoPlist: .default,
		  //            sources: ["Tests/**"],
		  //            dependencies: [.target(name: name)]
		  //        )
		  
		          let schemes: [Scheme] = [.makeScheme(target: .debug, name: name)]
		  
		          let targets: [Target] = [appTarget]
		  //        let targets: [Target] = [appTarget, testTarget]
		  
		          return Project(
		              name: name,
		              organizationName: organizationName,
		              packages: packages,
		              settings: settings,
		              targets: targets,
		              schemes: schemes
		          )
		      }
		  }
		  
		  extension Scheme {
		      static func makeScheme(target: ConfigurationName, name: String) -> Scheme {
		          return Scheme(
		              name: name,
		              shared: true,
		              buildAction: .buildAction(targets: ["\(name)"]),
		  //            testAction: .targets(
		  //                ["\(name)Tests"],
		  //                configuration: target,
		  //                options: .options(coverage: true, codeCoverageTargets: ["\(name)"])
		  //            ),
		              runAction: .runAction(configuration: target),
		              archiveAction: .archiveAction(configuration: target),
		              profileAction: .profileAction(configuration: target),
		              analyzeAction: .analyzeAction(configuration: target)
		          )
		      }
		  }
		  
		  ```
	- 여기에 설정해야 하는 값에 대한 정리
	  logseq.order-list-type:: number
		- SPM 패키지 종속 처리 (Facebook Login / Kakao Login / Google Login / 검색 엔진 처리를 위한 Algolia) 추가
		  logseq.order-list-type:: number
		- cocoaPods 설치 및 진행 (NaverMaps)
		  logseq.order-list-type:: number
		- SwiftLint 적용을 위한 run Scripts 설정
		  logseq.order-list-type:: number
		- build Setting에 Enable_Sandbox : Yes로 설정
		  logseq.order-list-type:: number
		- Info.plist 값 (네이버 지도설정을 위한 Client 값 기입) 설정 + Secrets.plist 값 사용 시에 설정
		  logseq.order-list-type:: number
	- logseq.order-list-type:: number
-
- ## 삭제
	- curl -Ls https://raw.githubusercontent.com/tuist/tuist/main/script/uninstall | bash
-
-
- https://leeari95.tistory.com/74