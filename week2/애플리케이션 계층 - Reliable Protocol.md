## RDT 1.0 : 이상적 환경

패킷 에러와 유실이 발생하지 않는다.

## RDT 2.0 : 패킷 에러가 발생할 수 있음

- 에러감지 : 체크섬 비트를 더한다.
- 피드백 : ACK / NAK
- 재전송 : NAK을 받으면 패킷을 재전송한다.

### 문제점

- Receiver 입장에서 ACK과 메시지를 구별하는 방법?
- 피드백에 에러가 발생하면?
    - syn-ack을 하나 더 보낸다. → 3 way handshake

## RDT 2.1 : Handling Duplicate Packets

### Sender

- seq number를 packet에 추가한다.
    - Header의 부가적인 정보이므로 최소화시켜야한다.
- ACK/NAK의 체크섬을 확인해서 오류가 있는지 확인한다.
- NAK이거나 오류가 있는 feedback이면 재전송한다.

### Receiver

- 패킷이 복사된 것인지 확인한다.(0,1,0,1이 반복되는지)
- Sender가 보낸 체크섬을 확인해서 오류가 있으면 NAK을 보낸다.

## RDT 2.2 : NAK Free protocol

- NAK 대신 가장 마지막으로 받은 정상적인 시퀀스 번호를 보낸다.

## RDT 3.0 : 패킷 유실이 발생할 수 있음

유실되는 경우 아예 피드백이 돌아오지 않음

- 서로 기다리는 상태.
- Timer를 이용해서 Packet loss를 감지할 수 있다.
    - Sender가 응답이 오지 않으면 재전송한다.
        - 전송 데이터가 유실? ACK가 유실? → 동일하게 Sender가 재전송하지만 Receiver 입장에서 ACK가 유실된 경우는 데이터는 버리고 ACK만 재전송한다.
- 타이머의 시간을 얼마로해야할까? → 정답이 없는 문제
    - 타이머가 짧으면 → 패킷 로스 시 반응이 빠르다.
    - 길면 → 지연인데도 중복 패킷이 전송되어 네트워크 오버헤드가 커진다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/112251635/a9863e50-3430-44f3-80c1-0118482de72f)


### 실제로는?

- 한번에 많은 데이터를 Pipelined protocols 방식으로 전송한다.

![image2](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/112251635/b865e01a-b69d-406d-85ed-c168ef09355e)


언제부터 패킷이 잘못되었을지? 어떻게 알까요
