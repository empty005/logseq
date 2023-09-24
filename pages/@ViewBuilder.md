- #+BEGIN_QUOTE
  정의 : **Closure에서 View를 구성하는 custom parameter attribute**
  #+END_QUOTE
- 뭔소린지 모르겠지만 "**Closure에서 (Child) View를 구성한다**"만 알면 된다.
-
- # 사용 1. @ViewBuiler Prameter
  HStack, VStack은 많이 쓰고 봤을것이다.
  
  var body: some View {
    HStack {
        Text("Cha_nyeong")
        Text("Cha_nyeong")
    }
  }
  
  보통 이런식으로 쓸텐데, 저 HStack을 보면 Closure안에!!!!!! View들을 넣어주고 있다.
  
  HStack의 생성자는 다음과 같이 생겼는데,
- ```swift
  @inlinable public init(alignment: VerticalAlignment = .center,
                           spacing: CGFloat? = nil,
                           @ViewBuilder content: () -> Content)
  ```
- @ViewBuilder content 파라미터(Closure타입)가 있는 것을 볼 수 있다.
  
  다시 ViewBuilder의 정의를 보자.
-
- **" Closure에서 View를 구성하는 custom parameter attribute "**
-
- 위 HStack 예제에서 봤듯이, @ViewBuilder 파라미터를 두어 View를 구성할 수 있는 나만의 View(?)를 만들 수 있다.
-
- ```swift 
  
  struct ContentView: View {
         
    var body: some View {
        HStack {
            Text("Zedd")
            Text("Zedd")
        }
        .background(Color.purple)
    }
  }
  
  ```
-
- HStack이긴 HStack인데, 맨날 background color를 명시적으로 넣어줘야한다고 가정해보자. 
  
  Color를 받아 자동으로 background를 넣어주는 HStack을 만들어보자.
-
- ```swift
  struct ZeddHStack<Content>: View where Content: View {
    
    let content: () -> Content
    let color: Color
    
    init(color: Color = .clear,
         @ViewBuilder content: @escaping () -> Content) {
        self.color = color
        self.content = content
    }
  
    var body: some View {
        HStack {
            content()
        }
        .background(color)
    }
  }
  그럴 때 이렇게 @ViewBuilder 파라미터를 사용하여 child view를 생성할 수 있도록 해주면 된다. 
  
  struct ContentView: View {
                
    var body: some View {
        ZeddHStack(color: .purple) {
            Text("Zedd")
            Text("Zedd")
        }
    }
  }
  ```
-
- 이 방법은 공통적인 부모 Container를 만드는데 유용하게 쓸 수 있다. (HStack, VStack같이)
  
  
  
  만약 Swift 5.4 이상을 사용하고, 딱히 생성자가 필요없다면
  ```swift
  struct ZeddHStack<Content>: View where Content: View {
  
  @ViewBuilder let content: Content
  
  var body: some View {
      HStack {
          content
      }.background(Color.purple)
  }
  }
  ```
- 생성자를 안만들고 content를 @ViewBuilder로 mark하여 사용해도 된다.
  
  이렇게 @ViewBuilder파라미터를 사용하여 해당 Closure가 여러 Child View를 제공할 수 있도록 할 수 있다.
-
- ## @ViewBuilder computed property, Method
	- 기본적으로 ViewBuilder로 유추(infer)하지 않기 때문에 @ViewBuilder를 명시적으로 넣어줘야한다.
-