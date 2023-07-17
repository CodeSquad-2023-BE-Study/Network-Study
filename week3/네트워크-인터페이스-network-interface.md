# Network interfaces

- 서버와 서버의 연결을 어떻게 할 것인가?
- 네트워크에서 interface란 다른 디바이스로부터 데이터를 받는 물리적인 포트 (유선, 무선 등등)로 접속 포인트를 말한다.

<aside>
💡 **인터페이스**(interface)는 서로 다른 두 개의 시스템, 장치 사이에서 정보나 신호를 주고받는 경우의 접점이나 경계면이다.

</aside>

- 어떤 면에서 인터페이스는 규약, 약속이라고 할 수 있다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/70c95c85-a06f-4a31-ad19-185151016a23)
![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/103120173/b32ad27e-fd69-4e88-98ca-c104d9bb37d9)

- 네트워크는 여러개의 호스트가 서로서로 연결되어 있는데, 이 호스트를 찾는 방법은 호스트가 가진 포트의 IP주소를 아는 것이다.
- 장치는 한 개의 네트워크 인터페이스를 가지는 것이 아니다.
    - 이 각각의 네트워크는 IP 주소를 별도로 가진다.
- 각각의 인터페이스는 독립적인/글로벌 적인 설정값을 가지게 된다.
    - 즉, 같은 컴퓨터에 연결된 인터페이스라도 각각의 네트워크는 별도로 취급되며 공유되지않는다.
- 특수한 IP
    - 127.0.0.1 loopback address
    - 0.0.0.0 broadcast address

# 프로토콜

- 통신 규약
- 인터페이스간 통신을 할 수 있기 위해서는 같은 프로토콜을 가질 수 있을 때 가능

# 포트

- well known port
    - 80 : http
    - 22 : ssh
    - 21 : ftp
- 포트 번호를 바꿔 같은 프로토콜의 서비스를 할 수 있다.

---
# Refs.
- [Understanding Network Interfaces](https://youtu.be/PYTG7bvpvRI)
- [TCP/IP 네트워크 인터페이스](https://www.ibm.com/docs/ko/aix/7.1?topic=protocol-tcpip-network-interfaces)
