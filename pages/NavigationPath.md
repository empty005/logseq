- https://developer.apple.com/documentation/swiftui/navigationpath
- 이를 활용하는 방법
	- ```swift
	  enum Route: Hashable {
	      case view1
	      case view2
	      case view3
	      case view4
	      case view5
	  } //와 같이 라우팅에 대한 페이지를 뷰로 처리함
	  
	  
	  class NavigationState: ObservableObject {
	      
	      @Published var routes: [Route] = []
	      
	    // 원하는 뷰까지 팝 해서 가져오기
	      func popTo(_ route: Route) {
	          guard let index = routes.firstIndex(of: route) else { return }
	          routes = Array(routes[0...index])
	      }
	      
	    // 루트뷰로 오는 방법 
	      func popToRoot() {
	          routes.removeLast(routes.count)
	      }
	  } //네비게이션 State를 확인하는 사항을 ObservableObject로 사용
	  
	  
	  NavigationStack(path: $navigationState.routes) {
	                  View1()
	                      .navigationDestination(for: Route.self) { route in
	                          switch route {
	                              case .view1:
	                                  View1()
	                              case .view2:
	                                  View2()
	                              case .view3:
	                                  View3()
	                              case .view4:
	                                  View4()
	                              case .view5:
	                                  View5() 
	                          }
	                      }
	              }
	              .environmentObject(navigationState)
	  
	  //Injection과 함께 Destination 선언을 활용하여 넣어두고 나서
	  ```
-
-