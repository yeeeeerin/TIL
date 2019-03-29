# item6 - 불필요한 객체 생성을 피해라

기존 객체를 재사용해야 한다면 새로운 객체를 만들지 마라!

만약 아주 무거운 객체를 반복해서 생성한다면 성능을 떨어뜨릴 수 있다.

불필요한 객체 생성을 피하는 방법으로는

* 정적팩터리메서드를 사용하는 법
* 오토박싱을 피하는 법

이렇게 두가지 정도가 있다.



**정적팩터리 메서드를 사용하는 법**

🔐 **Boolean** - 부분 발췌

```
	 public Boolean(String s) {
        this(parseBoolean(s));
   }
   
   //정적팩터리 메서드
   public static Boolean valueOf(String s) {
        return parseBoolean(s) ? TRUE : FALSE;
   }
```



```java
public class item6Main {

    public static void main(String[] args){
        
        Boolean b1;
        Boolean b2;

        Long start;
        Long end;

        start = System.currentTimeMillis();
        for (int i=0;i<1000000;i++) {
            b1 =new Boolean("true");
        }
        end = System.currentTimeMillis();
        System.out.println(end-start);

        start = System.currentTimeMillis();
        for (int i=0;i<1000000;i++) {
            b2 = Boolean.valueOf("true");
        }
        end = System.currentTimeMillis();
        System.out.println(end-start);

    }

}
```

결과

```
8
3
```

미묘하지만 정적팩터리 메서드가 빠르긴 빠르다.

책에서도 나와있듯이 비싼 객체를 사용해야한다면 그 시간의 차는 더욱 커질것이다.



</br>

**오토박싱을 피하는 법**

```
			Long sum = 0L;
      for(long i = 0; i<=Integer.MAX_VALUE;i++){
          sum+=i;
      }
```

예제와 같이 오토박싱은 sum에 i를 더해줄 때 마다 오토박싱이 일어난다.



프로그램의 명확성, 간결성, 기능을 위해서 객체를 추가로 생성하는 것이라면 좋은 일이지만 아주 무거운 객체, 비싼객체를 재사용 해야한다면 불필요한 객체 생성을 하고있지는 않은지 다시 한번 생각해보아야한다.