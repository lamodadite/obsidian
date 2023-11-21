## Pipe?
- Uni-directional byte stream
- name or ID가 없음
- related process 간에 사용 가능 (e.g. fork())
- [[IPC]] 구현 중 하나
![[Pasted image 20231121141402.png]]

## Pipe를 이용한 IPC
- ![[Pasted image 20231121142148.png]]
- 프로세스가 pipe를 생성하면, read를 하는 쪽과 write를 하는쪽이 나뉘어져 있음
- fork()를 이용하여 자식 프로세스를 생성하면 부모 프로세스의 파일 디스크립터를 그대로 복제하여 자식에게 공유
-  하나의 파이프가 두 개의 프로세스에 의해 공유됨
-  ![[Pasted image 20231121142615.png]]
- 부모 프로세스가 파이프에 write한 정보를 자식 프로세스가 read하기 위해서는 각자의 파일 디스크립터를 닫아야 함
- 이렇게 사용되지 않는 파일 디스크립터들을 닫지 않으면, 각각의 파일 디스크립터가 모두 열려있기 때문에 원하는 동작을 하지 않을 수 있음

## Pipe API

> int pipe(int pipefd[2]);
> 	- 파이프를 생성함
> 파라미터
> 	- pipefd[2] : 생성될 pipe fd를 저장할 버퍼
> 	- pipefd[0] : reader-side fd
> 	- pipefd[1] : writer-side fd
> 반환값
> 	- 성공시 0 리턴
> 	- 실패시 -1 리턴

## Pipe I/O
- pipe가 full일 때 write 시도하면 blocking
- pipe가 empty일 때 read 시도하면 blocking
- write size가 PIPE_BUF보다 작으면 atomic, 크면 interleaved 될 수 있음
	- interleaved : 데이터가 분리돼서 넣어질 수도 있음