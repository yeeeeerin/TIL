#### Flowable

- Reactive Streams 인터페이스를 구현함

- Subscriber에서 데이터를 처리한다

- 데이터 개수를 제어하는 배압 기능이 있다

- Subscription으로 전달 받는 데이터 개수를 제아할 수 있다

- Subscription으로 구독을 해지한다

  ```
  Flowable.interval(1L, TimeUnit.MILLISECONDS) //데이터를 통제하고
                  .doOnNext(data -> logger.info("{}",data)) 
                  .observeOn(Schedulers.computation()) 
                  .subscribe( //Subscriber 객체의 3개 함수를 오버라이드
                          data -> { //onNext()
                              logger.info("#소비자 대기");
                              Thread.sleep(1000l);
                              logger.info("{}",data);
                          },
                          error -> logger.error("{}",error), //onError()
                          ()->logger.info("") //onComplete
                  );
  ```

  



#### Observable

- Reactive Streams 인터페이스를 구현하지 않음

- Observer에서 데이터를 처리한다

- 데이터 개수를 제어하는 배압 기능이 없음

- 배압 기능이 없기때문에 데이터 개수를 제어할 수 없다.

- Disposable로 구독을 해지한다

  ```
  Flowable이 Subscriber를 구현했다면
  Observable은 Observer를 구현함
  --onSubscribe 함수 빼고 나머지 오버라이드 함수는 똑같음--
  ```

  



이 둘의 결정적인 차이는 `배압` 이다.

#### 배압이란?

Flowable에서 데이터를 통지하는 속도가 Subscriber에서 통지된 데이터를 전달받아 처리하는 속도 보다 빠를 때 밸런스를 맞추기 위해 데이터 통지량을 제어하는 기능을 말한다.

#### 배압 전략

* MISSING 
* ERROR
* BUFFER
  * DROP_LATEST : 버퍼가 가득 찬 시점, 버퍼 내 가장 최근 들어온 데이터 DROP(stack)
  * DROP_OLDEST : 버퍼가 가득 찬 시점, 버퍼 내 가장 오래된 데이터 DROP (queue)
* DROP
* LATEST