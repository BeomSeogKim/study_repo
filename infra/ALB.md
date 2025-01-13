## ALB (Application Load Balancer)

> OSI 7계층 (애플리케이션 계층)에서 동작하는 로드 밸런서로, HTTP/HTTPS 트래픽을 처리하고 URL 기반의 트래픽 라우팅이 가능함

특징

- 애플리케이션 계층 라우팅 : URL 경로, 도메인, HTTP 헤더, 쿼리 매개변수 등을 기준으로 트래픽을 라우팅
- 웹소켓 지원
- Target Group : 특정 조건에 따라 다른 Target Group으로 트래픽 전달
- 컨테이너 환경에 최적화 : Amazon ECS 또는 쿠버네티스와의 통합을 통해 각 컨테이너의 IP로 트래픽 분산 가능.
- SSL/ TLS 종료 : HTTPS트래픽을 처리하며 TLS 종료를 통해 백엔드 서버의 부담을 줄임



> SSL / TLS 종료 
>
> 로드 밸런서에서 SSL / TLS 암호화 통신을 해독(복호화)하는 작업을 수행하고, 그 이후에는 http 통신을 하는 것
>
> 동작 원리
>
> 클라이언트가 HTTPS 요청을 보냄 -> 로드 밸런서가 클라이언트와 SSL/TLS 핸드셰이크 수행 -> 로드밸런서에서 백엔드 서버로 전달



### ELB(Elastic Load Balancer)

> AWS에서 로드 밸런싱 서비스를 포괄적으로 지칭하는 용어 ALB도 ELB의 한 유형임

ELB 종류

1. CLB (Classic Load Balancer)
   1. ELB의 가장 초기 형태
   2. OSI 4계층과 7계층을 모두 지원하지만 제한된 기능 제공
   3. 현재는 주로 사용 X 
2. ALB (Application Load Balancer)
   1. 애플리케이션 계층 로드 밸런서
3. NLB (Network Load Balancer)
   1. OSI 4계층에서 동작
   2. 대량의 TCP/UDP 트래픽을 처리할 때 성능이 우수함
   3. 초저지연 트래픽 처리에 적합