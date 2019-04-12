# item18 - 상속보다는 컴포지션을 사용하라

#### 상속 사용이 옳은 경우

* 상위 클래스와 하위 클래스를 모두 같은 프로그래머가 통제할 때
* 확장할 목정으로 설계되었고 문서화도 잘 된 클래스



#### 상속 사용의 문제점

* 상위 클래스가 어떻게 구현되느냐에 따라 하위 클래스의 동작에 이상이 생길 수 있다.



#### 상속을 피하는 방법

* 확장대신 새로운 클래스를 만들고 private필드로 기존 클래스의 인스턴스를 참조





<https://stackoverflow.com/questions/28254116/wrapper-classes-are-not-suited-for-callback-frameworks>

