## Reactor란 무엇인가?  
리액티브 프로그래밍을 위한 리액티브 라이브러리  
Reactive Streams 스펙을 구현한 구현체 중 하나   
리액티브 프로그래밍 : 비동기적이고 반응형으로 데이터 스트림을 처리하는 프로그래밍 모델     

주요 특징 
- Mono & Flux 
  - Mono : 0 또는 1개의 데이터를 처리하는 Stream 
  - Flux : 0 에서 N개의 데이터를 처리하는 Stream
- 비동기 처리 : 비동기 프로그래밍을 지원해 데이터 스트림을 효율적으로 처리할 수 있음
- 연산자 제공 : 데이터 스트림을 조작하고 결합하는 데 도움이 되는 다양한 연산자 제공
  - e.g) map, filter, buffer
- 스케쥴러 : 비동기 프로그래밍을 위해 사용되는 스레드를 관리해주는 역할을 함

Mono
> 0개 또는 1개의 데이터를 emit하는 Publisher  
> 데이터 emit 과정에서 에러가 발생하면 onError signal을 emit 함

![mono](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/doc-files/marbles/mono.svg)  
*출처 : projectreactor.io*

Flux 
> 0개 ~ N개의 데이터를 emit하는 Publisher  
> 데이터 emit 과정에서 에러가 발생하면 onError signal을 emit 함

![Flux](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/doc-files/marbles/flux.svg)  
*출처 : projectreactor.io*

### Cold Sequence vs Hot Sequence
Cold Sequence  
> cold sequence의 경우 subscriber가 구독을 할 때 마다 publisher가 emit한 모든 데이터들을 받을 수 있음  
> cold sequence의 경우 subscriber가 하나 씩 증가할 때 마다 타임라인이 증가함  
![Cold Sequence](https://projectreactor.io/docs/core/release/reference/images/gs-cold.png)  
*출처 : projectreactor.io*

Hot Sequence  
> hot sequence의 경우 subscriber가 구독한 시점의 타임라인 부터 emit된 데이터들을 받을 수 있음    
> subscribe 전에 emit 된 데이터들은 따로 받아 볼 수 없음    
![Hot Sequence](https://projectreactor.io/docs/core/release/reference/images/gs-hot.png)  
*출처 : projectreactor.io*

### Backpressure 
publisher에서 emit되는 데이터를 subscriber에서 안정적으로 처리하기 위한 제어기능  

Backpressure 전략
- IGNORE : Backpressure를 적용하지 않음
- ERROR : DownStream으로 전달해야하는 데이터가 버퍼에 가득찬 경우 Exception을 발생시킴
- DROP : DownStream으로 전달해야하는 데이터가 버퍼에 가득찬 경우, 버퍼 밖에서 대기하는 가장 먼저 emit된 데이터들을 drop
- LATEST : DownStream으로 전달해야하는 데이터가 버퍼에 가득찬 경우, 버퍼 밖에서 대기하는 가장 나중에 emit된 데이터를 버퍼에 채움
- BUFFER
  - DROP_LATEST : BUFFER내에 가장 최근에 emit된 데이터를 drop
  - DROP_OLDEST : BUFFER내에 가장 먼저 emit된 데이터를 drop

### Sinks
데이터를 발행하는 주체이자, Flux 또는 Mono 형태로 발행된 데이터를 구독자에게 제공할 수 있는 도구  
다양한 종류의 Sinks를 통해 데이터 발행 방식을 세밀하게 제어할 수 있음  

- Sinks.one() : 단일 값을 발행할 때 사용. 완료 또를 오류를 발행 할 수 있음
- Sinks.many().unicast() : 하나의 구독자에게만 데이터를 발행하는 unicast 방식
- Sinks.many().multicast() : 여러 구독자에게 데이터를 발행하는 multicast 방식
- Sinks.many().replay() : 여러 구독자에게 발행된 모든 데이터를 재발행 하는 replay 방식

### Scheduler 
Reactor에서 비동기 작업을 실행할 실행 컨텍스트를 정의 함  
이를 통해 특정 스레드 혹은 스레드 풀에서 작업을 실행하거나, 스레드 간 작업을 전환하는 등의 작업을 할 수 있음

Scheduler의 종류
1. Schedulers.immediate()
   - 동일한 스레드에서 즉시 실행됨
   - 주로 간단한 작업이나 추가적인 스레드 처리가 필요 없는 경우에 사용됨
2. Schedulers.single()
   - 단일 스레드에서 모든 작업을 처리함
   - 하나의 고정된 스레드로 비동기 작업을 처리하며, 동일한 스레드에서 모든 작업을 실행할 필요가 있을 때 사용됨
   - low latency 일회성 실행에 최적화 되어 있음
3. Schedulers.parallel()
   - 고정된 스레드 풀에서 작업을 처리함
   - 일반적으로 시스템의 가용 CPU 코어 수에 맞춰 스레드 풀이 생성되며, CPU 연산이 많은 작업에 적합함
   - Non-Blocking I/O 작업에 최적화 되어 있음
4. Schedulers.boundedElastic()
   - 유연한 스레드 풀을 사용해 필요할 때 마다 스레드를 생성하고 일정 시간 동안 유휴 상태가 되면 스레드를 제거
   - 블로킹 I/O 작업에 보통 사용되며 장시간 실행되는 작업에 적합함
5. Schedulers.newSingle()
   - 새로운 단일 스레드를 생성하여 작업을 처리함
   - 특정 작업을 고정된 하나의 스레드에서 처리하고자 할 때 사용됨
6. Schedulers.fromExecutorService(ExecutorService)
   - custom executorservice를 사용해 Scheduler를 생성할 수 있음
   - 의미있는 식별자를 제공하기 때문에 Metric에서 주로 사용 됨
