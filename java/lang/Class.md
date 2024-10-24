## Class

클래스의 정보 (메타데이터)를 다루는데 사용됨

주요 메서드 

- 타입 정보 얻기 : 클래스의 이름, 슈퍼클래스, 인터페이스, 접근제한자 등과 같은 정보를 조회 할 수 있음
- 리플렉션 : 클래스에 정의된 메서드, 필드, 생성자 등을 조회하고, 이들을 통해 객체 인스턴스를 생성하거나 메서드를 호출하는 등의 작업
- 동적 로딩과 생성 : Class.forName() 메서드를 사용해 클래스를 동적으로 로드하고, newInstance() 메서드를 통해 새로운 인스턴스 생성
- 어노테이션 처리 : 클래스에 적용된 어노테이션을 조회하고 처리하는 기능을 제공 

```java
//모든 필드 출력
Field[] fields = clazz.getDeclaredFields();
for (Field field : fields) {
    System.out.println("field = " + field.getType() + " " + field.getName());
}

// 모든 메서드 출력
Method[] declaredMethods = clazz.getDeclaredMethods();
for (Method declaredMethod : declaredMethods) {
    System.out.println("method = " + declaredMethod);
}

// 상위 클래스 정보 출력
System.out.println("Superclass : " + clazz.getSuperclass().getName());

// 인터페이스 정보 출력
Class[] interfaces = clazz.getInterfaces();
for (Class i : interfaces) {
    System.out.println("interfaces = " + i);
// 클래스 생성   
Hello hello = (Hello) clazz.getDeclaredConstructor().newInstance();
String result = hello.hello();
System.out.println("result = " + result);

```

