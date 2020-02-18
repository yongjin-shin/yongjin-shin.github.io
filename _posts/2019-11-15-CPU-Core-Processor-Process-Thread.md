---
title: "CPU, Core, Processor, Process, Thread"
usemathjax: true
toc: true
toc_sticky: true

categories:
- Programming
tags:
- Thread
---





처음의 궁금점은 MARL 코드가 Multi-Process와 Multi-Thread로 구현이 많이 이루어져서, (그래서 ray를 봤건만 결국 활용은 못했다) 나중에 코드 짤 때에는 활용해보고자 찾아보기 시작했는데, 끝이 없이 공부해야할 것 같다(~~학부때 마이크로프로세서를 날로 먹어서 지금에서야 후회 중~~).  나중에 분명히 까먹을게 뻔하므로 깨작깨작 적어본다.



## CPU(Central Processing Unit)

<img src="/assets/images/2019-10-18-Static-Games-Complete-Info%20(copy)/Screenshot%20from%202019-11-15%2014-29-32.png" alt="Screenshot from 2019-11-15 14-29-32" style="zoom:80%;" />

* 책에서 많이 봤던 그림. CPU는 1)ALU, 2)Control Unit, 3)Register으로 이루어진다. (=Processor)
*  이런 구조가 중복적으로 많아지고 하나의 칩셋에 들어가면 CPU = Core. 물론, CPU가 여러 개의 Core를 포함하는 개념이 되겠지만, 구조적으로는 동일.
  * Register에서 당장 필요하고 중요한 데이터가 저장되어 있고,
  * 메인 메모리, 하드디스크 순으로 저장된다. 멀리갈수록 데이터 retrieving이 느릴 수 밖에 없음
  * 캐시는 데이터의 전달을 빨리 해주기 위한 장치 (이것만으로도 한챕터 배웠던듯)
  * 메인 메모리는 우리가 잘 아는 RAM이 담당
  * 하드 디스크는 SSD같은 것들이 있음. 하드디스크에서 데이터를 가져오려면 우선 메인 메모리에 저장을 하고, 다시 메인 메모리에 데이터를 가져와야 한다.

[그림 출처](https://m.blog.naver.com/PostView.nhn?blogId=ya3344&logNo=221245662649&proxyReferer=https%3A%2F%2Fwww.google.com%2F)



## Process

* *"While a computer program is a passive collection of instructions, a process is teh actual execution of those instructions. Several processes may be associated with the same program; for example, opening up several instances of the same program often results in more than one process.*

  *Multitasking is a method to allow multiple processes to share processors and other system resources. Each core executes a single task at a time. However, multitasking allows each processor to switch between tasks that are being executed without having to wait for each task to finish. [wikipedia 'process']"*

![Screenshot from 2019-11-15 13-49-38](/assets/images/2019-10-18-Static-Games-Complete-Info%20(copy)/Screenshot%20from%202019-11-15%2013-49-38.png)



* OS는 이런 저런 프로그램을 조율하는 역할 (아마도?) [블로그](https://wkdtjsgur100.github.io/os-summary-2/) 참고
* 프로세스를 만들기 위해서는 '프로그램'을 실행시켜야 한다.
  * OS가 프로그램에 관련된 정보(코드, 데이터)를 HDD에서 빼와서 RAM에 로드시키면,
  * CPU는 RAM의 코드 영역에서 코드를 실행하면서 연산을 실행한다.
  * 이게 하나의 프로세스이다.



### Main Memory

* 이것도 간만에 보는 그림

<img src="/assets/images/2019-10-18-Static-Games-Complete-Info%20(copy)/Screenshot%20from%202019-11-15%2014-43-31-1573796634948.png" alt="Screenshot from 2019-11-15 14-43-31" style="zoom:67%;" />

[그림출처](https://jinshine.github.io/2018/05/17/%EC%BB%B4%ED%93%A8%ED%84%B0%20%EA%B8%B0%EC%B4%88/%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B5%AC%EC%A1%B0/?source=post_page-----23b2aa5cfb25----------------------)



### Process State

* Process에서 Thread로 넘어가기 위해서는 다음 그림을 이해하자.
  OS kernel에서는 다음과 같은 state를 명시적으로 알아차리지는 못 한다. 하지만 finite state machine을 공부할 때 처럼 개념적으로 이해하면 될 것 같다.

  * **Created/New**: 일단, 프로세스가 생성이 되는 단계이다. 원칙적으로라면 Creating이 되자마자 다음 상태로 넘어가는게 맞지만, 그러면 프로세서가 oversaturation하게 되므로, 일단은 스케쥴러가 지시를 내릴동안은 기다리고 있는다. 말하자면 달리기 선수들이 "Ready" 상태로 대기하고 있는 것처럼. 

  * **Waiting**: 프로세스 스케쥴러는 Waiting 단계로 프로세스를 할당하면 (위에서 언급했던 것처럼)메인 메모리에 프로세스를 로드하고 스케쥴러가 context-switching을 하기까지 기다린다. 위에서 처럼 비유하자면 "Get Set!" 상태로 스타팅 포지션에서 자세를 잡고 있는 상태이다.

    하나의 프로세서에서는 여러 개의 프로세스가 waiting 상태로 존재한다. 왜냐하면, (하나의 프로세서만 있다고 생각하자) 하나의 프로세서는 하나의 프로세스만 실행할 수 있기 때문이다. 즉, 달리기 선수들은 여러 명이지만 트랙은 하나인 경우이다. 뛰고 있는 한 선수를 제외하고 나머지 선수들은 줄 서서서 기다리고 있는 것이다. 이 줄은 computer scheduling에서 이루어진다. 이에 대한 것은 [블로그](https://preamtree.tistory.com/19) 참고.

  * **Running**: context-switching이 일어나면, 프로세스는 프로세서(cpu/core)에 할당을 받게 된다 (만약 여러 개의 core(=processor)를 가졌다면, 이것 중 하나에 할당이 된다. Running상태에서는 프로세서가 프로세스 명령어들을 처리한다.

  * **Blocked**: 만약, 프로세스가 외부의 변화가 없이는 작업을 수행할 수 없는 상태라면(유저의 입력이나, 파일을 열어야 한다, 프린트를 해야하는데 프린트가 잡히지 않는다 등), 해당 프로세스는 Blocked 상태로 바뀐다. 더 이상 리소스를 기다리지 않아도 되는 상태라면 다시 waiting 상태로 바꾼다.

  * **Terminated**: 프로세스의 작업이 모두 끝나거나 혹은 OS에 의해서 프로세스가 종료(killed)되면, 해당 프로세스는 더 이상 필요하지 않는다. 따라서 terminatd 상태로 변하고, 프로세스는 삭제된다. 삭제는 곧바로 진행되지는 않고, process table에 여전히 남아 zombie process로 유지된다. Parent process가 wait 시스템콜 로 exit status를 읽으면, 그제서야 process table에서 삭제되고 메인 메모리에서 삭제가 된다. 만약 부모 프로세스가 wait 콜에 실패하면 PID에 계속 남아서 결국 리소스 리킹(resource leaking)을 발생시키는 원인이 된다.

  

  * 가상 메모리를 지원하는 시스템에서는, 추가적으로 두 가지 상태가 있다. 이 상태에서는 프로세스가 HDD와 같은 2차 메모리에 저장이 된다.
    * **Swapped out and waiting**: (suspend and waiting) 스케쥴러에 의해서 프로세스가 메인 메모리에서 삭제가 되고 외부 저장소에 이동한다. 프로세스는 외부 저장소에서 다시 메인 메모리로 불러와져서 waiting상태로 돌아올 수 있다.
    * **Swapped out and blocked**: (suspend and blocked) 위와 동일하다. 다만 blocked된 상태에서 외부 저장소에 저장되었다가 다시 메인 메모리로 불러와질 수 있다.



<img src="/assets/images/2019-10-18-Static-Games-Complete-Info%20(copy)/Screenshot%20from%202019-11-15%2014-48-51.png" alt="Screenshot from 2019-11-15 14-48-51" style="zoom:40%;" />

[그림출처](https://en.wikipedia.org/wiki/Process_(computing))



## Thread

* *"A process is the instance of a computer program that is being executed by one or many thread. It contains the program code and its activity. Depending on the operating system, a process may be mad up of multiple threads of execution that execute instruction concurrently. [wikipedia 'process']"*
* *"A thread of execution is the smallest sequence of programmed instructions that can be managed independently by a scheduler. In most cases a thread is a component of a process. Multiple threads can exist within one process, executing concurrently and sharing resources such as memory, while different processes do not share these resources. In particular, the threads of a process share its executabe code and the values of its dynamically allocated variables and non-thread-local global variables at any given time. [wikipedia 'thread']*"



<img src="/assets/images/2019-10-18-Static-Games-Complete-Info%20(copy)/Screenshot%20from%202019-11-16%2010-23-57.png" alt="Screenshot from 2019-11-16 10-23-57" style="zoom:80%;" />



* 단일 프로세서에서는 time slicing에 의해 multithreading이 이루어진다. context switching이 매우 빠르게 이뤄지므로 사람이 인지하기에는 parallel하게 여러 작업이 처리되는 것으로 보여진다.

  * 만약 멀티 프로세서가 존재한다면, 이러한 과정이 프로세서마다 진행되므로 여러 프로세서가 동시에 여러가지 작업을 parallel하게 수행하는 것으로 보일 수 있겠다.

  * 멀티 프로세서에 hardward thread가 존재한다면, 멀티 프로세서가 또 다시 thread를 만들어내어 동시에 여러작업을 하는 것처럼 보일테고, 이 thread에서 OS의 thread가 또 parallel하게 작업하므로 사용자 입장에서는 엄청나게 많은 작업들이 동시에 이뤄지는 것처럼 느껴질 것이다.

    **CPU의 thread는 연산 자체를 병렬적으로 처리해주는 반면, OS의 thread는 프로세스를 병렬적으로 처리해주는것. (참고: [코멘트1](https://kldp.org/comment/626279#comment-626279) [코멘트2](https://quasarzone.co.kr/bbs/board.php?bo_table=qf_cmr&wr_id=12864) [블로그](https://badcandy.github.io/2019/01/14/concurrency-01/))*

  

### Thread vs Process

* Process는 독립적이지만, thread는 프로세스에 종속한다.
*  하나의 Process에 종속된 여러 Thread는 해당 프로세스의 state 뿐만 아니라 메모리와 다른 자원들을 공유한다.
* 따라서, process는 각기 다른 주소 공간을 가지고 있지만, thread는 이것을 공유한다.
* Process는 오직 IPC(inter process communication)에 의해 상호 작용이 이루어진다.
* Process간의 context-switching보다 Thread 사이의 context-switching 시간이 훨씬 빠르다.



### Multithreading Drawbacks

* Synchronization
  * Thread는 address space를 공유하므로, 프로그래머는 race condition(한 사람은 저쪽으로 가고, 다른 사람은 이쪽으로 오면 둘은 언제 만나나, 평생 왔다갔다하다가 시간 다 보낼 것) 혹은 다른 직관적이지 않은 행동에 대해서 주의해야 한다. 따라서 랑데부(rendezvous) 구조 등을 통해 이를 해소시켜야 한다. 뿐만 아니라 상호 배타적인 행동을 통해서 공통의 데이터가 한번에 접근되는 것을 막아야 한다. 
  * 그렇지 않다면 deadlock(서로의 자원을 기다리다가 무한히 기다리게 되는 현상) 등의 문제가 발생할 것이다.

<img src="/assets/images/2019-10-18-Static-Games-Complete-Info%20(copy)/Screenshot%20from%202019-11-16%2010-24-43.png" alt="Screenshot from 2019-11-16 10-24-43" style="zoom:67%;" />





* Thread crahses a process
  * 만약 잘못되면, 하나의 thread가 전체 thread를 포괄하는 process를 죽여버리는 결과를 가지고 올 수 있다.



좀 더 자바에 특화된 질문과 답변은 [여기](https://junyongs.wordpress.com/2014/08/05/process-vs-thread-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-vs-%EC%93%B0%EB%A0%88%EB%93%9C/)

[그림출처](https://preamtree.tistory.com/10)



