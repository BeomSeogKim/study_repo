### 스레드 생성

스레드 생성에는 크게 두가지 방식이 존재

1. Thread 클래스를 상속
2. Runnable 인터페이스를 구현

#### Thread 상속

> Thread를 호출하는 경우에 run을 호출하면 안됨
>
> run을 직접 호출하는 경우 main 스레드에서 실행을 하기 때문에 의도한 대로 동작 X
>
> start()를 호출해야 스택 공간을 할당받고 스레드가 작동함

```java
public class ThreadMain {
  
  public static void main(String[] args) {
    Thread thread = new MyThread();
    thread.start();	// run을 호출하면 안됨.
  }
  static class MyThread extends Thread {

    @Override
    public void run() {
      // do something...
    }
  }  
}

```

#### Runnable 구현

```java
public class RunnableMain {
  
  public static void main(String[] args) {
  	MyRunnable runnable = new MyRunnable();  
    Thread thread = new Thread(runnable, "my-runnable");
    thread.start();
  }
  
  static class MyRunnable implements Runnable {
    
    @Override
    public void run() {
      
    }
  }
}
```

비교

- Thread 상속
  - Thread 클래스를 상속받아 run() 메서드만 재정의 하면 되므로 구현이 간단함
  - 이미 다른 클래스를 상속받고 있는 경우 Thread 클래스를 상속받을 수 없음
  - 인터페이스 구현에 비해 유연성이 떨어짐
- Runnable 구현
  - 다른 클래스를 상속받아도 문제없이 구현 할 수 있음
  - 스레드와 실행할 작업을 분리하여 가독성을 높일 수 있음
  - 여러 스레드가 동일한 Runnable을 공유할 수 있어, 자원 관리를 효율적으로 할 수 있음
  - 스레드 생성시에 코드가 조금 더 복잡해짐



### Daemon Thread vs Non-Daemon Thread

- 사용자 스레드 (non-daemon)
  - 프로그램의 주요 작업을 수행
  - 작업이 완료될 때 까지 실행
  - 모든 사용자 스레드가 종료되면 JVM도 종료됨
- 데몬 스레드
  - 백그라운드에서 보조적인 작업을 수행
  - 모든 사용자 스레드가 종료되면 데몬 스레드는 자동으로 종료 됨

```java
public class ThreadMain {
  
  public static void main(String[] args) {
    Thread thread = new MyThread();
    thread.setDaemon(true); // 데몬 스레드 설정
    thread.start();	// run을 호출하면 안됨.
  }
  static class MyThread implements Runnable {

    @Override
    public void run() {
      // do something...
    }
  }  
}
```



### 스레드의 생명 주기

스레드의 상태

- NEW : 스레드가 생성되었으나 아직 시작되지 않은 상태
- RUNNABLE : 스레드가 실행 중이거나, 실행될 준비가 된 상태
- BLOCKED : 스레드가 동기화 락을 기다리는 상태
- WAITING : 스레드가 무기한으로 다른 스레드의 작업을 기다리는 상태
- TIME WATING : 스레드가 일정 시간 동안 다른 스레드의 작업을 기다리는 상태
- TERMINATED : 스레드의 실행이 완료된 상태



### join

join을 호출하는 스레드는 대상 스레드가 TERMINATED 상태가 될 때 까지 대기함

대상 스레드가 TERMINATED 상태가 되면 호출한 스레드는 다시 RUNNABLE 상태가 되어 다음 코드를 수행

- join() ; 호출 스레드는 대상 스레드가 완료될 때 까지 무한정 대기함
- join(ms) : 호출 스레드는 특정 시간 만큼만 대상 스레드를 기다림 

```java
public class JoinMain {
  
  public static void main(String[] args) throws InterruptedException {
  	Thread thread = new Thread(new MyRunnable(), "thread-1");
    thread.start();	// thread 실행 
    thread.join();	// thread의 실행을 기다림
//    thread.join(100);	// thread의 실행을 100ms 까지만 기다림
  }
  
  static class MyRunnable implements Runnable {
    
    @Override
    public void run() {
      // do something...
    }
  }
}
```



### interrupt

interrupt를 사용하면, WATING, TIME_WAITING 같은 대기 상태의 스레드를 직접 깨워 RUNNABLE 상태로 만들 수 있음

- Thread.currentThread().isInterrupted() : 인터럽트 상태 변경 X, 인터럽트 여부 확인
- Thread.isInterrupted() : 인터럽트 상태라면 상태 변경, 아니라면 false를 반환하고, 해당 스레드의 인터럽트 상태를 변경 X

```java
public class InterruptMain {
    public static void main(String[] args) throws InterruptedException {
  	Thread thread = new Thread(new MyRunnable(), "thread-1");
    thread.start();	// thread 실행 
		Thread.sleep(1000);
    thread.interrupt();	// thread 깨움
  }
  
  static class MyRunnable implements Runnable {
    
    @Override
    public void run() {
			try {
        Thread.sleep(3000);
        // do something        
      } catch (InterruptedException e) {
        log.info(e);
        // do something
      }
    }
  }
}
```



### yield

CPU 실행 기회를 양보하는 것

운영체제에서의 스케줄링은 다음과 같은 상태들을 가짐

- RUNNING : 스레드가 CPU에서 실제로 실행 중 
- READY : 스레드가 실행될 준비가 되었으며, CPU 스케줄링 큐에서 대기 중 

Thread.yield()를 하게 되면 해당 스레드가 자발적으로 CPU를 앙보하여 다른 스레드가 실행 될 수 있도록 함

```java
public class YieldMain {
    public static void main(String[] args) throws InterruptedException {
  	Thread thread = new Thread(new MyRunnable(), "thread-1");
    thread.start();	// thread 실행 
		// do something
  }
  
  static class MyRunnable implements Runnable {
    Queue<Integer> queue = new ArrayDeque();
    @Override
    public void run() {
			// do something
      if (queue.isEmpty) {
        Thread.yield();	// 스레드 양보
      }
    }
  }
}
```

