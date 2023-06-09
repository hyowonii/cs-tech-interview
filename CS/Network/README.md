# Network

- [OSI 7 계층](#osi-7-계층)
- [TCP/IP](#tcpip)
- [UDP](#udp)
- [대칭기 & 공개키](#대칭키--공개키)
- [HTTP & HTTPS](#http--https)
- [TLS/SSL Handshake](#tlsssl-handshake)
- [로드 밸런싱](#로드-밸런싱load-balancing)
- [Blocking/Non-blocking & Synchronous/Asynchronous](#blockingnon-blocking--synchronousasynchronous)
- [웹 통신의 큰 흐름](#웹-통신의-큰-흐름)

--- 

# OSI 7 계층

![OSI 7 layers](https://s7280.pcdn.co/wp-content/uploads/2018/06/osi-model-7-layers-1.png)

## 💡 OSI(Open Systems Interconnection Reference Model) 7 계층이란?
- 국제표준과기구(ISO)에서 개발한 모델로, 컴퓨터 네트워크 [프로토콜](#프로토콜protocol) 디자인과 통신을 계층으로 나누어 설명한 것
- 프로토콜을 기능별로 나눈 모델
- 각 계층은 하위 계층의 기능을 이용하고, 상위 계층에는 기능을 제공

## 🔎 7 계층으로 나눈 이유
- 통신이 일어나는 과정을 단계별로 알 수 있고, 특정한 곳에 이상이 생기면 그 단계만 수정이 가능

## 📍 7 layers

### 1. Physical layer(물리 계층)
- _리피터, 케이블, 허브 등_
- 디지털 데이터를 전기적인 아날로그 신호로 변환하여 주고받는 기능을 수행
- 데이터를 전송하는 역할만 진행
- 주소 개념X, 물리적으로 연결된 노드 간에 신호를 주고 받음
- 데이터 전송 단위: Bit

### 2. Data link layer(데이터 링크 계층)
- _브릿지, 스위치 등_
- 물리 계층으로 송수신되는 정보를 관리하여 안전하게 전달되도록 도와주는 역할
- Mac 주소를 통해 통신
- 신뢰성 있는 전송을 위해 흐름제어(Flow Control), 오류제어(Error Control), 회선제어(Line Control)을 수행
- 가장 잘 알려진 예는 이더넷
- 데이터 전송 단위: Frame

### 3. Network layer(네트워크 계층)
- _라우터, IP_
- 데이터를 목적지까지 가장 안전하고 빠르게 전달하는 기능을 담당
- End-to-End 혹은 Host-to-Host Delivery
- 라우터를 통해 이동할 경로를 선택하여 IP 주소를 지정하고, 해당 경로에 따라 패킷을 전달
- 라우팅, 흐름 제어, 오류 제어, 세그멘테이션 등을 수행
- 데이터 전송 단위: Packet

### 4. Transport layer(전송 계층)
- _TCP, UDP_
- 종단간 신뢰성 있는 데이터 전송을 담당(End-to-End Reliable Delivery) => 상위 계층들이 데이터 전달의 유효성이나 효율성을 생각하지 않도록 해줌
- Port number 주소로 전송 수행
- 시퀀스 넘버 기반의 오류 제어 방식 사용
- TCP: 신뢰성, 연결지향적 / UCP: 비신뢰성, 비연결성, 실시간
- 데이터 전송 단위: Segment

### 5. Session layer(세션 계층)
- _API, Socket_
- 응용 프로그램 간에 데이터가 통신하기 위한 논리적 연결을 담당
- TCP/IP 세션을 만들고 없애는 책임을 지님
- 데이터 전송 단위: Data or Message

### 6. Presentation layer(표현 계층)
- _JPEG, MPEG 등_
- 데이터 표현 방식, 상이한 부호체계 간의 변화에 대해 규정하여 데이터 표현에 대한 독립성을 제공하고 암호화하는 역할 담당
- 파일 인코딩 및 명령어 포장, 압축, 암호화
- 데이터 전송 단위: Data

### 7. Application layer(응용 계층)
- _HTTP, FTP, DNS, TELNET 등_
- 최종 목적지
- 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행
- 사용자 인터페이스, 전자우편, 데이터베이스 관리 등의 서비스 제공
- 데이터 전송 단위: Data

---

# TCP/IP

# TCP(Transmission Control Protocol)
- Transport layer(전송 계층)에서 사용하는 프로토콜
- IP와 함께 사용. IP가 데이터의 배달을 처리한다면 TCP는 패킷을 추적 및 관리
- Reliable network 보장
- 연결형 서비스
- 전이중, 점대점
- 멀티태스킹 or 브로드캐스팅 지원 X
- 흐름제어/혼잡제어 제공

### 🚨 Reliable network를 보장한다는 것은 아래 4가지의 문제를 해결한다는 뜻
- 손실: packet이 손실될 수 있는 문제
- 순서 바뀜: packet의 순서가 바뀌는 문제
- 혼잡: 네트워크가 혼잡한 문제
- 과부하: 수신자가 overload 되는 문제

=> 흐름제어 & 혼잡제어 두가지 기법으로 문제 해결

### 🔎 흐름제어/혼잡제어
- 흐름제어
  - Endpoint들 간의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지하는 것
  - 수신자가 데이터를 지나치게 많이 받지 않도록 조절하는 것
  - 기본 개념은 receiver가 sender에게 현재 자신의 상태를 feedback 한다는 것
- 혼잡제어
  - 송신 측의 데이터 전달과 네트워크의 데이터 처리 속도 차이를 해결하기 위한 방법

### 1. 흐름제어 (Flow Control)
- 수신 측이 송신 측보다 데이터 처리 속도가 빠르면 문제가 없지만, 송신 측이 더 빠를 경우 문제가 생김
- 수신 측에서 제한된 저장 용량을 초과하는 데이터는 손실 될 수 있으며, 만약 손실 된다면 불필요한 응답과 데이터 전송이 빈번히 발생
- 이러한 문제를 발생시키지 않기 위해 송신 측의 데이터 전송량을 수신 측에 따라 조절하는 기법
- **해결방법**
  - Stop and Wait: 매번 전송한 패킷에 대한 확인 응답을 받아야만 그 다음 패킷 전송하는 기법
  - Sliding Window: 수신 측에서 설정한 윈도우 크기만큼 송신 측에서 확인 응답 없이 세그먼트를 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어 기법
    - 일정한 크기의 윈도우에 포함되는 모든 패킷을 전송하고, 패킷 전달 확인 응답을 받으면 응답 받은 개수만큼 윈도우를 옆으로 옮겨 그 다음 패킷들을 전송
    - 송신 윈도우를 수신 윈도우보다 작거나 같은 크기로 설정하면 흐름 제어 가능

### 2. 혼잡제어 (Congestion Control)
- 송신 측의 데이터는 지역망이나 인터넷으로 연결된 대형 네트워크를 통해 전달됨. 만약 한 라우터에 데이터가 몰릴 경우 모든 데이터를 처리하기가 힘듦 -> 오버플로우나 데이터 손실 발생
- 이러한 네트워크 혼잡을 피하기 위해 송신 측에서 보내는 데이터의 전송 속도를 강제로 줄이는 기법
- 네트워크 내에 패킷의 수가 과도하게 증가하는 현상을 혼잡이라고 하며, 혼잡 현상을 방지/제거하는 기능이 혼잡제어
- 흐름제어가 송신 측과 수신 측 사이의 전송 속도를 다루는 것이라면, 혼잡제어는 호스트와 라우터를 포함한 보다 넓은 관점에서의 전송 문제를 다룸
- **해결방법**

  ![혼잡제어](https://t1.daumcdn.net/cfile/tistory/256E39425715F10103)
  - AIMD(Additive Increase / Multiplicative Decrease)
    - 처음에 패킷을 하나씩 보내고, 문제 없이 도착하면 window 크기를 1씩 증가시켜가면서 전송
    - 패킷 전송에 실패하거나 일정 시간을 넘으면 패킷 보내는 속도를 절반으로 줄임
    - 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만, 시간이 흐르면 평형상태로 수렴하게 되어 공평한 방식임
    - 문제점은, 초기에 네트워크의 높은 대역폭을 사용하지 못하여 오랜 시간이 걸리게 되고, 네트워크가 혼잡해지는 상황을 미리 감지하지 못함. 즉, 네트워크가 혼잡해지고 나서야 대역폭을 줄이는 방식
  - Slow Start
    - AIMD 방식과 마찬가지로 패킷을 하나씩 보내다가 window 크기를 1씩 늘림. 한 주기가 지나면 window 크기가 2배가 됨
    - 전송 속도는 지수 함수 꼴로 증가. 대신 혼잡 현상이 발생하면 window 크기를 1로 떨어뜨림
    - 처음에는 네트워크의 수용량을 예상할 수 있는 정보가 없지만, 한번 혼잡 현상이 발생하고 나면 네트워크의 수용량 어느 정도 예상 가능
    - 그러므로 혼잡 현상이 발생했던 window 크기의 절반까지는 이전처럼 지수 함수 꼴로 window 크기를 증가시키고, 그 이후부터는 완만하게 1씩 증가시킴
  - Fast Retransmit
    - 패킷을 받는 쪽에서 먼저 도착해야 할 패킷이 도착하지 않고 다음 패킷이 도착한 경우에도 ACK 패킷을 보냄
    - 순서대로 잘 도착한 마지막 패킷의 다음 패킷 순번을 ACK 패킷에 실어 보냄 -> 중간에 하나가 손실되게 되면 송신 측에서는 순번이 중복된 ACK 패킷을 받게 될 것이므로 이것을 감지한 순간 문제가 되는 순번의 패킷을 재전송 해줄 수 있음
    - 중복된 순번의 패킷을 3개 받으면 재전송 함. 약간 혼잡한 상황이 일어난 것을 감지했으므로 window 크기를 줄임
  - Fast Recovery
    - 혼잡 상태가 되면 window 크기를 1로 줄이지 않고 반으로 줄이고 선형증가시키는 방식
    - 이 정책까지 적용하면 혼잡 상황을 한번 겪고 난 후부터는 순수한 AIMD 방식으로 동작하게 됨


## 💡 TCP 3 & 4 way handshake
연결을 성립하고 해제하는 과정

### 1. 3 way handshake - 연결 성립
TCP는 정확한 전송을 보장해야 하므로 통신하기에 앞서 논리적인 접속을 성립하기 위해 3 way handshake 과정을 진행

![3-way-handshake](https://media.geeksforgeeks.org/wp-content/uploads/TCP-connection-1.png)

1. 클라이언트가 서버에서 SYN 패킷을 보냄 (sequence: x)
2. 서버가 SYN(x)를 받고, 받았다는 신호인 ACK과 SYN 패킷을 클라이언트로 보냄 (sequence: y, ACK: x+1)
3. 클라이언트는 응답으로 ACK(x+1)과 SYN(y) 패킷을 받고, ACK(y+1)을 서버로 보냄


### 2. 4 way handshake - 연결 해제
모든 통신이 끝났다면 연결 해제

![4-way-handshake](https://media.geeksforgeeks.org/wp-content/uploads/CN.png)

1. 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보냄
2. 서버는 FIN을 받고, 확인했다는 ACK을 클라이언트에게 보냄
3. 데이터를 모두 보냈다면 연결이 종료되었다는 FIN 플래그를 클라이언트에게 보냄
4. 클라이언트는 FIN을 받고, 확인했다는 ACK을 서버에게 보냄 (아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 TIME_WAIT을 통해 기다림)
- 서버는 ACK을 받은 이후 소켓 close, 클라이언트는 TIME_WAIT 시간이 끝나면 close

---

# UDP

## 💡 UDP 통신이란?
- User Datagram Protocol. 데이터를 데이터그램 단위로 처리하는 프로토콜
- 비연결형, 신뢰성X
- Transport layer(전송 계층)에서 사용

### TCP와 UDP는 왜 나오게 됐는가?
1. IP의 역할은 Host-to-Host(장치-장치)만 지원. 하나의 장비 내에서 수많은 프로그램들이 통신을 할 경우 IP만으로는 한계가 있음 => 포트 번호
2. IP에서 오류가 발생한다면 ICMP에서 알려주지만, 알려주기만 할 뿐 대처를 못하기 때문에 IP보다 위에서 처리를 해줘야 함 => TCP, UDP

### 그렇다면, TCP와 UCP가 어떻게 오류를 해결하는가?
- TCP : 데이터의 분실, 중복, 순서 뒤바뀜 등을 자동으로 보정해 송수신 데이터의 정확한 전달을 가능케 함
- UDP : TCP와는 다르게 여러가지 문제 발생 가능 -> 어플리케이션에서 처리하는 번거로움 존재

### UDP는 왜 사용하나?
- 데이터의 신속성. TCP보다 데이터 처리 속도 빠름
- 용량이 가벼움
- 주로 실시간 방송과 온라인 게임에 사용됨

## UDP Header
![UDP Header](https://t1.daumcdn.net/cfile/tistory/272A5A385759267B36)
- Source port : 시작 포트
- Destination port : 도착지 포트
- Length : 길이
- Checksum : 오류 검출 (중복 검사의 한 형태로, 오류 정정을 통해 공간이나 시간 속에서 송신된 자료의 무결성을 보호하는 단순한 방법)

## DNS와 UDP 통신 프로토콜
DNS는 Application layer protocol. 모든 Application layer protocol은 TCP, UCP 중 하나의 Transport layer protocol을 사용해야 함
### _UDP를 사용하는 이유_
1. Connection을 유지할 필요 없음
2. DNS request는 UDP segment에 들어갈 정도로 작음
3. Reliability는 application layer에 추가될 수 있음
- DNS는 UDP를 53번 포트에서 사용
- 하지만 TCP를 사용하는 경우도 있음
  - Zone Transfer(DNS 서버 간에 요청을 주고 받을 때 사용하는 transfer)를 사용해야 하는 경우
  - 데이터가 512 bytes(UDP 제한)을 넘거나, 응답을 못받은 경우

---

# 대칭키 & 공개키

## 대칭키(Symmetric Key) 암호화 방식
: 암호화와 복호화에 동일한 암호키(대칭키)를 사용하는 알고리즘

![대칭키 암호화](https://velog.velcdn.com/images%2Fgs0351%2Fpost%2Fe6ba5378-7c0d-4e3b-9106-1ce0055bb1b3%2Fimage-20201228143331890.png)

- 장점: 동일한 키를 주고받으므로 속도가 빠름, 대용량 Data 암호화에 적합
- 단점: 키를 교환해야 하는 문제, 탈취 및 관리 걱정 등
- 기밀성을 제공하나, 무결성/인증/부인방지 보장X

## 공개키(Public Key) / 비대칭키(Asymmetric Key) 암호화 방식
: 암호화와 복호화에 사용하는 암호키를 분리한 알고리즘

![공개키 암호화](https://velog.velcdn.com/images%2Fgs0351%2Fpost%2Ff8e3eb30-2eda-47ac-954e-915515066bbc%2Fimage-20201228143511804.png)

- 자신이 가지고 있는 고유한 암호기(비밀키)로만 복호화할 수 있는 암호키(공개키)를 대중에게 공개
- 장점: 기밀성/인증/부인방지 기능 제공, 키분배 필요X
- 단점: 느린 속도, 대칭키에 비해 암호화/복호화가 매우 복잡

### 공개키 암호화 방식 진행 과정
1. A가 웹 상에 공개된 B의 공개키를 이용해 평문을 암호화하여 B에게 보냄
2. B는 자신의 비밀키로 복호화된 평문을 확인, A의 공개키로 응답을 암호화하여 A에게 보냄
3. A는 자신의 비밀키로 암호화된 응답문을 복호화함

=> 하지만 이 방식은 Confidentiallity만 보장해줄 뿐, Integrity나 Authenticity는 보장해주지 못함 -> MAC(Message Authentication Code)나 서명(Digital Signature)으로 해결

### **하이브리드 방식(대칭키와 공개키 암호화 방식을 적절히 혼합한 것)**
_SSL 탄생의 시초가 됨_
1. A가 B의 공개키로 암호화 통신에 사용할 대칭키를 암호화하고 B에게 보냄
2. B는 암호문을 받고 자신의 비밀키로 복호화함
3. B는 A로부터 얻은 대칭키로 A에게 보낼 평문을 암호화하여 A에게 보냄
4. A는 자신의 대칭키로 암호문을 복화하함
5. 앞으로 이 대칭키로 암호화 통신

즉, 대칭키를 주고받을 때만 공개키 암호화 방식을 사용하고 이후에는 계속 대칭키 암호화 방식으로 통신

---

# HTTP & HTTPS

## HTTP(Hyper Text Tranfer Protocol)
: 인터넷 상에서 클라이언트와 서버가 요청/응답(request/reponse)으로 정보를 주고 받을 때 쓰는 통신 규약
- 주로 HTML 문서를 주고받는 데에 사용
- TCP와 UDP를 사용, 80번 포트 사용
- 비연결(Connectionless): 클라이언트가 요청을 서버에 보내고 서버가 적절한 응답을 클라이언트에 보내고 나면 바로 연결이 끊어짐
- 무상태(Stateless): 연결을 끊는 순간 클라이언트와 서버의 통신은 끝나며 상태 정보를 유지하지 않음
- HTTP는 평문 통신이기 때문에 도청 및 가로채기가 가능하다는 문제가 있음 => 이 보안 문제를 해결해주는 프로토콜이 HTTPS

## HTTPS(Hyper Text Tranfer Protocol Secure)
: 인터넷 상에서 정보를 암호화하는 SSL 프로토콜을 사용해 클라이언트와 서버가 자원을 주고 받을 때 쓰는 통신 규약
- 기본 TCP/IP 포트로 443번 포트 사용
- 텍스트를 암호화(공개키 암호화 방식으로) => 데이터의 적절한 보호를 보장

### HTTPS의 장단점
- 장점
  - 네트워크 상에서 열람, 수정이 불가능하므로 안전
- 단점
  - 암호화하는 과정에서 웹 서버에 부하를 줌
  - 설치 및 인증서를 유지하는데 추가 비용이 발생
  - HTTP에 비해 느림
  - 인터넷 연결이 끊긴 경우 재인증 시간이 소요됨
    - HTTP는 비연결형으로 웹 페이지를 보는 중 인터넷 연결이 끊겨도 페이지를 계속 볼 수 있지만, HTTPS는 소켓(데이터를 주고 받는 경로) 자체에서 인증을 하기 때문에 인터넷 연결이 끊기면 소켓이 끊어져서 다시 HTTPS 인증이 필요함

[HTTP & HTTPS의 동작 과정](https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md#http%EC%99%80-https-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95)

### HTTP의 GET과 POST 비교
둘 다 HTTP 프로토콜을 이용해 서버에 무언가를 요청할 때 사용하는 방식. 둘의 특징을 제대로 이해하여 기술의 목적에 알맞은 용도로 사용해야 함.

#### **GET**
우선 GET 방식은 요청하는 데이터가 `HTTP Request Message`의 Header 부분에 url에 담겨 전송됨(url 상에 ? 뒤에 데이터가 붙어서 request 전송). 때문에 전송할 수 있는 데이터의 크기 제한, 데이터가 url에 그대로 노출되기 때문에 보안적인 이슈 발생 가능.

#### **POST**
POST 방식은 `HTTP Request Message`의 Body 부분에 데이터가 담겨서 전송됨. 전송 가능한 데이터의 크기가 GET 방식보다 크고 보안 면에서 나음.

- GET은 서버에서 데이터를 가져올 때, POST는 서버의 값이나 상태를 변경 또는 추가할 때 사용
- 또한 GET 방식의 요청은 브라우저에서 Caching이 가능함. 때문에 POST 방식으로 요청해야 할 데이터를 보내는 크기가 작고 보안적인 문제가 없다는 이유로 GET 방식으로 요청한다면 기존에 caching 되었던 데이터가 응답될 가능성이 존재. **때문에 목적에 맞는 기술을 사용해야 하는 것이다.**

---

# TLS/SSL Handshake
HTTPS에서 클라이언트와 서버간 통신 전 SSL 인증서로 신뢰성 여부를 판단하기 위해 연결하는 방식

![TLS/SSL Handshake](https://user-images.githubusercontent.com/34904741/139517776-f2cac636-5ce5-4815-981d-33905283bf13.png)

### 진행 순서
1. 클라이언트는 서버에게 `client hello` 메시지를 담아 서버로 보낸다. 이 때 암호화된 정보(버전, 암호 알고리즘, 압축 방식 등)를 함께 담는다.
2. 서버는 클라이언트가 보낸 암호 알고리즘과 압축 방식을 받고 세션 ID와 CA 공개 인증서를 `server hello` 메시지와 함께 담아 응답한다. 이 CA 인증서에는 앞으로 통신 이후 사용할 대칭키가 생성되기 전 클라이언트에서 handshake 과정 속 암호화에서 사용할 공개키를 담고 있다.
3. 클라이언트 측은 서버에서 보낸 CA 인증서가 유효한지 CA 목록에서 확인하는 과정을 진행한다.
4. CA 인증서에 대한 신뢰성이 확보되었다면 클라이언트는 난수 바이트를 생성하여 서버의 공개키로 암호화한다. 이 난수 바이트는 대칭키를 정하는데 사용 되고, 앞으로 서로 메시지를 통신할 때 암호화 하는데 사용된다.
5. 만약 2번 단계에서 서버가 클라이언트 인증서를 함께 요구했다면, 클라이언트의 디지털 인증서와 클라이언트의 개인키로 암호화된 임의의 바이트 문자열을 함께 보낸다. 디지털 인증서가 없을 경우 "디지털 인증서 없음 경고"를 보낸다.
6. 서버는 클라이언트의 인증서를 확인 후 난수 바이트를 자신의 개인키로 복호화한 후 대칭 마스터 키 생성에 활용한다.
7. 클라이언트는 handshake 과정이 완료되었다는 `finished` 메시지를 서버에 보내면서 지금가지 보낸 교환 내역들을 해싱 후 그 값을 대칭키로 암호화하여 같이 담아 보낸다.
8. 서버도 동일하게 교환 내용들을 해싱한 후 클라이언트에서 보내준 값과 일치하는 지 확인한다. 일치하면 서버도 마찬가지로 `finished` 메시지를 이번에 만든 대칭키로 암호화하여 보낸다.
9. 클라이언트는 해당 메시지를 대칭키로 복호화하여 서로 통신이 가능한 신뢰받은 사용자란 것을 인지하고, 앞으로 클라이언트와 서버는 해당 대칭키로 데이터를 주고받을 수 있게 된다.

---

# 로드 밸런싱(Load Balancing)
둘 이상의 CPU 또는 저장장치와 같은 컴퓨터 자원들에게 작업을 나누는 것

웹사이트에 접속하는 인원이 많으면 모든 트래픽을 1대의 서버로만 감당하는 것이 무리다. 대응 방안으로 하드웨어의 성능을 올리거나(Scale-up) 여러대의 서버가 나눠서 일하도록 하는 것(Scale-out)이 있다. 하드웨어 향상 비용이 더욱 비싸기도 하고, 서버가 여러대면 무중단 서비스를 제공하는 환경 구성이 용이하므로 Scale-out이 효과적이다. 이 때 여러 서버에게 균등하게 트래픽을 분산시켜주는 것이 바로 **로드 밸런싱**이다.

**로드 밸런싱**은 하나의 인터넷 서비스가 발생하는 트래픽이 많을 때 여러 대의 서버가 분산 처리하여 서버의 로드율 증가, 부하량, 속도 저하 등을 해결해주는 서비스이다. 서비스를 운영하는 사이트의 규모에 따라 웹 서버를 추가로 증설하면서 로드 밸런서로 관리해주면 웹 서버의 부하를 해결할 수 있다.

<img width=400px src="https://nesoy.github.io/assets/posts/20180602/3.png">

### 로드 밸런서가 서버를 선택하는 방식
- 라운드 로빈(Round Robin): CPU 스케쥴링의 라운드 로빈 방식 활용
- Least Connections: 연결 개수가 가장 적은 서버 선택 (트래픽으로 인해 세션이 길어지는 경우 권장)
- Source: 사용자 IP를 해싱하여 분배 (특정 사용자가 항상 같은 서버로 연결되는 것을 보장)

### 로드 밸런서 장애 대비
- 로드 밸런서를 이중화하여 대비

<img width=400px src="https://nesoy.github.io/assets/posts/20180602/7.png">
</br></br>

---

# Blocking/Non-blocking & Synchronous/Asynchronous
![Blocking/Non-blocking&Synchronous/Asynchronous](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fda50Yz%2Fbtq0Dsje4ZV%2FlGe8H8nZgdBdgFvo7IczS0%2Fimg.png)

## Blocking/Non-blocking
`호출된 함수`가 `호출한 함수`에게 제어권을 건네주는 유무의 차이
- `호출된 함수`가 자신이 할 일을 모두 마칠 때까지 제어권을 계속 가지고 있으면서 `호출한 함수`에게 바로 돌려주지 않고 할 일을 모두 마친 후 돌려주면 Block
- `호출된 함수`가 자신이 할 일을 마치지 않았더라도 바로 제어권을 건네주어(return) `호출한 함수`가 다른 일을 진행할 수 있도록 해주면 Non-block

## Synchronous/Acynchronous
`동시성`에 주목
- `호출된 함수`의 수행 결과 및 종료를 `호출한 함수`가(`호출된 함수`뿐만 아니라 `호출한 함수`도 함께) 신경 쓰면 Synchronous
- `호출된 함수`의 수행 결과 및 종료를 `호출한 함수`는 신경 쓰지 않고 `호출된 함수` 혼자 직접 신경 쓰고 처리한다면(callback으로 `호출한 함수`에게 완료 여부 알림) Asynchronous

(1) Blocking & Synchronous (2) Blocking & Asynchronous (3) Non-blocking & Synchronous (4) Non-blocking & Asynchronous 네 가지 경우가 나올 수 있음
</br></br>

## Blocking & Non-blocking I/O
I/O 작업은 Kernel level에서만 수행할 수 있으므로 Process, Thread는 커널에세 I/O를 요청해야 함

### 1. Blocking I/O
I/O 작업이 진행되는 동안 유저 프로세스가 자신의 작업을 중단한 채 I/O가 끝날 때까지 대기하는 방식

<img width=500px src=https://blog.kakaocdn.net/dn/bxZNAt/btrCbnSh7pb/yUukImLUvt2zYMewRAgG10/img.png>

- 특징
  - I/O 작엄이 진행되는 동안 User Process(Thread)는 자신의 작업을 중단한 채 대기해야 함
  - Resource 낭비가 심함 (I/O 작업이 CPU 자원을 거의 쓰지 않으므로)
- _여러 Client가 접속하는 서버를 Blocking 방식으로 구현하는 경우_ : 다른 클라이언트가 진행 중인 작업을 중지하면 안되므로 클라이언트 별로 별도의 Thread를 생성해야 함 -> 이로 인해 많아진 Threads로 _컨텍스트 스위칭 횟수가 증가_ => 비효율적인 동작 방식

### 2. Non-blocking I/O
A함수가 I/O 작업을 호출했을 때 I/O 작업이 완료될 때까지 A함수의 작업을 중단하지 않고 I/O 호출에 대해 즉시 리턴하고, A함수가 이어서 다른 일을 수행할 수 있도록 하는 방식

<img width=500px src=https://blog.kakaocdn.net/dn/bd5SNK/btrB7bxxRhx/es0YitcU5QazXaDyrH4vu0/img.png>

- read I/O를 하기 위해 system call을 수행하면 커널의 I/O 작업 완료 여부와 무관하게 즉시 응답한다. 이는 커널이 시스템 콜을 받자마자 CPU 제어권을 다시 애플리케이션에게 넘겨주어 애플리케이션은 I/O 작업이 완료되기 전에 다른 작업을 수행할 수 있다. 그리고 애플리케이션은 다른 작업들을 수행하다가 중간중간 시스템 콜을 보내서 I/O가 완료 되었는지 커널에 물어보고 완료되면 I/O 작업을 완료한다.

---

# 웹 통신의 큰 흐름

_우리가 Chrome을 실행시켜 주소창에 특정 url을 입력하면 어떤 일이 일어나는가?_

<img src="https://media.vlpt.us/images/woo0_hooo/post/e119383c-61cc-46d5-a85d-b27b65ddee1e/Untitled.png" alt="웹 통신 흐름">

1. 사용자가 웹 브라우저를 통해 url 입력
2. 입려된 url 중 도메인 네임을 DNS 서버에서 검색
3. [DNS 서버](#dns-서버)에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 url 정보와 함께 전달
4. 웹페이지 url 정보와 전달받은 IP 주소를 이용해 HTTP 요청 메세지를 생성
5. 요청은 TCP를 통해 서버로 전송됨
6. 서버는 클라이언트의 요청을 받고 그에 대한 응답을 전송

---
### *각주*

#### **프로토콜(Protocol)**
: 컴퓨터 내 또는 컴퓨터 간의 데이터 교환 방식을 정의한 규칙 체계

: 네트워크 내의 컴퓨터는 서로 상이한 하드웨어와 소프트웨어를 사용할 수 있지만, 프로토콜을 활용해 서로 통신할 수 있음

#### **DNS 서버**
: Domain Name System, 인터넷 전화번호부

: 사용자가 도메인 네임을 통해 접속하면 DNS 서버에서 해당 도메인 네임에 일치하는 IP 주소를 반환해줌(웹 브라우저는 IP 주소를 통해 통신하기 때문에)