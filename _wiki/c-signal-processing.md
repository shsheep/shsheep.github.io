---
layout  : wiki
title   : C 시그널 처리 방법 정리
summary : 시스템 콜
date    : 2023-07-20 09:34:36 +0900
updated : 2023-07-24 18:49:55 +0900
tag     : 
toc     : true
public  : true
parent  : 
latex   : false
resource: 3516F4DC-3435-40C4-9423-E7616260B2A5
---
* TOC
{:toc}

# C 시그널 처리 방법 정리
C에서 시그널(Signal)을 처리하는 방법을 정리하자.

## 정의
위키피디아에서는 시그널을 인터럽트와 비교하여 다음과 같이 설명하고 있다.
> Signals are similar to interrupts, the difference being that interrupts are mediated by the CPU and handled by the kernel while signals are mediated by the kernel (possibly via system calls) and handled by individual processes.[citation needed] The kernel may pass an interrupt as a signal to the process that caused it (typical examples are SIGSEGV, SIGBUS, SIGILL and SIGFPE).[^SIG_WIKI]
 
 
시그널이란 인터럽트와 유사하나, 인터럽트는 CPU에 의해 발생해서 커널에 의해 처리되지만, 시그널은 커널에 의해 발생해서 프로세스에서 처리된다고 한다.`[citation needed]`라고 되어있는걸 보니 저 설명도 확신은 없어보인다. 인터럽트에 대해서 명확히 알면 시그널과 구분도 명확하게 할 수 있지 않을까싶다.

## 흐름
### 배경지식
- 시그널 '마스킹'이란?

### 시그널 처리 관련 함수
- `int sigempyset(sigset* set)` (Standard C library libc) \<signal.h\>
    - `sigset* set`을 비움
- `int sigfillset(sigset_t* set)` (Standard C library libc) \<signal.h\>
    - 모든 시그널을 `sigset_t* set`에 추가
- `int pthread_sigmask(int how, const sigset_t* restrict set, sigset_t* restrict oset)` \<signal.h\>
    - 호출하는 쓰레드의 시그널 마스크를 변경한다.
    - `const sigset_t* restrict set` 값이 널이 아니면 이 값에 해당하는 시그널들을 `int how` 값에 해당하는 방식으로 취합해서 마스킹한다.
    - `int how`
        - SIG_BLOCK : 현재 이미 마스킹되어 있는 시그널과 `const sigset_t* restrict set`의 시그널들을 블록한다.
        - SIG_UNBLOCK : 현재 이미 마스킹되어 있는 시그널 중 `const sigset_t* restrict set`의 시그널들을 제외하고 마스킹한다.
        - SIG_SETMASK : `const sigset_t* restrict set`만을 마스킹한다.
    - `sigset_t* restrict oset` : 널이 아니면 이전 마스킹 시그널 셋이 해당 값에 저장된다.
    - `int sigprocmask(int how, const sigset_t* restrict set, sigset_t* restrict oset)`와 동일해보인다.
- `int sigaction(int sig, const struct sigaction* restrict act, struct sigaction* restrict oact)`
    - `sig` 시그널 발생시 실행할 핸들러(`act->sa_handler`)를 등록. `oact`가 널이 아닐시, 해당 시그널에 대한 기존 핸들러 정보를 포함하는 `struct sigaction*`이 값에 저장된다.
    - 해당 핸들러내에서는 `act->sa_mask`에 해당하는 시그널들이 블록된다.
    -  ```cpp
       struct sigaction {
              void (*sa_handler)(int);
              void (*sa_sigaction)(int, siginfo_t*, void*);
              sigset_t sa_mask;                  /* signal mask to apply */
              int     sa_flags;                  /* see signal options below */
       };
       ```
    - `fork`, `vfork`로 파생된 프로세스들은 모두 부모 프로세스의 시그널 마스크, 시그널 스택, 시그널별 플래그를 동일하게 상속받는다.
    - `execve` reinstates the default action for all signals which were caught and resets all signals to be caught on the user stack.  Ignored signals remain ignored; the signal mask remains the same; signals that restart pending system calls continue to do so. 
    - 시그널별 기본 처리 동작
        - | NAME      | Default Action    | Description                                     |
          | --------- | ----------------- | ----------------------------------------------- |
          | SIGHUP    | terminate process | terminal line hangup                            |
          | SIGINT    | terminate process | interrupt program                               |
          | SIGQUIT   | create core image | quit program                                    |
          | SIGILL    | create core image | illegal instruction                             |
          | SIGTRAP   | create core image | trace trap                                      |
          | SIGABRT   | create core image | abort(3) call (formerly SIGIOT)                 |
          | SIGEMT    | create core image | emulate instruction executed                    |
          | SIGFPE    | create core image | floating-point exception                        |
          | SIGKILL   | terminate process | kill program                                    |
          | SIGBUS    | create core image | bus error                                       |
          | SIGSEGV   | create core image | segmentation violation                          |
          | SIGSYS    | create core image | non-existent system call invoked                |
          | SIGPIPE   | terminate process | write on a pipe with no reader                  |
          | SIGALRM   | terminate process | real-time timer expired                         |
          | SIGTERM   | terminate process | software termination signal                     |
          | SIGURG    | discard signal    | urgent condition present on socket              |
          | SIGSTOP   | stop process      | stop (cannot be caught or ignored)              |
          | SIGTSTP   | stop process      | stop signal generated from keyboard             |
          | SIGCONT   | discard signal    | continue after stop                             |
          | SIGCHLD   | discard signal    | child status has changed                        |
          | SIGTTIN   | stop process      | background read attempted from control terminal |
          | SIGTTOU   | stop process      | background write attempted to control terminal  |
          | SIGIO     | discard signal    | I/O is possible on a descriptor (see fcntl(2))  |
          | SIGXCPU   | terminate process | cpu time limit exceeded (see setrlimit(2))      |
          | SIGXFSZ   | terminate process | file size limit exceeded (see setrlimit(2))     |
          | SIGVTALRM | terminate process | virtual time alarm (see setitimer(2))           |
          | SIGPROF   | terminate process | profiling timer alarm (see setitimer(2))        |
          | SIGWINCH  | discard signal    | Window size change                              |
          | SIGINFO   | discard signal    | status request from keyboard                    |
          | SIGUSR1   | terminate process | User defined signal 1                           |
          | SIGUSR2   | terminate process | User defined signal 2                           |

## TODO
1. `sigprocmask`, `pthread_sigmask`는 호출하는 쓰레드의 signal blocking에만 영향을 주는가?
2. 메인 쓰레드에 모든 시그널이 블럭되었다면, 프로세스에 전달되는 시그널은 어디로 가는가? 쓰레드로 전달된다면 어떤 쓰레드로 가게 되는가? main tid와 가장 가까운 tid의 쓰레드로 가는가?

    
## 주석

[^SIG_WIKI]: [시그널 위키피디아](https://en.wikipedia.org/wiki/Signal_(IPC)) 
