# DB

Tags: MySQL

### Key

- primary
- foreign

- 스키마: database라 씀

- 인바운드: EC2 → sql

### 스토리지

- innodb엔진: 커밋하면 파일에 들어감
- myi3엔진: 파일에 바로 저장

### 접속하기

`mysql -u root -p`

- -u: user
- -p: pw
- -h: host

### drop

- db 삭제: `drop database`

### alter

- 패스워드 변경: alter user <username>

### grant

- 권한주기: show grants for ‘username’@‘<host(엔드포인트)>
- 권한삭제: drop user

### column type

- tinyint(1B), smallint(2B), mediumint(3B), int(4B), bigint(8B), float(M, D), decimal/numeric(M, D)
- date(3B), time(3B), datetime(8B), timestamp(4B, 1970-2038-01-19), year(1B, -2155)
- char(n), varchar(n): 이제는 글자수로 쳐서 바이트 아님

### Datetime vs

### default CharSet

id (int unsigned, auto_increment, PK)

- `unsigned` 붙이면 양수만: 1 ~ 42억
- 안붙이면: -21억 ~ +21억

- 같은 구조/테이블 복사: `create table UserLike like User;`

### Relation

ON [DELETE | UPDATE] [RESTRICT | CASCADE | SET NULL | NO ACTION | SET DEFAULT]

- delete: 참조하던게 지워졌을 때
- update: id 값이 갑자기 바꼈을 때
- CASCADE: 같이 삭제
- restrict: 삭제 안함, 수정 안함 (참조하던 테이블 다 정리하고 와)
- set default: default

### Union

- 합칠때 사용