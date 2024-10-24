## Object
자바에서 최상위 부모 클래스

객체지향 관점에서의 Object  
**공통 기능 제공**  
객체마다 기본적으로 필요한 기본 기능에 대해서 구현이 되어 있음  
필요한 경우 개발자가 오버라이딩 해서 사용하면 됨

**다형성의 기본 구현**  
자바에서 부모클래스는 자식 클래스를 담을 수 있음 (즉, 모든 객체를 참조할 수 있음)
모든 자바 객체는 Object 타입으로 처리 될 수 있기에 다양한 타입의 객체를 통합적으로 처리 할 수 있음

### toString()
```java
public String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```
toString은 기본적으로 패키지를 포함한 객체 이름과 객체의 참조값을 16진수로 제공함

### equals()
동등성 비교를 위한 메서드를 제공 

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

기본적으로 제공하는 equals() 메서는 동일성 비교를 제공  
동등성을 비교하고자 한다면 오버라이딩 해서 사용  
