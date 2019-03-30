# item9 - try-finally보다는 try-with-resource를 사용하라

자원을 회수해야한다면 try-with-resource를 이용하는것이 좋다.

그 장점으로는

* 가독성이 좋아진다.

* 문제를 진단하기 수월해진다.

* try문을 중첩하지 않고도 다수의 예외를 처리할 수 있다.


///finally는 동시성관련 있음 집가서 찾아보기

  
