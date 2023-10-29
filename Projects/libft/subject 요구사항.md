### Technical considerations
- 전역변수 x
- 복잡한 함수를 작성하기 위한 헬퍼 함수가 필요한 경우 *정적 함수*로 정의해야함. (해당 파일로 범위를 제한하기 위해)
- 리파지토리의 root에 모든 파일들을 위치시켜야함
- 쓰지 않는 파일은 있어선 안됨
- -Wall -Werror -Wextra flag를 달아서 컴파일해야함
- libtool 대신 ar로 라이브러리를 생성해야 함
- libft.a는 리파지토리의 root에 생성되어야함
### Part.1 - Libc functions
- libc에 있는 함수들을 만듦
- original과 같은 동작과 프로토타입을 가져야 함
- man 과 똑같이 작동해야함
- 접두사로 ft_가 붙어야함
> 일부 함수의 프로토타입은 '제한' 한정자를 사용하여 다시 실행해야 합니다. 이 키워드는 c99 표준의 일부입니다. 따라서 자체 프로토타입에 이 키워드를 포함하거나 -std=c99 플래그를 사용하여 코드를 컴파일하는 것은 금지되어 있습니다.

테스트기
- https://github.com/xicodomingues/francinette
- 
