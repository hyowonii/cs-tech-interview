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

#### 참조
<https://gyoogle.dev/blog/computer-science/network/Load%20Balancing.html></br>
<https://nesoy.github.io/articles/2018-06/Load-Balancer>