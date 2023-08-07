- https://developer.apple.com/documentation/uikit/uitableview
- Delegate 패턴 활용으로 해당 섹션 생성, 열 개수 생성, 해당 셀에 어떤 값이 들어갈지 등등 을 만들어진 패턴을 활용하여 제작이 가능하다.
- 2가지로 가능한데
	- UITableViewDelegate를 채택하여 지금 쓰고 있는 ViewController에서 대리자로 사용하여 페이지에서 구현
	  logseq.order-list-type:: number
	- UITableViewController를 사용하여 해당 페이지에서 func를 override 하여 사용한다.
	  logseq.order-list-type:: number
-
- 이전 코드 자료 참고
- ```swift
  //섹션 값 수정해주는 함수 -> return에 원하는 숫자(섹션 값을) 작성
      override func numberOfSections(in tableView: UITableView) -> Int {
          // #warning Incomplete implementation, return the number of sections
          return 2
      }
  
  // 테이블 뷰 생성인데 한 섹션 별 가지는 row 개수 -> return row 개수를 지정해주면 되는데 section < 조건을 달아서 해당 하는 정수값
      // return 에 작성을 해주시면 섹션 별 row 개수 지정해주는 함수
      override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
          //섹션 별 row행 개수 반환 개수 지정
          switch section {
          // 결과를 보고 싶은 0번에 지정한 단일 행
          case 0:
              return 1
          //실제로 팀원에 대한 리스트를 담을 섹션 = 팀원 수 만큼 지정
          case 1:
              return teams.count
          // 그밖의 경우
          default:
              return 0
          }
      }
  
  //이제 테이블 뷰 내의 셀 설정 (그러면 어떻게 설정할 것인가?) "indexPath" 값을 통해 섹션 번호 + 해당 섹션번호의 row 번호를 지정하여
      //커스텀 된 cell을 설정하여 사용 할 수 있다.
      //따라서 cell의 디자인,스타일,값, 등을 설정할 수 있는 구역으로 return 값은 테이블 한 줄의 cell 이다
      override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
          
          //cell의 스타일 및 cell을 생성하여 해당하는 셀이 들어갈 수 있도록 (동적 셀을 생성해주어야 한다.)
          //tableview그리는 구조에서 값이 많아지면 데이터 처리
          let cell = UITableViewCell(style: .subtitle, reuseIdentifier: "MyCell")
          
          //switch 문으로 섹션별 다른 cell을 만들고 싶어..
          //조건문 on (section 값 비교)
          switch indexPath.section {
          case 0:
              cell.textLabel?.text = resultMessage //  변수 처리 -> 나중에 변수 수정되면 여기 셀 수정
              break
          case 1:
              //우리팀 인원 이름 + 별명 리스트
              cell.textLabel?.text = teams[indexPath.row].name
              cell.detailTextLabel?.text = teams[indexPath.row].nickName
          default:
              break
          }
          return cell
      }
  
   // row가 선택 되었을때 실행 시킬 함수 (즉 눌렸을 때 실행 되는 함수)
      // 해당 값의 조작은 눌린 "indexPath"에 대하여 접근하여 눌린 row의 index값을 "indexPath"로 읽어옴
      // 따라서 "indexPath"를 통하여 조건을 통해 이벤트를 설정할 수 있음.
      
      // 그렇지만 현재 누르면 환영인사의 문구 + 음성으로 인사말을 원하기 때문에
      // 눌린 데이터 + 음성 출력을 해주는 함수부분
      override func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
          
          switch indexPath.section {
          case 0:
              break
          case 1:
              soundNuna.stopSpeaking(at: .immediate)
              //0섹션 1번셀 수정하기로 했고 이거는 resultMessage를 참조하니까 이거 수정해야지
              resultMessage = "어서옵쇼, \(teams[indexPath.row].nickName)님"
              tableView.reloadData()
              //사운드 출력
              readme()
              break
          default:
              break
          }
      }
  ```
-