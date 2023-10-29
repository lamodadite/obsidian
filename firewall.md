## firewall?
- 네트워크 방화벽 프로그램
- 방화벽은 외부의 침입을 막기 위해 실행됨
- 기본 포트인 22번(SSH)만 허용되어있음
- 다른 모든 서비스 접근을 위한 포트를 열려면 허용을 따로 해야함
## rocky 명령어
~~~
방화벽 포트 추가
firewall-cmd --zone=public --add-port=PortNumber/tcp --permanent

방화벽 포트 삭제
firewall-cmd --zone=public --remove-port=PortNumber/tcp --permanent

방화벽 재시작 (변경 내용 적용)
firewall-cmd --reload

추가한 설정 조회하기
firewall-cmd --list-all
~~~
