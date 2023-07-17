# 이더넷 Ethernet

- TCP/IP 모델에서 네트워크 인터페이스층의 프로토콜
    - OS 7layers에서는 1~2단계를 말한다.
- 이더넷은 컴퓨터 네트워크를 위한 LAN, WAN에서 가장 많이 활용되는 기술 규격
- 이더넷은 LAN을 구성하는 데 가장 많이 사용되는 기술 규격. 이더넷은 다양한 장비와 호환되기 때문에 LAN을 구성하는 데 적합하다.
    - LAN : Local Area Network
    - 로컬 영역 네트워크를 말한다.
 
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/c8b0bf56-e160-46ac-892a-e45fbdd025c9)

- link 레이어에서의 데이터 송수신에 대한 protocol

# Physical layer

## Devices

- 스위치와 라우터
    - 스위치 : 네트워크 단위를 연결하는 통신장비. 허브와 유사하지만 훨씬 빠른 전송속도를 가진다.
    - 라우터 : 네트워크 간의 중계 역할. 다른 네트워크끼리 연결한다. (네트워크 계층 장비)
- 게이트웨이와 브리지
    - 게이트웨이 : 서로 다른 네트워크간 통신을 할 수 있도록 한다. 전계층에 걸쳐 동작할 수 있다.
    - 브릿지 : 데이터가 충돌할 공간 (충돌 도메인)을 나눠주는 역할을 한다. 하나의 네트워크 내부에서 동작한다.

## Cables
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/bc8bc400-74e6-4c48-898a-a9914c9cd873)
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/baf61604-ccb6-4ef4-b93e-fd9a7fa55e0c)

# Data Link layer

## LLC : Logical Link Control

- 장치간 전송할 이더넷의 데이터 경로를 설정한다.

## MAC : Media Access Control

- NIC에 할당된 하드웨어 주소를 사용하여 데이터 전송의 소스 및 대상을 표시. 특정 컴퓨터나 장치를 식별한다.
- NIC : Network Interface card
- MAC 주소는 48 비트로 포현된 식별자를 이용

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/7deff9da-3d0d-414a-b610-050d976e7dc4)
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/e1229c5d-fda6-4aad-9cf8-081fbbd86ed0)

### 사용하는 알고리즘들

- 이 알고리즘을 사용하여 데이터 링크 계층에서 데이터 패킷을 전송한다.
- **CSMA : Carrier Sense Multiple Access with Collision Detection**
    - 다른 호스트에서 전송 채널을 사용하는지 확인하는 신호 감지 프로토콜
- **CSMA/CD**
    - CSMA + 충돌 감지 기능이 추가된 신호 감지 프로토콜
    - 데이터 충돌을 줄이고, 성공적으로 패킷을 전송하기 위함
- **알고리즘 순서**
    - 네트워크에 트래픽이 있는지 확인
    - 충돌이 발생하는지 확인하려고 첫번째 정보를 보낸다. bit
    - 이 비트가 성공하면, 두번째 정보를 보낸다.
    - 만약 충돌이 발생하면 조금 기다린 후, 다시 프로세스를 처음부터 다시 시작한다.
- 근데 더이상 사용하지 않는다.
    - 현재는 star topology를 많이 사용하는데, 여기에 전이중 장치는 데이터 충돌이 발생하지 않는 구조.
    - 전이중 스위치 네트워크에는 더 이상 CSMA/CD가 필요하지 않으며, 각 링크가 분리되어 충돌이 제거된다.
    - 반이중 링크는 더 이상 1Gbps 이더넷에서 지정되지 않으며, 10Gbps에서는 전혀 허용되지 않고 있다.
    - 802.11 Wi-Fi는 반이중 특성으로 인해 충돌 문제가 있지만, 대신 관련 CSMA/충돌 회피 방법을 사용한다.
 
### 이더넷 성능에 대하여
- 이더넷 스위치 구조 토폴로지 Topology에 따라 성능이 달라진다.
    - 초기 이더넷 : 버스형을 주로 사용
    - 현재 이더넷 : 스타형
  
    ![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/84f1eb3d-74b1-499a-af25-d9583af03765)
- 피지컬 레이어의 조건에 따라도 많이 달라진다.

### 전이중 장치, 반이중 장치

- 전이중 장치 : 송수신 케이블을 별도로 둠
- 반이중 장치 : 하나로 둠

## Mac 주소 테이블

- 브릿지 테이블 bridge table 이라고도 함.
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/a361387c-b1b1-4df9-bfc6-3882e19fee77)

- Data Flooding : 모두에게 다 전송. 수신되는링크 만을 제외시킨 채, 패킷을 나머지 모든링크에로 단순하게 복사전송한다.
- LAN에서 Flooding 이 일어나는 이유
    - 목적지 주소가 멀티캐스트 혹은 브로드캐스트 형태
    - 목적지 주소가 MAC 주소 테이블에 존재하지 않음
    - 미인식 프로토콜
    - 프레임 버퍼 메모리가 가득 찼을 경우
- Data Fowarding : 목적지 주소에 대응된 출력 포트로 패킷을 전달한다.
- Data Filtering : 패킷을 다른 MAC주소로 넘어가지 못하게 막는다.

### 이더넷의 장점

- 속도가 매우 빠르다.
- 비용이 저렴하다.
- 설치가 편리하다.
- 컴퓨터 시장에서 수용력이 넓다.
- 거의 모든 네트워크 프로토콜을 지원할 수 있다.

### 이더넷의 단점

- 토폴로지가 취약하고
- 충돌이 발생하면 네트워크 지연된다.
