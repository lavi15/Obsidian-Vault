
### 쓰레드와 코루틴의 공통점
- 동시성 프로그래밍을 위한 기술
- 컨텍스트 스위칭이 중요한 개념
   **※ 컨텍스트 스위칭 : 하나의 프로세스가 CPU를 사용 중인 상태에서 다른 프로세스가 CPU를 사용하도록 하기 위해, 이전의 프로세스의 상태(문맥)를 보관하고 새로운 프로세스의 상태를 적재하는 작업**

### 쓰레드와 코루틴의 차이점
- 코루틴은 Thread를 대체하는 기술이 아님
- 하나의 Thread를 더욱 잘개 쪼개서 사용하는 기술
- 코루틴은 쓰레드보다 CPU 자원을 절약하기 때문에 **Light-Weight Thread**라고 함
- 구글에서는 코틀린의 코루틴 사용을 **적극 권장**

## 쓰레드
- 작업 하나하나의 단위 : **Thread**
    - 각 Thread 가 독립적인 **Stack 메모리 영역**을 가짐
- 동시성 보장 수단 : **Context Switching**
    - 운영체제 커널에 의한 **Context Switching** 을 통해 동시성을 보장해요
    - **블로킹** (Blocking)
        - Thread A가 Thread B 의 **결과를 기다림**
        - 이 때, Thread A는 블로킹 상태
        - A는 Thread B 의 **결과가 나올 때 까지** 해당 자원 사용 불가
![[Pasted image 20231020100715.png]]
- Thread A가 Task 1을 수행하는 동안 Task 2 의 결과가 필요하면 Thread B를 호출
- 이때 Thread A는 블로킹 되고 Thread B로 프로세스간에 스위칭이 일어나 Task 2을 수행
- Task 2가 완료되면 Thead A로 다시 스위칭해서 결과 값을 Task 1에게 반환
- 이때 Task 3, Task 4는 A, B작업이 진행되는 도중에 멈추지 않고 각각 동시에 실행
- 이때 컴퓨터 운영체제 입장에서는 각 Task를 쪼개서 얼마나 수행할지가 중요
- 스케쥴링 : 어떤 쓰레드를 먼저 실행해야할지 결정하는 행위
- 이러한 행위를 통해 동시성을 보장

## 코루틴
- 작업 하나하나의 단위 : **Coroutine Object**
    - 여러 작업 각각에 Object 를 할당
    - Coroutine Object 도 엄연한 객체이기 때문에 **JVM Heap 에 적재** (코틀린 기준)
- 동시성 보장 수단 : **Programmer Switching (No-Context Switching)**
    - 소스 코드를 통해 **Switching** 시점을 **마음대로** 결정 (OS는 관여하지 않아요)
    - **Suspend** (Non-Blocking)
        - Object 1이 Object 2의 **결과를 기다릴 때** Object 1의 상태는 **Suspend로 바뀌어요**
        - 그래도 Object 1을 수행하던 **Thread는 그대로 유효**
        - 그래서 Object 2도 Object 1과 **동일한 Thread**에서 실행
![[Pasted image 20231020101954.png]]
- Coroutine은 작업 단위가 Object
- Task 1을 수행하다가 Task 2의 수행요청이 발생했을때 컨텍스트 스위칭 없이 동일한 Thread A에서 수행 가능
- Thread C처럼 하나의 쓰레드에서 여러 Task Object들을 동시에 수행 가능
- 이러한 특징때문에 코루틴을 Light-Weight Thread라고 함
    - 동시처리를 위해 스택영역을 별도로 할당하는 쓰레드처럼 동작하지 않음
    - 하지만 동시성을 보장 가능
    - 하나의 쓰레드에서 다수의 코루틴을 수행 가능
    - **커널의 스케쥴링을 따르는 컨텍스트 스위칭을 수행하지 않음**