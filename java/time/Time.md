## 날짜 라이브러리

실제 날짜 및 시간을 직접 계산하기에는 실제로 매우 어렵고 복잡한 문제가 존재함

1. 날짜와 시간 차이 계산
   - 윤년, 각 달의 일수 등을 고려해 날짜 및 시간 차이를 고려해야 함
2. 윤년 계산
   - 현재, 4년마다 2월의 날짜에 하루를 추가하는 윤년이 도입되어 있음
   - 보통 2월은 28일까지 있지만 4년에 한번은 2월이 29일
3. 일광절약시간 변환 (Daylight Saving Time)
   - 보통 3월에서 10월은 태양이 일찍 뜨고, 나머지는 태양이 상대적으로 늦게 뜸
   - 이를 고려하여 시간을 1시간 앞당기거나 늦추는 제도를 일광 절약 시간제라고 한다.
   - 일광절약시간제는 국가나 지역에 따라 적용 여부, 시작 및 종료 날짜가 다름
4. 타임존 계산
   - 세계는 다양한 타임존으로 나뉘어 있으며, 각 타임존은 UTC로부터의 시간 차이로 정의됨

자바에서는 이런 복잡한 계산을 추상화하여 제공함

현재 우리가 사용하는 time 클래스들은 JDK 1.8부터 java.time package에 포함되어 나옴

![스크린샷 2024-10-29 23.00.41](/Users/tommy/github/study_repo/images/스크린샷 2024-10-29 23.00.41.png)



### LocalXxx

LocalXxx의 경우 세계 시간대를 고려하는 것이 아닌 현지 또는 특정 지역의 시간이 적용됨

- LocalDate : 날짜만 표현을 해야 할 때 사용 (ex 2024-11-01)
- LocalTime : 시간만 표현을 해야 할 때 사용 (ex 16:00:00.195)
- LocalDateTime : LocalDate 및 LocalTime을 합한 개념 (ex 2024-11-01T16:00:00.195)

###### 비교

- isBefore() : 현재 날짜와 시간이 비교대상보다 이전 일 경우 true
- isAfter() : 현재 날짜와 시간이 비교대상보다 이후 일 경우 true
- isEquals() : 다른 날짜 시간과 시간적으로 동일한 지 비교 (TimeZone 고려)
  - Asia/Seoul : 09:00시, UTC 00:00시 일 경우 isEquals()는 true

#### ZonedDateTime

타임존 안에 일광 절약 시간제에 대한 정보와 UTC로 부터 시간 차이인 오프셋 정보를 모두 포함하고 있음

ZonedDateTime은 다음과 같은 정보들을 가지고 있음

```java
public class ZonedDateTime {
  private final LocalDateTime dateTime;
  private final ZoneOffset offset;	// UTC로 부터의 시간 차이
  private final ZoneId zone;	// 일광 절약 시간제에 대한 정보
}
```

#### OffsetDateTime

ZoneId가 없기 때문에 일광 절약 시간제가 적용되지 않음

```java
public class OffsetDateTime {
  private final LocalDateTime dateTime;
  private final ZoneOffset offset;
}
```

#### Instant

1970년 1월 1일 0시0분0초(UTC)를 기준으로 경과한 시간을 초로 표현한 단위 (나노 초 포함)

```java
public class Instant {
  private final long seconds;
  private final int nanos;
}
```

#### Duration, Period

Duration

두 날짜 사이의 간격을 년, 월, 일 단위로 나타낸 것

Period 

두 시간 사이의 간격을 시, 분, 초 (나노초)로 나타낸 것 

#### 날짜 및 시간 핵심 인터페이스 

![스크린샷 2024-11-01 16.42.59](/Users/tommy/github/study_repo/images/스크린샷 2024-11-01 16.42.59.png)

TemporalAccessor 

- 날짜와 시간을 읽기 위한 기본 인터페이스
- 특정 시점의 날짜와 시간정보를 읽을 수 있는 최소한의 기능을 제공함

Temperal 

- 날짜와 시간을 조작 (추가, 빼기...)하기 위한 기능을 제공함

TemporalAmount

- 시간의 간격을 나타내며 날짜와 시간 객체에 적용하여 그 객체를 조정할 수 있음

#### 시간의 단위와 시간 필드

![스크린샷 2024-11-01 16.47.24](/Users/tommy/github/study_repo/images/스크린샷 2024-11-01 16.47.24.png)

TemporalUnit 

날짜와 시간을 측정하는 단위를 나타내며 ChronoUnit은 다양한 시간 단위를 제공함

TemporalField

날짜 및 시간을 나타내는 데 사용되며 ChronoField는 다양한 필드를 통해 날짜와 시간의 특정 부분을 나타냄

#### 날짜와 시간 문자열 파싱과 포맷팅 

파싱 : 문자열을 날짜와 시간 데이터로 변경하는 것 

포맷팅 : 날짜와 시간 데이터를 문자열로 변경하는 것

```java
public class Formatter {
  private final DateTimeFormatter formatter = DateTimeForamtter.ofPattern("yyyy년 MM월 dd일")
  public String format(LocalDate localDate) {
    return  localDate.format(formatter); // 2024년 11월 01일
  }
}
```

DateTimeFormatter의 패턴 공식 문서 : [Link](https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html#patterns)

















