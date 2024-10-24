## WrapperClass

primitive type의 한계

- Null 값 허용 X
  - primitive type은 null값을 가질 수 없음
- 객체가 아님
  - 객체가 아니기에 객체 지향 언어가 지니는 장점을 활용 할 수 없음
    - 자주 사용되는 유용한 메서드를 사용 할 수 없음
    - 객체 참조가 필요한 컬렉션 프레임워크 및 제네릭을 사용 할 수 없음

### 자바에서 제공하는 래퍼 클래스

| primitive | Wrapper Class |
| :-------: | :-----------: |
|   byte    |     Byte      |
|   short   |     Short     |
|    int    |    Integer    |
|   long    |     Long      |
|   float   |     Float     |
|  double   |    Double     |
|   char    |   Character   |
|  boolean  |    Boolean    |

특징

- 불변
- equals() 메서드로 값 비교

### Boxing vs UnBoxing

Boxing

primitive type 변수를 래퍼클래스로 변환하는 과정

보통 .vaueOf()를 사용해 박싱을 진행

```java
Integer integer = Integer.valueOf(10); // Boxing
Double double = Double.valueOf(10);	// Boxing
```

> Integer 의 경우 -128 ~ 127 범위의 값은 미리 생성해둔 후 재사용하는 방식으로 되어 있음

Unboxing

래퍼클레스에 들어있는 기본형 값을 다시 꺼내는 메서드 

```java
Integer integer = Integer.valueOf(10);
int intValue = integer.intValue();	// Unboxing
```

***Auto(Boxing/Unboxing) : 컴파일러가 개발자 대신 valueOf 혹은 xxxValue()등의 코드를 추가해주는 기능***



