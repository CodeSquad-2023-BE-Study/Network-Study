# **시작하며**

- [스위치와 라우터](https://www.notion.so/a62467d2583b4985b49699de319991b1?pvs=21) 에서 **스위치는 왜 송신 포트와 수신 포트가 같으면 패킷을 폐기하는 지** 알아보다가 흘러흘러 패킷까지 옴.
- 결론적으로 루핑을 방지하고, 불필요한 패킷 전송을 방지하기 위함이다.

## 스위칭 허브가 받은 패킷에서 수신한 포트와 송신하는 포트가 같을 경우

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/d04d5369-9aae-4948-b99e-3185e3097fab)

- 만약 이상황에서 패킷을 폐기하지 않는다면 looping 에 걸리게 된다.
- 근데 왜 포트로 비교하냐면… LAN 어댑터에는 MAC주소가 할당되어 있는데, 스위칭 허브의 포트는 MAC주소가 할당되어 있지 않기 때문이 아닐까 (추측임).
    - 포트를 비교하여 바로 필요없는 패킷 전송/수신을 안하는 것.
    - 참고로 라우터의 포트에는 MAC주소가 할당되어 이더넷의 송신처나 수신처가 됩니다.
    - 그래서 라우터는 포트 부분에 할당된 MAC 주소가 수신한 패킷의 수신처 MAC 주소에 해당하는 경우에만 패킷을 수신합니다.
- 왜 이런 식으로 구성하게 되는걸까?
    - 안정성을 위해 2개 이상의 스위치로 연결시켜 놓고 싶어진다….. 스위치 이중화
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/c7b5d895-9540-40ef-8540-25328825c9b6)

## 루핑 looping

- 루핑이 발생하는 사례 1
    - 기본적으로 스위치는 스위치의 MAC Table이 초기상태일 경우
    - 프레임을 들어온 포트외의 모든 포트로 보내도록 되어있다(Flooding)
    - 이때, 목적지로의 경로가 2개 이상 존재하는 경우 계속해서 같은 프레임을 서로 주고받게 되어 트래픽이 증가하게 된다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/8b41fcde-f1fd-4e83-9bd2-8ae88c55e065)

- 특히 호스트 사이에 스위치 또는 브릿지가 2개 있다면 하나의 호스트에서 다른 하나로 가는 경로가 2개 이상 만들어진다….
- 루핑이 발생하는 사례 2
    - 호스트가 브로드캐스트 패킷(Broadcast packet)을 보내게 된다면
    - 이더넷의 특성상 같은 세그먼트에 있는 모든 장비에 브로드캐스트한다…
 
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/d187d26a-2e14-446b-bd37-f3aee70e9a0b)

- **루핑이 일어나면 무엇이 좋지 않나요?**
    - 전송되는 프레임이 급격하게 늘어난다.
    - 전체적인 네트워크 성능 저하
    - 버퍼 오버플로우로 필요한 패킷이 폐기되기도 한다.
    - MAC address table이 불안정하게 된다. 장비 CPU가 굉장히 과부하 될 수 있다.
    - 이중 Frame을 받으면서 프로그램 동작이 의도한바와 다를 수 있다.

<aside>
  
💡 스위칭 허브가 패킷을 폐기하는 사례들

- MAC 주소 불일치
- *[VLAN 제한](https://www.fibermall.com/ko/blog/what-is-vlan-and-how-it-work.htm) → 추가 공부가 필요한 것들*
- *[포트 상태](https://nhj12311.tistory.com/532)*
- 버퍼 오버플로우

</aside>

- L3 계층에서는 IP packet은 header에 TTL 이 있어서 Looping 을 막아준다.
- 한번 Loop이 발생하면 무한 loop이 발생할 수 있다..!

## 루핑 해결방법 STP

- 루프는 식별하는 것보다 방지하는 것이 항상 목표여야한다.
- 스패닝트리 프로토콜

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/bd0cda2d-6c9d-4948-98a4-816d66f03e49)

## 스패닝 트리 프로토콜 STP

- spanning Tree Protocol
- loop가 절대 발생하지 않는 구조 → tree

### 무엇을 해주나

- 룹이 발생하는 스위치 구조를 보고 한 포트를 사용하지 않게 한다면?
- 사용하지 않는 포트는 disable 시키는 것이 아니라 block port로 만든다.
    - 링크는 살아있다.
    - 다른 포트에서 들어온 패킷을 그 포트로 보내지 않고, 이 포트로 들어온 것을 다른 포트로 보내지 않으면 block된 것처럼 작동한다.
- 무엇을 block해야하나?

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/f7aee114-9646-4200-b8e6-11dc2598dc3b)

- Root로부터 가장 멀리 떨어진 Band width (cost) 가 큰 포트를 block한다.
- root는 누가 되는가?
    - root가 되는 기준은 Bridge ID가 가장 낮은 switch가 root가된다.
    - Bridge ID는 유일한(Unique)한 값. 불변의 48비트 System mac 주소를 사용하여 가장 낮은 값.
    - 그럼 당연히 제일 오래된 (낮은 값의) 것이 root가 되게 된다.
    - 사람이 추가로 관리를 하기 위해서 Manage 하는 Priority (16 bit) 를 추가하여 `Bridge ID`를 관리한다.
- 스위치끼리는 Bridge ID를 모두 공유하고, 이중 낮은 것을 root로 삼는다면 `상대적인 root` 가 나온다.
    - Root 가 던져준 것을 모두에게 전달

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/00381e29-b2a7-40b6-9c64-75f92f8feeed)

- 그래서 BPDU를 사용한다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/293c5f78-fcbf-4794-8f15-f7d81ef31c4d)

- Cost의 계산
    - 1계층의 장비인 케이블을 비교한다.
    - Bridge ID를 비교한다. 낮은 것이 더 우선순위
    - 만약 동일한 스위치에 2개 포트가 연결되어있고, 장비도 똑같다면?
        - 포트 ID(수동관리값 Priority 4bit + 사용하지 않는 4bit +유니크한 값 interface 8bit)가 더 낮은 것을 우선시한다.
     
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/cdb0784a-4e8a-4166-b3e1-24529be13452)

- 그럼 한쪽은 무조건 블럭시킨다.

### BPDU

- L2 헤더 + payload

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/6e5db62b-db8a-4e4a-8bab-c0bfaef609b0)
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/8d029b23-1ac9-4cf4-a614-caca0b2ee5b4)

- protocol Id (2byte) : 항상 0x0000. BPDU 프를 사용하는 게 스패닝 트리 프로토콜밖에 없음..
- Protocol version ID
    - 0x00 ~ 0x03, 0x04 까지 나왔다.
- BPDU type
    - config bpdu
        - root만 만듬.
        - flag를 적용시켜서 보내면 다른 switch 에게 전달
    - **tcn** : Topology change notification 트리구조에 변화가 발생했을 때. forwarding 으로 된 포트가 다운되었을 때 발생. root를 향해서 간다.
    - 등등…
- **Flags** : 지금 BPDU의 역할, 내 스위치 상태에 대한 표시.
    - 0 topology change ack
        - tcn bpdo에 응답으로 ack
    - 1 aggrement
    - 2 forwarding
    - 3 learning
    - 4~5 port role
    - 6 proposal
    - 7 topology change
- Root id, Root path cost, bridge id, port Id
- Message age : 최초 bpdu가 만들어지고 얼마나 지났는지
- Max age : aging time. 동일한 값이 되면 bpdu는 사라진다. Default 20초
- hello time : 몇초에 한번씩 bpdu를 보내는가. default 2초
- forward delay
    - block - | listening → learning → forwarding 다음 단계로 전환되는 딜레이시간 default 15초
- 왜 default time 이 이렇게 default가 설정되었는가? → 추가 학습이 필요
    - hello : 2초
    - forward delay : 15초
    - max age : 20초

### 동작 과정

- Root switch 선출
    - Bridge ID를 확인한다.
- root 까지 얼마나 걸리는지 경로값 계산
- root port 선출
    - 가장 작은 경로값을 가지는 포트를 root port로 사용.
- 단순 포트 포워딩 designated port 선출
- alternated port 선출 (block 포트)
    - 포트로 데이터 통신을 하지 않는 포트
    - BPDU는 수신한다.
 
