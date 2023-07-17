# DHCP
- 들어가며
    - 4월 21일자 마스터 클래스 정리 도중 해야할 키워드 뽑다가 발견했습니다.
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/7526bcbc-66c1-43ef-8954-b2a20c08c47a)

## DDI
- DNS, DHCP 및 IP 주소관리 (IPAM)를 나타낸다.

## 일단 짧게 요약하고 지나가자면

점점 더 많은 기기들이 상호 연결되는 환경에서 필연적으로 발생하게 되는 문제들을 극복하기 위한 툴의 모음이다.

- DNS : Domain name system
    - 전화번호부
    - 사람이 쉽게 기억할 수 있는 도메인 이름을 IP주소로 변환한다.
- DHCP : Dynamic Host Configuration Protocol
    - 네트워크 내에서 IP주소를 동적으로 할당하는 매커니즘
    - 스마트폰 등 클라이언트가 DHCP서버에 IP주소를 요청하면, 서버는 자동으로 IP주소와 관련 매개변수를 할당한다. 클라이언트가 이 할당을 수락하면 내부 및 공용 인터넷을 이용할 수 있다.
- IPAM : IP address management
    - 기관 내부에서 IP 주소를 기획, 추적 및 관리하는 데 사용

# DHCP

> **Dynamic Host Configuration Protocol**
> 

> 동적으로 호스트의 IP를 설정하는 프로토콜
> 
> 
> IP주소를 관리해주는 프로토콜이다.
>

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/be93fccf-6a12-4ae1-b947-37c481e046c9)

- 클라이언트가 같은 네트워크 대역에서 IP를 관리해주는 서버를 말한다.
    - 특정 사용자가 IP가 없을 때, DHCP가 특정 기간동안 쓸 수 있는 IP를 **빌려주는** 개념이다. 사용자가 필요할 때마다 가져가서 쓰면 된다.
- 7계층의 프로토콜로, 서버-클라이언트 구조를 갖는다.

## 왜 수동으로 하면 안되는 거죠?

- 물론 수동으로도 가능하다. 홈 네트워크의 경우 수동 구성이 매우 쉽지만, 네트워크가 대규모라면 관리자는….?
- 만약 DHCP 서버가 없다면, 동일 IP를 사용하는 등 충돌이 날 수 있다.

## 이걸 어디에 쓰냐면…

- Client
    - 인터넷을 사용할 수 있는 모든 기기는 DHCP Client이다.
- 대규모 환경의 경우
    - 전용 컴퓨터를 사용한다. IP 주소 관리를 단순화함으로 비용을 절감하고 보안을 강화한다.

- 홈네트워크의 경우
    - 공유기(라우터)에 내장되어 있다. Wi-Fi를 사용한다면 일반적으로 라우터는 DHCP 서버이다.
    - wifi에 접속하면 자동으로 IP를 할당받게 된다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/751d42df-813a-4b7e-bcd5-b702bdca0b72)

- DHCP 서버는 DHCP 매개변수(옵션) 을 제공한다. DHCP 매개변수에소 IP주소의 다양한 정보를 확인할 수 있다.
- DHCP 매개변수는 다음 내용이 포함되어 있다.
    - 기본 게이트웨이 주소
    - 서브넷 마스크 (IP주소 내의 호스트 주소와 네트워크 주소를 분리)
    - DNS 서버

# DHCP Process

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/933fd4d8-9874-4f35-88dc-eaf49f19db48)

- 최소 4개의 메시지 교환을 통해 정한다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/2156a8b2-1851-41fd-ad76-686aeaba0300)

## DHCP Discover

- DHCP 클라이언트가 DHCP 서버를 발견하기 위해 보낸 메시지
- 새로운 노드가 네트워크에 연결되면 소스 주소가 `0.0.0.0`인 discover 메시지를 서버를 포함한 네트워크의 모든 노드에 브로드캐스팅 `255.255.255.255` 한다.
    - 현재 호스트는 자신이 접속될 네트워크 주소도 모르고 DHCP 서버의 주소도 모르기 때문이다.
    - 나머지 클라이언트들은 그냥 무시한다.
- 메시지를 수신한 DHCP 서버는 노드에 대한 서버 주소와 새 IP 주소를 포함하는 요청된 호스트에 offer 메시지를 반환한다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/4c72e060-026e-49c3-9ad8-5b279553928f)

## DHCP Offer

- 클라이언트에 필요한 고유한 IP 주소 및 기타 매개 변수를 주기 위해 DHCP 서버에서 보낸다.
    - 마찬가지로 아직 `255.255.255.255` 로 브로드 캐스팅한다.
- 이런 내용들이 포함되어 있다.
    - 수신된 메시지의 트랜잭션 ID
    - 클라이언트에게 제공된 IP 주소
    - 네트워크 마스크
    - IP 주소 임대 기간 (IP 주소가 유효한 시간)

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/62c326c0-7e19-4c34-ad30-822501e4cfb6)

- DHCP는 다른 기기가 어떤 IP를 사용하는 지 기억하고 있기 때문에 자동 부여가 가능한다.

## DHCP Request

- offer 메시지에 있는 매개 변수를 빌리도록 서버에 요청하는 DHCP 클라이언트에서 보낸다. (아직 ip가 없으므로 마찬가지로… 브로드캐스트 한다.)
- 이 메시지를 받은 서버는 할당할 주소가 사용가능한지 스토리지에서 여부를 확인한다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/11e2a085-a6cb-40b2-8544-bf5780130750)

## DHCP Ack

- IP 주소, 마스크, 게이트웨이 주소 및 DNS 서버 주소를 클라이언트에 할당하기 위해 DHCP 서버에서 보낸다.
- 주소가 할당되면 일관성을 보장하기 위해 저장소의 IP 주소를 사용할 수 없는 것으로 표시한다.
- 그 사이에 주소가 다른 기계에 할당되면 서버는 IP 주소가 다른 기계에 할당되었음을 나타내는 **`NAK`** 패킷을 요청된 클라이언트로 보낸다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/4b713d91-35ff-413a-abd2-58ce451c8868)

## Release

- 마지막으로 호스트가 다른 네트워크로 이동하기를 원하거나 작업이 완료되면 연결을 끊고 싶다는 release 패킷을 서버에 보낸다.
- 이 패킷을 받은 서버는 다른 시스템에 할당할 수 있도록 스토리지에서 사용 가능한 IP 주소를 표시한다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/d277e8d0-ab36-4a09-8a9c-48ec20af23a3)

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/8ce94499-3359-4d1c-8dc5-590bef1ac267)

