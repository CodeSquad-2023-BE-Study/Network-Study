팀 프로젝트에서 AWS 인프라 환경을 구축하던 중 VPC 설정을 진행해야 했습니다. 그런데 VPC 에서 다음과 같은 화면이 보였습니다.

![](https://velog.velcdn.com/images/bruni_23yong/post/a9e3cb89-b991-47c1-a394-12841d4d31e6/image.png)

![](https://velog.velcdn.com/images/bruni_23yong/post/8065550e-ae48-45c5-8feb-96f1110608f1/image.png)

IPv4 CIDR 에는 위와 같은 숫자가 있었고 각 VPC를 구성할 때 서브넷들도 함께 구성해야 했습니다. 이것들이 무엇을 의미하는지 학습 후 정리해보고자 합니다.

## IP Address (IPv4)

![](https://velog.velcdn.com/images/bruni_23yong/post/83583bec-34ad-4bc1-8f42-32ecc3f209b0/image.png)

IP 주소는 32 bit 주소 체계로 `네트워크 인터페이스`를 지칭하는 주소입니다. 우리가 사용하는 각 컴퓨터에도 모두 IP주소가 부여되어 있습니다. 좀 더 정확히 말하자면 Network Interface Card(NIC)에 부여된 주소입니다.

이 말은 한 컴퓨터도 여러 개의 IP 주소를 가질 수 있다는 것을 의미합니다. (가장 대표적인 예가 라우터입니다.)

잠시 인터넷 얘기로 넘어가겠습니다.
인터넷은 많은 네트워크로 이루어져 있고 네트워크들은 주소가 필요합니다.

![](https://velog.velcdn.com/images/bruni_23yong/post/6454d50a-cdc9-467d-9da7-73ec187dc86e/image.png)

그런데 각 호스트들에 IP주소를 마구잡이로 배정하면 어떻게 될까요?

![](https://velog.velcdn.com/images/bruni_23yong/post/6edc1445-2c3a-48d9-a7ed-7eb7864a3762/image.png)

그러면 각 라우터의 forwading table은 모든 호스트들의 정보를 가지고 있어야 할 것이고, 이에 따라 테이블의 크기도 늘어날 것입니다.

이를 해결하기 위해 IP 주소를 계층화 시켰습니다.

## Hierarchical Addressing : IP Prefixes

IP 주소를 `Network 파트`와 `Host 파트`로 나누어 IP 주소를 나타낼 수 있습니다.
![](https://velog.velcdn.com/images/bruni_23yong/post/dbacffa6-d1ce-4fb0-b965-7b86ae75cdc5/image.png)

위의 그림에서는 IP 주소를 12.34.158.0/24 로 나타낼 수 있습니다.
24가 의미하는 바는 앞의 24bit를 네트워크 파트로 사용한다는 것이고 나머지 8bit 가 호스트 파트로 사용된다는 것을 의미합니다.

이때 24bit는 고정되어 있으며 이를 `prefix`라고 합니다. prefix의 범위는 0~32이고 상위 고정 비트 혹은 네트워크 파트를 의미합니다.
나머지 고정되지 않은 하위 비트가 호스트 파트이며 범위값을 가집니다.

![](https://velog.velcdn.com/images/bruni_23yong/post/6e59d94e-d493-463e-9334-998b34f42aec/image.png)

그런데 이는 사람이 알기쉽게 표기하는 방법입니다. 컴퓨터가 알기 위해서는 `서브넷 마스크(subnet mask)`를 이용합니다. 서브넷 마스크는 IP주소 중 어디까지가 네트워크 파트인지 알려주는 역할을 합니다. 즉 IP prefix의 크기를 직관적으로 알 수 있게 해줍니다.

![](https://velog.velcdn.com/images/bruni_23yong/post/d6ca2541-b02c-4648-b6b1-7f8b01de6d68/image.png)

그러면 우리는 이제 IP prefix를 이용해 forwarding table을 구성할 수 있습니다.
위의 그림에서 1.2.3.0/24의 IP 주소는 왼쪽으로 가도록 하고 5.6.7.0/24의 IP 주소는 오른쪽으로 가도록 하는 것입니다. 이렇게 되면 forwarding table의 크기도 줄게 되고 새로운 host를 추가해도 forwarding table에 수정을 가할 필요가 없어집니다. 즉 확장성이 좋아집니다.

## Classful addressing

그러면 모든 네트워크들은 자신만의 prefix를 가지고 있을텐데 그 기준은 어떻게 될까요?
IP Prefix를 할당하기 위해 이전에는 `클래스 방식`의 IP주소 할당을 사용했습니다.

- class A : 0\*
  - 최상위 비트를 0으로 가지며 네트워크 파트의 크기가 8bit인 클래스입니다. (/8)
  - 네트워크 수는 128개, 호스트의 수는 2^24 개입니다.
- class B : 10\*
  - 최상위 비트를 10으로 가지며 네트워크 파트의 크기가 16bit인 클래스입니다. (/16)
  - 네트워크 수는 2^14개, 호스트의 수는 2^16 개입니다.
- class C : 110\*
  - 최상위 비트를 110으로 가지며 네트워크 파트의 크기가 24bit인 클래스입니다. (/24)
  - 네트워크 수는 2^21개, 호스트의 수는 2^8 개입니다.

클래스 D, E도 존재하지만 넘어가겠습니다.

이 방식은 예전에 사용하던 방식으로 비효율적이기 때문에 지금은 사용하지 않습니다.
왜냐하면 클래스 기반 IP 주소 할당은 비효율적이고 IP 주소 공간 낭비로 이어지기 때문입니다.

예를 들어 어떤 조직이 호스트가 300개가 필요하다고 가정해보겠습니다. 그런데 이 조직은 class C를 사용할 수 없습니다. 왜냐하면 class C의 호스트 수는 256이기 때문입니다. 그래서 이 조직은 class B IP 주소를 신청해야 했고 약 6만 개의 IP 주소 공간은 사용되지 않은 채로 남게 됩니다.

따라서 요즘은 CIDR 방식을 사용하고 있습니다.

## CIDR(Classless Inter-Domain Routing)

CIDR은 IP Address와 IP Mask 만으로 IP 주소를 할당하는 방법입니다. CIDR 주소는 가변길이 서브넷 마스킹을 사용해 IP 주소의 네트워크와 호스트 주소 비트 간의 비율을 변경합니다.

위와 동일하게 300개의 호스트가 필요하다면 조직은 prefix가 `/9` 의 IP 주소를 사용하면 됩니다.

CIDR을 사용하게 되면서 IP 주소 낭비를 감소시킬 수 있었습니다.

![](https://velog.velcdn.com/images/bruni_23yong/post/5c3af4fb-1757-4950-9d06-7f353fb2d561/image.png)

위 그림에서는 IP 주소가 12.4.0.0/15 라고 했을 때 앞의 15bit 를 prefix로 가져가는 것을 의미하며 나머진 17bit는 호스트 파트임을 의미합니다.

### Longest Prefix Matching Forwarding

이제 CIDR 을 이용해 포워딩을 수행할 수 있게 되었습니다. IP 주소의 계층적 구조 때문에 prefix 비트에 따른 포워딩이 가능해집니다. 그러면 어떻게 포워딩 시킬지에 대한 규칙이 필요한데 가장 긴 prefix가 매칭되는 쪽에 포워딩 시킨다는 규칙인 `Longest Prefix Matching Forwarding` 규칙을 이용합니다.

아래 그림에서 `201.10.6.17` IP 주소를 찾아간다고 했을 때 forwarding 테이블이 아래와 같다고 해보겠습니다.

그러면 3번째, 4번째 항목 중 어디에 포워딩 시킬지 고민해야 할것입니다.

![](https://velog.velcdn.com/images/bruni_23yong/post/45ae53ed-3e57-4755-bfc1-30415ad95960/image.png)

`201.10.0.0/21` 이라고 했을 때를 생각해보겠습니다.
![](https://velog.velcdn.com/images/bruni_23yong/post/280fbc86-3427-4b0e-9070-de10435934a8/image.png)

이번에는 `201.10.6.0/23`이라고 했을 때를 생각해보겠습니다.
![](https://velog.velcdn.com/images/bruni_23yong/post/036a117f-1369-45e8-af6a-73e23f926cb2/image.png)

더 길게 일치하는 쪽이 `201.10.6.0/23` 이기 때문에 `201.10.6.0/23`으로 포워딩을 시키게 됩니다.

## 서브넷 (subnet)

(맨 처음을 다시 생각해보면서) 그러면 서브넷은 무엇일까요?

서브넷은 같은 prefix를 가진 device의 집합입니다. 다른 말로 하면 라우터를 거치지 않고 접근이 가능한 호스트들의 집합이라고 할 수 있습니다.

아래 그림에서 prefix가 /24 라고 가정했을 때 해당 네트워크에는 3개의 서브넷이 존재합니다.

![](https://velog.velcdn.com/images/bruni_23yong/post/c3434d74-9e30-4cf3-a17d-78cd4f01c01c/image.png)

> 참고로 위의 그림에서 라우터가 3개의 subnet에 걸쳐있는 것에 관심을 가져봅시다. 라우터는 여러 개의 NIC를 가질 수 있기 때문에 가능하며 다른 서브넷으로 가기 위해서는 라우터를 거쳐야 합니다.

## 결론

- IP 주소는 네트워크 파트와 호스트 파트로 구성되어 있다.
  - 네트워크 파트는 고정되어 있으며 IP prefix라고 한다.
  - 10.3.2.0/24는 네트워크 파트가 24bit이고 호스트 파트가 8비트임을 의미한다.
- CIDR은 이전 클래스 기반 주소할당 방식을 개선한 방법이다.
  - 이름부터 `Classless` 라는 점을 기억해두자.
- 서브넷은 같은 prefix를 가진 디바이스의 집합이다.

## 참고 자료

- http://www.kocw.or.kr/home/cview.do?cid=6166c077e545b736
  - KOCW 이석복 교수님 강의
- https://aws.amazon.com/ko/what-is/cidr/
