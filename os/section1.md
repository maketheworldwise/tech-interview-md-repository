- [Process vs Thread vs Multi-Thread vs Multi-\*](#process-vs-thread-vs-multi-thread-vs-multi-)
- [Context Switching](#context-switching)
- [Race Condition](#race-condition)
- [Sync vs Async](#sync-vs-async)

---

### Process vs Thread vs Multi-Thread vs Multi-\*

> 💁🏻 : Process와 Thread의 차이는 메모리를 공유하느냐 안하느냐의 차이다. Process가 Thread보다 보다 더 큰 단위라고 생각하면 된다.

해당 내용은 [Java Multi-Thread](/java/section3.md#java-multi-thread)에서도 다뤘었다.

**Program**

Program은 파일이 저장 장치에 저장되어있지만 메모리에는 올라가 있지 않은 정적인 상태를 의미한다.

**Process**

Program이 메모리에 올렸을 때 Program은 동적인 상태로 변하는데 이를 Process라 지칭한다. 더 전문적으로 이야기하자면, 프로세스는 실행 중인 프로그램으로 디스크로부터 메모리에 적재되어 CPU의 할당을 받을 수 있는 것을 의미한다.

Process간 정보 공유가 불가능한 것은 아니다. IPC/LPC를 사용하거나 별도의 공유 메모리를 만들어 정보를 주고 받도록 구성하여 공유할 수는 있다. 하지만 그 경우에는 자원 부담이 크다는 단점이 있다.

Process에는 PCB(Process Control Block)가 있다. PCB는 특정 Process에 대한 중요한 정보를 저장하고 있는 OS의 자료구조다. OS는 Process를 관리하기 위해 Process의 생성과 동시에 고유한 PCB를 생성한다. Process는 CPU를 할당받아 자원을 처리하다가도 Process 전환이 발생하면 진행하던 작업을 저장하고 CPU를 반환해야하는데 이때 작업의 진행 상황을 모두 PCB에 저장하게 된다. 그리고 다시 CPU를 할당받게 되면 PCB에 저장되어있던 내용을 불러와 이전에 종료됬던 시점부터 다시 작업을 수행한다.

(PCB는 [Context Switching](#context-switching)과 관련되어있기에 동작 흐름은 설명하지 않았다.)

**Thread**

Process는 실행하다 오류가 발생하여 강제 종료되면 공유하고 있는 파일을 손상시키는 경우가 아니라면 Process에 영향을 주지 않는다. 하지만 Thread는 메모리 영역을 공유하기 때문에 하나의 Thread에서 문제가 발생하면 Process 내의 다른 Thread 모두 강제로 종료된다.

![](/docs/images/multi-process.png)

각 Thread에는 독립적인 스택과 PC Register 영역이 존재한다.

스택이 존재하는 이유는 함수 호출시 전달되는 인자, 되돌아갈 주소값 및 함수 내에서 선언한 변수 등을 저장하기 위해 사용되는 메모리 공간이므로 스택 메모리 공간이 독립적이라는 의미는 독립적인 함수를 호출할 수 있다는 것이고 이는 독립적인 실행 흐름이 추가되는 것이다. 즉, Thread의 정의에 따라 독립적인 실행 흐름을 추가하기 위한 최소 조건으로 독립적인 스택 영역이 할당되는 것이다.

PC Register는 Thread의 명령어의 어디까지 수행되었는지를 나타내기 위해 필요하다. Thread는 CPU를 할당받아 스케줄러에 의해 다시 선점당하기 때문에 명령어가 연속적으로 수행되지 못하고 어느 부분까지 수행했는지 기억해야한다. 따라서 PC Register를 독립적으로 할당한다.

**Multi-Programming**

Multi-Programming은 하나의 CPU가 하나의 Process를 수행하는 동안 다른 Process에 접근할 수 있도록 하는 기법이다. 과거에는 하나의 CPU가 하나의 Process만을 처리할 수 있도록 설계되어 입출력 처리가 완료될때까지 기다려야하는 비효율적인 상황이 발생했다. 하지만 Multi-Programming은 입출력이 완료될 때까지 기다리는 시간을 버리지 않고 다른 Process를 처리할 수 있도록 가능해졌다.

**Multi-Tasking**

Multi-Tasking은 다양한 작업을 동시에 수행할 수 있도록 해주는 개념이다. 다수의 작업을 운영체제의 스케줄링에 의해 우리가 느끼지 못하는 시간마다 작업을 번갈아가며 수행하여 우리 눈에는 동시에 처리되는 것처럼 보이게 해주는 것을 의미한다. 보통 이러한 Multi-Tasking을 Context Switching이라고 한다.

**Multi-Thread**

Multi-Thread는 하나의 Process를 다수의 Thread로 구분하여 자원을 공유하고 자원의 생성과 관리의 중복성을 최소화하여 수행 능력을 향상시키는 것을 의미한다.

### Context Switching

> 💁🏻 : 앞에서 말한 Multi-Tasking을 의미한다. 사람들이 동시에 사용하는 것처럼 보이도록 빠른 속도로 Task를 번갈아가며 실행하여 실시간으로 처리하도록 보이는 과정을 의미한다.

Context Switching은 현재 진행중인 Task(Process, Thread)의 상태를 저장하고 다음 진행할 Task의 상태값을 읽어 적용하는 과정을 의미한다.

Process와 Thread에서의 Context Switching은 각각 PCB, TCB로 관리된다. PCB, TCB에는 상태, 번호, 다음 명령어 주소, Register 정보 등 다양한 정보를 가지고 있다. 그리고 이러한 정보들을 저장하고 가져와야하는 과정에서 CPU가 어떠한 일도 수행하지 못하는 순간이 오는데, 이러한 현상이 많을 수록 성능 저하로 이어지게 된다는 단점이 있다.

또한 메모리를 공유하지 않는 Process에 비해 Thread에서의 Context Switching이 비용적으로 효율적이다. Thread는 스택 영역을 제외한 모든 메모리를 공유하기 때문에 Context Switching이 일어날 때 스택 영역만 변경하면 되기 때문이다.

### Race Condition

> 💁🏻 : Race Condition은 공유하고 있는 같은 자원에 대해 접근할 때 경쟁을 벌이는 현상을 의미한다. 해결 방법은 공유하고 있는 자원에 동시에 접근하는 것을 방지하는 방법인 Mutex와 Semaphore가 있다.

**Race Condition**

Race Condition은 공유 자원에 동시에 접근했을 때 결과에 영향을 줄 수 있는 현상을 의미한다.

**Critical Section**

임계구역은 공유 자원에 접근할 때 순서와 같은 이유로 결과가 달라지는 영역을 의미한다. Mutex와 Semaphore 같이 락을 이용하여 해결하는 동기화 메커니즘을 통해 해결할 수 있다.

**Dead Lock**

교착 상태는 공유 자원에 대한 요구가 엉켜서 자원 관리를 잘못하여 Process나 Thread가 자원의 락을 획득하기 위해 무한 대기하는 현상을 의미한다. 교착 상태가 발생하는 조건들은 다음과 같다.

- 상호 배제 : 한번에 한 개의 Process/Thread만이 공유 자원을 사용할 수 있음
- 비선점 : Process/Thread에 할당된 자원은 끝날 때까지 빼앗을 수 없음
- 점유 대기 : 공유 자원에 락을 획득하여 점유한 상태임에도 다른 자원의 락을 획득하기 위해 대기히는 상황
- 순환 대기 : Process/Thread가 자원을 점유하는 구조가 순환적인 상태를 이루는 상황

교착 상태를 해결하기 위한 방법은 다음과 같다.

- 예방 : 위의 조건들 중 어느 하나라도 지켜지지 않으면 된다.
- 회피 : 교착 상태가 발생할 가능성을 배제하지 않고 발생하면 피해가는 방법으로 Banker's 알고리즘을 사용하면 된다.
- 검출 : 현재의 운영체제에서 사용하는 방법으로 교착 상태가 발생했는지 점검하여 발생한 Process/Thread와 자원을 찾는 방법이다. 타임아웃과 자원할당 그래프를 이용할 수 있다. 단, JVM에서는 교착 상태를 추적하는 기능은 가지고 있지 않기에 교착 상태가 발생하면 강제 종료하는 방법 말고는 없다.
- 회복 : 교착 상태를 발생한 Process/Thread를 종료하거나 교착 상태 지점의 할당된 자원을 선점하여 프로세스나 자원을 회복할 수 있다.

자바에서는 `synchronized, volatile`를 이용하여 동시성 문제를 해결한다. 그렇다고 무지성으로 이 키워드들을 이용해서는 안된다. 예를 들어 `synchronized`는 임계 영역의 크기나 실행시간에 따라 성능 하락 및 자원 낭비의 원인이 될 수 있고, `volatile`은 CPU 캐시를 이용해 메인 메모리에서 데이터를 읽어와 동시성 이슈를 해결할 수는 있으나 여러 쓰레드가 동시에 쓰기 작업을 수행할 경우 일관성이 깨지는 문제가 있다. 개인적으로 가장 좋은 방법은 CAS(Compare And Swap) 방식을 기반으로 `synchronized`처럼 병렬성을 해치지 않고 `volatile`처럼 가시성의 문제를 해결한 `Atomic Class`를 이용한다면 더 좋은 성능을 기대할 수 있다고 본다.

이 세가지 키워드에 대한 내용은 [내가 이전에 블로그에 정리해놓은 글](https://velog.io/@maketheworldwise/ConcurrentHashMap)을 다시 읽어보면 좋을 것 같다. 또한 이 키워드를 이용하여 [직접 테스트한 블로그](https://devwithpug.github.io/java/java-thread-safe/) 내용도 같이 살펴보면 좋을 것 같다.

참고로 교착 상태는 데이터베이스에서 가장 많이 발생하는 이슈중에 하나다. 데이터베이스에서는 트랜잭션을 활용할 때 락이 필요해지는데 해당 트랜잭션이 커밋될 때까지 락이 풀리지 않아 교착 상태가 벌어진다. 그래서 데이터베이스에서는 체크포인트와 롤백을 이용한다. 데이터베이스에서 락을 요청하면 체크포인트를 만들고 해당시점의 스냅샷을 저장을 한 뒤에 작업을 진행하다 락에 타임아웃이 걸려 중단하거나 포기해야할 때는 롤백하여 체크포인트 시점으로 복귀시켜 데이터의 유실을 막는다. 그 외로는 데이터베이스에서 교착 상태를 해결하는 방법으로 인덱스 설정, 테이블 정규화, Isolation 레벨 조정, 락 타임아웃 조정, 프로시저 우선 순위 설정 등이 있다.

**Mutex, Semaphore**

Mutex, Semaphore는 결국 데이터를 한 번에 하나의 Process/Thread만 접근할 수 있도록 제한을 두는 동기화 도구다.

Mutex는 Key를 기반으로한 도구다. Key가 있을 경우에만 공유 자원에 접근할 수 있는 방법을 이용한다. 대표적으로는 화장실로 가기 위한 키가 있느냐 없느냐에 따라 대기, 해제, 접근하는 예시가 있다.

Semaphore는 Multi-Programming 환경에서 공유된 자원에 대한 접근을 제한하는 도구다. 즉, Mutex가 동기화 대상이 오로지 1개일 때라면 Semaphore는 동기화 대상이 1개 이상일 때 이용하는 도구다. 따라서 Semaphore는 Mutex가 될 수 있지만 그 반대는 불가능하다. Semaphore는 Signaling 메커니즘을 이용하여 Signal을 보내 락을 해제할 수 있다는 특징을 가지고 있다. Mutex와 다르게 Semaphore는 화장실이 여러 개 있을 때 비어있는 화장실의 수를 확인하는 예시가 있다.

### Sync vs Async

> 💁🏻 : 호출된 함수가 호출한 함수의 결과에 영향을 미치느냐 안미치느냐의 차이에 따라 동기, 비동기로 분리할 수 있다.

해당 내용은 이미 [Blocking vs Non-blocking (sync/async)](/java/section3.md#blocking-vs-non-blocking-syncasync)에서 다룬 내용이다. 결국 동기와 비동기의 차이는 손쉽게 말하자면 순서와 관련이 있다.
