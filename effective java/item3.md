# item3 - private생성자나 열거 타입으로 싱글턴임을 보증하라

싱글턴이란 인스턴스를 오직 하나만 생성할 수 있는 클래스이다.

제목과 같이 싱글턴을 만드는 방식으로는

* private으로 생성자를 감춰두고 유일한 인스턴스에 접근할 수 있도록한다.
* emum을 이용한다.

이렇게 두가지 이다.



### private으로 생성자를 감춰두고 public static final 필드 사용하기

방법으로는 2가지가 있다.

* public static final 필드 사용하기
* 정적 팩터리 메서드 사용하기

예제를 먼저 보자



**public static final 필드 사용하기**

말 그대로  `public static final User INSTANCE` 를 선언하고 사용하면 된다.

```java
public class User {
    public static final User INSTANCE = new User();
    private User(){}
}

public class UserMain {
    public static void main(String[] args){
        System.out.println(User.INSTANCE);
    }
}
```

public static final 필드 사용하여 싱글턴을 만드는 장점은

* API에 해당 클래스가 싱글턴임을 명백히 알 수 있다.

* 간결하다

  

하지만 이런 방법은 `AccessibleObject.setAccessible()`(리플렉션)을 사용해 `private` 생성자를 다시 호출할 수 있다는 문제가 있다.

아래는 그 방법을 사용해 새로운 객체를 얻는 코드이다.

```java
public class UserMain {
    public static void main(String[] args) throws IllegalAccessException, NoSuchMethodException, 
            InvocationTargetException, InstantiationException {
        System.out.println(User.INSTANCE);

        Constructor<User> constructor = User.class.getDeclaredConstructor();
        constructor.setAccessible(true);
        User user = constructor.newInstance();

        System.out.println(user);

    }
}
```

결과

```
effective_test.flyweight.item3.User@2d6a9952
effective_test.flyweight.item3.User@22a71081
```

싱글턴으로 만들었음에도 불구하고 싱글턴이 깨지는 현상을 보고있다.



이러한 현상을 막기 위해서

```java
public class User {
    public static final User INSTANCE = new User();
    private User(){
        if(INSTANCE != null){
            throw new RuntimeException("싱글톤 깨짐");
        }
    }
}
```

이렇게 생성자에서 두번째 인스턴스를 만드려 할 때 예외를 던져주면 된다.



**정적 팩터리 메서드 사용하기**

필드는 private로 숨기고 정적 팩터리 메서드로 객체를 반환하는 방법이다.

```java
public class User {
    private static final User INSTANCE = new User();
    private User(){
    }
    
    public static User getInstance(){return INSTANCE;}
}


public class UserMain {
    public static void main(String[] args){
      
        System.out.println(User.getInstance());

    }
}
```



정적팩터리 메서드를 사용하여 싱글턴을 만드는 장점은

* API를 바꾸지 않고도 싱글턴이 아니게 변경할 수 있다
* 유일한 인스턴스를 반환하던 팩터리 메서드가 호출하는 스레드별로 다른 인스턴스를 넘겨주게 할 수 있다.
* 정적 팩터리를 제네릭 싱글 메서드 참조를 공급자로 사용할 수 있다.

하지만 이 방법 역시 리플렉션을 통한 예외는 똑같다.(따로 확인해보지는 않겠다.)



이 두가지 방법의 문제는 또 하나 더 있다.

싱글턴 클래스를 직렬화하면 역직렬화할 때마다 새로운 인스턴스가 만들어진다.

이러한 문제를 예방하고싶다면

```java
public class User {
    private static final User INSTANCE = new User();
    private User(){
    }
    
    public static User getInstance(){return INSTANCE;}
    
    private Object readResolve(){
      return INSTANCE;
    }
}
```



<br>

<br>

### emum을 이용한다.

원소가 하나인 열거 타입을 선언하는 방법이다.

```java
public enum  Fruit {
    INSTANCE;

    public void getHi(){
        System.out.println("hi");
    }
}

public class FruitMain {
    public static void main(String[] args){

        Fruit.INSTANCE.getHi();
    }
}
```

이러한 방법의 장점은

* 간결하다
* 추가 노력없이 직렬화할 수 있다
* 리플렉션 공격에서도 제2의 인스턴그가 생기는 일을 완벽히 막아준다.

원소가 하나뿐이라면 열거타입으로 싱글턴을 만드는 법이 가장 좋은 방법이다.

단, emun외의 클래스를 상속하지 못한다.