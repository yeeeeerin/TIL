# item17 - 변경 가능성을 최소화하라

#### 불변 클래스란

인스턴스의 내부 값을 수정할 수 없는 클래스



</br>

#### 불변 클래스의 장점

* 설계, 구현, 사용이 쉽다
* 스레드에 안전하여 따로 동기화할 필요가 없다
* 자유롭게 공유할 수 있으며 불변 객체끼리는 내부 데이터를 공유할 수 있다.
* 객체를 만들 때 다른 불변 객체들을 구성요소로 사용하면 이점이 많다.
* 예외가 발생한 후에도 그 객체는 여전히 유효한상태이다.



</br>

#### 불변 클래스의 단점

* 값이 다르면 반드시 독립된 객체로 만들어야한다.

  ->해결 방법

  * 다단계 연산을 제공
  * pakage-private의 가변 동반 클래스



</br>

#### 불변 클래스의 규칙

* 객체의 상태를 변경하는 메서드를 제공하지 않는다.

* 클래스를 확장할 수 없도록 한다.

* 모든 필드를 final로 선언한다.

  ```
  17.5 final Field Semantics
  Fields declared final are initialized once, but never changed under normal circumstances. The detailed semantics of final fields are somewhat different from those of normal fields. In particular, compilers have a great deal of freedom to move reads of final fields across synchronization barriers and calls to arbitrary or unknown methods. Correspondingly, compilers are allowed to keep the value of a final field cached in a register and not reload it from memory in situations where a non-final field would have to be reloaded.
  final fields also allow programmers to implement thread-safe immutable objects without synchronization. A thread-safe immutable object is seen as immutable by all threads, even if a data race is used to pass references to the immutable object between threads. This can provide safety guarantees against misuse of an immutable class by incorrect or malicious code. final fields must be used correctly to provide a guarantee of immutability. An object is considered to be completely initialized when its constructor finishes. A thread that can only see a reference to an object after that object has been completely initialized is guaranteed to see the correctly initialized values for that object's final fields. The usage model for final fields is a simple one: Set the final fields for an object in that object's constructor; and do not write a reference to the object being constructed in a place where another thread can see it before the object's constructor
  is finished. If this is followed, then when the object is seen by another thread, that thread will always see the correctly constructed version of that object's final fields. It will also see versions of any object or array referenced by those final fields that are at least as up-to-date as the final fields are.
  ```

* 모든 필드를 private으로 선언한다.

* 자신 외에는 내부의 가변 컴포넌트에 접근할 수 없도록 한다.



</br>

#### 불변클래스를 만드는 설계 방법(번외)

* 모든 생성자를 private 또는 pakage-private으로 만들고 public정적 팩터리를 제공한다.
* 계산 비용이 큰 값을 나중에 계산하여 final이 아닌 필드에 캐시해놓는다.



</br>

#### 정리

* 클래스는 꼭 필요한 경우가 아니라면 불변이어야 한다.

* 불변으로 만들 수 없는 클래스라도 변경할 수 있는 부분을 최소한으로 줄이자.

* 다른 합당한 이융가 없다면 모든 필드는 private final이어야한다.

* 생성자는 불변식 설정이 모두 완료된, 초기화가 완벽히 끝난 상태의 객체를 생성해야 한다.

  