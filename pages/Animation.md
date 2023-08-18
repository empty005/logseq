## [[SwiftUI]]
	- https://medium.com/@amosgyamfi/learning-swiftui-spring-animations-the-basics-and-beyond-4fb032212487
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
		- blendDuration =>
		  logseq.order-list-type:: number
-