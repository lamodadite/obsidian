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
- 