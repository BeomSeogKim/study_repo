## Enumeration

### Type Safe Enum Pattern (타입 안전 열거형 패턴)

개발자가 나열한 항목만 사용할 수 있음

```java
public class Grade {
  public static final Grade BRONZE = new Grade();
  public static final Grade SILVER = new Grade();
  public static final Grade GOLD = new Grade();
  
  private Grade() {
    
  }
}
```

- private 생성자
  - 외부에서 Grade 인스턴스를 임의로 생성하지 못하도록 방지
- 장점
  - 정해진 객체만 사용할 수 있어, 잘못된 값을 입력하는 문제를 미리 방지할 수 있음
  - 정해진 객체만 사용하기에 데이터의 일관성이 보장됨

### Enum Type

자바에서 Type Safe Enum Pattern을 편리하게 사용 할 수 있도록 제공하는 타입

장점

- 사전에 정의된 상수들로만 구서오디므로, 유효하지 않은 값이 입력될 가능성이 없음
- 코드가 간결해지고 명확해지며, 데이터의 일관성이 보장됨
- 새로운 타입을 추가하고자 할 때 Enum에 새로운 상수를 추가 하면됨 (확장성)

주요 메서드

- values() : 모든 enum 상수를 포함하는 배열을 반환
- valuesOf(String name) : 주어진 이름과 일치하는 enum 상수를 반환
- name(): enum 상수의 이름을 문자열로 반환
- ordinal(): enum 상수의 선언 순서를 반환

특징 

- enum의 경우 이미 java.lang.Enum 클래스를 자동으로 상속받음, 이에 따라 다른 클래스를 상속받을 수 없음
- enum의 경우 인터페이스를 구현할 수 있음
- enum에 추상 메서드를 선언하고 구현 할 수 있음