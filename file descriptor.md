## File Descriptor란?
- 리눅스 시스템에서는 모든 것이 파일. 모든 행동과 객체는 파일로 관리됨
	- 정규파일, 디렉토리, 소켓, 파이프, IO디바이스 등등 모든 것을 포함
- 프로세스에서 이 파일에 접근하는 가장 기본적인 방법은 [[시스템 콜]]을 활용하는 것
- 시스템 콜로 파일을 열었을때, 이 파일의 정보를 담은 주소값을 가리키는 번호가 파일 디스크립터!
## 번호의 의미
- 0 : 표준 입력
- 1 : 표준 출력
- 2 : 표준 에러
- 3번 부터는 접근한 파일을 가리킴
- 파일 디스크립터는 파일을 다루기 위해서 해당 파일의 주소를 참조하여 접근하는 형태
## 파일 디스크립터 확인
~~~
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>

int main(void)
{
	int fd;

	fd = open("test.txt", O_RDONLY);
	if (fd < 1)
	{
		printf("open() error");
		exit(1);
	}
	printf("FD : %d\n", fd);
	close(fd);
	return (0);
}
// 실행 결과
// FD : 3
~~~
## 어떻게 정수값으로 원하는 파일에 접근하는가?
![[Pasted image 20231017201531.png]]
- fd는 프로세스가 유지하고 있는 FD Table의 인덱스
- 위의 그림에서 보듯이, fd를 이용해서 해당 칸이 가리키는 주소로 가서 원하는 정보를 얻을 수 있음
- FD Table의 각 칸들은 File Table의 주소값(pointer)를 가지고 있음
- File Table의 각 칸들은 mode와 inode Table Pointer의 offset을 가지고 있음
- [[inode]] Table은 소유자 그룹, 접근 모드, 파일 형태 등 해당 파일에 관한 정보를 가지고 있음
## 기존의 파일 디스크립터를 복제하면?
~~~
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <unistd.h>

int main(void)
{
	int fd;
	int fd2;

	fd = open("test.txt", O_RDONLY);
	fd2 = open("test.txt", O_RDONLY);
	if (fd < 1 || fd2 < 1)
	{
		printf("open() error");
		exit(1);
	}
	printf("fd\t: %d\n", fd);
	printf("fd2\t: %d\n", fd2);

	printf("fd2 = dup(fd)\n");
	fd2 = dup(fd);

	printf("fd\t: %d\n", fd);
	printf("fd2\t: %d\n", fd2);

	close(fd);
	close(fd2);
	return (0);
}
// 실행 결과
fd : 3
fd2 : 4
fd2 = dup(fd)
fd : 3
fd2 : 5
~~~
- 기존 fd는 3을, fd2는 4를 할당받음
- 이후 dup()함수로 fd를 복사해서 fd2에 넣었더니 fd2가 5가 됨
	- 즉, 새로 할당된 것
- 복제를 하면 새로운 파일 디스크립터를 생성한다는 것을 확인
