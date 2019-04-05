# item14 - Comparable을 구현할지 고려하라

**순서가 명확한 클래스를 작성한다면 반드시 Comparable인터페이스를 구현하자.**

* Comparator의 compare메서드를 활용하여 비교하자
* 비교자 생성 메서드를 활용하자



</br>

#### compareTo 메서드의 일반 규약

* 두 객체 참조의 순서를 바꿔 비교해도 예상한 결과가 나와야 한다.

* equals와 같이 반사성, 대칭성, 추이성을 충족해야한다.

* compareTo 메서드로 수행한 동치성 테스트의 결과가 equals와 같아야한다.

  -> 컬렉션들은 동치성을 비교할 때 compareTo를 사용한다.



</br>

#### compareTo 메서드 작성 주의점

* Comparable을 구현하지 않은 필드나 표준이 아닌 순서로 비교해야 한다면 Comparator를 대신 사용한다.

* comparaTo 메서드에서 관계 연산자 사용을 지양한다. 

  ->comparator를 이용하자

* 값의 차를 기준으로 비교하지 말자