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

#### 참고
<https://gyoogle.dev/blog/computer-science/operating-system/PCB%20&%20Context%20Switching.html>