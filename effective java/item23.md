# item23 - 태그 달린 클래스보다는 클래스 계층구조를 활용하라

#### 태그 달린 클래스의 단점

* 열거 타입 선언
* 태그 필드
* switch문 
* 가독성이 나쁨
* 메모리도 많이 사용
* 의미없는 필드들까지 초기화
* 새로운 의미를 추가할 때마다 모든 switch문을 추가해야함



#### 태그 달린 클래스를 클래스 계층구조로 바꾸는 방법

1. 계층구조의 루트가 될 추상 클래스를 정의
2. 태그 값에 따라 동작이 달라지는 메서드들을 루트 클래스의 추상 메서드로 선언
3. 동작이 일정한 메서드, 공통으로 사용하는 데이터 필드들을 루트 클래스에 선언
4. 루트 클래스를 확장한 구체 클래스를 의미별로 하나씩 정의