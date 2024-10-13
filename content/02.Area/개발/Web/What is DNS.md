---
date: 2024 년 10 월 05 일 20 시 10 분
tags:
  - Dev
  - Dns
  - Web
share: true
---
# DNS(Domain Name System) 란?

우리가 사용하는 휴대폰, 인터넷, TV, 서버(Server) 등 인터넷에 연결된 모든 것은 ***디지털 신호(0과 1)*** 로 이루어진 숫자를 통해 통신합니다. 이 숫자들이 바로 [IP 주소](What%20is%20IP%20Address.md)입니다.

한국에서 자주 사용하는 naver.com, google.com 같은 사이트의 서버도 각각 고유한 IP 주소를 가지고 있습니다.

우리가 크롬이나 엣지 같은 클라이언트 프로그램에서 `naver.com`에 접속하는 것은 결국 네이버의 서버에 접속하는 행위입니다. 하지만, 위에서 언급했듯이 모든 통신은 디지털 신호로 이루어집니다.

즉, `naver.com`을 입력해도 실제로는 `naver.com`과 연결된 IP 주소에 접속하게 됩니다. 그렇다면 우리가 입력한 `naver.com`의 IP 주소는 누가, 어떻게 알고 있는 것일까요?

여기서 바로 DNS의 개념이 등장하게 됩니다. DNS는 쉽게 말해 인터넷의 전화번호부 같은 역할을 합니다. 사람이 기억하기 어려운 숫자(IP 주소)를 사람이 이해할 수 있는 도메인 이름으로 변환해줍니다.

![Pasted image 20230917202657.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241005202433.png)

# DNS Server

DNS 서버는 계층적인 구조로 되어 있습니다.

![](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241005202523.png)


`www.example.com`을 브라우저에 입력하는 순간 가장 먼저 가는 곳은 인터넷 공급업체(KT, SKT 등)의 DNS Resolver입니다. 이곳에서 `www.example.com`에 대한 IP가 없다면, 다음으로 Root(.) DNS 서버에 요청해 `.com`에 대한 정보(IP 주소)를 받습니다. 이후 `.com` DNS 서버는 해당 도메인의 네임 서버인 `example.com`에 대한 정보를 제공하며, 192.0.2.44라는 IP 주소를 DNS Resolver에 반환합니다. 이를 통해 우리는 `www.example.com`의 IP 주소가 192.0.2.44라는 것을 알게 되고, 해당 서버에 정보를 요청할 수 있습니다.

### Recursive DNS 와 Authoritative DNS

DNS는 두 가지 주요 역할을 가지고 있습니다. **Recursive DNS**는 사용자의 요청을 받아 여러 DNS 서버에 정보를 물어봅니다. 반면, **Authoritative DNS**는 특정 도메인에 대한 최종적인 IP 주소 정보를 제공합니다. 이 두 가지 역할의 상호작용 덕분에 인터넷에서 도메인 이름을 효율적으로 관리할 수 있습니다.


# Hosts 파일

위 설명에서 한 가지 더 중요한 점은, DNS Resolver가 작동하기 전에 우리 컴퓨터, 즉 Local 환경에서 데이터를 먼저 확인한다는 점입니다.

![Pasted image 20230917221151.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241005202589.png)

바로 **hosts** 파일입니다. 이 파일에 기록된 정보가 우선적으로 확인되기 때문에, 만약 hosts 파일이 위변조된다면 해킹 위험이 발생할 수 있습니다. 예를 들어, 악성 프로그램이 이 파일을 수정하면 특정 웹사이트 대신 공격자가 설정한 가짜 사이트로 접속하게 될 수 있습니다.

## hosts 파일 수정

다음은 ping 명령을 사용해 `www.naver.com`의 IP 주소를 확인한 예입니다.

`www.naver.com`의 실제 IP 주소는 23.44.52.223입니다. 만약 hosts 파일을 수정해 다음과 같이 설정한 후 다시 ping을 입력하면,

![Pasted image 20230917221759.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241005203003.png)

![Pasted image 20230917221814.png](https://file-api.ksq9511.synology.me:5353/obsidian-image/20241005203030.png)

111.111.111.111에 요청을 보내는 것을 확인할 수 있습니다. 이는 hosts 파일에서 IP 주소를 재정의했기 때문입니다.

[[https://en.wikipedia.org/wiki/Hosts_(file)| OS 별 host 파일 위치]]는 위키피디아에서 잘 설명되어 있습니다.

# DNS 캐시

DNS 캐시는 운영 체제나 브라우저가 최근에 방문한 도메인의 IP 주소를 저장해두는 메커니즘입니다. 이는 웹사이트 접근 속도를 빠르게 하고 네트워크 부하를 줄이는 역할을 합니다. 다만 캐시가 업데이트되지 않아 오래된 정보를 참조할 경우, 웹사이트 접근에 문제가 발생할 수 있습니다.


--- 
# 참조 : 
https://aws.amazon.com/ko/route53/what-is-dns/
https://en.wikipedia.org/wiki/Hosts_(file)