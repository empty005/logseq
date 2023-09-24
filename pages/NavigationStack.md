- 왜 기존의 NavigationView가 이제 Deprecated 되고 NavigationStack을 사용하는가?
- 꽤 많은 부분이 다르다 라고 이야기 하고 있다.
- 출처 : SwiftUI Navigation API. Udemy
- ## 사용법
	- NavigationStack 안에 NavigationLink로 사용 (간단하게 기존의 사용 방법대로 )
	  logseq.order-list-type:: number
	  ```swift
	  NavigationLink(destination, label) 
	  ```
	- NavigationLink(titleKey, value) + .navigationDestination 활용  
	  logseq.order-list-type:: number
	  ```swift
	  NavigationLink(titleKey, value) 사용법
	  
	   NavigationLink("DetailView", value: 99)
	       .navigationDestination(for: Int.self) { value in
	               DetailView(value: value)
	        }
	   NavigationLink("CustomerView", value: "c")
	                  .navigationDestination(for: String.self) { value in
	                      CustomerView(name: value)
	                  }
	  ```
	- navigationDestination 를 사용할 때 중요한 사항..
	  logseq.order-list-type:: number
	  ```swift
	  VStack{
	        NavigationLink("DetailView", value: 99)
	        NavigationLink("CustomerView", value: "dayexx")
	  }
	  .navigationDestination(for: Int.self) { value in
	  	DetailsView(value: value)
	  }
	  .navigationDestination(for: Int.self) { value in
	  	DetailView(value: value)
	  }
	  이렇게 사용 될 경우 for에 동알한 값이 들어가면 맨 위에인
	  DetailsView가 실행됨
	  ```
	  이게 뒤에 value 값의 형태에 따라서 네비게이션을 보내주는 것을 설정 할 수 있다.
	  여기에 들어가는 값은 Hashable 프로토콜을 따라야 하며 그에 대한 값을 전달 받아서 줄 수 있다. 매번 navigation 마다 힘들게 하지 않아도 작동하며 동일한 타입이 오는 경우에는 먼저 작성된 navigationDestination 이 실행된다.
	- 3번을 활용하여 Customer 라는 객체를 생성해서 Hashable 프로토콜을 채택하여 사용하면 사용 가능하다 라는 예시를 보여 주었으며, 추가적으로 navigationDestination을 활용하면 최상단의 리스트 같은 반복적인 생성 이후에 해당 속성 값을 따라가기 때문에 이를 해결하여 빠르게 적용이 가능하다.
	  logseq.order-list-type:: number
	  ```swift
	  List() {
	  	ForEach(customers) { customer in
	  		NavigationLink(customer.name, value: customer)
	  	}
	  }
	  .navigationDestination(for: Customer.self) { value in
	  	CustomerView(customer: value)
	  }
	  ```
	  * NavigationDestination을 반복해서 사용하도록 만들지 마라.
	- NavigationStack 의 좋은 점 : path 와 root 기능을 활용하면 정말로 간편해진다.
	  logseq.order-list-type:: number
	  
	  이유 :
	  ```swift
	  @State private var numbers: [Int] = []
	      
	  var body: some View {
	    //numbers로 바인딩 한 다음에 해당 배열에다가 차곡차곡 쌓으면 쌓인다.
	    //추가적으로 버튼이 뉼렸을때 뒤로 올 경우에 페이지에서 자동적으로 삭제도
	    //진행한다
	    //여기 패스 배열에서 99를 가지고 있다가 보낸다.
	    NavigationStack(path: $numbers) {
	      VStack {
	        Button("Navigation") {
	          //99를 추가한다.
	          numbers.append(99)
	        }
	      }
	      //99가 들어온걸 알고 navigation 한다. 어디로?
	      //Text(99) 페이지로
	      .navigationDestination(for: Int.self) { value in
	  		Text("\(value)")
	  	}
	    }
	  }
	  ```
	- routes 라우팅 할때 enum으로 활용도 가능하다!
	  logseq.order-list-type:: number
	- 추가적으로 Global Injection을 통해서 App 파일에 주입 시킨 다음에 EnvironmentObject 형태로 해당 뷰를 만들거나 Navigation 시킬 수 있다!
	  logseq.order-list-type:: number
-
- ## [[NavigationPath]]
-