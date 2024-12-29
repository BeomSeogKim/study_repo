## CAS (Compare And Swap)

### 원자적 연산

> 해당 연산이 더 이상 나눌 수 없는 단위로 수행되는 연산

```java
int i = 0;	// 원자적 연산	-- (1)
i = i + 1; 	// 비원자적 연산	-- (2)
```

(1) 의 경우 더이상 쪼개질 수 없는 연산이기 때문에 원자적 연산

(2)의 경우 다음과 같이 쪼개질 수 있음 -> 원자적 연산인 아님

- i 값 읽어오기
- 읽어온 i 값에 +1 을 더해서 값을 생성
- 생성된 값을 i에 대입

멀티스레드 환경에서 원자적 연산의 경우 안전한 반면, 비원자적 연산인 경우 안전하지 않음.



자바에서는 원자적인 연산을 수행할 수 있는 다양한 클래스를 제공 ( AtomicXxx 클래스)

```java
private AtomicInteger atomicIntger = new AtomicInteger(0);	
atomicInteger.incrementAndGet();
atomicInteger.get();
```

- new AtomicInteger(value)
  - value값으로 초기값을 지정함. (생략하면 0부터 시작)
- incrementAndGet()
  - 값을 하나 증가하고 증가된 결과를 반환
- get()
  - 현재 값을 반환

### CAS 연산

> 대부분의 복잡한 동시성 라이브러리의 경우 CAS 연산을 사용함
>
> 락을 걸지 않고 원자적인 연산을 수행하는 방법으로, lock-free 기법이라고도 함
>
> CAS연산의 경우 작은 단위의 일부 영역에 적용 가능 함

```java
private int incrementAndGet(AtomicInteger atomicInteger) {
  int getValue;
  int result;
  do {
    getValue = atomicInteger.get();
    result = atomicInteger.compareAndSet(getValue, getValue + 1);
  } while (!result);
  return getValue + 1;
}
```

락을 걸지 않고 CAS 연산을 통해 안정적으로 값을 변경할 수 있음

- 만약 다른 스레드에서 값을 변경해 getValue값이 메모리에서 읽은 값과 동일하지 않은 경우 재시도 

#### Lock vs CAS

Lock

- 비관적 접근법
  - 다른 스레드가 방해할 것이라고 가정
  - 데이터 접근 전 매번 락 획득
  - 다른 스레드의 접근 제한

CAS

- 낙관적 접근법
  - 대부분의 경우 충돌이 없을 것이라고 가정
  - 락을 사용하지 않고 데이터 접근
  - 충돌이 발생하면 그 때 재시도

#### CAS 단점

1. 충돌이 빈번한 경우 충돌일 발생 할 때 마다 CAS 연산 루프를 재시도 해야한다. 이에 따라 CPU 자원을 계속 소모할 가능성이 있음
2. CAS는 충돌 시 반복적인 재시도를 하므로, 이 과정이 계속 되면 스핀락과 유사한 성능 저하가 발생 할 수 있음