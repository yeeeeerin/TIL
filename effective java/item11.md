# equals를 재정의하려거든 hashCode도 재정의하라

**equals를 재정의한 클래스 모두에서 hashCode도 재정의해야한다.**

그렇지 않으면 HashMap, HashSet같은 컬렉션을 사용할 때 문제가 발생할 수 있다.

Object 명세에서는

**equals가 두 객체를 같다고 판단했으면, 두 객체의 hashCode는 똑같은 값을 반환해야한다.**

라고 명시되어 있다.



</br>

#### hashCode를 작성하는 요령

1. int 변수 result를 선언한 후 값 c로 초기화한다. 

2. 해당 객체의 나머지 핵심 필드 f 각각에 대해 다음 작업을 수행한다.
   1. 해당 필드의 해시코드 c를 계산
   2. 위에서 계산한 해시코드 c로 result를 갱신, result = 31* result + c;

3. result를 반환한다.



</br>

#### hashCode 작성 시

* 다른 필드로부터 계산해낼 수 있는 필드는 모두 무시해도 된다.
* equals 비교에 사용되지 않은 필드는 '반드시'제외해야 한다.
* 31* result는 순서에 따라 result 값이 달라지게 한다.(이나그램의 해시코드 중복 방지)



</br>

#### 클래스가 불변이고 해시코드를 계산하는 비용이 크다면, 매번 새로 계산하기보다는 캐싱하는 방식을 고려하자

또는 지연초기화

