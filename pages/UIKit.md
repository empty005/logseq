- 프로젝트 생성 중 StoryBoard 없이 코드 베이스로 프로젝트를 시작하는 방법 정리
	- 프로젝트를 생성
	  logseq.order-list-type:: number
	- Main.storyboard 삭제
	  logseq.order-list-type:: number
		- 이 상태에서 실행하게 되면 오류가 발생함. (Main.storyboard가 없다고)
		  logseq.order-list-type:: number
	- info.plist 에 가서 Scene Configure - Storyboard Name : Main으로 기입된 사항을 삭제
	  logseq.order-list-type:: number
	- 프로젝트 targets 설정에서 BuildSettings - info.plist Value - UIKit Main StoryBoard File Base Name : Main 도 삭제
	  logseq.order-list-type:: number
	- 초기 View Controller (Root) 설정
	  logseq.order-list-type:: number
		- SceneDelegate.swift에서 설정이 필요.
		  logseq.order-list-type:: number
		- logseq.order-list-type:: number
		  > iOS 13 이전의 AppDelegate의 UI LifeCycle의 역할을 IOS 13부터는 SceneDelegate에게 넘겨주어, Scene(화면)과 관련된 부분을 SceneDelegate가 담당하게 되었다. iOS 13부터 SceneDelegate에서 scene을 지정해주어야합니다.
		- logseq.order-list-type:: number
		  ```swift
		  class SceneDelegate: UIResponder, UIWindowSceneDelegate {
		  
		      var window: UIWindow?
		  
		      func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
		          // guard let _ = (scene as? UIWindowScene) else { return } - 삭제
		          guard let windowScene = (scene as? UIWindowScene) else { return }
		        	//UIScreen 크기 만큼의 윈도우 생성
		          window = UIWindow(frame: UIScreen.main.bounds)
		        	//윈도우 장면을 생성
		          window?.windowScene = windowScene
		        	//해당 화면의 루트 뷰 컨트롤러
		          window?.rootViewController = ViewController()
		        	//키 윈도우로 설정하고 보이게 설정.
		          window?.makeKeyAndVisible()
		      }
		  
		  }
		  ```
	- **NavigationController를 rootViewController로 지정하고 싶을때는?**
	  logseq.order-list-type:: number
	- logseq.order-list-type:: number
	  ```swift
	      func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
	          guard let windowScene = (scene as? UIWindowScene) else { return }
	          window = UIWindow(windowScene: windowScene) // SceneDelegate의 프로퍼티에 설정해줌
	          let mainViewController = ViewController() // 맨 처음 보여줄 ViewController
	  
	  		let navigationController = UINavigationController(rootViewController : mainViewController)
	          // NavigationController에 처음으로 보여질 화면을 rootView로 지정
	          window?.rootViewController = navigationController
	          window?.makeKeyAndVisible()
	      }
	  ```
	- 그 다음 확인해주면 된다.
	  logseq.order-list-type:: number
- ## [[UITableView]]
	- 단일 열의 행을 사용하여 데이터를 표시하는 뷰.
-