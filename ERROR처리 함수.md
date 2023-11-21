### perror

```c
#include <stdio.h>

void perror(const char *_string_)
```

- 오류 메세지를 STDERR로 출력하는 함수 
- _string_ 이 NULL 또는 NULL을 가리키지 않으면  **_string : error에 해당하는 메세지\n_** 와 같이 STDERR에 출력됨

> 이 함수는 확인하고 싶은 라이브러리 함수 뒤에 바로 사용해야 한다.
> 
> 해당 **라이브러리 함수에서 에러 발생시 그 에러에 맞는 errno가 세팅되고,  
> perror 함수를 호출하면 현재 errno에 맞는 error message가 출력**이 된다.
> 
> 따라서 라이브러리 함수 사용 직후에 사용하지 않으면 **다른 상황에 의해 errno가 변경될 수 있어** 원하지 않는 결과가 나올 수 있다!

```c
- 라이브러리 함수 사용 -
perror("ERROR MESSAGE : ");
```

- 위와 같이 코드를 작성했을 때 라이브러리 함수에서 에러가 발생했다면 다음과 같은 출력이 진행됨

```c
ERROR MESSAGE : errno에 해당하는 message
```

### strerror

```c
#include <string.h>

char *strerror(int errnum)
```

- **오류 메세지 string에 errnum의 오류 번호를 mapping**함
- 이 함수는 인자로 넘겨준 errno에 대한 오류 메세지를 반환

```c
printf("%s\n", strerror(errno));
```

위와 같이 사용하면 errno라는 수에 대응하는 error message가 strerror에 의해 return되고 그 메세지를 출력할 수 있게 됨