- 왜 사용하는가?
	- -> 컴파일 때 코드를 생성하기 위해 매크로를 사용한다.
	- 매크로는 소스코드를 컴파일 해서 변환 할때 반복적인 코드를 작성하지 않아도 축약된 형태의 어떠한 것을  제작하고 추가적으로 생성한다.
	- `여기서 특징은 작성한 코드가 삭제나 수정되지 않는다.`
- 매크로 입력과 매크로 확장의 출력은 Swift 코드가 유효한지 확인한다. 마찬가지로 매크로에 전달하는 값과 매크로에 의해 생성된 코드의 값이 올바른 타입인지 확인한다.
-
- ## 독립 매크로
- #+BEGIN_QUOTE
  선언에 첨부되지 않고 자체적으로 나타내는 매크로
  이름 앞에 숫자 기호 `#` 를 작성하고 이름 뒤 소괄호 안에 매크로의 인수를 작성한다.
  #+END_QUOTE
	- ```swift
	  func myFunction() {
	      print("Currently running \(#function)")
	      #warning("Something's wrong")
	  }
	  // Currently running myFunction()
	  에서 #function / #warning 과 같이 컴파일 때 동작을 수행 할 수 있다.
	  ```
	- `#function` 은 컴파일 시 이를 호출한 함수 이름을 작성해줌과 같이 함수 명을 알려줌.
	- `#warning`은 사용자 지정 컴파일 경고를 생성할 수 있다.
		- 이를 활용해서 미리 컴파일 이전에 처리해야 하는 곳에 `#error` 처리도 가능하다.
-
- ## 첨부 매크로
- #+BEGIN_QUOTE
  매크로 이름 앞에 기호 (@) 를 작성하고 매크로 이름 뒤 소괄호에 인수를 작성합니다.
  #+END_QUOTE
- 첨부 매크로는 첨부된 선언을 수정한다. 새로운 메서드를 정의하거나, 프로토콜의 준수성을 추가하는 것과 같이 해당 선언에 코드를 추가하는 형식이다.
- 아래는 매크로를 사용하지 않는 코드.
- ```swift
  struct SundaeToppings: OptionSet {
      let rawValue: Int
      static let nuts = SundaeToppings(rawValue: 1 << 0)
      static let cherry = SundaeToppings(rawValue: 1 << 1)
      static let fudge = SundaeToppings(rawValue: 1 << 2)
  }
  ```
- 이 코드에서 `sundaeToppings` 옵션 셋의 각 옵션은 반복적이고 수동적인 초기화 구문 호출을 포함합니다.
- 다음은 매크로를 사용한 버전의 코드입니다.
- ```swift
  @OptionSet<Int>
  struct SundaeToppings {
      private enum Options: Int {
          case nuts
          case cherry
          case fudge
      }
  }
  ```
- 이 버전의 `SundaeToppings`는 `@OptionSet` 매크로를 호출해서, 이 매크로는 private 열거형에 케이스의 목록을 읽고 각 옵션에 대한 상수 목록을 생성하고 `OptionSet` 프로토콜의 준수성을 추가하였습니다. 
  id:: 66398507-7a1c-47d7-b916-342d583e7936
- 이를 풀어서 쓴 macro의 경우 확인 해보면
- ```swift
  struct SundaeToppings {
      private enum Options: Int {
          case nuts
          case cherry
          case fudge
      }
      //여기부터
      typealias RawValue = Int
      var rawValue: RawValue
      init() { self.rawValue = 0 }
      init(rawValue: RawValue) { self.rawValue = rawValue }
      static let nuts: Self = Self(rawValue: 1 << Options.nuts.rawValue)
      static let cherry: Self = Self(rawValue: 1 << Options.cherry.rawValue)
      static let fudge: Self = Self(rawValue: 1 << Options.fudge.rawValue)
    	//여기까지 자동으로 매크로에서 생성해줌.
  }
  // 필요시 익스텐션까지
  extension SundaeToppings: OptionSet { }
  ```
- static 변수를 생성하기 위해 매크로를 쓰는 버전은 수동으로 작성된 코드보다 읽기 쉽고 유지보수에 편리합니다.
-
- ## 매크로의 선언
- 대부분 Swift 코드에서 함수 또는 타입을 구현 할 때 별도의 선언이 없습니다.
- 그러나, 매크로의 경우 선언과 구현은 분리되어 있습니다. 매크로의 선언은 매크로의 이름, 매크로가 가지는 파라미터, 어디서 사용될 수 있는지, 어떤 코드가 생성되는지 포함 됩니다.
-
- `macro` 키워드로 선언하게 됩니다.
- ```swift
  public macro OptionSet<RawType>() =
          #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
  ```
- 첫번째 줄은 매크로의 이름과 매크로의 인수를 지정합니다.
- 이름은 `OptionSet` 이고 인수는 가지고 있지 않으며 RawType을 받고 있습니다.
- 두번째 줄은 Swift 표준 라이브러리의 `externalMacro(module:type:)` 매크로를 사용하여 Swift에 매크로의 구현 위치를 알려줍니다. 이 경우 `@OptionSet` 매크로를 구현하는 `OptionSetMacro`를 포함합니다.
- `OptionSet`은 첨부 매크로이므로, 매크로의 이름은 구조체와 클래스 이름 처럼 `UpperCamelCase`로 사용한다
- 독립 매크로는 변수나 함수 이름처럼 `lowerCamelCase`로 사용한다.
- #+BEGIN_QUOTE
  매크로는 항상 `public`으로 선언됩니다. 매크로를 선언 하는 코드는 매크로를 사용하는 코드의 모듈과 다르므로 `public`이 아닌 매크로는 적용할 수 없습니다.
  #+END_QUOTE
- ```swift
  @attached(member)
  @attached(extension, conformances: OptionSet)
  public macro OptionSet<RawType>() =
          #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
  ```
- @attached -> 각 매크로 역할에 대해 한번씩 선언해서 두번 나타납니다.
- @attached(member)를 사용하고 매크로가 적용된 타입에 새로운 멤버를 추가한다고 나타냅니다.
- @OptionSet 매크로는 OptionSet 프로토콜에 의해 요구되는 init(rawValue:) 초기화 구문과 멤버를 추가합니다. 
  @attached(extension, conformances: OptionSet)를 사용하고 OptionSet의 프로토콜을 준수한다고 하면, @OptionSet 매크로는 적용한 타입을 확장하여 준수성을 추가한다.
- ```swift
  @freestanding(expression)
  public macro line<T: ExpressibleByIntegerLiteral>() -> T =
          /* ... location of the macro implementation... */
  ```
- 독립 매크로 선언에는 매크로의 역할을 지정하기 위해 @freeStanding 속성을 작성한다.
- 여기서 `#line` 매크로는 expression 역할을 가지는데 매크로 표현식은 값을 생성하거나 컴파일 때 경고를 생성하듯 어떠한 동작을 수행합니다.
-
- ```swift
  @attached(member, names: named(RawValue), named(rawValue),
          named(`init`), arbitrary)
  @attached(extension, conformances: OptionSet)
  public macro OptionSet<RawType>() =
          #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
  ```
- 여기서 member로 매크로는 `RawValue`, `rawValue`, 그리고 `init` 의 이름인 기호에 대한 선언을 추가합니다 - 이러한 이름은 미리 알고 있기 때문에, 명시적으로 매크로 선언에 나열합니다.
-
- ### #attached
	- 여기서 매크로 선언에 attached 속성이 있는데, 이 속성의 인수는 매크로의 역할을 나타냅니다.
	- Peer 매크로: 이 속성에 첫번째 인수로 `peer` 를 작성합니다. 이 매크로 구현 타입은 `PeerMacro` 프로토콜을 준수합니다. 이러한 매크로는 매크로가 첨부된 선언과 동일한 범위에 새로운 선언을 생성합니다. 예를 들어, 구조체의 메서드에 peer 매크로를 적용하면 해당 구조체에 추가로 메서드와 프로퍼티를 정의할 수 있습니다.
	- Member 매크로: 이 속성에 첫번째 인수로 `member` 를 작성합니다. 이 매크로 구현 타입은 `MemberMacro` 프로토콜을 준수합니다. 이러한 매크로는 매크로가 첨부된 타입 또는 확장의 멤버인 새로운 선언을 생성합니다. 예를 들어, 구조체 선언에 member 매크로를 적용하면 해당 구조체에 추가로 메서드와 프로퍼티를 정의할 수 있습니다.
	- Member 속성: 이 속성에 첫번째 인수로 `memberAttribute` 를 작성합니다. 이 매크로 구현 타입은 `MemberAttributeMacro` 프로토콜을 준수합니다. 이러한 매크로는 매크로가 첨부된 타입 또는 확장의 멤버에 속성을 추가합니다.
	- Accessor 매크로: 이 속성에 첫번째 인수로 `accessor` 를 작성합니다. 이 매크로 구현 타입은 `AccessorMacro` 프로토콜을 준수합니다. 이 매크로는 저장된 프로퍼티에 접근자를 추가하여 계산된 프로퍼티로 변경합니다.
	- Extension 매크로: 이 속성에 첫번째 인수로 `extension` 을 작성합니다. 이 타입은 `ExtensionMacro` 프로토콜을 준수하는 매크로 구현입니다. 이러한 매크로는 `where` 절로 프로토콜 준수성을 추가할 수 있고, 매크로가 첨부된 타입의 멤버인 새로운 선언을 추가할 수 있습니다. 매크로가 프로토콜 준수성을 추가하면, `conformances:` 인수를 포함하고 프로토콜을 지정합니다. 준수성 목록은 프로토콜 이름, 준수하는 목록 아이템의 타입 별칭, 또는 복합 프로토콜 준수성 목록 아이템을 포함합니다. 중첩된 타입에서 확장 매크로는 해당 파일의 최상위 레벨에서 확장합니다. 확장, 타입 별칭, 또는 함수 내에 중첩된 타입에 확장 매크로를 작성하거나 확장 매크로를 사용하여 peer 매크로가 있는 확장을 추가할 수 없습니다.
	- peer, member, 그리고 accessor 매크로 역할은 매크로가 생성하는 기호의 이름을 나열하는 `names:` 인수를 요구합니다. 매크로가 확장 내에 선언을 추가한다면 확장 매크로 역할도 `names:` 인수를 요구합니다. `names:` 인수가 매크로 선언에 포함되면, 매크로 구현은 해당 목록과 일치하는 이름의 기호로만 생성해야 합니다.
	  
	  매크로 선언이 `named:` 인수를 포함할 때, 매크로 구현은 해당 리스트에 일치하는 이름의 기호만 생성해야 합니다. 다시 말해, 매크로는 나열된 이름의 기호를 생성할 필요는 없습니다. 해당 인수의 값은 다음 중 하나 이상의 값입니다:
	- `named(<#name#>)` 여기서 *이름 (name)* 은 미리 알려진 이름에 대한 고정 기호입니다.
	- `overloaded` 기존 기호와 동일한 이름인 경우.
	- `prefixed(<#prefix#>)` 여기서 *접두사 (prefix)* 는 고정된 문자열로 시작하는 이름의 경우 기호 이름 앞에 추가됩니다.
	- `suffixed(<#suffix#>` 여기서 *접미사 (suffix)* 는 고정된 문자열로 끝나는 이름의 경우 기호 이름 뒤에 추가됩니다.
	- `arbitrary` 매크로 확장까지 이름이 결정될 수 없는 경우.
	  
	  특수한 경우로 프로퍼티 래퍼 (property wrapper) 와 유사하게 동작하는 매크로에 대해 `prefixed($)` 를 작성할 수 있습니다.
-
- ## 매크로의 확장
- ![](https://bbiguduk.gitbook.io/~gitbook/image?url=https%3A%2F%2F2352141256-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-M7Zt2HBfR67oi6QnKHI%252Fuploads%252Fgit-blob-ae7bd15c8325a1c21c0a6126349b6387f3eca413%252Fmacro-expansion-full%7Edark%25402x.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=d774e548a52faa0a7edf7e94ad5aa14cbbfec347fb534dc291b63b4a3ea0596f)
	- 특히, Swift 는 아래와 같은 방식으로 매크로를 확장합니다:
	- 컴파일러는 코드를 읽고 구문의 메모리 표현을 생성합니다.
	  logseq.order-list-type:: number
	- 컴파일러는 메모리 표현의 일부분을 매크로 구현에 전송하여 매크로를 확장합니다.
	  logseq.order-list-type:: number
	- 컴파일러는 확장된 형태로 매크로 호출을 대체합니다.
	  logseq.order-list-type:: number
	- 컴파일러는 확장된 소스 코드를 사용하여 완료될 때까지 계속 진행합니다.
	  logseq.order-list-type:: number
-
- 매크로는 AST라는 추상 구문 트리라는 메모리 표현을 활용한다.
	- 매크로를 확장하기 위해, 컴파일러는 Swift 파일을 읽고 *추상 구문 트리 (abstract syntax tree)* 또는 AST 라고 알려진 해당 코드의 메모리 표현을 생성합니다. AST 는 컴파일러나 매크로 구현과 같이 해당 구조와 상호작용하는 코드를 더 쉽게 작성하기 위해 코드의 구조를 명시적으로 만듭니다. 다음은 일부 상세정보를 단순화한 위 코드에 대한 AST 표현입니다.
- ![](https://bbiguduk.gitbook.io/~gitbook/image?url=https%3A%2F%2F2352141256-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-x-prod.appspot.com%2Fo%2Fspaces%252F-M7Zt2HBfR67oi6QnKHI%252Fuploads%252Fgit-blob-f8afac36f45ddf2e7b374b3841d15de5b71db4b9%252Fmacro-ast-original%7Edark%25402x.png%3Falt%3Dmedia&width=768&dpr=4&quality=100&sign=0466e87a232996bd3754c4b9e1293bc970022009e822c60753322cf8ebb295ba)
  id:: 6639b8ce-93a8-4ab8-9bc8-919aeb6705f4
-
	- 매크로 구현에 전달된 AST 는 매크로를 표현하는 AST 요소만 포함하고 앞 또는 뒤에 오는 코드를 포함하지 않습니다.
	- 매크로 구현은 파일 시스템 또는 네트워크 접근을 방지하는 샌드박스 환경 (sandboxed environment) 에서 실행됩니다.
-
- ## 매크로 구현
- 매크로를 구현하기 위해, 두 개의 구성요소를 만듭니다: 매크로 확장을 수행하는 타입과 API 로 노출하도록 매크로를 선언한 라이브러리 입니다. 매크로 구현은 매크로의 클라이언트 빌드의 부분으로 수행되기 때문에, 매크로와 해당 클라이언트를 함께 개발하는 경우에도 이러한 부분은 매크로를 사용하는 코드와 별개로 빌드됩니다.
  
  Swift Package Manager 를 사용하여 새로운 매크로를 생성하기 위해, `swift package init --type macro` 를 수행합니다 - 이것은 매크로 구현과 선언에 대한 템플릿을 포함하여 몇 개의 파일을 생성합니다.
  
  기존 프로젝트에 매크로를 추가하기 위해, 다음과 같이 `Package.swift` 파일에 처음을 수정합니다:
- `swift-tools-version` 에 Swift tools 버전을 5.9 이상으로 지정합니다.
- `CompilerPluginSupport` 모듈을 가져옵니다.
- `platforms` 목록에 최소 배포 타겟으로 macOS 10.15 를 포함합니다.
- {{embed [[https://bbiguduk.gitbook.io/swift/language-guide-1/macros#implementing-a-macro]]}}