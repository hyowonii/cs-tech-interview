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


#### 참고
<https://gyoogle.dev/blog/computer-science/network/UDP.html>