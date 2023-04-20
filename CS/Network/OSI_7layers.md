# OSI 7 계층

![OSI 7 layers](https://s7280.pcdn.co/wp-content/uploads/2018/06/osi-model-7-layers-1.png)

## 💡 OSI(Open Systems Interconnection Reference Model) 7 계층이란?
- 국제표준과기구(ISO)에서 개발한 모델로, 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것
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

#### 참조
<https://gyoogle.dev/blog/computer-science/network/OSI%207%EA%B3%84%EC%B8%B5.html>

<https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md>

<https://itwiki.kr/w/OSI_7%EA%B3%84%EC%B8%B5>