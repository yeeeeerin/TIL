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



<br>

<br>

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

   책에서 설명한 생성자는 하나의 시그니처밖에 못만든다는 단점 + 우리가 생성자 대신 정적 팩터리 메서드를 사용하지 않는다면 가독성을 잃는 동시에 이 많은 생성자들을 구분해서 기억해둬야하는 역할까지 생기는 것이다.



<br>

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



<br>

3. **반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.**

   대표적으로 ` Collections`를 간단하게 분석해보았다.

   🔐 **Collections** - 부분 발췌

   ```java
   public class Collections {
       // Suppresses default constructor, ensuring non-instantiability.
       private Collections() {}
       
       //...생략생략...v^^v
       
       public static <T> List<T> singletonList(T o) {
           return new SingletonList<>(o);
       }
       
       //...생략생략...v^^v
   }
   ```

   보시다싶이 생성자가 `private`인것으로 보아 `Collections`는 인스턴스화 불가 클래스다.

   그리고 `Collections`의 `singletonList`는 `Collections`의 하위 클래스인 List를 반환하고있다.

   

   <br>

4. **입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.**

   이제는 안보고 넘어갈 수 없다. `Enumset`도 보자.

   🔐 **Enumset** - 부분발췌

   ```java
   public abstract class EnumSet<E extends Enum<E>> extends AbstractSet<E>
       implements Cloneable, java.io.Serializable
   {
   
   	//...생략생략...v^^v
   	
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
       
      //...생략생략...v^^v
      
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

   이렇게 나온다. 내부적으로는 `RegularEnumSet`을 받은것이지만, 그냥 `allOf `함수로 `Num`을 넘겨줬을 뿐 안에서 무슨일이 일어나는지는 모르고 원하는 값을 얻었다. 

   우리는 앞으로도 내부적으로 무슨일이 일어나는지 신경쓰지 않아도 되고 그냥 `allOf`함수를 편하게 이용하면 된다.^^

   

   <br>

5. **정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.**

   JDBC는 MySQL, Oracle, SqlServer 등 다양한 데이터베이스를 다룬다는 사실을 알고있다. 

   우리는 JDBC를 상용할 때 getConnection(url,name,password)을 통해 우리가 원하는 데이터베이스 Connection을 얻는다. 단순히 와 편하네..했던 기술이였는데 생각해보니 JDBC가 어떻게 이 데이터베이스를 구분하는지 궁금해졌다.

   알아보자!

   🔐 **DriverManager** - 부분발췌

   ```java
   public class DriverManager {
     
     	private DriverManager(){}
     
     	//...생략생략...v^^v
     
     	@CallerSensitive
       public static Connection getConnection(String url,
           String user, String password) throws SQLException {
           java.util.Properties info = new java.util.Properties();
   
           if (user != null) {
               info.put("user", user);
           }
           if (password != null) {
               info.put("password", password);
           }
   
           return (getConnection(url, info, Reflection.getCallerClass()));
       }
     
       //...생략생략...v^^v
     
     
    		private static Connection getConnection(
         String url, java.util.Properties info, Class<?> caller) throws SQLException {
   
   	  		//...생략생략...v^^v
         
           for (DriverInfo aDriver : registeredDrivers) {
   
               if (isDriverAllowed(aDriver.driver, callerCL)) {
                   try {
                       println("    trying " + aDriver.driver.getClass().getName());
                       Connection con = aDriver.driver.connect(url, info);
                       if (con != null) {
                           // Success!
                           println("getConnection returning " + aDriver.driver.getClass().getName());
                           return (con);
                       }
                   } catch (SQLException ex) {
                       if (reason == null) {
                           reason = ex;
                       }
                   }
   
               } else {
                   println("    skipping: " + aDriver.getClass().getName());
               }
   
           }
         
           //...생략생략...v^^v
       }
     
       //...생략생략...v^^v
     
   }
   ```

   

   전체적으로 DriverManager도 생성자가 닫혀있음을 볼 수 있고 발췌한 ` getConnection` 정적메소드로 이루어져있음을 볼 수 있다.

   

   위에 ` getConnection` 은 우리가 드라이버주소, 유저이름, 비밀번호를 넘겨주면서 사용했던 메소드다.

   그리고 아래 ` getConnection` 는 우리가 넘겨준 정보로 드라이버를 찾는 로직을 가지고있다.

   아래 ` getConnection` 는 등록된 드라이버들을 비교하며 우리가 사요하려는 드라이버를 찾고 있다.

   `isDriverAllowed`는 가져오지 않았지만 우리가 처음 JDBC 드라이버 로딩할 때 `Class.forName("oracle.jdbc.driver.OracleDriver");` 이렇게 JDBC드라이버 파일을 메모리에 로딩하는데 `isDriverAllowed` 에서는 반대로 드라이버 이름을 메모리로부터 찾는 역할을 한다.

   

   

<br>

<br>

### 단점들을 파해쳐보자

다행히 단점은 두개뿐이다.

1. **정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.**

   상속을 하려면 public이나 protected 생성자가 필요하다. 하지만 위에 클래스들을 보면 대부분 생성자가 private로 되어있었다. 상속보다 컴포지션을 사용하도록 유도하고 불변 타입으로 만들기 위한 자연스러운 제약이다.

2. **정적 펙터리 메서드는 프로그래머가 찾기 어렵다.**

   생성자는 다른 메서드와 뚜렸하게 구별되지만 정적 팩터리 메서드는 명확하게 들어나있지 않고 사용법을 알기 어렵다.

   그래서 정적팩터리 메서드에 흔히 사용하는 명명 방식들이 있다.

   ```
   from :
   매개변수를 하나 받아서 해당 타입의 인스턴스를 반환하는 형변환 메서드
   
   of :
   여러 매개변수를 받아 적합한 타입의 인스턴스를 반환하는 집계 메서드
   
   valueOf :
   from과 of의 더 자세한 버전
   
   instance or getinstance : 
   매개변수로 명시한 인스턴스를 반환하지만, 같은 인스턴스임을 보장하지는 않는다.
   
   create or newInstance : 
   instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스를 생성해 반환함을 보장한다.
    
   getType :
   getInstance와 같지만 생성할 클래스와 다른 클래스에 팩토리 메소드가 있을 때 사용한다.
   Type은 팩토리 메소드가 반환할 객체의 타입이다.
    
   newType :
   newInstance와 같지만 반환된 객체의 클래스와 다른 클래스에 팩토리 메소드가 있을 때 사용한다.
   Type은 팩토리 메소드가 반환할 객체의 타입이다.
   
   type : 
   getType과 newType의 간결한버전
   
   ```

   