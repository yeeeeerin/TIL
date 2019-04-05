# item10 - equals는 일반 규약을 지켜 재정의하라



**`equals`메서드는 재정의하지 않는 것이좋다.**

* 각 인스턴스가 본질적으로 고유하다.
  * ex)Thread
* 인스턴스의 '논리적 동치성'을 검사할 일이 없다.
* 상위 클래스에서 재정의한 equals가 하위 클래스에도 딱 들어맞는다.
* 클래스가 private이거나 package-private이고 equals 메서드를 호출할 일이 없다.
  * equals메서드를 재정의 하여 exception을 던져 호출을 막는다.

이 중 하나에 해당한다면 **`equals`메서드는 재정의하지 않는 것이좋다.**



</br>

#### euqals를 재정의해야 할 때

* 객체 식별성이 아니라 논리적 동치성을 확인해야 하는데 그렇게 재정의 되지 않았을 때



#### equals 메서드 정의 규약

* 반사성

  객체는 자기 자신과 같아야한다

* 대칭성

  두 객체는 서로에 대한 동치 여부에 똑같이 답해야 한다.

* 추이성

  o1 == o2 이고 o2==o3 이라면, o1 == o3 이여야한다.

  보통 상속받은 클래스에 값을 추가 했을때 자주 위배된다.

* 일관성

  수정하지 않았을 때, 두 객체가 같다면 앞으로도 영원히 같아야 한다.

  일관성을 위배하지 않기 위해서는 항시 메모리에 존재하는 객체만을 사용한 결정적계산만 수행해야한다.

* null-아님

  모든 객체가 null과 같지 않아야 한다.

  묵시적 null 검사인 `instanceof` 를 사용한다.



</br>

#### 양질의 equals 메서드 구현 방법

1. == 연산자를 사용해 입력이 자기 자신의 참조인지 확인한다.
2. instanceof 연산자로 입력이 올바른 타입인지 확인한다.
3. 입력을 올바른 타입으로 형변환한다.
4. 입력 객체와 자기 자신의 대응되는 '핵심' 필드들이 모두 일치하는지 하나씩 검사한다.



</br>

#### euqals 사용 시 주의할 점

* float와 double은 제외(특수한 부동 소수값을 다뤄야함)

- 배열필드는 Arrays.equals를 이용한다.
- null도 정상 값으로 취급하는 참조 타입 필드는 Object.equals(Object, Object)로 비교하여 NullPointerException 발생을 예방하자.
- 비교하기 복잡한 필드는 그 필드의 표준형을 저장해둔 후 표준형끼리 비교하라.
- 비용이 싼 필드를 먼저 비교하자
- 동기화용 락 필드 같이 객체의 논리적 상태와 관련 없는 필드는 비교하면 안 된다.
- equals를 재정의할 땐 hashCode도 반드시 재정의하자
- 별칭(alias)는 비교하지 않는게 좋다
- Object외의 타입을 매개변수로 받는 equals메서드는 선언하지 말자.
  - 다중정의를 유발할 수 있다.



</br>

#### equals를 재정의하고 싶다면 AutoValue프래임워크를 사용하자

편하다.ㅎ


