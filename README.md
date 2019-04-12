# TIL

# 작성법
* 문서 생성은 GFM (Github Flavored Markdown) 을 사용한다. (확장자 .md)
* 언어나 기술명으로 폴더를 만든다. (root에 문서를 만들지 않는다.)
* 파일명은 영어로



### effective java

**2장 객체 생성과 파괴**

[[item1]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item1.md) 생성자 대신 정적 팩터리 메서드를 고려하라

[[item2]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item2.md) 생성자에 매개변수가 많다면 빌더를 고려하라

[[item3]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item3.md) private 생성자나 열거 타입으로 싱글턴임을 보증하라

[[item4]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item4.md) 인스턴스화를 막으려거든 private 생성자를 사용하라

[[item5]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item5.md) 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

[[item6]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item6.md) 불필요한 객체 생성을 피하라

[[item7]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item7.md) 다 쓴 객체 참조를 해제하라

[[item8]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item8.md) finalizer와 cleaner 사용을 피하라

[[item9]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item9.md) try-finally보다는 try-with-resources를 사용하라

**3장 모든 객체의 공통 메서드**

[[item10]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item10.md) equals는 일반 규약을 지켜 재정의하라

[[item11]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item11.md) equals는 재정의하려거든 hashCode도 재정의하라

[[item12]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item12.md) toString을 항상 재정의하라

[[item13]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item13.md) clone 재정의는 주의해서 진행하라

[[item14]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item14.md) Comparable을 구현할지 고려하라

**4장 클래스와 인터페이스**

[[item15]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item15.md) 클래스와 멤버의 접근 권한을 최소화하라

[[item16]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item16.md) public 클래스에서는 public필드가 아닌 접근자 메서드를 사용하라

[[item17]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item17.md) 변경 가능성을 최소화하라  

[[item18]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item18.md) 상속보다는 컴포지션을 사용하라 

[[item19]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item19.md) 상속을 고려해 설계하고 문서화하라. 그러지 않았다면 상속을 금지하라

[[item20]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item20.md)  추상 클래스보다는 인터페이스를 우선하라

[[item21]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item21.md) 인터페이스는 구현하는 쪽을 생각해 설계하라  

[[item22]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item22.md) 인터페이스는 타입을 정의하는 용도로만 사용하라

[[item23]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item23.md) 태그 달린 클래스보다는 클래스 계층 구조를 활용하라

[[item24]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item24.md) 멤버 클래스는 되도록 static으로 만들라

[[item25]](https://github.com/yeeeeerin/TIL/blob/master/effective%20java/item25.md) 톱레벨 글래스는 한 파일에 하나만 담으라        