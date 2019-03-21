# item2 - 생성자에 매개변수가 많다면 빌더를 고려하라

#### 점층적 생성자 패턴 (단점)

매개변수를 하나 받는 생성자부터 시작해서 N개를 받는 생성자까지 늘려가는 방식

```java
public class NutritionFacts {
     private final int servingSize;    // 필수
     private final int servings;        // 필수
     private final int calories;        // 선택
     private final int fat;            // 선택
     private final int sodium;        // 선택
     private final int carbohydrate;    // 선택
     
     public NutritionFacts(int servingSize, int servings) {
         this(servingSize, servings, 0);
     }
     
     public NutritionFacts(int servingSize, int servings, int calories) {
         this(servingSize, servings, calories, 0);
     }
     
     public NutritionFacts(int servingSize, int servings, int calories, int fat) {
         this(servingSize, servings, calories, fat, 0);
     }
     
     public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium) {
         this(servingSize, servings, calories, fat, sodium, 0);
     }
     
     public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium,
             int carbohydrate) {
         this.servingSize = servingSize;
         this.servings = servings;
         this.calories = calories;
         this.fat = fat;
         this.sodium = sodium;
         this.carbohydrate = carbohydrate;
     }
 }

public class NutritionMain {
    public static void main(String[] args){
      	//fat을 빼고 영양성분을 표시하고 싶다.
        NutritionFacts 콜라 = new NutritionFacts(  ??  );
    }
}
```

이 코드에서 나는 fat의 정보를 빼고 영양성분을 표시하고 싶다면 어떤 생성자를 써야할까?

`NutritionFacts cocalCola = new NutritionFacts(240, 8, 100, 0, 35,27);`

아마 이런식으로 fat에 0을 주고 생성자를 호출해야할 것이다.

이 외에 다른 단점도 많다

* 매개변수가 많아지면 생성자를 만드는 시간이 많이 소요된다.

* 매개변수가 많아지면 생성자를 호출할 때 값의 의미가 헷갈릴 수 있다. 

  -> 엉뚱한 동작, 버그로 이어질 수 있다

  

#### 자바빈즈패턴

Setter메서드들을 호출해 원하는 매개변수의 값을 설정하는 방식

```java
public class NutritionFacts {

    private int servingSize;    // 필수
    private int servings;        // 필수
    private int calories;        // 선택
    private int fat;            // 선택
    private int sodium;        // 선택
    private int carbohydrate;    // 선택


    public void setServingSize(int servingSize) {
        this.servingSize = servingSize;
    }

    public void setServings(int servings) {
        this.servings = servings;
    }

    public void setCalories(int calories) {
        this.calories = calories;
    }

    public void setFat(int fat) {
        this.fat = fat;
    }

    public void setSodium(int sodium) {
        this.sodium = sodium;
    }

    public void setCarbohydrate(int carbohydrate) {
        this.carbohydrate = carbohydrate;
    }

}


public class NutritionMain {
    public static void main(String[] args){
        NutritionFacts 코카콜라 = new NutritionFacts();
        코카콜라.setServingSize(240);
        코카콜라.setServings(8);
        코카콜라.setSodium(35);
        코카콜라.setCarbohydrate(27);

        NutritionFacts 나초 = new NutritionFacts();
        나초.setServingSize(240);
        나초.setServings(8);
        나초.setCalories(100);
        나초.setSodium(35);
        나초.setCarbohydrate(27);


        System.out.println(코카콜라.getCalories() + 나초.getCalories());

				...수많은 작업들..^^
				
        코카콜라.setCalories(100);
        
        ...수많은 작업들..^^

        System.out.println(코카콜라.getCalories() + 나초.getCalories());

    }
}
```

안타깝게도 자바빈즈는 immutable하게 만들지 못합니다. setter로 값을 받아야하기 때문입니다.

그리고 객체가 완전히 생성되기 전까지 setter를 여러번 호출해야하고 그 동안 일관성이 깨지게 됩니다.

예제의 NutritionMain에서 처음 출력은 100을 출력하지만 다음 출력에는 200을 출력하는 당연한 결과가 나옵니다.

만약 그 사이에 수많은 코드들이 있고 깜빡하고 setter를 통해 값을 바꿔주거나 새로 값을 주는 일이 있다면 그 오류를 찾기 힘들 수도 있습니다.



이 문제는 freeze메서드로 해결할 수 있는데 이 방법은 까다로우며 거의 쓰이지도 않습니다. 그리고 컴파일러가 이 메서드를 호출했는지 보증할 방법이 없어 런타임 오류에 취약합니다.



#### 빌더패턴

빌더의 세터 메서들이 자신을 반환하며 연쇄적으로 객체를 생성한다.

```java
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    private NutritionFacts(Builder builder) {
        this.servingSize = builder.servingSize;
        this.servings = builder.servings;
        this.calories = builder.calories;
        this.fat = builder.fat;
        this.sodium = builder.sodium;
        this.carbohydrate = builder.carbohydrate;
    }

    public static class Builder {
        // 필수 인자
        private int servingSize;
        private int servings;

        // 선택적 인자, 기본 값으로 초기화
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(int servingsize, int servings) {
            this.servingSize = servingsize;
            this.servings = servings;
        }

        public Builder calories(int val) {
            this.calories = val;
            return this;
        }

        public Builder fat(int val) {
            this.fat = val;
            return this;
        }

        public Builder carbohydrate(int val) {
            this.carbohydrate = val;
            return this;
        }

        public Builder sodium(int val) {
            this.sodium = val;
            return this;
        }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }
}

public class NutritionMain {
    public static void main(String[] args){

        NutritionFacts 콜라 = new NutritionFacts.Builder(240, 8)
                .calories(100)
                .sodium(35)
                .carbohydrate(27)
                .build();

    }
}
```

장점

* 불변식을 적용할 수 있다.

  ```
  public Builder calories(int val) {
  	if(val값이 잘못되었다면)
  		throw new IllegalArgumentException("값이 잘못되었다");
  		
    this.calories = val;
    return this;
  }
  ```

* 가변신수 매개변수를 여러 개 사용할 수 있다.

* 계층적으로 설계된 클래스에서 형변환에 신경쓰지않고도 빌더를 사용할 수 있다.

단점

* 빌더를 만들어야한다.

* 점층적 생성자 패턴보다는 코드가 장황해서 매개변수가 4개 이상은 되어야 값어치를 한다.

  





