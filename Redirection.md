## Redirection?
- 리눅스에서 프로그램은 보통 세 개의 파일 서술자를 열게 됨
	- 표준입력(STDIN)
	- 표준출력(STDOUT)
	- 표준에러(STDERR)
## cmd > file_name
- cmd가 출력하는 내용을 file_name에 덮어쓰기 저장
- file_name이 존재하지 않으면 새로 만듦
- ![[Pasted image 20231121115321.png]]
- 아래의 경우에는 stdout이 아니라 stderror로 출력된 내용이기 때문에 실행되지 않음
- ![[Pasted image 20231121115357.png]]
- 위 명령어의 경우 `ls -l 10 > tt.txt` 는 `ls -l 10 1> tt.txt`와 똑같이 동작함. 
- 즉, fd 1의 값인 stdout을 저장한다는 뜻. `ls -l 10 2> tt.txt`로 에러문구를 저장할 수 있음

![](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9d55ae2c-77ab-4cfa-b374-23f975c8afa3%2FUntitled.png&blockId=b4fb1165-04c4-4a71-9412-91c9857198fd)

## cmd >> file_name
- cmd에서 출력하는 것을 file_name의 마지막에 추가하여 저장
- ![[Pasted image 20231121124725.png]]

## cmd < file_name
- file_name의 파일 내용을 표준입력으로 사용한다는 의미
- ![[Pasted image 20231121124830.png]]
- ls와 같이 표준입력을 받지 않는 명령어는 argv(argument vector)를 통해 사용자 입력을 받음
- ![[Pasted image 20231121124938.png]]
- ls의 표준입력으로 tt.txt 를 넣었지만 이를 무시하고 현재 폴더의 결과를 출력
- 표준입력은 무시되고, argument vector를 통해 입력을 받았기 때문