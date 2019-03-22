# item4 - 인스턴스화를 막으려거든 private 생성자를 사용하라



정적 메서드는 상태를 가지고 있지 않고 단순히 메소드만 가지고 있는 구조이다. 이러한 이유때문에 객체지향적이지 않다고 그리 곱게 보지않는 시선들이 있다.  —>[참고자료] [객체지향 프로그래밍으로 유틸리티 클래스를 대체하자.](http://www.mimul.com/pebble/default/2016/01/06/1452060559741.html)

하지만 Math, Arrays, Collections 처럼 이러한 유틸리티 클래스들은 많은 곳에서 사용되는 공통 기능을 제공하고 널리 쓰이고 있습니다.

이러한 유틸리티 클래스들의 몇가지 공통점이 있습니다.

그중 하나는 바로 **클래스를 객체로 만들지 않는다**는 것입니다.

실제 위의 클래스들을 살펴보면

```java
public class Arrays {

    /**
     * The minimum array length below which a parallel sorting
     * algorithm will not further partition the sorting task. Using
     * smaller sizes typically results in memory contention across
     * tasks that makes parallel speedups unlikely.
     */
    private static final int MIN_ARRAY_SORT_GRAN = 1 << 13;

    // Suppresses default constructor, ensuring non-instantiability.
    private Arrays() {}
    
    ...
}

public class Collections {
    // Suppresses default constructor, ensuring non-instantiability.
    private Collections() {
    }
    
    ...
}


public final class Math {

    /**
     * Don't let anyone instantiate this class.
     */
    private Math() {}
    
    ...
}
```

이와같이 생성자가 private로 설정되어 있음을 볼 수 있습니다.

이는 인스턴스로 만들지 말라는 메세지 이기도 합니다.

여기서 의문이 들수도 있습니다.

생성자를 만들지 않으면 되는거 아니야??

우리의 생각과 달리 생성자를 명시하지 않으면 아래와같이 컴파일러가 자동으로 기본 생성자를 만들어줍니다.

```java
public class User {
    private static final User INSTANCE = new User();
    public static User getInstance(){return INSTANCE;}
}

public class UserMain {
    public static void main(String[] args) {
        System.out.println(User.getInstance());

        User user = new User();
        System.out.println(user);
    }
}
```

결과

```
effective_test.item.item3.User@2d6a9952
effective_test.item.item3.User@22a71081
```



여기서 만약 User를 상속받게되면 또한 인스턴트화될 수 있습니다.

이러한 문제를 방지하기 위해 private생성자를 만들어 인스턴트화 되는것을 막으면 됩니다.

그러면 상속을 막아 하위클래스로 인스턴트화하는것을 막을 수 있습니다.