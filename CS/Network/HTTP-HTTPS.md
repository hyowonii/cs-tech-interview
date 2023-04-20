# HTTP & HTTPS

## HTTP(Hyper Text Tranfer Protocol)
: 인터넷 상에서 클라이언트와 서버가 요청/응답(request/reponse)으로 정보를 주고 받을 때 쓰는 통신 규약
- 주로 HTML 문서를 주고받는 데에 사용
- TCP와 UDP를 사용, 80번 포트 사용
- 비연결(Connectionless): 클라이언트가 요청을 서버에 보내고 서버가 적절한 응답을 클라이언트에 보내고 나면 바로 연결이 끊어짐
- 무상태(Stateless): 연결을 끊는 순간 클라이언트와 서버의 통신은 끝나며 상태 정보를 유지하지 않음
- HTTP는 텍스트를 교환하는데, 누군가 네트워크에서 신호를 가로채면 내용이 노출되는 보안 이슈가 존재함 => 이 보안 문제를 해결해주는 프로토콜이 HTTPS

## HTTPS(Hyper Text Tranfer Protocol Secure)
: 인터넷 상에서 정보를 암호화하는 SSL 프로토콜을 사용해 클라이언트와 서버가 자원을 주고 받을 대 쓰는 통신 규약
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

### [HTTP & HTTPS의 동작 과정](https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md#http%EC%99%80-https-%EB%8F%99%EC%9E%91-%EA%B3%BC%EC%A0%95)
</br>

#### 참조
<https://gyoogle.dev/blog/computer-science/network/HTTP%20&%20HTTPS.html>

<https://github.com/WeareSoft/tech-interview/blob/master/contents/network.md#http%EC%99%80-https>