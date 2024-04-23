# SQL
## DDL(데이터 정의어)

#### CREATE
#### ALTER
#### DROP


## DCL(데이터 제어어)
- 데이터의 보안, 무결성, 회복, 병행 제어 등을 정의하는 데 사용하는 언어이다.

#### COMMIT : 명령에 의해 수행된 결과를 실제 물리적 디스크로 저장하고, 데이터베이스 조작 작업이 정상적으로 완료되었음을 관리자에게 알려줌
#### ROLLBACK : 데이터베이스 조작 작업이 비정상적으로 종료되었을 때 원래의 상태로 복구함
#### GRANT : 데이터베이스 사용자에게 사용 권한을 부여함
#### REVOKE : 데이터베이스 사용자의 사용 권한을 취소함


<br>

### 테이블 및 속성에 대한 권한 부여 및 취소
```
- GRANT 권한_리스트 ON 개체 TO 사용자 [WITH GRANT OPTION];
- REVOKE [GRANT OPTION FOR] 권한_리스트 ON 개체 FROM 사용자 [CASCADE];
```

- 권한 종류 : ALL, SELECT, INSERT, DELETE, UPDATE 등
- WITH GRANT OPTION : 부여받은 권한를 다른 사용자에게 다시 부여할 수 있는 권한을 부여함
- GRANT OPTION FOR : 다른 사용자에게 권한을 부여할 수 있는 권한을 취소함
- CASCADE : 권한 취소 시 권한을 부여받았던 사용자가 다른 사용자에게 부여한 권한도 연쇄적으로 취소

## DML(데이터 조작어)
- 데이터베이스 사용자가 저장된 데이터를 실질적으로 관리하는 데 사용되는 언어이다.

#### SELECT
#### INSERT
#### DELETE
#### UPDATE

