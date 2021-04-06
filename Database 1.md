# Database 1

- 입력
  1. 생성(**C**reate)
  2. 수정(**U**pdate)
  3. 삭제(**D**elete)

- 출력
  1. 읽기(**R**ead)
- file vs database
  -  파일은 찾기 ,  수정  등등이 어렵다 -> 엑셀(스프레드 시트)을 이용하게 된다. -> 구조적으로 데이터를 저장할 수 있다. -> 프로그래밍 적으로 더 발전 -> 데이터베이스 소프트웨어

----



# Database 2 - MySQL

## 스프레드 시트 VS 데이터베이스

- 공통점
  1. 표의 형태로 표현해준다
  2. 기능이 거의 비슷하다.
  3. 

- 차이점
  1. 데이터베이스는 코딩을 통해서 제어할 수 있다.

## MySQL 구조

1. 정보는 표(TABLE)에 저장된다.
2. 연관 표들을 그룹핑 한 것을 스키마라고 한다.
3. 스키마들을 저장하는 공간을 database server 

## 스키마

```sql
CREATE DATABASE opentutorials;

USE opentutorials;
```

## SQL 과 테이블의 구조

- Structured Query Language :  구조화(정리정돈,표)된 요청 언어

## 테이블의 생성