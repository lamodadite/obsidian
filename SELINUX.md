## SELinux
- Security Enhanced Linux
- 관리자가 시스템 엑세스 권한을 효과적으로 제어할 수 있게 하는 보안 아키텍처
- 강제 접근 제어 모델을 사용하기 때문에 악의적인 행동이나 실수로 인한 보안 위반을 방지함
## 동작 모드
- enforce
	-  정책을 엄격하게 시행하며, 정책 위반을 차단하고 보안 로그를 기록
	- 시스템 및 데이터의 안전을 보장함
- permissive
	- 정책 위반을 감지하지만, 액세스를 차단하지는 않음
	- 시스템은 정상적으로 작동하며, 허용되지 않은 액세스 시도에 대한 알림 및 로깅만 생성
	- 정책을 테스트하거나 디버깅할 때 유용
- disable
	- 보안 기능이 아예 비활성화됨
	- 기존 DAC모델에 따라 동작하며, SELinux 정책은 무시됨
## 명령어
~~~
SELinux 모드 확인
sestatus // 자세한 정보를 제공
getenforce // 어떤 모드인지만

SELinux 설정 파일 편집
vi /etc/selinux/config

SELinux 동작모드 변경
setenforce 0 // 임시로 permissive 모드로 바꿈
vi /etc/selinux/config 내의 SELINUX= 를 disabled로 수정. 영구적으로 적용됨.
~~~
