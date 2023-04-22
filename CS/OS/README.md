# Operating System(운영체제)

- [운영체제란?](#🔎-운영체제란)
  - 운영체제의 역할
- [프로세스 & 스레드](#프로세스--스레드)
  - 프로세스
  - 스레드
  - 멀티 프로세스 & 멀티 스레드
- [CPU Schecduling](#cpu-scheduling)
  - 선점/비선점 스케줄링
  - CPU 스케줄링의 종류
  - CPU 스케줄링 성능 척도
- [Interrupt](#interrupt인터럽트)
  - 인터럽트의 개념
  - 인터럽트 처리 과정
- [IPC(Inter Process Communication)](#ipc-inter-process-communication)
  - IPC 종류
- [PCB & Context Switching](#pcb--context-switching)
  - PCB
  - Context Switching
- [System Call](#system-call)

--- 

## 🔎 운영체제란?
- 컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층
- 사용자와 컴퓨터 하드웨어 사이에서 하드웨어를 운영 및 관리하고 명령어를 제어하여 응용 프로그램 및 하드웨어를 소프트웨어적으로 제어 및 관리
- 컴퓨터 시스템의 자원을 효율적으로 관리하여 사용자가 컴퓨터를 편리하고 효과적으로 사용할 수 있도록 환경을 제공
- 종류: Windows, Linux, UNIX, MS-DOS 등

![Operating System](https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Operating_system_placement_kor.svg/180px-Operating_system_placement_kor.svg.png)


### 📝 운영체제의 역할
1. 프로세스 관리
- 운영체제에서 작동하는 응용 프로그램을 관리하는 기능
- 현재 CPU를 점유해야 할 프로세스를 결정하고, 실제로 CPU를 프로세스에 할당하며, 이 프로스세 간 공유 자원 접근과 통신 등을 관리
- 프로세스, 스레드, 스케줄링, 동기화, IPC 통신

2. 저장장치 관리
- 메인 메모리(1차 저장장치)와 하드디스크, NAND(2차 저장장치) 등을 관리
- 1차 저장장치(Main Memory)
  - 프로세스에 할당하는 메모리 영역의 할당과 해제
  - 각 메모리 영역 간의 침범 방지
  - 메인 메모리의 효율적 활용을 위한 가상 메모리 기능
- 2차 저장장치(HDD, NAND Flash Memory 등)
  - 파일 형식의 데이터 저장 -> 파일 데이터 관리를 위한 파일 시스템을 OS에서 관리
  - FAT, NTFS, EXT2, JFS, XFS 등의 파일 시스템들 관리

3. 네트워킹
- 컴퓨터 활용의 핵심
- 응용 프로그램이 네트워크를 사용하려면 OS에서 네트워크 프로토콜을 지원해야 함. 현재 상용 OS들은 다양한 네트워크 프로토콜을 지원

4. 사용자 관리
- 한 컴퓨터를 여러 사람이 사용하는 환경도 지원
- 한 컴퓨터 내에서 여러 계정을 각각 관리할 수 있는 기능이 필요. 사용자 별로 프라이버시와 보안을 위해 다른 사용자가 개인 파일에 접근할 수 없도록, 파일이나 시스템 자원에 접근 권한을 지정할 수 있도록 지원

5. 디바이스 드라이버
- 시스템에 있는 여러 하드웨어를 인식하고 관리하게 만들어 응용 프로그램이 하드웨어를 사용할 수 있게 만들어야 함
- 운영체제 안에 하드웨어를 추상화 해주는 계층이 필요 - 이 계층이 바로 디바이스 드라이버이고 운영체제가 이를 관리
- 하드웨어의 종류가 많은 만큼 운영체제 내부의 디바이스 드라이버도 많이 존재


# 프로세스 & 스레드

프로세스 : 메모리 상에서 실행 중인 프로그램

스레드 : 프로세스 안에서 실행되는 여러 흐름 단위

![프로세스&스레드](https://camo.githubusercontent.com/3dc4ad61f03160c310a855a4bd68a9f2a2c9a4c7/68747470733a2f2f74312e6461756d63646e2e6e65742f6366696c652f746973746f72792f393938383931343635433637433330363036)

## 💡 프로세스(Process)
컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램

### 개념
- 메모리에 올라와 실행되고 있는 프로그램의 인스턴스(독립적인 개체)
- 운영체제로부터 시스템 자원(주소공간, 파일, 메모리 등)을 할당받는 작업의 단위
- 동적인 개념으로는 실행된 프로그램을 의미

### 할당받는 시스템 자원의 예
  - CPU 시간
  - 운영되기 위해 필요한 주소 공간
  - Code, Data, Stack, Heap의 구조로 되어 있는 독립된 메모리 영역
    - Code : 코드 자체를 구성하는 메모리 영역 (프로그램 명령)
    - Data : 전역변수, 정적변수, 배열 등
      - 초기화 된 데이터 data 영역에 저장
      - 초기화 되지 않은 데이터는 bss 영역에 저장
    - Heap : 동적 할당 시 사용 (new(), malloc() 등)
    - Stack : 지역변수, 매개변수, 리턴 값 (임시 메모리 영역)
  ![메모리 영역](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/process.png)

### 특징
- 기본적으로 프로세스당 최소 1개의 스레드(메인 스레드)를 가짐
- 각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근 불가능
- 한 프로세스가 다른 프로세스의 자원에 접근하려면, 프로세스 간의 통신(IPC, Inter-Process Communication) 사용 (ex. 파이프, 파일, 소켓 등을 이용한 통신 방법 이용)

### 프로세스의 주소 공간
_: 프로그램이 CPU에 의해 실행되면 -> 프로세스가 생성되고 메모리에 **프로세스 주소 공간**이 할당됨_

**Code, Data, Stack, Heap**
- Code Segment : 사용자가 작성한 프로그램 함수들의 코드가 CPU에서 수행할 수 있는 기계어 명령 형태로 변환되어 저장되는 공감. 컴파일 타임에 결정되고, 중간에 코드를 바꿀 수 없도록 **Read-Only**로 되어있음
- Data Segment : 전역 변수 또는 static 변수 등 프로그램이 사용하는 데이터를 저장하는 공간. 전역 변수가 변결될 수도 있어 **Read-Write**로 되어있음
- Stack Segment : 호출된 함수의 수행을 마치고 복귀할 주소 및 데이터(지역변수, 매개변수, 리턴값 등)를 임시로 저장하는 공간. LIFO. 컴파일 시 stack 영역의 크기가 결정되므로 무한정 할당할 수 없음. 메모리를 초과하면 stack overflow 발생.
- Heap segment : 프로그래머가 필요할 때마다 사용하는 메모리 영역. 런타임에 결정됨.

_Q. 왜 Code 부분을 따로 두었나?_

_A. 프로그램의 코드는 프로그램이 컴파일 되고 나서는 바뀔 일이 없다(Read Only). 따라서 같은 프로그램을 실행시켜 몇 개의 프로세스가 실행되더라도 다 똑같은 코드 내용을 가지고 있는 것이기 때문에 Code 부분을 공유하여 메모리 사용량을 줄이는 목적이다._

_Q. 왜 Stack 부분과 Data 부분을 나누었나?_

_A. 함수 외부와 함수에 따라 Stack 구조 활용을 위해 나누어 둔 것이다. 전역 변수의 경우 어떤 함수에서도 접근할 수 있어야 하므로 Data 영역에 넣고, 함수의 경우 호출 시 Stack 영역에 넣었다가 종료 시 Stack 영역에서 빼는 식으로 사용된다._

<img width=300 src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2630833B58F1C0363A">

## 💡 스레드(Thread)
프로세스 내에서 실행되는 여러 흐름의 단위

### 개념
- 프로세스의 특정한 수행 경로
- 프로세스가 할당받은 자원을 이용하는 실행의 단위

### 특징
- 프로세스 내에서 각각 Stack만 따로 할당 받고, Code, Data, Heap 영역은 공유
- 각각의 스레드는 별도의 레지스터와 스택을 갖고 있지만, 힙 메모리는 서로 읽고 쓸 수 있음
- 한 스레드가 프로세스 자원을 변경하면 다른 이웃 스레드(sibling thread)도 그 변경 결과를 즉시 볼 수 있음

_**프로세스는 자신만의 고유 공간과 자원을 할당받아 사용하는데 반해, 스레드는 다른 스레드와 공간 및 자원을 공유하면서 사용하는 차이가 존재**_
</br></br>

## 📍 멀티 프로세스 & 멀티 스레드

### **멀티 프로세스**
: 하나의 프로그램의 여러 개의 프로세스로 구성하여 각 프로세스가 병렬적으로 작업을 수행하는 것
- 장점 
  - 안정성 (메모리 침범 문제를 OS차원에서 해결)
- 단점
  - 각각 독립된 메모리 영역을 갖고 있어 작업량이 많을수록 오버헤드 발생. Context Switching으로 인한 성능 저하

_Context Switching?_

프로세스의 상태 정보를 저장하고 복원하는 일련의 과정

즉, 동작 중인 프로세스가 대기하면서 해당 프로세스의 상태를 보관하고, 대기하고 있던 다음 순번의 프로세스가 동작하면서 이전에 보관했던 프로세스 상태를 복구하는 과정

-> 프로세스는 각 독립된 메모리 영역을 할당받아 운영하므로, 캐시 메모리 초기화와 같은 무거운 작업이 진행되었을 때 오버헤드 발생 문제 존재

### **멀티 스레드**
: 하나의 응용 프로그램에서 여러 스레드가 각각 하나의 작업을 처리하는 것. 스레드들이 공유 메모리를 통해 다수의 작업을 동시에 처리하도록 함

- 장점
  - 독립적인 프로세스에 비해 공유 메모리만큼의 시간, 자원 손실 감소. 
  - 전역 변수와 정적 변수에 대한 자료 공유 가능
- 단점
  - 안정성 문제. 하나의 스레드가 데이터 공간을 망가뜨리면 모든 스레드가 작동 불가능 (공유 메모리)
  - 동기화 이슈로 인한 비정상적 동작 가능. 실행 순서 또는 메모리 접근 과정에서 생기는 문제


### 🚩 **멀티 프로세스 대신 멀티 스레드를 사용하는 이유**
![멀티 스레드](https://github.com/WeareSoft/tech-interview/raw/master/contents/images/multi-thread.png)

1. 자원의 효율성 증대
- 프로세스를 생성하여 자원을 할당하는 시스템 콜이 줄어듦 (프로세스 간의 Context Switching 시 단순히 CPU 레지스터 교체 뿐만 아니라 RAM과 CPU 사이의 캐시 메모리에 대한 데이터까지 초기화되므로 오베헤드가 큼)
- 스레드 간 데이터를 주고 받는 것이 더 간단하고 따라서 시스템 자원 소모가 줄어듦

2. 처리 비용 감소 및 응답 시간 단축
- 프로세스 간 통신(IPC)보다 스레드 간의 통신의 비용이 적어 작업들 간의 통신 부담이 줄어듦
- 프로세스 간의 전환 속도보다 스레드 간의 전환 속도가 더 빠름

---

# Interrupt(인터럽트)

: 프로그램을 실행하는 도중에 예기치 않은 상황이 발생할 경우 현재 실행 중인 작업을 즉시 중단하고 발생한 상황에 대한 우선 처리가 필요함을 마이크로프로세서에게 알리는 것. 현재 수행 중인 일보다 더 중요한 일(ex. 입출력, 우선 순위 연산 등)이 발생하면 그 일을 먼저 처리하고 나서 하던 일을 계속해야함.

- 외부 / 내부 인터럽트는 `CPU의 하드웨어 신호에 의해 발생`
- 소프트웨어 인터럽트는 `명령어의 수행에 의해 발생`

### 외부 인터럽트
입출력 장치, 타이밍 장치, 전원 등 외부적인 요인으로 인해 발생
- `전원 이상, 기계 착오, 외부 신호, 입출력`

### 내부 인터럽트
Trap이라고 부르며, 잘못된 명령이나 데이터를 사용할 때 발생
- `0으로 나누기, 오버플로우, 명령어 잘못 사용` 등 (Exception)

### 소프트웨어 인터럽트
프로그램 처리 중 명령의 요청에 의해 발생 (SCV 인터럽트)
- 사용자가 프로그램을 실행시킬 때 발생
- 소프트웨어 이용 중에 다른 프로세스를 실행시키면 시분할 처리를 위해 자원 할당 동작이 수행됨

## 💊 인터럽트 처리 과정

![인터럽트 처리 과정](https://mblogthumb-phinf.pstatic.net/20160310_124/scw0531_14575366291105WjS7_PNG/ERTRTETRE.png?type=w2)

process A 실행 중 디스크에서 어떤 데이터를 읽어오라는 명령을 받았다고 가정해보자.
- process A는 `system call`을 통해 인터럽트를 발생시킨다.
- CPU는 현재 진행 중인 기계어 코드를 완료한다.
- 현재까지 수행 중이었던 상태를 해당 process의 PCB(Process Control Block)에 저장한다. (수행 중이던 메모리 주소, 레지스터 값, 하드웨어 상태 등등..)
- PC(Program Counter, IP)에 다음에 실행할 명령의 주소를 저장한다.
- 인터럽트 벡터를 읽고 ISR(Interrupt Service Routine) 주소값을 얻어 ISR로 점프하여 루틴을 실행한다.
- 해당 코드를 실행한다.
- 처리하고 나면 대피시킨 레지스터를 복원한다.
- ISR의 끝에 IRET 명령어에 의해 인터럽트가 해제 된다.
- IRET 명령어가 실행되면, 대피시킨 PC 값을 복원하여 이전 실행 위치로 복원한다.

만약 인터럽트 기능이 없었다면, 폴링(Polling) 기법
- 인터럽트 방식 & 폴링 방식 (컨트롤러가 우선순위를 판별하는 법)
  - 폴링 방식
    - 사용자가 명령어를 사용해 입력 핀의 값을 계속 읽어 변화를 알아내는 방식
    - 인터럽트 요청 플래그를 차례로 비교하여 우선순위가 가장 높은 인터럽트 자원을 찾아 이에 맞는 인터럽트 서비스 루틴을 수행 (하드웨어에 비해 속도가 느림)
  - 인터럽트 방식
    - MCU 자체가 하드웨어적으로 신호를 확인하여 변화 시에만 일정한 동작을 하는 방식
  - 인터럽트 방식은 하드웨어 지원을 받아야 하는 제약이 있지만 폴링에 비해 신속하게 대응할 수 있음. 실시간 대응이 필요할 때는 필수적인 기능.
  - 즉, 인터럽트는 발생시기를 예측하기 힘든 경우에 컨트롤러가 가장 빠르게 대응할 수 있는 방법

---

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

---

# PCB & Context Switching

_프로세스가 여러 개일 때, CPU가 CPU 스케쥴링을 통해 관리하는 것을 Process Management라고 한다. 이 때, CPU는 각 프로세스들이 누군지 알아야 관리가 가능하다. 프로세스의 특징을 가지고 있는 것이 바로 Process Metadata(Process ID, Process State, Process Priority, CPU Registers, Owner, CPU Usage, Memory Usage)이고, 이 메타데이터는 프로세스가 생성되면 PCB(Process Control Block)에 저장된다._

## PCB(Process Control Block)
: 프로세스 메타데이터를 저장하는 곳, 한 PCB 안에는 한 프로세스의 정보가 담김

<img width=300 src="https://t1.daumcdn.net/cfile/tistory/25673A5058F211C224">

> `프로그램 실행 -> 프로세스 생성 -> 프로세스 주소 공간에 (코드, 데이터, 스택) 생성 -> 이 프로세스의 메타데이터들이 PCB에 저장됨`

**PCB가 왜 필요한가?**

CPU에서는 프로세스의 상태에 다라 교체작업이 이루어짐. 이 때, 앞으로 다시 수행할 대기 중인 프로세스에 관한 저장 값을 PCB에 저장해두는 것

**PCB는 어떻게 관리되나?**

LinkedList 방식으로 관리됨. PCB List Head에 PCB들이 생성될 때마다 붙게 됨. 주소값으로 연결이 이루어져 있는 연결리스트이기 때문에 삽입 및 삭제가 용이. 즉, 프로세스가 생성되면 해당 PCB가 생성되고 프로세스 완료시 제거됨

이렇게 수행 중인 프로세스를 변경할 때, CPU의 레지스터 정보가 변경되는 것을 **Context Switching**이라고 함

## Context Switching [참고](https://nesoy.github.io/articles/2018-11/Context-Switching)

: 현재 진행하고 있는 Task(Process, Thread)의 상태를 저장하고 다음 진행할 Task의 상태 값을 읽어 적용하는 과정

- 보통 인터럽트가 발생하거나, 실행 중인 CPU 사용 허가 시간을 모두 소모하거나, 입출력을 위해 대기해야 하는 경우 발생

- 즉, 프로세스가 Read -> Running, Running -> Ready, Running -> Waiting 처럼 상태 변경 시 발생

### **Context Switching Cost**
Context Switching이 발생하게 되면 많은 Cost가 소요됨
- Cache 초기화
- Memory Mapping 초기화
- Kernel은 항시 실행 (메모리 접근 위해)

_Process Context Switching Cost > Thread Context Switching Cost_

---

# IPC (Inter Process Communication)

프로세스는 독립적으로 실행됨. 독립 되어있다는 것은, 다른 프로세스에게 영향을 받지 않는다는 것 (스레드는 프로세스 안에서 자원을 공유하므로 서로 영향)

이런 독립적 구조를 가진 **프로세스 간의 통신**을 수행해야 하는 상황이 있을 수 있음 => **IPC 통신**

프로세스 커널이 제공하는 IPC 설비를 이용해 프로세스간 통신을 할 수 있음

_*[커널이란?](https://minkwon4.tistory.com/295)_
운영체제 중 항상 메모리에 올라가 있는 운영체제의 핵심 부분으로써 하드웨어와 응용 프로그램 사이에서 인터페이스를 제공하는 역할을 하며 컴퓨터 자원들을 관리하는 역할을 한다. 즉, 커널은 인터페이스로써 응용 프로그램 수행에 필요한 여러가지 서비스를 제공하고 여러가지 하드웨어(CPU, 메모리) 등의 리소스를 관리하는 역할을 한다.

## IPC 종류
IPC 설비 종류도 여러가지가 있음. 필요에 따라 선택해서 사용

1. 익명 PIPE
- 파이프는 두 개의 프로세스를 연결. 하나의 프로세스는 데이터를 쓰기만 하고, 다른 하나는 데이터를 읽기만 함
- **한쪽 방향으로만 통신이 가능한 반이중 통신** -> 양쪽으로 모두 송/수신을 원하면 2개의 파이프를 만듦
- 장점 : 사용이 매우 간단. 단순한 데이터 흐름을 가질 땐 파이프를 사용하는 것이 효과적
- 단점 : 전이중 통신을 위해 2개를 만들어야 할 때는 구현이 복잡

2. Named PIPE (FIFO)
- 익명 파이프는 통신할 프로세스를 명확히 알 수 있는 경우에 사용(부모-자식 프로세스 간 통신처럼)하는 반면, Named 파이프는 전혀 모르는 상태의 프로세스들 사이의 통신에 사용됨
- 익명 파이프의 확장 상태. **부모 프로세스와 무관한 다른 프로세스도 통신이 가능**한 것 (통신을 위해 이름 있는 파일 사용)
- 하지만 Named 파이프 역시 읽기/쓰기 동시에 불가능함. 따라서 전이중 통신을 위해서는 익명 파이프와 마찬가지로 2개를 만들어야 가능

3. Message Queue
- 입출력 방식은 Named 파이프와 동일
- 데이터의 흐름(파이프)이 아닌 메모리 공간
- 사용할 데이터에 번호를 붙여 여러 프로세스가 동시에 데이터를 쉽게 다룰 수 있음

4. 공유 메모리
- 파이프, 메시지 큐가 통신을 이용한 설비라면, 공유 메모리는 **데이터 자체를 공유하도록 지원하는 설비**
- **프로세스간 메모리 영역을 공유해서 사용할 수 있도록** 허용해줌 (스레드처럼)
- 프로세스가 공유 메모리 할당을 커널에 요청하면 커널은 해당 프로세스에 메모리 공간을 할당해주고, 이후 모든 프로세스는 해당 메모리 영역에 접근할 수 있게 됨
  - **중계자 없이 곧바로 메모리에 접근할 수 있어 IPC 중 가장 빠르게 작동**

5. 메모리 맵
- 메모리 공유. **열린 파일을 메모리에 맵핑시켜서 공유**하는 방식 (즉, 공유 매개체가 파일+메모리)
- 주로 파일로 대용량 데이터를 공유해야 할 때 사용

6. 소켓
- 네트워크 소켓 통신을 통해 데이터 공유
- 클라이언트와 서버가 소켓을 통해 통신하는 구조, 원격에서 프로세스 간 데이터를 공유할 때 사용
- 서버(bind, listen, accept), 클라이언트(connect)

이러한 IPC 통신에서 프로세스 간 데이터를 동기화하고 보호하기 위해 [세마포어와 뮤텍스](https://heeonii.tistory.com/14)를 사용 (공유된 자원에 한번에 하나의 프로세스만 접근시킬 때)

---

# CPU Scheduling
: 실행 중인 프로세스 간의 전환을 의미. 이 전환을 통해 CPU 활용률을 극대화하고 컴퓨터 생산성을 향상시킴 (멀티 프로그램 시스템의 기초를 형성)

## 선점 / 비선점 스케줄링
- 선점(preemptive) : OS가 CPU의 사용권을 선점할 수 있는 경우, 강제 회수하는 경우 (처리시간 예측 어려움)
- 비선점(nonpreemptive) : 프로세스 종료 or I/O 등의 이벤트가 있을 때까지 실행 보장 (처리시간 예측 용이)

### 프로세스 상태
![프로세스 상태](https://t1.daumcdn.net/cfile/tistory/99E85E3A5C460F1906)
- 선점 스케줄링 : `Interrupt`, `I/O or Event Completion`, `I/O or Event Wait`, `Exit`
- 비선점 스케줄링 : `I/O or Event Wait`, `Exit`

프로세스의 상태 전이
- Admiited(승인) : 프로세스 생성이 가능하여 승인됨
- Scheduler Dispatch(스케줄러 디스패치) : 준비 상태에 있는 프로세스 중 하나를 선택하여 실행시킴
- Interrupt(인터럽트) : 예외, 입출력, 이벤트 등이 발생하여 현재 실행 중인 프로세스를 준비 상태로 바꾸고 해당 작업을 먼저 처리하는 것
- I/O or Event Wait(입출력 또는 이벤트 대기) : 실행 중인 프로세스가 입출력이나 이벤트를 처리해야 하는 경우, 입출력/이벤트가 모두 끝날 때까지 대기 상태로 만드는 것
- I/O or Event Completion(입출력 또는 이벤트 완료) : 입출력/이벤트가 끝난 프로세스를 준비 상태로 전환하여 스케줄러에 의해 선택될 수 있도록 만드는 것

## CPU 스케줄링의 종류
- 비선점 스케줄링
  1. FCFS(First Come First Served)
      - 큐에 도착한 순서대로 CPU 할당
      - 실행 시간이 짧은 게 뒤로 가면 평균 대기 시간이 길어짐
  2. SJF(Shortest Job First)
      - 수행시간이 가장 짧다고 판단되는 작업을 먼저 수행
      - FCFS보다 평균 대기 시간 감소, 짧은 작업에 유리
  3. HRN(Highest Response-ratio Next)
      - 우선순위를 계산하여 점유 불평등을 보완한 방법(SJF의 단점 보완)
      - 우선순위 = (대기시간 + 실행시간) / (실행시간)
- 선점 스케줄링
  1. Priority Scheduling
      - 정적/동적으로 우선순위를 부여하여 우선순위가 높은 순서대로 처리
      - 우선 순위가 낮은 프로세스가 무한정 기다리는 Starvation이 생길 수 있음
      - Aging 방법으로 Starvation 문제 해결 가능 (*Aging: 시간이 갈수록 우선순위를 증가시킴)
  2. Round Robin
      - FCFS에 의해 프로세스들이 보내지면 각 프로세스는 동일한 시간의 Time Quantum만큼 CPU를 할당 받음 (*Time Qunatum or Time Slice: 실행의 최소 단위 시간)
      - 할당 시간(Time Quantum)이 크면 FCFS와 같게 되고, 작으면 문맥 교환(Context Switching)이 잦아져 오버헤드 증가
  3. Multilevel-Queue (다단계 큐)
      - 작업들을 여러 종류의 그룹으로 나누어 여러 개의 큐를 이용하는 기법
          ![Multilevel Queue](https://notesformsc.org/wp-content/uploads/2019/05/Multilevel-Queue.png)
      - 우선순위가 낮은 큐들이 실행하지 못하는 것을 방지하고자 각 큐마다 다른 Time Quantum을 설정해주는 방식 사용
      - 우선순위가 높은 큐는 작은 Time Quantum 할당, 우선순위가 낮은 큐는 큰 Time Quantum 할당
  4. Multilevel-Feedback-Queue (다단계 피드백 큐)
      ![Multilevel-Feedback-Queue](https://user-images.githubusercontent.com/13609011/91695489-2cb0b500-eba9-11ea-8578-6602fee742ed.png)
      
      - 다단계 큐에서 자신의 Time Quantum을 다 채운 프로세스는 밑으로 내려가고 그렇지 못한 프로세스는 원래 큐 그대로
      - 짧은 작업에 유리, 입출력 위주(Interrupt가 잦은) 작업에 우선권을 줌
      - 처리 시간이 짧은 프로세스를 먼저 처리하기 때문에 Turnaround 평균 시간을 줄여줌

## CPU 스케줄링 성능 척도
- CPU utilization : keep the CPU as busy as possible
- Throughput : number of processes that complete their execution per time unit
- Turnaround time(average) : amount of time to execute a particular process
- Waiting time : amount of time a process has been waiting in the ready queue
- Response time : amount of time it takes from when a request was submitted until the first response is produced, not ouput


