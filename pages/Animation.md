## [[SwiftUI]]
	- https://medium.com/@amosgyamfi/learning-swiftui-spring-animations-the-basics-and-beyond-4fb032212487
	- 여기서 소스 코드 받을 수 있음
	  https://github.com/GetStream/swiftui-spring-animations
	-
	- ---
	- ## [[SwiftUI Spring Animation Types]]
	- ### .spring() 
	  logseq.order-list-type:: number
		- 스프링 형태로 따로 파라미터가 존재하지 않은 가장 정적인 스프링 형태로 표현됨
		  logseq.order-list-type:: number
		- 부드럽고 감각적인 스프링 형태로 제공한다고 설명 되어있음.
		  logseq.order-list-type:: number
	- ### .interactiveSpring()
	  logseq.order-list-type:: number
		- 파라미터 없는 interactiveSpring으로 단단함과 낮은 응답
		  logseq.order-list-type:: number
	- ### .interpolatingSpring(stiffness, damping)
	  logseq.order-list-type:: number
		- stiffness => 스프링(용수철 느낌)의 힘 속도를 설정하는 값 / 빠를수록 멀리 다녀옴
		  logseq.order-list-type:: number
		- damping => 마찰력과 비슷하게 멈춰지는 정도 따라서 작을 수록 반동의 크기가 커짐
		  logseq.order-list-type:: number
	- ### .interpolatingSpring(mass, stiffness, damping, initialVelocity)
	  logseq.order-list-type:: number
		- mass => 물체의 질량이라고 생각하면 되고 비슷하게, 질량이 무거울 수록 움직이거나 멈추게 하기에 어렵다는 원리가 동일하게 적용됨
		  logseq.order-list-type:: number
		- InitialVelocity => 초기 속력으로 애니메이션이 시작되는 속도를 뜻한다. (애니메이션에서 초 단위로 측정된다.)
		  logseq.order-list-type:: number
	- ### .spring(response, dampingFraction, blendDuration)
	  logseq.order-list-type:: number
		- response => 응답으로 애니메이션 속성 값이 대상에 도달하는 속도를 제어 
		  logseq.order-list-type:: number
		  response 값이 높을수록 애니메이션이 느려집니다. response가 낮을수록 속도가 빨라집니다.
		- Damping Fraction => 감쇠 비율로 스프링의 진동을 점진적으로 감소 시킨다. 0이면 무한으로 진동 끝나지 않고 계속 진행됨.
		  logseq.order-list-type:: number
			- Over Damping => Damping Fraction 값을 1보다 크게 설정. 이렇게 하면 애니메이션 중인 개체가 신속하게 정지 위치로 돌아갈 수 있습니다.
			  logseq.order-list-type:: number
			- Critical Damping => DF = 1 가능한 빠르게 원래 상태로 돌아옴. 가능한 가장 짧은 시간 내에 물체가 정지 위치로 돌아갈 수 있도록 합니다.
			  logseq.order-list-type:: number
			- Under Damping => DF ≦ 1 설정하면 원래 위치보다 튕겨져서 다시 돌아오는 충격 애니메이션 구현 가능
			  logseq.order-list-type:: number
			- Undamped => DF = 0 해당 오브젝트가 영원히 진동한다.
			  logseq.order-list-type:: number
		- blendDuration => 이전 애니메이션이 중지되고 다음 애니메이션이 시작되는 시간 프레임 값. 여기서 blendDuration 값을 수정해도 시각적으로는 변경되지 않아 확인하기 어렵다.
		  logseq.order-list-type:: number
	- ### interactiveSpring(response, dampingFraction, blendDuration)
	  logseq.order-list-type:: number
		- 기본값: InteractiveSpring(response: Double = 0.15, dampingFraction: Double = 0.86, blendDuration: Double = 0.25).
		  logseq.order-list-type:: number
-
-
- ## Damping Fraction 값에 따른 이미지
	- ![](https://miro.medium.com/v2/resize:fit:700/1*mfLtJXj3rukjckujBa1zQQ.gif)
- ## Stiffness Bounce: High, low, medium, and very low stiffness
	- ![](https://miro.medium.com/v2/resize:fit:700/1*Hi4HBE5xc1dxjGdFYrnqRA.gif)
- ## Stiffness and Damping: Stiff, gentle, wobble, and no wobble
	- ![](https://miro.medium.com/v2/resize:fit:700/1*MUY5IuS3TJgLYRXXBvjSCw.gif)
-
- 이를 활용한 SwiftUI Spring Animation 예제
	- 포지션 도착하는 스프링 애니메이션 ( 잡아 당겨서 도착하는 듯한 느낌의 애니메이션 )
	  logseq.order-list-type:: number
		- ![](https://miro.medium.com/v2/resize:fit:700/1*smSXPaw1CCJexBaA8uiV-g.gif)
	- 크기 스프링 애니메이션 ( 크기가 1에서 2로 커져 바운싱 하는 것 같은 애니메이션 )
	  logseq.order-list-type:: number
		- ![](https://miro.medium.com/v2/resize:fit:700/1*BH4U1H4VUuVFaxwPqvLO8w.gif)
	- 녹음 할때 사용하는 형태의 스프링 형태
	  logseq.order-list-type:: number
		- ![](https://miro.medium.com/v2/resize:fit:700/1*yL7vu6rTokbcdzL2z1gSiQ.gif)
	- 사진 미리보기에서 자세히 보기로 전환 하는 애니메이션
	  logseq.order-list-type:: number
		- ![](https://miro.medium.com/v2/resize:fit:700/1*5hclNI4O1a0vieO_bj0mLQ.gif)
	- 공이 떨어지고 튕기는듯한 애니메이션
	  logseq.order-list-type:: number
		- ![](https://miro.medium.com/v2/resize:fit:700/1*-koOwK6zmnq1gFFWRWjEFA.gif)
	- 스프링 효과를 통한 종 움직임
	  logseq.order-list-type:: number
		- ![](https://miro.medium.com/v2/resize:fit:700/1*HrSjgno0RniYoP1hBP_toQ.gif)
-