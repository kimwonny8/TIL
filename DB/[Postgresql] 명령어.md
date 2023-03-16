1. psql 명령어

- psql 접속
  psql -U username -d database
- psql 비밀번호 접속
  PGPASSWORD=password psql -U username -d database
- 데이터베이스 목록
  \l
- 데이터베이스 내 릴레이션 정보 확인
  \d
- 데이터베이스 내 테이블 조회
  \dt
- 스키마 조회
  \dn
- USER 조회 (각 user가 가진 role, 롤, 권한을 확인할 수 있다.)
  \du
- TABLESPACE 조회
  \db
- 다른 데이터베이스 접속
  \c {database_name}
- psql 종료
  \q
- query 수정 및 실행
  \e 명령어를 실행하면 psql.edit 메모장 파일이 열려 query를 수정하고 실행할 수 있다.
  쿼리 입력 후, esc+:wq 로 빠져나오면 bash 창에 쿼리 결과가 표시
- Variable (변수) 선언
  \set {name} {value} 로 변수 사용 가능
  \echo 명령어로 {name} 변수를 호출하면 변수에 저장된 값이 출력
- Special Variable 선언
  psql 에는 환경설정 셋팅을 변경할 수 있는 특별한 변수가 존재
  변수 선언과 마찬가지로 \set {special_variable} {option} 을 통해 세팅 변경을 위한 변수 설정을 할 수 있다.