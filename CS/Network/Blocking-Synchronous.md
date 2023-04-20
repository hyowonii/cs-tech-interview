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

</br></br>

#### 참고
<https://gyoogle.dev/blog/computer-science/network/Blocking,Non-blocking%20&%20Synchronous,Asynchronous.html>

<https://etloveguitar.tistory.com/140>