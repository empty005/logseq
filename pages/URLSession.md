- [[Async/Await]]
- ```swift
  import UIKit
  
  let url = URL(string: "https://google.com")!
  /*
  // URLSession은 URLSession.shared라는 싱글턴 패턴의 공통 인스턴스 호출로 진행하는 것이 보통이다
  let task: URLSessionDataTask = URLSession.shared.dataTask(with: url) { (data, response, error) in
      guard let data = data else {
          print("nothing!")
          return
      }
      
      // iOS의 String은 보통 UTF-16으로 구성되기 때문에
      // UTF-8 형식으로 문자가 적힌 데이터를 가져와 변환하는 게 필요하다
      print(String(data: data, encoding: .utf8)!)
      
      print("진행완료")
  }
  
  print("진행시켜")
  
  task.resume()
  */
  
  func fetchData(from url: URL) async -> Data? {
      do {
          let (data, response) = try await URLSession.shared.data(from: url)
          print("\(response)")
          return data
      } catch {
          return nil
      }
  }
  
  
  // async가 붙은 함수를 실행하는 방법 중에 대표적인 것은
  // Task를 만들어 그 안에서 진행하는 것이다
  Task {
      // async가 붙은 함수는 await 붙여서 호출해야한다
      if let data = await fetchData(from: url) {
          print(String(data: data, encoding: .utf8)!)
      } else {
          print("nothing!")
      }
      
      var greeting = "Hello, playground"
  
      for _ in 0..<5 {
          print(greeting)
      }
  }
  ```