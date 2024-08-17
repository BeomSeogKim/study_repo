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
