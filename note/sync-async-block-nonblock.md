# Sync-Async, Blocking-NonBlocking

이전에 내가 정리한 생각은, synchronous는 동기화로서 어떤 작업을 실행하면 그 실행 결과를 반환받을때까지 시스템이 대기하는 것이고, asynchronous는 비동기화로서 어떤 작업을 실행하면 그 실행 결과를 받환받을때까지 시스템이 대기하지 않고, 호출 즉시 응답을 반환하는 것이다.

blocking은 어떤 task가 종료될 때까지 다음 task는 실행되지 못하고 대기 중인 상태가 되어버리는 것이고, non blocking은 어떤 task가 종료되지 않더라도 다음 task가 실행될 수 있는 것이다.

일요일날 낮잠 퍼질러 자다가 커피 한잔하러 카페 나왔다가 정리해본다. 분명 난 이걸 공부했고 실무에서도 사용하고 있는데, 표현을 못함 -_-

한방에 정리한 그림이 있더라...

# Summary

|                                      | Synchronous | Blocking | Asynchronous | Nonblocking |
|:------------------------------------:|:-----------:|:--------:|:------------:|:-----------:|
| Waiting for system call's completion |      O      |     O    |              |             |
| immediate return                     |             |          |       O      |      O      |
| return with data                     |      O      |     O    |              |      O      |
| waiting in a waiting queue           |             |     O    |              |             |

정리한 그림을 보니깐, 내가 생각하고 있던거랑 비슷하네




