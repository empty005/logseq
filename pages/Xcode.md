## [[키보드 단축키]]
-
-
- # [[Tuist]]
- 와 연계된 Xcode 개념
- Project.swift는 Tuist에서 .xcodeproj를 어떻게 만들지 정의하고 설정하는 파일
- ```swift
  public init(
      name: String,
      organizationName: String? = nil,
      options: ProjectDescription.Project.Options = .options(),
      packages: [ProjectDescription.Package] = [],
      settings: ProjectDescription.Settings? = nil,
      targets: [ProjectDescription.Target] = [],
      schemes: [ProjectDescription.Scheme] = [],
      fileHeaderTemplate: ProjectDescription.FileHeaderTemplate? = nil,
      additionalFiles: [ProjectDescription.FileElement] = [],
      resourceSynthesizers: [ProjectDescription.ResourceSynthesizer] = .default
  )
  ```
- 이중에서 가장 핵심적인 사항은 name, Organization, packages, targets, scheme 인데
- 여기서 Name은 우리가 사용하고 싶은 프로젝트의 이름을 정의하는 구간이다.
- Organization은 프로젝트를 Inspector에서 볼 수 있는 Organization을 의미한다.
- Options = Tuist가 xcodeproj 파일을 만들 때의 옵션을 설정할수 있는 구간으로 https://tuist.github.io/tuist/main/documentation/projectdescription/project/options-swift.struct/ 에서 확인 가능하다.
- Packages는 SPM의 그 패키지를 의미한다. 패키지 꾸러미를 미리 받아올수 있도록 하는 설치 사항도 값을 가지고 있다. 자세한 사항은 동일하게 https://tuist.github.io/tuist/main/documentation/projectdescription/package/ 에서 확인 가능하다.
- Settings 프로젝트 파일에 있는 Build Settings의 정보들을 설정해줄 수 있다.
	- 여기서 Xcode 빌드 세팅은 key값은 찾을수 있다.  https://xcodebuildsettings.com/
	- 세팅의 추가적인 사항은 https://tuist.github.io/tuist/main/documentation/projectdescription/settings/ 여기서 참고해보자
- Targets 프로젝트의 타겟을 의미합니다. 프로젝트 파일을 보면 있는 Targets이고 타겟을 만들어서 Array에 넣어주면 그대로 만들어줍니다.
- Schemes 도 설정이 가능하다.
	- Scheme 이란? 특정 빌드 환경이 미리 정해진 하나의 collection
	- configuration 정보
		- 하드웨어, 아키텍쳐 정보
		- 등등
	- 일종의 설정이 포함된 상태의 상태 값들을 관리하여 Dev/Prod/Debug 등의 상태에 따라서 접속 서버, 실제 실서버 등을 관리할 수 있는 사항이다.
	- https://tuist.github.io/tuist/main/documentation/projectdescription/scheme/
- FileHeaderTemplate 는 내장 Xcode 템플릿에 Custom으로 파일 헤더를 생성할 수 있다.
	- ```
	  ex) fileHeaderTemplate: "  ___COPYRIGHT___" 
	  ```
	- 이런식으로 생성하고 난 이후에 swift파일을 생성하면
	- ```swift
	  //  Copyright © 2022 tuist.io. All rights reserved.
	  import Foundation
	  ```
	- 이렇게 카피라이트가 생긴다.
- AdditionalFiles 의 경우 Tuist에서 프로젝트를 만들 때 Xcode에 자동으로 연결해주지 않는 파일을 넣으면 프로젝트에 연결 시켜준다. ex)Readme.md 파일 같은 경우 Xcode에서 원래 보이지 않는데 여기에 추가해두면 보인다.
- ResourceSynthesizers 이걸 알기전에 Tuist에서 제공해주는 기능중 하나는 Tuist는 프로젝트를 생성할 때 Resources/안에 파일 확장자에 따라 enum을 제공해준다.
	- ex) Asset에 색상과 이미지를 넣었다면
	- ![](https://blog.kakaocdn.net/dn/xWm5L/btrJAnYFnOG/L3QW02o1Ez4rkQRJrUJPH0/img.png)
	- ```swift
	  public enum CoreAsset {
	    public enum Colors {
	      public static let grewGray = CoreColors(name: "GREW_Gray")
	      public static let grewBackground = CoreColors(name: "GREW_Background")
	      public static let grewBlack = CoreColors(name: "GREW_Black")
	      public static let grewCompetePrimary = CoreColors(name: "GREW_CompetePrimary")
	      public static let grewCompeteSecondary = CoreColors(name: "GREW_CompeteSecondary")
	      public static let grewOnboardMain = CoreColors(name: "GREW_OnboardMain")
	      public static let grewPrimary = CoreColors(name: "GREW_Primary")
	      public static let grewPrimaryTextColor = CoreColors(name: "GREW_PrimaryTextColor")
	      public static let grewSecondaryTextColor = CoreColors(name: "GREW_SecondaryTextColor")
	      public static let grewWhite = CoreColors(name: "GREW_White")
	    }
	    public enum Images {
	      public static let grewCompeteIcon = CoreImages(name: "GREW_CompeteIcon")
	      public static let grewGithubIcon = CoreImages(name: "GREW_GithubIcon")
	      public static let grewLogo = CoreImages(name: "GREW_Logo")
	      public static let grewOnboard1 = CoreImages(name: "GREW_Onboard1")
	      public static let grewOnboard2 = CoreImages(name: "GREW_Onboard2")
	      public static let grewOnboard3 = CoreImages(name: "GREW_Onboard3")
	      public static let grewOnboard4 = CoreImages(name: "GREW_Onboard4")
	      public static let grewSword = CoreImages(name: "GREW_Sword")
	    }
	  }
	  ```
	- 이렇게 열거형을 생성해준다.
	- 위에서 제공해주지 않는 확장자들이 Resources/ 에 들어가 있을때 
	  어떻게 자동 생성해줄지에 대해 .stencil파일로 정의해주면 프로젝트를 생성할 때 .stencil내용에 따라 생성해준다 
	  예시로 프로젝트에 Lottie를 쓴다면 Resources/ 에 .json파일이 들어갔을때 어떻게 자동 생성 시킬지에 대해 만들 수 있습니다.
	- .default는 strings, plists, fonts, assets를 자동으로 생성해줍니다.
	- https://tuist.github.io/tuist/main/documentation/projectdescription/resourcesynthesizer/ 참고
- ## Workspace.swift 는 Swift Workspace 설정하는 방법,
	- 이는 프로젝트를 설정하는 뭉치의 일종 공간으로써 이름과, 어떤 프로젝트가 종속되는지가 중요하다.
	- 여기선 Projects에 기본 경로는 프로젝트의 루트 경로의 하위에서 하나하나 추가 가능하다.
	- GenerationOptions는 workspace 파일을 생성할 때 옵션을 설정
- Config.swift는 프로젝트 전역으로 쓰이는 설정 값
	- *Config.swift는 프로젝트 루트 디렉토리/Tuist/Config.swiftd에 있을 때만 적용
- ## Target
	- 타겟은 사용할 모듈을 정의하는 하나의 집합체
	- name, platform, product, deploymentTarget, infoPlist, sources, resources, entitlements, scripts, dependencies
	- Platform = iOS, macOS, tvOS ... 등등을 정의
	- product는 app, appClips, staticFramework, framework, unitTest 등..
	- **bundleID - 프로젝트 파일을 열었을 때 보이는 번들 값
	- deploymentTarget - 배포 및 디플로이 시킬 타겟에 대한 버전 값
	- infoPlist - Info.plist를 정의하는데 추가적으로 key와 값을 입력함 (예를 들면 네이버 API연동을 위한 custom key 라던가 iOS 17 버전을 위한 Enable Sandbox 기능을 끄는 등 사용이 필요한 사항에 대한 설정)
	- Sources = 소스 코드의 경로를 입력 [ ] 사이에 문자열로 경로 입력도 가능
	- Entitlements = APNs 사용을 위한 push 알람 설정을 위한 경로 입력
	- scripts = SwiftLint에 들어가는 사항 의 경우 스크립트 설정한 후 입력
	- dependencies = 타겟의 의존성에 대한 사항 / 라이브러리나, 다른 모듈을 의존성으로 기입 시킬 때 사용
	- settings = Target의 세팅을 정의합니다.
	- coreDataModels = coreData 모델의 경로와 버전 설정
	- environment = scheme에서 Edit Scheme 에서 Environment Variables 설정하면 자동으로 생성합니다.
	- launchArguments = launch 시에 나오는 인자 값들에 대한 프로젝트 셋업 항목입니다.
	- additionalFiles = 프로젝트 생성시 자동으로 생겨나지 않는 파일 (ex. Readme.md 파일) 을 등록해두면 Xcode에서 볼 수 있다.
-