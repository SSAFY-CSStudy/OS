# CPU burst, IO burst
**burst** : 짧은 시간동안 집중적으로 발생하는 활동이나 사건

**CPU burst** : CPU를 사용하여 계산 작업을 수행하는 기간

**IO burst** : 입출력 작업을 수행하는 기간

![](https://i.imgur.com/JW8LAxH.png)

## CPU burst 길이
![](https://i.imgur.com/mFbbjcx.png)
주로 짧은 시간을 가짐 -> I/O burst외에도 다양한 이유가 있음

1️⃣ **시분할, Context Switch**
- 프로세스가 독점하는 것을 방지하기 때문에 CPU burst가 짧아짐

2️⃣ **시스템 콜, 인터럽트**
- 운영체제 서비스를 요청하거나 외부 이벤트에 반응할 때 CPU를 다른 작업에 할당

# CPU Scheduling
## CPU 스케줄링 발생 조건
1️⃣ 프로세스의 상태가 `running` -> `waiting`으로 변경

	- IO request의 결과가 필요할 때
	- 자식 프로세스의 중단을 위해 `wait()`을 호출할 때
2️⃣ 프로세스의 상태가 `running` -> `ready`로 변경

	- 비 IO 하드웨어 인터럽트 (타이머 인터럽트)
3️⃣ 프로세스의 상태가 `waiting` -> `ready`로 변경

	- IO 완료
4️⃣ 프로세스의 상태가 `terminated`로 변경

	- 프로세스 종료

### 비선점형 스케줄링
1️⃣ : 프로세스가 CPU를 자발적으로 양도

4️⃣ : 자발적으로 CPU를 마치고 프로세스가 시스템에서 완전히 제거

> 💡 프로세스가 CPU를 선점하지 않음

### 선점형 스케줄링
2️⃣ : 타이머 인터럽트 등을 사용하여 운영체제가 프로세스를 선점하여 상태 변경

3️⃣ : I/O 작업 완료 인터럽트가 발생할 때 운영체제가 프로세스를 `ready` 상태로 변경

> 💡 운영체제가 선점을 통해 프로세스를 관리
## Dispatcher
### 역할
1. 한 프로세스에서 다른 프로세스로 Context Switch
2. user mode로 스위치
3. 프로그램을 다시 시작하기 위해 유저 프로그램의 적절한 위치로 점프 (PCB 정보를 저장하고 불러옴)

## 스케줄링 기준
1️⃣ **CPU 사용률**

: 최대한 많이 사용할 수 있도록 함
40~90% 사용

2️⃣ **Throughput**

: 시간 단위 당 완료되는 프로세스의 수

3️⃣ **Turnaround time**

: 해당 프로세스를 실행하는데 걸리는 시간

ready queue 대기 시간 + CPU 실행 시간 + I/O 시간

4️⃣ **Waiting time**

: 프로세스가 ready queue에서 대기하는데 걸리는 시간

5️⃣ **Response time**

: 요청 후 첫 번째 응답이 생성될 때까지의 시간

응답을 시작하는데 걸리는 시간

> CPU 사용률, Throughput 🔺
>
> Turnaround time, Waiting time, Response time 🔻

## 스케줄링 알고리즘
### 1️⃣ FCFS
**First-Come First-Serve** : 선입선출
비선점형
FIFO 큐를 사용하여 관리
![](https://i.imgur.com/7qnm1cD.png)
### 원리
1️⃣ 프로세스가 Ready Queue에 들어가면 프로세스의 PCB가 대기열의 꼬리 부분에 연결

2️⃣ CPU가 여유가 생기면 대기열의 맨 앞에 있는 프로세스에 CPU 할당

3️⃣ 실행 중인 프로세스는 대기열에서 삭제

![](https://i.imgur.com/o2zPKGp.png)

처리 순서가 P2 -> P3 -> P1일 경우
![](https://i.imgur.com/YnNOg23.png)
P1 대기시간 : 6

P2 대기시간 : 0	

P3 대기시간 : 3
> 💡총 대기시간 : (6 + 0 + 3) / 3 = 3
### 장점
이해하기 쉽고 간단
### 단점
비선점형 -> 대화형 시스템에서 문제
평균 대기 시간이 긴 경우가 많음
CPU burst 시간에 의해 크게 달라짐
 
> 위 예시에서 처리순서가 P1 -> P2 -> P3일 경우
>	![](https://i.imgur.com/w9boZZB.png)
>	P1 대기시간 : 0
>	P2 대기시간 : 24
>	P3 대기시간 : 27
>
>	💡 총 대기시간 : (0 + 24 + 27) / 3 = 17

### 2️⃣ SJF
**Shortest-Job-First**
현재 대상 프로세스의 길이가 아닌 **다음 프로세스의 길이**에 따라 할당

같은 실행시간에 대하여 FCFS 사용

특별한 형태의 Priority Scheduling

![](https://i.imgur.com/O5SqTa5.png)

정렬 순서 : P4 -> P1 -> P3 -> P2
![](https://i.imgur.com/QgZ78re.png)
P1 대기 시간 : 3

P2 대기 시간 : 16

P3 대기 시간 : 9

P4 대기 시간 : 0
> 💡 총 대기 시간 : (3 + 16 + 9 + 0) / 4 = 7

_FCFS를 사용할 경우 <br>
P1 대기 시간 : 0 <br>
P2 대기 시간 : 6 <br>
P3 대기 시간 : 14 <br>
P4 대기 시간 : 21 <br>
으로 총 (0 + 6 + 14 + 21) / 4 = 10.25의 대기 시간 발생_

### 변형
![](https://i.imgur.com/j493mYc.png)

새로운 프로세스가 들어왔을 때, 현재 실행 중인 프로세스의 실행 시간이 새로 들어온 프로세스의 실행 시간보다 많을 경우,

1️⃣ **비선점형**

현재 실행중인 프로세스의 실행을 지속

![](https://i.imgur.com/m9KTuu9.png)

P1 대기 시간 : 0

P2 대기 시간 : 7 (8 - 1)

P3 대기 시간 : 15 (17 - 2)

P4 대기 시간 : 9 (12 - 3)
> 💡총 대기 시간 : (0 + 7 + 15 + 9) / 4 = 7.75

2️⃣ **선점형**

현재 실행중인 프로세스의 CPU burst를 끝냄

![](https://i.imgur.com/B3Zqdq6.png)

P1 대기 시간 : 9 (10 - 1)

P2 대기 시간 : 0

P3 대기 시간 : 15 (17 - 2)

P4 대기 시간 : 2 (5 - 3)
>💡 총 대기 시간 : (9 + 0 + 15 + 2) / 4 = 6.5
### 원리
1️⃣ 예상 시간에 따라 정렬
- Priority Queue 등을 사용

2️⃣ 새로운 프로세스가 도착하면 예상 실행 시간을 기반으로 큐에 삽입

### 장점
짧은 프로세스의 대기 시간이 감소하기 때문에 **최소 평균 대기 시간**을 제공하므로 최적
### 단점
다음 프로세스의 길이를 정확히 알 수 없음
-> 다음 프로세스의 실행 시간을 예측(Approximate SJF)하는 방법 사용

### 3️⃣ RR
**Round-Robin**

선점형 FCFS와 유사
### 원리
**time quantum** : 10 ~ 100ms의 작은 시간 단위

1️⃣  FIFO queue와 동일하게 프로세스가 ready queue에 등록

2️⃣ 새로운 프로세스는 queue의 tail에 등록

3️⃣ 프로세스를 실행할 때 head에 위치한 프로세스를 선점하고, 1번의 time quantum 부여

4️⃣ 타임 퀀텀이 만료될 경우 타이머 인터럽트가 발생해 큐의 맨 뒤로 이동, 프로세스 작업이 완료되면 제거

![](https://i.imgur.com/7otPP6y.png)
1 time quantum = 4ms
![](https://i.imgur.com/ZahuN9I.png)

P1 대기 시간 : 6 (10 - 4)

P2 대기 시간 : 4

P3 대기 시간 : 7
>💡 총 대기 시간 : (6 + 4 + 7) / 3 = 5.66
### 장점
각 프로세스에게 균등한 처리량 보장
### 단점
성능, Turnaround Time이 Time quantum에 달려 있음
	![](https://i.imgur.com/FSRPVe2.png)
- 매우 클 경우, FCFS와 다를 바가 없음
- 매우 작을 경우, 과도한 인터럽트가 일어남
> 💡 CPU burst의 80%가 time quantum보다 짧은 것이 최적
### 4️⃣ Priority Scheduling
각각의 프로세스의 PCB에 priority를 저장하고 이에 따라 스케줄링
높은 정수의 priority가 우선순위가 높을지 낮을지는 운영체제에 따라 다름

선점형 혹은 비선점형일 수 있음

같은 priority에 대해 FCFS 스케줄링 기법 사용
### 원리
![](https://i.imgur.com/K05oOIx.png)
![](https://i.imgur.com/Vhtxynh.png)

P1 대기 시간 : 6

P2 대기 시간 : 0

P3 대기 시간 : 16

P4 대기 시간 : 18

P5 대기 시간 : 1
> 💡 총 대기 시간 : (6 + 0 + 16 + 18 + 1) / 5 = 8.2
#### 우선순위 결정
1️⃣ **외부적** : OS 외부적인 요소 (프로세스의 중요성, 타입, 비용, 정책 등)
2️⃣ **내부적** : 시간 제한, 필요한 메모리, 열어야 하는 파일의 개수, I/O burst 비율 등

### 단점
**고립** : low priority를 무한으로 `waiting`상태에 둠
> 해결방안
> 1. **aging**
> : 시간이 지날 때마다 priority를 증가시킴
> 2. RR과 함께 사용
> : 같은 priority에 대해 RR 사용
### 5️⃣ Multilevel Queue Scheduling
queue가 여러 개인 스케줄링

1️⃣ **Priority + RR** scheduling
![](https://i.imgur.com/e8bwytN.png)
여러 개의 priority queue가 있고, 전체적인 queue는 RR 스케줄링 사용

2️⃣ **Process Type**
![](https://i.imgur.com/cwbaZCN.png)
foreground에서 동작하는 process가 높은 우선순위를 지니고,
background에서 동작하는 process가 낮은 우선순위를 지님

>💡 foreground process가 background process보다 더 빠른 응답 시간을 요구함
>
>큐들 사이에서의 스케줄링은 고정 우선순위 선점형 스케줄링으로 구현됨 -> 실시간 프로세스가 더 우세

### queue 내부 스케줄링
1️⃣ **foreground**
- RR
- Priority Scheduling
> 💡 **선점형 스케줄링** : 빠른 응답시간이 중요하기 때문

2️⃣ **background**
- FCFS
- SJF
> 💡 **비선점형 스케줄링** : 처리 속도보다는 자원 활용도가 더 중요하고, 긴 작업을 처리하는 데 적합하기 때문

### queue간 스케줄링
1️⃣ **우선순위**

	- interactive, system process queue가 비어있지 않으면 batch process는 실행되지 않음
    - batch process 실행 중 우선순위가 높은 process가 들어오면 해당 프로세스가 CPU 선점
2️⃣ **time slice**

	- 각 큐가 CPU 시간의 일정 비율을 할당
	ex) foreground 큐에 80%, background 큐에 20% 할당
	
## 단점
process가 queue에 할당될 때 foreground, background에 영구적으로 할당 <br>
-> 유연하지 않음 <br>
-> Multilevel Feedback Queue Scheduling으로 해결
### 6️⃣ Multilevel Feedback Queue Scheduling
## Multi-Processor란?
1️⃣ Multicore CPU

2️⃣ Multithreaded core

3️⃣ NUMA system

4️⃣ HMP
## 방법
### 1️⃣ Asymmetric multiprocessing
**원리**
- 단일 프로세서 (마스터 서버)가 모든 스케줄링, 입출력 등을 처리
- 다른 프로세서는 사용자 코드만 실행

**장점**
- 하나의 코어만 데이터 구조에 엑세스하기 때문에 간단하고 데이터 공유 감소

**단점**
- 마스터 서버가 전체 시스템 성능을 저하시킬 수 있음
### 2️⃣ Symmetric multiprocessing
**원리**
- 각 프로세서가 자체 스케줄링 수행

**방법**
1. 모든 스레드가 동일한 ready queue에 존재
	![](https://i.imgur.com/q64X7XY.png)
	데이터 레이스가 발생할 수 있으므로 lock이 필요하지만 원격 접근 시 병목 발생
2. 각각의  프로세서가 자신만의 스레드 queue를 가짐
	![](https://i.imgur.com/yXMc4dr.png)
	일반적인 SMP
	공유 메모리에 대한 문제 해결
