# item5 - 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라



의존성 관계란 객체와 객체의 결합관계이다.

의존 객체 주입의 장점

* 유연하게 싱글턴, 정적유틸리티를 만들 수 있다.
* 의존 관계 설정이 런타임때 이루어져 모듈들간의 결합도를 낮출 수 있다.
* 코드 재사용을 높여 작성된 모듈을 어려 곳에소 소스코드의 수정 없이 사용할 수 있다.
* 모의 객체 등을 이용한 단위 테스트의 편의성을 높여준다.



Java8의 Supplier<T> interface는 간단하고 쉽게 팩터리를 이해시켜준다.

사용 예)

```java
public class item5Main {
    
    public static Lexicon returnDic(Supplier<? extends Lexicon> dic){
        return dic.get();
    }

    public static void main(String[] args){
        HanaDic hanaDic = new HanaDic();

        Lexicon dic1 = returnDic(() -> hanaDic);
        Lexicon dic2 = returnDic(() -> hanaDic);

        System.out.println(dic1);
        System.out.println(dic2);
    }
    
}

interface Lexicon {
}
class HanaDic implements Lexicon {
}
```

결과

```
effective_test.item.item5.HanaDic@23223dd8
effective_test.item.item5.HanaDic@23223dd8
```









 참고

[위키 - 의존성 주입](<https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85>)