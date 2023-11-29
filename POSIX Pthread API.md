## Pthreads 데이터형
| 데이터형            | 설명                    |
| ------------------- | ----------------------- |
| pthread_t           | 스레드 ID               |
| pthread_mutex_t     | 뮤텍스                  |
| pthread_mutexattr_t | 뮤텍스 속성 객체        |
| pthread_cond_t      | 조건 변수               |
| pthread_condattr_t  | 조건 변수 속성 객체     |
| pthread_key_t       | 스레드 고유 데이터의 키 |
| pthread_once_t      | 1회 초기화 제어 문맥    |
| pthread_attr_t      | 스레드 속성 객체        | 
- 이식성 있는 프로그램은 이를 불투명한 데이터로 다뤄야 함
- 이 데이터형 변수를 == 연산자로 비교하면 안됨!!

## 스레드와 errno
- 전통적인 유닉스 API에서 errno는 전역 정적 변수
- 이는 멀티스레드 프로그램에서 주의해야 할 요소
	- 어떤 스레드가 전역 errno 변수에 에러를 설정하면, 함수 호출을 하고 errno를 확인하는 다른 스레드에 문제를 일으킬 수 있음
	- 이를 **경쟁상태**라고 함

## Pthreads 함수의 리턴값
- 유닉스의 전통적인 방식은 성공하면 0 리턴, 에러가 발생하면 -1을 리턴하고  errno를 설정
- 하지만 Pthread API에서는 성공하면 0을 리턴하고 실패하면 양수를 리턴
	- 이 양수값은 errno에 설정되는 값과 같음

## 스레드 생성
### pthread_create()
~~~c
#include <pthread.h>

int pthread_create(pthread_t *thread, const pthread_attr_t *attr,
										void *(*start)(void *), void *arg);
~~~
### 리턴값
- 성공하면 0 리턴
- 에러 발생시 양수를 리턴. 양수는 errno 값.
### 매개변수
- 새 스레드는 start로 지정된 함수를 인자 arg로 호출해 시작
- pthread_create()를 호출한 스레드는 계속해서 호출 다음의 실행문을 실행
- arg 인자는 void *로 선언되어 있음
	- 어떤 종류의 객체를 가리키는 포인터든 start 함수에 넘길 수 있다는 뜻
	- NULL로 지정할 수도 있음
	- start 함수에 여러 인자를 넘겨야 하면 해당 인자들을 필드로 갖는 구조체의 포인터를 넘기면 됨
- thread 인자는 이 스레드의 고유한 ID가 복사되는 버퍼를 가리킴
- attr 인자는 새로운 스레드의 다양한 속성을 지정하는 pthread_attr_t 객체의 포인터
	- NULL로 지정하면 스레드는 기본 속성으로 만들어짐
	- 여러 속성을 지정할 수 있지만, 당장은 필요 없기 때문에 생략

## 스레드 종료
### 스레드가 종료되는 경우
1. 스레드의 시작 함수가 스레드의 리턴값을 지정하며 return 수행
2. 스레드가 pthread_exit()을 호출
3. 스레드가 pthread_cancel()을 통해 취소됨
4. 스레드 중 아무나 exit()을 수행하거나, 주 스레드가 main()함수에서 return을 수행해서 프로세스 내의 모든 스레드가 즉시 종료됨
### pthread_exit()
~~~c
#include <pthread.h>

void pthread_exit(void *retval);
~~~
- 호출 스레드를 종료시키고, 다른 스레드가 pthread_join()을 통해 얻을 수 있는 리턴값을 지정
- 스레드의 시작 함수에서 return을 수행하는것과 비슷함
	- 차이점은 pthread_exit()는 스레드의 시작 함수에서 호출된 어느 함수에서든 호출될 수 있다는 점
- retval 인자는 스레드의 리턴값을 지정
- 스레드가 종료되면 스택의 내용이 유효하지 않기 때문에 retval로 리턴값을 빼줘야함

## 스레드 ID
### pthread_self()
~~~c
#include <pthread.h>

pthread_t pthread_self(void);
~~~
- 다양한 Pthreads 함수가 스레드 ID를 사용해 자신이 작용할 스레드를 식별
	- 다양한 Pthreads 함수의 예 : pthread_join(), pthread_detach(),  pthread_cancel(), pthread_kill()등등
- 일부 응용 프로그램에서는 특정 스레드의 ID를 가지고 동적 데이터 구조에 꼬리표를 붙여두면 편리할 때가 있음
### pthread_equal()
~~~c
#include <pthread.h>

int pthread_equal(pthread_t t1, pthread_t t2);
~~~
- t1 과 t2가 같으면 0이 아닌 값을 리턴하고, 그렇지 않으면 0을 리턴
- pthread_t 데이터형을 불투명한 데이터로 다뤄야 하기 때문에 필요함
- 리눅스에서 pthread_t는 우연히 unsigned long으로 정의되어 있지만, 다른 구현에서는 포인터나 구조체일 수 있음
- 리눅스 스레드 구현에서 스레드 ID는 모든 프로세스에서 고유하지만, 다른 구현에서도 그런 것은 아님
	- 이식성을 위해서는 모든 프로세스에서 스레드 ID가 고유하다고 생각하고 구현하면 안됨

## 종료된 스레드와 조인
### pthread_join()
~~~c
#include <pthread.h>

int pthread_join(pthread_t thread, void **retval);
~~~
- 성공하면 0을 리턴, 에러가 발생하면 에러 번호(양수)를 리턴
- thread로 식별된 스레드가 종료되기를 기다림
	- 해당 스레드가 이미 종료됐으면, pthread_join()은 즉시 리턴
	- 이 동작을 **조인** 이라고 함
- retval이 NULL이 아닌 포인터이면 거기에 종료된 스레드의 리턴값이 복사됨
- 이미 조인된 스레드에 대해 다시 조인을 해선 안됨
	- 나중에 생성되어 우연히 같은 스레드 Id를 재사용하는 스레드와 조인할 수도 있음
- 스레드가 분리되지 않았으면, pthread_join()을 이용해 꼭 조인해야함
	- 그렇지 않으면, 스레드가 종료했을 때 좀비 프로세스와 비슷한 스레드가 생겨 문제가 됨
### pthread_join()과 wait()의 차이
- 스레드는 서로 동등함
	- 프로세스의 어느 스레드든 pthread_join()을 사용해 다른 스레드와 조인할 수 있음
	- 이는 프로세스의 계층 구조와 아예 다름
	- 부모 프로세스가 자식을 만들면 wait을 통해 자식을 기다릴 수 있는 것은 부모뿐
- 모든 스레드와 조인할 방법이 없음
	- 프로세스는 waitpid(-1, &status, options) 호출로 가능
	- 비블로킹 조인 방법 도한 없음
> 하나의 스레드와만 조인할 수 있다는 것은 의도적인 제약이다. 프로그램이 '알고 있는' 스레드와만 조인하라는 뜻이다. '모든 스레드와 조인' 해버리면 라이브러리가 내부적으로 만들어서 사용하는 스레드와도 조인하게 된다. 그렇게 되면 라이브러리는 상태를 얻기 위해 해당 내부 스레드와 조인할 수 없게 되고, 이미 조인된 스레드 ID와 조인하려고 하다가 에러를 발생시킨다.

## 스레드 분리
- 기본적으로 스레드는 조인할 수 있음
	- 즉 스레드가 종료되면, 다른 스레드가 pthread_join()을 통해 그 리턴 상태를 얻을 수 있음
- 하지만 스레드의 리턴 상태에는 관심이 없고, 종료하면 자동으로 스레드를 제거하고 뒷정리하기를 바란다면?
- 해당 스레드를 분리(detach)할 수 있음.
### pthread_detach()
~~~c
#include <pthread.h>

int pthread_detach(pthread_t thread);
~~~
- 성공하면 0을 리턴, 에러가 발생하면 에러 번호(양수)를 리턴함
- 스레드가 일단 분리되면 pthread_join()으로 리턴 상태를 얻을 수 없고, 다시 조인할수도 없음
- 스레드를 분리해도 다른 스레드에서 exit()을 부르거나 주 스레드가 return하면 함께 종료됨
	- 즉, pthread_detach()는 단순히 스레드가 종료된 뒤에 일어날 일들을 제어하는 것이지, 종료되는 방법인아 시기를 제어하는 것이 아님