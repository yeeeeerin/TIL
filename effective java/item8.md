# item8 - finalizer 와 cleaner 사용을 피하라

Finalizer와 Cleaner은 예측 불가능하고, 위험하며, 대부분 불필요하다. 오동작, 낮은 성능, 이식성과 같은 부분에서 문제를 일으킬 수 있다.

Finalizer와 Cleaner 쓰임

- 자원의 소유자가 close메서드를 호출하지 않는 것에 대비한 안전망 역할
- 네이티브 피어와 연결된 객체를 회수해야할 때

이렇게 두가지 정도 쓰임새가 있지만 기본적으로 **쓰지 말아야**한다.

**객체 소멸자를 쓰면 안되는 이유**

* Finalizer와 Cleaner은 즉시 수행된다는 보장이 없다. 

  만약 파일닫기를 수행해야한다면 언제 파일을 닫는지 모르고 실행을 게을리해서 파일을 계속 열어둔다면 새로운파일을 열지 못하는 상황이 발생할 수 있다.

* 데이터를 수정하는 작업을 Finalizer와 Cleaner에 의존하지 말아라

  만약 데이터베이스 자원의 락을 헤제하는 작업을 Finalizer와 Cleaner에 맡긴다면 전체 분산 시스템이 멈춰 버릴 수도 있다.

* `System.gc`나`System.runFinalization` 메서드에 현혹되어 사용하지 말자.

  Finalizer와 Cleaner이 실행될 가능성을 높여줄 수는 있지만 보장해주지는 않는다.

* ㅇ

* Finalizer동작 중 발생한 예외는 무시되며 중간에 작업이 종료된다.

* finalizer와 cleaner는 심각한 성능 문제를 야기한다.

* finalizer를 사용한 클래스는 finalizer공격에 노출되어 심각한 보안 문제를 야기한다.

