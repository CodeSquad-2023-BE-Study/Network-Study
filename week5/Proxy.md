# Proxy에 관하여
nginx를 학습하기전 알아야할  Forward Proxy , Reverse Proxy,Load Balancer 기능들에 대해 학습합니다. 또한 Cash에 대해서도 간단하게 알아보겠습니다.

# Proxy Server
## Proxy Server란?
프록시 서버는 정보를 찾는 사용자와 서버 사이에서 게이트키퍼 역할을 하는 컴퓨터 응용 프로그램입니다. 사용자의 모든 요청을 체계적인 방식으로 수집하고 전달하는 동시에  DDoS 공격 등에 대한 보안 및 보호를 제공합니다.
![](https://velog.velcdn.com/images/leekhy02/post/24f563c1-4ce9-4ec4-b5a0-32d0ddf2a14e/image.png)
## 역할
- Privacy 프록시 서버는 요청자의 위치를 숨기고 익명 브라우징을 보장합니다.
- Security 프록시 서버는 첫 번째 방어선 역할을 하고 사용자와 서버 사이에 서서 모든 파일을 저장하는 기업의 핵심 서버 인프라를 보호할 수 있습니다.
- Load Balancing
 프록시 서버가 위의 예에서 모든 것을 해결하지는 못하지만 로드 밸런싱을 지원하고 최종 서버의 작업을 가볍게 할 수 있습니다.
- 사이트 속도 향상
프록시를 사용하면 캐싱을 통해 성능을 향상시킬 수 있습니다. 
![](https://velog.velcdn.com/images/leekhy02/post/be080d3f-c25b-4485-9f55-c93fa2c97dae/image.png)
위 그림처럼 프록시 캐시 서버를 중간에둬 성능을 향상 시킬수 있습니다.
프록시 캐시 서버처럼 공용으로 사용하는 곳은 public 캐시, 로컬에서 사용하는 웹 브라우저 캐시는 private 캐시라고 합니다.
## 어떻게 작동합니까?
![](https://velog.velcdn.com/images/leekhy02/post/c9d77dec-6017-40af-84e2-f5716cc883dc/image.png)
![](https://velog.velcdn.com/images/leekhy02/post/0306cae5-d67a-4964-b104-9f85effc3477/image.png)


### Forward Proxy
일반적으로 말하는 Proxy 서버가 Forward Proxy 서버입니다. 위에서말한 여러 역할들 중 **캐싱**, Privacy등 역할을 합니다.
### Reverse Proxy
응답을 보내주는 측 역시, 어떠한 요청에 대한 응답을 보내줄 때 **캐싱**과 보안 취약점 제거를 위해 리버스 프록시를 사용합니다. 클라이언트 입장에서는 서버 측의 실제 IP가 아닌, 리버스 프록시의 IP만 알 수 있기 때문에 보안상 취약점을 커버할 수 있게 됩니다. **Load Balancing** 역할 또한 해줍니다.

정리하자면 사용자/클라이언트는 Forward Proxy를 사용하고 원본 서버는 Reverse Proxy를 사용합니다. 좀더 자세한 역할을 보자면

- Load Balancing
실제 서버(오리진 서버)는 수백만 명의 트래픽을 감당 할수 업습니다. 이런한 경우 트래픽을 분산시켜 웹사이트를 안정화 시킵니다. 대부분의 인기 있는 리버스 프록시는 주로 로드 밸런싱에 사용됩니다.
  - Load Balancer 와 Reverse Proxy
단 짚고 넘어가야 할것이 있습니다. Reverse Proxy가 Load Balancing을 한다고 하였습니다. 그렇다면 Revers Proxy가 Load Balancer냐 라고하면 이것은 또 다른 개념입니다.
	 1.  Load Balancer란 들어오는 클라이언트 요청을 서버 그룹 간에 분산하며 각 경우에 선택한 서버에서 적절한 클라이언트로 응답을 반환합니다.
     2.Reverse Proxy 는 클라이언트의 요청을 수락하고 이를 이행할 수 있는 서버로 전달한 다음 서버의 응답을 클라이언트에 반환합니다.
     
로드 밸런싱(Load Balancing)은 트래픽을 여러 대의 서버로 분산시켜서 성능을 향상시키는 개념을 의미합니다. 로드 밸런서(Load Balancer)는 로드 밸런싱을 구현하는 기술이나 장치를 말합니다. 

정리하자면 리버스 프록시와 로드 밸런서는 서로 다른 개념이지만,  일부 경우에는 리버스 프록시가 로드 밸런서의 역할을 수행할 수 있습니다.

- Global Server Load Balancing (GSLB 글로벌 서버 로드 밸런싱)
GSLB는 전 세계에 전략적으로 배치된 많은 서버 간에 웹 사이트 트래픽을 분산하기 위한 고급 로드 밸런싱 방법입니다. 일반적 으로 Reverse Proxy가 클라이언트와 서버 간의 가장 빠른 이동 시간을 기준으로 서버 노드를 선택하는 애니캐스트 라우팅 기술을 통해 수행됩니다.
- Enhanced Security(향상된 보안) 
1.원본 서버의 IP 주소 및 기타 특성을 숨길 수 있습니다. 따라서 웹사이트의 원본 서버는 익	명성을 더 잘 유지하여 보안을 크게 강화할 수 있습니다.
	2. 리버스 프록시는 메인 서버에 도달하기 전에 모든 트래픽을 수신하므로 공격자나 해커가 DDoS 공격 과 같은 보안 위협으로 웹사이트를 공격하기가 더 어려워집니다.
    
그 밖에도 밑에와 같은 기능이 있습니다.
- Powerful Caching
- 우수한 압축
- 최적화된 SSL 암호화
- 더 나은 A/B 테스트
- 트래픽 모니터링 및 로깅
그 외에도 다양한 프록시 서버가 있습니다.

# 캐시
검색 엔진 최적화(SEO)는 페이지 속도가 웹사이트 순위를 매기는 주요 요소 라는 사실을 오랫동안 인식해 왔습니다. 현대 웹사이트는 복잡해지고 무거워지고 있습니다. 캐싱은 필수적이라 할수 있습니다. 캐시를 사용하는 다양한 방법들을 간단하게 알아보겠습니다.

## 캐시를 다루는 기술
- Browsers,Devices
CPU 장치와 브라우저의 앱은 종종 스토리지와 RAM을 사용하여 데이터를 캐시합니다.
- Apps
- Servers
더 빠른 실행 및 처리를 위해 서버 데이터 중 일부를 저장하고 불러올 수 있습니다. 예시로는 Redis 캐시가 있습니다.
- Domain Name Server (DNS) caching 
이미 방문한 도메인 이름의 DNS 레코드(IPv4 주소용 A 레코드, IPv6용 AAAA 레코드)를 포함하는 장치(컴퓨터, 스마트폰, 서버 등)의 임시 DNS 저장소입니다. TTL(Time-to-Live) 에 따라 해당 레코드를 유지합니다.

TTL(Time to Live)은 레코드가 캐시된 상태로 유지되는 기간을 나타냅니다.
레코드 저장소를 **DNS 캐시** 라 부릅니다. 레코드를 저장하는 행위를 **캐싱이라고** 합니다


참고자료 > https://www.nginx.com/resources/glossary/reverse-proxy-vs-load-balancer/

https://kinsta.com/blog/reverse-proxy/#how-to-set-up-nginx-as-a-reverse-proxy
https://www.geeksforgeeks.org/difference-between-apache-and-nginx/
https://kinsta.com/blog/nginx-vs-apache/
https://www.baeldung.com/nginx-forward-proxy
