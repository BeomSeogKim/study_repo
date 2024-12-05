## Synchronized

### 임계 영역 (critical section)

여러스레드가 동시에 접근해서는 안되는 공유 자원을 접근하거나 수정하는 영역을 의미함

### syncrhonized

syncrhonized 키워드를 통해 임계영역을 보호 할 수 있음

모든 인스턴스는 내부에 고유의 lock을 가지고 있음 (monitor lock)

스레드가 synchonized 키워드가 있는 메서드에 진입하려면 반드시 모니터 락을 가지고 있어야 함

만약 스레드가 해당 메서드에 접근하고자 할 때 모니터 락이 없다면 `BLOCKED` 상태로 대기 함

```java
public class Calculate {
  private int value;
  
  public synchronized boolean add (int addValue) {
    // do something
    value += addValue;
    // do something
  }
  
  // synchronized 코드 블럭
  public boolean multiply(int multiplyValue) {
    // lock 이 필요하지 않은 코드 실행
    synchronized(this) {
      // do something
      value *= multiplyValue;
    }
        // lock 이 필요하지 않은 코드 실행
  }
  
}
```

