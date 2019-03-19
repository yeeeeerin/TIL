# item1 - 생성자 대신 정적 팩터리 메서드를 고려하라

정적 팩터리 메서드 - 클라이언트가 클래스의 인스턴스를 얻는 또하나의 방법

### 목차

* 정적 팩터리 메서드가 생성자보다 좋은 장점

  1. 이름을 가질 수 있다.

  2. 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

  3. 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

  4. 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

  5. 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.



* 정적 팩터리 메서드가 생성자보다 안좋은 단점

  1. 상속을 하려면 `public`이나 `protected` 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.

  2. 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.

     

* 부록 - 미래에 나올 아이템이나 모르는 용어들 정리





### 장점들을 파해쳐보자

1. **이름을 가질 수 있다.**

   `Boolean` 의 `valueOf, BigInteger.probablePrime, Integer.getInteger…` 등등 우리가 흔히 볼 수 있는 `static Object 함수();` 들이 해당한다.

   아니.. 생성자랑 함수하나 구분하는게 뭐 그렇게 어렵다고..?? 난 귀찮아 그냥 생성자 따로 만들지 뭐ㅋㅋ

   는 `BigInteger`의 생성자들을 파해쳐보자!

   🔐 **BigInteger** - 부분 발췌

   ```java
   //BigInteger의 생성자 1
   public BigInteger(byte[] val)
   
   //BigInteger의 생성자 2
   public BigInteger(int signum, byte[] magnitude)
   
   //BigInteger의 생성자 3
   public BigInteger(String val, int radix)
   
   //BigInteger의 생성자 4
   public BigInteger(String val)
   
   //BigInteger의 생성자 5
   public BigInteger(int numBits, Random rnd)
   
   //BigInteger의 생성자 6
   public BigInteger(int bitLength, int certainty, Random rnd)
   
   ```

   실화인가..? 실화이다. 

   우리가 생성자 대신 정적 팩터리 메서드를 사용하지 않는다면 가독성을 잃는 동시에 이 많은 생성자들을 구분해서 기억해둬야하는 역할까지 생기는 것이다.





2. **호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다**

   * 호출될 때마다 인스턴스를 새로 생성하지는 않아도 된다.

     응..? 난 이거 잘 공감 못하겠는데?? 뭐.. `Boolean valueof(boolean b)` 는 알겠어 그래서 이게 왜 좋아???

     그래서 준비했다 `Flyweight patten`으로 성능차이를 비교해보자!!

     ```java
     public class Fly {
         public Fly(String name){
             this.name = name;
             this.dateTime = LocalDateTime.now();
         }
         String name;
         LocalDateTime dateTime;
     }
     
     public class Flys {
         HashMap<String,Fly> flyMap = new HashMap<>();
         public void addFly(Fly fly){
             flyMap.put(fly.name,fly);
         }
         public Fly getFly(String s){
             return flyMap.get(s);
         }
     }
     
     public class FlyTestMain {
         public static void main(String[] args){
             long start;
             long end;
     
             start = System.currentTimeMillis();
             Fly fly1 = new Fly("fly1");
             end = System.currentTimeMillis();
             System.out.println("생성자 성능 : "+ (end-start));
     
             Flys flys = new Flys();
             flys.addFly(fly1);
     
             start = System.currentTimeMillis();
             Fly fly = flys.getFly("fly1");
             end = System.currentTimeMillis();
             System.out.println("인스턴트 재활용 성능 : "+ (end-start));
     
         }
     }
     ```

     **결과**

     ```
     생성자 성능 : 53
     인스턴트 재활용 성능 : 0
     ```

   * 인스턴트 통제 클래스

     통제…..???? 통제하는거 안좋은거 아냐???

     그래서 각 장점을 짧게 요약해보았다.

     * 싱글턴 - 고정 메모리를 얻으면서 전역 인스턴스이기 때문에 다른 클래스의 인스턴스들이 데이터를 공유하기 쉬움
     * 인스턴트화 불가 - `java.util.Collections` 처럼 특정 인터페이스를 구현하는 객체를 생성해 주는 정적 메서드(혹은 팩터리)를 모아놓을 수 있음
     * 불변 값 클래스 - 복사를 계속해도 원본이랑 같으므로 스레드에 안전
     * 열거타입 - 인스턴스가 하나만 만들어짐
     * 플라이웨이트 패턴 ⬆⬆⬆





3. **반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.**

   대표적으로` Collections`를 간단하게 분석해보았다.

   🔐 **Collections** - 부분 발췌

   ```java
   public class Collections {
       // Suppresses default constructor, ensuring non-instantiability.
       private Collections() {}
       
       ...생략생략...v^^v
       
       public static <T> List<T> singletonList(T o) {
           return new SingletonList<>(o);
       }
       
       ...생략생략...v^^v
   }
   ```

   보시다싶이 생성자가 `private`인것으로 보아 `Collections`는 인스턴스화 불가 클래스다.

   그리고 `Collections`의 `singletonList`는 `Collections`의 하위 클래스인 List를 반환하고있다.

   

   

4. **입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.**

   이제는 안보고 넘어갈 수 없다. `Enumset`도 보자.

   🔐 **Enumset**

   ```java
   public abstract class EnumSet<E extends Enum<E>> extends AbstractSet<E>
       implements Cloneable, java.io.Serializable
   {
   
   	...생략생략...v^^v
   	
   		public static <E extends Enum<E>> EnumSet<E> noneOf(Class<E> elementType) {
           Enum<?>[] universe = getUniverse(elementType);
           if (universe == null)
               throw new ClassCastException(elementType + " not an enum");
   
           if (universe.length <= 64)
               return new RegularEnumSet<>(elementType, universe);
           else
               return new JumboEnumSet<>(elementType, universe);
       }
       
       public static <E extends Enum<E>> EnumSet<E> allOf(Class<E> elementType) {
           EnumSet<E> result = noneOf(elementType);
           result.addAll();
           return result;
       }
       
      ...생략생략...v^^v
      
   }
   
   ```

   `EnumSet`의 `noneof`를 보면 64개 이하면 `RegularEnumSet`을 65개 이상이면 `JumboEnumSet`을 반환하는 것을 눈으로 직접 확인하였다.

   그럼 사용해보자.

   ```java
   public class Main {
   
       enum Num{
           one,two,three,four,five,six,seven,eight,nine,ten,eleven
       }
   
       public static void main(String[] args){
           EnumSet es = EnumSet.allOf(Num.class);
           System.out.println(es);
       }
   }
   ```

   결과는 

   ```
   [one, two, three, four, five, six, seven, eight, nine, ten, eleven]
   ```

   이렇게 나온다. 내부적으로는 `RegularEnumSet`을 받은것이지만, 그냥 `allOf `함수로 `Num`을 넘겨줬을 뿐 안에서 무슨일이 일어나는지는 모르고 원하는 값을 얻었다. 우리는 앞으로도 내부적으로 무슨일이 일어나는지 신경쓰지 않아도 되고 그냥 `allOf`함수를 편하게 이용하면 된다.^^

   

5. **정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.**

   

​	

