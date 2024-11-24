## 제네릭 (Generic)

```java
public class Box<T> {
  private T value;
  
  public void set(T value) {
    this.value = value;
  }
  
  public T get() {
    return this.value;
  }
  
}
```

##### 용어 

Generic Type (제네릭 타입)

- 클래스나 인터페이스를 정의할 때 타입 매개변수를 사용하는 것을 말함
- 제네릭 클래스, 제네릭 인터페이스 모두 합쳐서 Generic Type
- Ex) Box<T>  

Type Parameter (타입 매개변수)

- 제네릭 타입이나 메서드에 사용되는 변수로, 실제 타입으로 대체됨
- Box<T> 에서 T가 타입 매개변수

Type Argument (타입 인자)

- 제네릭 타입을 사용할 때  제공되는 실제 타입 (기본형은 사용할 수 없음)
- Box<Integer> 에서 Integer가 타입 인자

타입 매개변수로 자주 사용하는 키워드는 다음과 같음

- K : Key
- E : Element
- N : Number
- T : Type
- V : Value
- S,U,V etc : 2nd, 3rd,4th types



### 타입 추론

컴파일러가 타입 정보를 추론해 개발자가 타입 정보를 생략할 수 있음

```java
Box<Integer> box = new Box<>();	// 컴파일러에서 T 가 Integer임을 추론함
```



### 로 타입(raw type)

제네릭 타입임에도 불구 하고 <>으로 타입을 지정하지 않는 경우를 로 타입이라고 함

이런 경우 내부 타입 매개변수는 Object로 사용 됨

`보통의 경우 과거 코드와의 하위 호완을 위해 사용됨, 일반적인 경우 로 타입 사용 지양`

```java
Box gox = new Box();
```



### 제네릭 메서드

```java
public class Box<T> {
  
	public static <Z> Z staticMethod(Z z) {
    // do something..
  }
  
  public <T> T method(T t) {
    // do something..
  }
}
```

제너릭 메서드의 경우 `<T> T method(T t)` 와 같이 사용을 함

타입 인자 전달의 경우 메서드를 호출하는 시점에 인자 전달이 됨



### 와일드 카드

```java
public class WildCardMain {
  
  static<T extends Integer> void printGeneric(Box<T> box) {
    // do something...
  }
  
  static void printWildCard(Box<? extends Integer> box) {
    // do something...
  }
  
  static <T extends Integer> T printAndReturnGeneric(Box <T> box) {
    // do something...
  }
  
  static Integer printAndReturnWildcard(Box<? extends Integer> box) {
    // do somethign...
  }
}
```

와일드 카드는 제너릭 타입 혹은 제너릭 메서드를 선언 하는 것이 아님.

와일드 카드는 이미 만들어진 제네릭 타입을 활용할 때 사용



### 타입 이레이저

제네릭은 컴파일 단계에서만 사용되곡, 컴파일 이후에는 제네릭 정보가 삭제됨.

만약 매개변수 제한을 따로 두지 않았다면 Object, 매개변수 제한을 두었다면 제한한 타입으로 코드를 변경함

그렇기 때문에 new T(), instansce of T 와 같은 곳에서 사용할 수 없음
