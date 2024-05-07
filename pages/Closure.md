## 클로저
	- *클로저는* 코드에서 전달하고 사용할 수 있는 자체 포함된 기능 블록입니다. Swift의 클로저는 다른 프로그래밍 언어의 클로저, 익명 함수, 람다 및 블록과 유사합니다.
	- 실제로 클로저의 특별한 경우입니다. 클로저는 세 가지 형식 중 하나를 취합니다.
		- 전역 함수는 이름이 있고 어떤 값도 캡처하지 않는 클로저입니다.
		- 중첩 함수는 이름이 있고 둘러싸는 함수에서 값을 캡처할 수 있는 클로저입니다.
		- 클로저 표현식은 주변 컨텍스트에서 값을 캡처할 수 있는 경량 구문으로 작성된 이름 없는 클로저입니다.
	- Swift의 클로저 표현식은 일반 시나리오에서 간결하고 복잡하지 않은 구문을 장려하는 최적화를 통해 깔끔하고 명확한 스타일을 가지고 있습니다. 이러한 최적화에는 다음이 포함됩니다.
		- 컨텍스트에서 매개변수 및 반환 값 유형 추론
		- 단일 표현식 클로저의 암시적 반환
		- 단축 인수 이름
		- 후행 클로저 구문
- ## 클로저 표현식
	- ### 정렬 메서드에 사용된 방법
		- 스위프트의 표준화된 라이브러리는 `sorted` 라는 메서드를 제공한다. 해당 메서드는 알려진 타입의 값들의 배열을 정렬하는데, 제공된 정렬 클로저의 결과값을 토대로 정렬한다. 
		  일단 정렬 처리가 완료되면, `sorted(by:)` 메서드는 이전의 배열과 같은 타입과 크기의 정렬된 새로운 배열을 반환한다. 기존의 배열은 `sorted(by:)` 메서드에 의해 변형되지 않는다.
		- 클로저 표현식 예시는 `sorted(by:)` 문자열 값들의 알파벳 역순으로 배열을 정렬한다.
	- ### 클로저 표현식 문법
		- ```swift
		  { (<#parameters#>) -> <#return type#> in
		     <#statements#>
		  }
		  ```
	- ### 문맥에서의 타입 추론
		- ```swift
		  let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
		  
		  reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
		  ```
	- ### 단일 표현식 클로저에서의 암시적 반환
		- 단일 표현식 클로저는 암시적으로 return 이라는 키워드를 생략하며, 단일 표현식의 결과 값을 반환 할 수 있다.
		- ```swift
		  reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
		  ```
		- 왜냐하면 클로저 내부에 단일 표현식의 결과 값은 `Bool` 값을 반환하기 때문에, 애매함이 없어 지기 때문에 `return` 키워드가 생략 될 수 있다.
	- ### 줄임 매개변수명 사용
		- 변수나 아규먼트로 들어온 사항은 $0, $1로 네이밍을 생략하여 사용 할 수 있다. 
		  해당 이름이나 가공이 필요 없고 그에 대한 처리를 진행하여 단순 비교 할 경우에는 사용이 가능하다.
	- ### 연산자 방법
		- 더 극단적인 방법으로 단일 값의 경우 String이라는 타입에서의 연산자를 사용하며 비교가 가능하여 단일한 값, 다른 경우의 수가 제공되지 않는 경우의 연산자를 사용할 경우에 단순 연산자만으로도 표현식을 생략 할 수 있다.
- ## 후행 클로저
	- 사용 이유 : 함수의 마지막 매개변수거나, 해당 클로저 표현식이 너무 길다면, 후행 클로저를 사용하여 보다 편리하게 작성 할 수 있다.
	- ```swift
	  func someFunctionThatTakesAClosure(closure: () -> Void) {
	      // function body goes here
	  }
	  // Here's how you call this function without using a trailing closure:
	  
	  someFunctionThatTakesAClosure(closure: {
	      // closure's body goes here
	  })
	  // Here's how you call this function with a trailing closure instead:
	  
	  someFunctionThatTakesAClosure() {
	      // trailing closure's body goes here
	  }
	  ```
	- 메모
		- 딕셔너리의 첨자 호출 뒤에는 느낌표(!)가 붙습니다. 딕셔너리는 키가 없으면 딕셔너리의 값 조회가 실패할 수 있음을 나타내는 옵셔널 값을 반환하기 때문입니다. 
		  예시에서는 항상 사전에 유효한 첨자 키임을 보장 하므로 느낌표를 사용하여 결과 값에 대한 선택적 반환 값에 저장된 값을 강제로 래핑 해제합니다.
- ## 값 캡처
	- 클로저는 상수과 변수가 선언된 주변의 문맥으로부터 값을 캡쳐 할 수 있다. 
	  클로저는 그리고 그 상수와 변수를 클로저 표현식 본문 안에 연관 짓고 값을 변형 할 수 있고, 선언된 기존 범위를 벗어나 상수 변수가 존재하지 않더라도 가능하다.
	- 스위프트에서는 값을 캡쳐할 수 있는 가장 쉬운 형태의 클로저는 하나의 함수의 선언 부에 쓰여진 중첩 함수이다. 중첩 함수는 외부 함수 범위 안에 선언된 어떠한 상수, 변수들도 캡쳐가 가능하다.
	- 아래 예시를 보고 확인해 보자.
	- ```swift
	  func makeIncrementer(forIncrement amount: Int) -> () -> Int {
	      var runningTotal = 0
	      func incrementer() -> Int {
	          runningTotal += amount
	          return runningTotal
	      }
	      return incrementer
	  }
	  ```
	- `makeIncrementer`의 반환 타입은 `() -> Int` 형태이다. 이것은 함수를 반환 하는 것을 의미하고, 이 함수는 파라미터 없이 Int를 반환해주는 함수이다.
	- `makeIncrementer` 함수는 현재 실행되는 incrementer 의 총합을 저장하기 위한 정수형 변수 `runningTotal`을 선언하였다. 이 변수는 0의 값으로 초기화 되었다.
	- `makeIncrementer`함수는 amount 라는 이름의 매개변수와 forIncrement 라는 아규먼트 라벨을 단 정수 반환 함수를 뜻한다. 이 함수는 중첩 함수 (incrementer 실제 증감 처리를 하는)를 선언한 모양을 띈다. 이 함수는 단순히 runningTotal에 amount를 더하고 결과를 반환한다.
	- 내부의 incrementer 함수는 외부의 makeIncrementer의 매개변수인 amount에 대해서는 알지 못한다. 또한 runningTotal 값도 makeIncrementer에서 선언 된 값이다.
	- 이것이 주변 함수 (연관된 함수)로부터 온 `runningTotal` 과 `amount`의 주소값(연관값)을 캡쳐 했다는 것을 의미한다. 연관 값을 캡쳐하는 것은 `runningTotal` 과 `amount` 값이 사라지지 않도록 보장한다. `makeIncrementer` 함수가 끝날 때 까지. 그리고 `runningTotal` 이 다음에 incrementer 함수가 불려서 사용 될 경우도 보장한다.
	- ```swift
	  let incrementByTen = makeIncrementer(forIncrement: 10)
	  
	  incrementByTen()
	  // returns a value of 10
	  incrementByTen()
	  // returns a value of 20
	  incrementByTen()
	  // returns a value of 30
	  
	  let incrementBySeven = makeIncrementer(forIncrement: 7)
	  incrementBySeven()
	  // returns a value of 7
	  ```
	- ### 메모
		- 최적화로서 Swift는 값이 클로저에 의해 변경되지 않고 클로저가 생성된 후 값이 변경되지 않는 경우 값의 *복사본 을 캡처하고 저장할 수 있습니다.*
		- Swift는 또한 더 이상 필요하지 않은 변수 폐기와 관련된 모든 메모리 관리를 처리합니다.
		- 클래스 인스턴스의 속성에 클로저를 할당하고 클로저가 인스턴스나 해당 멤버를 참조하여 해당 인스턴스를 캡처하는 경우 클로저와 인스턴스 사이에 강력한 참조 순환이 생성됩니다. Swift는 이러한 강력한 참조 순환을 깨기 위해 *캡처 목록을* 사용합니다. 자세한 내용은 [클로저에 대한 강력한 참조 순환을](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/automaticreferencecounting#Strong-Reference-Cycles-for-Closures) 참조하세요
		  
		  -> 이 부분을 설명 해주실 테니 잘 정리 해야 할 것.
- ## 클로저는 참조 타입
	- 위의 예제를 통해 `incrementByTen` 과 `incrementBySeven`는 상수인데, 클로저 상수는 여전히 내부의 캡쳐된 runningTotal 값의 중가 할 수 있다. 왜냐하면 클로저와 함수는 **참조 타입** 이기 때문이다.
	- ```swift
	  let alsoIncrementByTen = incrementByTen
	  alsoIncrementByTen()
	  // returns a value of 50
	  
	  
	  incrementByTen()
	  // returns a value of 60
	  ```
	- 이 예시는 incrementByTen 과 이름만 다른 alsoIncrementByTen을 사용 했기에 기존에 캡쳐된  `runningTotal` 값을 활용하여 기존의 합계에 더 해지는 형식을 가지게 된다.
- ## 이스케이핑 클로저
	- 클로저는 함수에 인수로 전달될 때 함수를 *이스케이프* 한다고 말하지만 함수가 반환된 후에 호출됩니다. 클로저를 매개변수 중 하나로 취하는 함수를 선언할 때, `@escaping`매개변수의 유형 앞에 클로저의 이스케이프가 허용됨을 나타내는 내용을 쓸 수 있습니다.
	- 클로저를 이스케이프할 수 있는 한 가지 방법은 함수 외부에 정의된 변수에 저장하는 것입니다. 예를 들어, 비동기 작업을 시작하는 많은 함수는 클로저 인수를 완료 핸들러로 사용합니다. 함수는 작업을 시작한 후에 반환하지만 작업이 완료될 때까지 클로저가 호출되지 않습니다. 클로저는 나중에 호출하려면 이스케이프해야 합니다. 
	- 예를 들어)
	- ```swift
	  var completionHandlers: [() -> Void] = []
	  func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
	      completionHandlers.append(completionHandler)
	  }
	  ```
	- 클래스의 인스턴스를 참조하는 경우 이스케이프 클로저에 `self` 키워드 사용의 고려가 필요합니다.  이스케이프 클로저를 self로 캡쳐 할 경우 강한 순환 참조가 발생하기 쉽기 떄문이다.
	- 일반적으로 클로저 캡쳐 변수들은 암시적으로 클로저 내부에서 사용 되는데, 그러나 이런 경우에는 명확히 선언 할 필요가 있다. 만약 `self` 를 캡쳐 하기를 원한다면, 사용 시에 `self`를 명시적으로 작성하거나, 클로저의 캡쳐 리스트에 `self`를 포함시켜야 한다. `self` 키워드를 명시적으로 작성하는 것은 순환 참조가 발생하지 않는 것을 확인 시켜주는 의도이자 표현이다. 
	  예를 들어, 하단의 코드에서 클로저 `someFunctionWithEscapingClosure`는 `self`를 명시적으로 
	   For example, in the code below, the closure passed to `someFunctionWithEscapingClosure(_:)` refers to `self` explicitly. In contrast, the closure passed to `someFunctionWithNonescapingClosure(_:)` is a nonescaping closure, which means it can refer to `self` implicitly.
	- ```swift
	  var completionHandlers: [() -> Void] = []
	  func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
	      completionHandlers.append(completionHandler)
	  }
	  
	  func someFunctionWithNonescapingClosure(closure: () -> Void) {
	      closure()
	  }
	  
	  class SomeClass {
	      var x = 10
	      func doSomething() {
	          someFunctionWithEscapingClosure { self.x = 100 }
	          someFunctionWithNonescapingClosure { x = 200 }
	      }
	  }
	  
	  let instance = SomeClass()
	  instance.doSomething()
	  print(instance.x)
	  // Prints "200"
	  
	  completionHandlers.first?()
	  print(instance.x)
	  // Prints "100"
	  
	  class SomeOtherClass {
	      var x = 10
	      func doSomething() {
	          someFunctionWithEscapingClosure { [self] in x = 100 }
	          someFunctionWithNonescapingClosure { x = 200 }
	      }
	  }
	  
	  struct SomeStruct {
	      var x = 10
	      mutating func doSomething() {
	          someFunctionWithNonescapingClosure { x = 200 }  // Ok
	          someFunctionWithEscapingClosure { x = 100 }     // Error
	      }
	  }
	  ```
	- 구조체나 열거형의 인스턴스에서의 `self`라면 항상 암시적으로 `self`를 참조 할 수 있다. 그러나 이스케이핑 클로저는 mutable한 참조 (왜? -> 값을 복사하면 저장하는 공간과 값이 달라지기 때문에) 를 챕쳐 할 수 없다. 구조체와 열거형은 공유된 가변성을 허용하지 않기 때문이다.
	-
- ## AutoClosure
	- AutoClosure는 함수에 인수로 전달되는 표현식을 래핑하기 위해 자동으로 생성되는 클로저입니다. 인수를 사용하지 않으며, 호출되면 그 안에 포함된 표현식의 값을 반환합니다. 이러한 구문상의 편리함을 통해 명시적 클로저 대신 일반 표현식을 작성하여 함수 매개변수 주위의 중괄호를 생략할 수 있습니다. 
	  걍 비추네.. 그냥 명시적이지 않고 중갈호의 실행문 없이 진행 된다는 건데 PASS~~~