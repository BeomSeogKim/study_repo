## String

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence,
               Constable, ConstantDesc {
    @Stable
    private final char[] value;	// java 9 이전
    private final byte[] value;	// java 9 이후
}
```

String 클래스는 불변 클래스

String 클래스는 내부적으로 byte[] 배열에 저장됨

> Java 9 이전 까지는 char 배열에 저장을 했지만 Java 9 이후로는 byte 배열에 저장 
>
> Latin-1 인코딩의 경우 1byte 사용, 그 외 2 byte사용을 통해 효율적인 메모리 사용이 가능해짐

### String 생성 방법

String을 생성하는 방법에는 크게 두가지가 있음

- 리터럴(literal) 
- 객체 생성 

리터럴 방식

```java
String string = "hello";
```

리터럴 방식을 사용하는 경우 String Pool에 있는 값을 사용

- 자바가 실행되는 시점에 문자열 리터럴이 있는 경우 String Pool에 인스턴스를 미리 만들어 둠
- 만약 String Pool에 이미 있는 경우 있는 값을 사용
- String Pool은 **힙 영역**에 저장됨
- 문자를 찾을 때 해시 알고리즘을 사용하기 때문에 빠른 속도로 인스턴스를 찾을 수 있음

객체 생성 방식 

```java
String string = new String("hello");
```

객체 생성 방식을 사용하는 경우 동일한 문자열이 있어도 매번 새로운 객체가 **힙 영역**에 저장됨

### 주요 메서드 

문자열 정보 조회

- length() : 문자열 길이를 반환
- isEmpty() : 문자열이 비어 있는 지 확인(길이가 0)
- isBlank() : 문자열이 비어 있는 지 확인(길이가 0 혹은 공백만 있는 경우)
- charAt(int index) : 지정된 인덱스에 있는 문자를 반환 

문자열 비교

- equals(Object anObject) : 두 문자열이 동일한지 비교
- equalsIgnoreCase(String anotherString) : 두 문자열을 대소문자 구분 없이 비교
- compareTo(String anotherString) : 두 문자열을 사전 순으로 비교
- compareToIgnorecase(String str) : 두 문자열을 대소문자 구분 없이 사전 순으로 비교
- startsWith(String prefix) : 문자열이 특정 접두사로 시작하는 지 확인
- endsWith(String suffix) : 문자열이 특정 접미사로 끝나는 지 확인

문자열 검색

- contains(CharSequence s) : 문자열이 특정 문자열을 포함하고 있는 지 확인
- indexOf(String ch) / indexOf(String ch, int fromIndex) : 문자열이 청음 등장하는 위치를 반환
- lastIndexOf(String ch) : 문자열이 마지막으로 등장하는 위치를 반환

문자열 조작 및 변환 

- substring(int beginIndex) / substring(int beginIndex, int endIndex) : 문자열의 부분 문자열을 반환
- concat(String str) : 문자열의 끝에 다른 문자열을 붙임
- replace(CharSequence target, CharSequence replacement) : 특정 문자열을 새 문자열로 대체
- toLowercase() / toUpperCase() : 문자열을 소문자나 대문자로 변환 
- trim() : 문자열 양쪽 끝의 공백을 제거
- strip() : Whitespace와 유니코드 공백을 포함해 제거 (Java 11부터 지원)

문자열 분할 및 조합

- split(String regex) : 문자열을 정규 표현식을 기준으로 분할 
- join(CharSequence delimiter, CharSequence... elements) : 주어진 구분자로 여러 문자열을 결합.

기타

- valueOf(Object obj) : 다양한 타입을 문자열로 변환
- toCharArray() : 문자열을 문자 배열로 변환
- format(String format, Object... args) : 형식 문자열과 인자를 사용해 새로운 문자열을 생성
- matches(String regex) : 문자열이 주어진 정규표현식과 일치하는 지 확인 

### 가변 String

String은 기본적으로 불변클래스이나 변화 가능한 클래스를 제공함 (StringBuilder, StringBuffer)

```java
AbstractStringBuilder implements Appendable, CharSequence {
    /**
     * The value is used for character storage.
     */
    byte[] value;	// not final -> changeable
}
```

|             |           StringBuilder           |           StringBuffer            |
| :---------: | :-------------------------------: | :-------------------------------: |
| 동기화 여부 | X<br />멀티스레드 환경에서 안전 X |  O<br />멀티스레드 환경에서 안전  |
|    성능     |               빠름                |               느림                |
|    용도     | 단일 스레드 환경에서 빠르게 조작  | 멀티스레드 환경에서 안전하게 조작 |

### String 최적화

자바 컴파일러는 문자열 리터럴을 더하는 부분을 자동으로 합침

```java
String helloWorld = "hello " + "World!";	// 컴파일 전 
String helloWorld = "hello World!"; 	//컴파일 후 
```

문자열 변수의 경우 어떤 값이 저장될 지 알 수 없기 때문에 StringBuilder를 사용함

```java
String str = str1 + str2;	//컴파일 전 
String str = new StringBuilder().append(str1).append(str2).toString();	// 컴파일 후
```

**주의 사항**

> 문자열을 반복문에서 더하는 경우 최적화가 이루어지지 않음
>
> 따라서 반복문 내에 문자열을 처리해야하는 경우에는 반복문 밖에서 가변 String을 정의 후에 문자열을 합치는 것을 권장
