# VPC
AWS에서 제공하는 상용 클라우드 컴퓨팅 서비스로, 논리적으로 격리된 가상 네트워크 환경을 제공함  
VPC를 통해 사용자가 네트워크 환경을 제어할 수 있으며, 다양한 네트워크 설정을 사용자 정의하여 사용할 수 있음  
다양한 환경을 설정할 수 있으나, 그 중 대표적인 설정 메뉴들은 다음과 같음  

- 서브넷
- 인터넷 게이트웨이
- NAT 게이트웨이
- 라우팅 테이블 
- 보안그룹
- 네트워크 ACL 

## 서브넷
VPC 내의 IP 주소 범위를 지정해 네트워크를 세분화 하는 논리적 단위  
서브넷은 특정 가용 영역(Availability Zone)내에 존재하며, 여러 가용 영역에 걸쳐 확장할 수 없음  
서브넷을 통해 사용자는 네트워크 트래픽을 세분화하고, 리소스를 격리하며, 보안과 접근 제어를 보다 세밀하게 관리 할 수 있음  

*가용영역 : AWS region 내에 존재하는 독립적이고 격리된 데이터 센터 그룹*

주요 특징
- IP 주소 범위
  - 서브넷은 VPC의 IP 주소 범위 (CIDR 블록) 내에서 정의됨
- 서브넷 유형
  - 퍼블릭 서브넷 
    - 인터넷 게이트웨이로 직접 연결되는 통로가 있는 서브넷으로, 인터넷에 직접 접근할 수 있음
  - 프라이빗 서브넷
    - 인터넷 게이트웨이로 직접 연결되지 않는 서브넷, NAT 게이트웨이를 통해서만 인터넷에 접근할 수 있음
  - VPN 전용 서브넷
    - 사이트 간 VPN 연결을 통해서만 접근 가능한 서브넷, 인터넷 게이트웨이로의 경로가 없음
  - 격리된 서브넷
    - VPC 외부로의 경로가 없는 서브넷으로, 동일한 VPC 내의 다른 리소스와만 통신 할 수 있음

## 인터넷 게이트웨이
AWS VPC와 인터넷 간의 통신을 가능하게 하는 구성 요소  
수평 확장 가능하고, 고가용성을 제공하는 중복 가능한 VPC 컴포넌트로, VPC내의 리소스가 인터넷과 통신할 수 있도록 함

주요 특징 
- 인터넷 연결 
  - 인터넷 게이트웨이를 통해 VPC 내의 퍼블릭 서브넷에 있는 리소스가 인터넷에 접근할 수 있다. (반대로도 마찬가지)
- 라우팅
  - VPC 라우팅 테이블에서 인터넷 라우팅 가능 트래픽에 대한 대상을 제공함

## NAT 게이트웨이 (Network Address Translation)
VPC 내의 프라이빗 서브넷에 있는 인스턴스가 외부 인터넷이나 다른 AWS 서비스에 접근할 수 있도록 하면서, 외부에서 프라이빗 서브넷으로 직접적인 연결을 차단하는 역할을 함

주요 특징
- 프라이빗 서브넷의 인터넷 접근
  - 프라이빗 서브넷의 인스턴스가 인터넷에 접근할 수 있도록 함
  - e.g) 소프트웨어 업데이트, 외부 API 호출이 필요할 때
- 보안 강화
  - NAT 게이트웨이를 통해 외부에서 프라이빗 서브넷으로의 직접적인 인바운드를 차단할 수 있음


| 특성     | 인터넷 게이트웨이                    | NAT 게이트웨이                    |
|--------|------------------------------|------------------------------|
| 기능     | VPC와 인터넷 간의 양방향 통신           | 프라이빗 서브넷의 리소스가 인터넷에 접근 가능    |
| 트래픽 흐름 | 양방향 (인바운드 및 아웃바운드)           | 단방향 (아웃바운드만 허용)              |
| NAT 기능 | 공인 IP 주소를 할당받은 인스턴스에 대해 수행   | 프라이빗 IP 주소를 공인 IP 주소로 변환     |
| 보안     | 퍼블릭 서브넷의 리소스는 인터넷에서 직접 접근 가능 | 프라이빗 서브넷의 리소스는 외부에서 직접 접근 불가 |

## 라우팅 테이블
AWS VPC에서 라우팅 테이블은 VPC 내의 서브넷이나 게이트웨이의 네트워크 트래픽이 어디로 전송될지를 결정하는 규칙세트를 포함함

주요 구성 요소
- 기본 라우팅 테이블
  - VPC와 함께 자동으로 제공되는 라우팅 테이블
- 사용자 지정 라우팅 테이블
  - 사용자가 VPC에 대해 생성하는 라우팅 테이블 특정 서브넷이나 게이트웨이와 연결 할 수 있음
- Destination
  - 트래픽을 이동할 대상 IP 주소 범위 (CIDR 블록)
- Target
  - 트래픽을 전송할 때 사용할 게이트웨이, 네트워크 인터페이스,
