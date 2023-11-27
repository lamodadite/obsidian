## IPC?
- Inter-Process Communication 의 약자로, [[프로세스]]간의 정보를 주고받는 것을 뜻함
- 프로세스는 자신에게 할당된 메모리 내의 정보에만 접근할 수 있음
	- 안정성을 위해 OS에서 자기 프로세스의 메모리만 접근하도록 강제하기 때문
- IPC의 모델은 크게 두 가지가 있음
	- Message Passing
	- Shared Memory
	![[Pasted image 20231121132353.png]]
## 종류
- 메일슬롯
- [[Pipe]]
- Socket
- Signal
- 공유 메모리