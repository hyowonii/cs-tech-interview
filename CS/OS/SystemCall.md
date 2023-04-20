# System Call

프로세스 생성과 제어를 위한 시스템 콜

### 🎈 fork()
: 새로운 프로세스 생성
```c
int main()
{
  int pid;
  pid = fork();
  if(pid == 0)  /*child*/
    printf("\n Hello, I am child!\n");
  else if(pid > 0)  /*parent*/
    printf("\n Hello, I am parent\n");
}
```
[해석]

`pid` : 프로세스 식별자. UNIX 시스템에서 pid는 프로세스에게 명령 할 때 사용

fork()가 실행되는 순간 프로세스가 하나 더 생김(child) - 기존 프로세스(parent)의 복사본

-> **이 때, OS는 똑같은 2개의 프로그램이 동작한다고 생각하고 fork()가 return될 차례라고 생각함**

그 때문에 새로 생성된 child는 main에서 시작하지 않고, if문부터 시작하게 됨

그러나, 차이점은 리턴값이 다름(유용한 방식)
```
parent의 fork() 값 => child의 pid 값
child의 fork() 값 => 0
```
Scheduler가 부모를 먼저 수행할지 아닐지는 확신할 수 없음



### 🎈 exec()
: 어떠한 프로세스를 완전히 새로운 프로세스로 태어나도록 하는 것

```c
int main()
{
  int pid;
  pid = fork();
  if(pid == 0)
  {
    printf("\n Hello, I am child! Now I'll run date\n");
    execlp("/bin/date", "/bin/date", (char*)0);
  }
  else if(pid > 0)
    printf("\n Hello, I am parent\n");
}
```

[해석]

`execlp("/bin/date", "/bin/date", (char*)0);` 해당 블록이 실행되면 기존의 프로그램이 a.out 이었다면 a.out이 아닌 date 프로그램을 실행하게 되어 전혀 다른 프로세스로 변신함. 따라서 원래 프로세스인 a.out의 프로세스로 다시 돌아올 수 없게 됨


### 🎈 wait()
: 해당 프로세스를 잠들게 함
- 커널은 child가 종료될 때까지 프로세스 A를 sleep 시킴 (block 상태)
- Child proces가 종료되면 커널은 프로세스 A를 깨움 (ready 상태)

![wait()](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fs36qf%2FbtqBua0CLDR%2FwTx99fsVh8aPLgR5yMq6J1%2Fimg.png)


### 🎈exit()
: 프로세스 종료

- 자발적 종료
  - 마지막 statement 수행 후 exit() 시스템 콜을 통해
  - 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어줌
- 비자발적 종료
  - 부모 프로세스가 자식 프로세스를 강제 종료시킴
    - 자식 프로세스가 한계치를 넘는 자원을 요청
    - 자식에게 할당된 태스크가 더 이상 필요하지 않음
  - 키보드로 kill, break 등을 친 경우
  - 부모가 종료하는 경우
    - 부모 프로세스가 종료되기 전에 자식들이 먼저 종료됨


#### 참고
<https://gyoogle.dev/blog/computer-science/operating-system/System%20Call.html>

<https://higunnew.tistory.com/25>
