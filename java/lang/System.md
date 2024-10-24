## System

시스템과 관련된 기본 기능들을 제공 

- 표준 입력, 출력, 오류 스트림 : 차례로 System.in, System.out, System.err
- 시간 측정 : System.currentTimeMillis(), System.nanoTime() 
- 환경 변수 : System.getenv()메서드를 사용해 OS에서 설정한 환경 변수의 값을 얻을 수 있음
- 시스템 속성 : System.getProperties() 혹은 System.getProperty(String key)를 통해 특정 속성 값을 얻을 수 있음
- 시스템 종료 : System.ext(int status) 메서드를 사용해 프로그램을 종료
  - status = 0 : 정상 종료
  - status ! 0 : 오류나 예외적인 종료
- 배열 고속 복사 : System.arraycopy - 시스템 레벨에서 최적화된 메모리 복사 연산을 사용.