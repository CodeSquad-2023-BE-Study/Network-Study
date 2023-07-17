### **HTTPS란?**

HTTPS는 암호화 기능이 있는 HTTP이다. HTTPS는 TLS(SSL)을 이용해서 일반 HTTP 요청, 응답을 암호화 합니다. HTTP는 일반 텍스트로 요청, 응답을 송수신하므로 모니터링 시 모든 사람이 텍스트를 읽을 수 있다는 문제가 있습니다.

HTTPS는 텍스트를 읽지 못하도록하는 통신 암호화의 기능을 수행하며, 두 통신 행위자를 인증하는데도 사용됩니다. 신분증으로 신원확인하는 것처럼, 개인 키로 서버의 신원을 확인할 수 있는 것입니다. 클라이언트가 웹 사이트에 접속할 때, 웹 사이트의 SSL 인증서에 있는 공개 키와 일치하는 개인 키를 확인하여, 해당 서버가 실제 웹 사이트의 합법적 호스트임을 증명합니다.

### **TLS(SSL)?**

Transport Layer Security

전송 계층의 보안으로 기존 SSL(Secure Socket Layer)에 기반한 프로토콜로, 상호간 적당한 사용자인지 인증서를 통해 검증하고 서로 약속한 암호화 알고리즘을 통해 키를 교환한 후 암호화 통신을 진행하는 방식. 공개키 암호화 방식을 사용한다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/112251635/d43c23ed-56db-4bd2-a573-de2632ddf735)



**암호화** : 제3자로부터 전송되는 데이터를 숨긴다.

**인증** : 정보를 교환하는 당사자가 요청된 당사자임을 보장한다.

**무결성** : 데이터가 위조되거나 변조되지 않았는지 확인한다.

**TLS Handshake**

HTTPS에서 웹 사이트에 접속할 때, 클라이언트와 웹 서버간 TLS Handshake가 일어난다. 데이터를 주고 받기 전, 서버의 무결성을 확인하고 대칭키를 전달하는 과정이 TLS Handshake이다.

![image](https://github.com/CodeSquad-2023-BE-Study/Network-Study/assets/112251635/100d7e29-3601-4946-a180-26494c91fc90)


Client Hello

클라이언트가 서버에 Hello 전송, 이 때, 대칭키를 생성하기 위한 Client Random Data, 암호화 방식, TLS 버전 등 정보를 포함

Server Hello

TLS Version, 암호화 방식(Client가 보낸 암호화 방식 중에 서버가 사용 가능한 암호화 방식을 선택), Server Random Data(서버에서 생성한 난수, 대칭키를 만들 때 사용), SessionID(유효한 Session ID)

Server Certificate

서버의 인증서를 클라이언트에게 보낸다. 이 때, 이 인증서는 인증기관의 개인키로 암호화 되어있다. (이는 브라우저가 가지고 있는 공개키로 복호화 할 수 있다.)

Server Hello Done

Client Key Exchange

인증서의 무결성을 검증한 후, 1,2에서 받은 클라이언트+서버 난수를 조합해 대칭키를 생성한다. 그리고 대칭키를 방금 해독한 서버의 공개키로 암호화한다. 암호화한 정보를 서버에 전송한다.

이 정보를 pre-master secret 이라고 하며 대칭키에 사용되는 것으로 절대 노출이 되어서는 안된다. pre-master secret은 난수를 조합하여 생성한 것이다. 이 값을 서버의 공개키로 암호화해서 전송하면 서버는 개인키로 복호화가 가능하다. 그렇게 되면 서로가 pre-master secret을 공유하게 된다. 이 값을 사용해서 세션에 사용될 키를 생성하게 되는데, 이 키가 바로 대칭키이다.

Change Cipher Spec

이제부터 전송되는 모든 패킷은 협상된 알고리즘과 키를 이용해 암호화한다는 메시지.

Finished

Handshake가 완료되고 나면 Handshake 과정을 통해 협상된 알고리즘과 키로 세션 키를 생성한다. 즉, 하나의 통신 세션을 암호화하는 대칭키를 통해 이제 통신을 주고 받는다.

**꼬리질문**

HTTPS는 대칭, 또는 비대칭 암호화 방식을 사용하는가?

결론은, TLS Handshake에서는 비대칭 암호화 방식을 사용하며, 그 이후 설정된 세션 키로는 대칭 암호화로 통신을 진행한다.

대칭 암호화 방식의 문제점은 최초의 비밀키를 안전하게 주고 받기 어렵다는 점인데, 이를 개선하기 위해 비대칭 암호화 방식을 이용하는 것이다.

TLS 프로토콜과 결합된 HTTP인 HTTPS는 두 가지 유형의 암호화를 모두 사용한다. TLS를 통한 모든 통신은 TLS 핸드셰이크로 시작됩니다. 비대칭 암호화는 TLS 핸드셰이크가 작동하도록 하는 데 중요한 요소이다.

TLS 핸드셰이크가 진행되는 동안 두 개의 통신 장치가 세션 키를 설정하고 세션의 나머지 부분에서 대칭 암호화에 사용된다(장치가 세션 중에 키를 업데이트하도록 선택하지 않는 한). 일반적으로 두 통신 장치는 클라이언트 또는 랩톱이나 스마트폰과 같은 사용자 장치와 웹 사이트를 호스팅하는 웹 서버인 서버이다.

TLS 핸드셰이크에서 클라이언트와 서버는 다음을 수행한다.

사용할 암호화 알고리즘 협상(비대칭 암호화를 통해 안전하게 수행)

TLS 인증서에 대해 서버 ID 인증(비대칭 암호화 사용)

[https://sunrise-min.tistory.com/entry/TLS-Handshake는-어떻게-진행되는가?category=1055346](https://sunrise-min.tistory.com/entry/TLS-Handshake%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%A7%84%ED%96%89%EB%90%98%EB%8A%94%EA%B0%80?category=1055346)