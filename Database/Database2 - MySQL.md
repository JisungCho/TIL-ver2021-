# Database2 - MySQL

```
mysql> CREATE TABLE topic(
    -> id INT(11) NOT NULL AUTO_INCREMENT,
    -> title VARCHAR(100) NOT NULL,
    -> description TEXT NULL,
    -> created DATETIME NOT NULL,
    -> author VARCHAR(30) NULL,
    -> profile VARCHAR(100) NULL
    -> ,
    -> PRIMARY KEY(id));
```

- PRIMARY KEY는 데이터에서 가장 중요한 값으로 중복되지 않는 값이다.

## CREATE

```
INSERT INTO topic(title,description,created,author,profile) VALUES ('PostgreSQL','Postgre SQL is....',NOW(),'taeho','data scientist,developer');
```

## READ

- 모든 데이터 출력

  ```
  SELECT * FROM topic
  ```

- 내가 원하는 데이터만 출력

  ```
  SELECT id,title,created,author,profile from topic WHERE author = 'jisung' ORDER BY id DESC;
  ```

## UPDATE

```
 UPDATE topic SET description='Oracle is...' , title ='Oracle' WHERE id= 2;
```

## DELETE

```
DELETE FROM topic WHERE id=4;
```

## 관계형 데이터 베이스

- 데이터가 중복 ? => 개선 할 부분이 있다.

![image-20210412114639246](C:\Users\jisun\AppData\Roaming\Typora\typora-user-images\image-20210412114639246.png)

- 위에 작성한 db스타일의 장점 

  1. 직관적

- 관계형 DB 단점

  1. 별도의 표를 열어서 비교해보며 분석해야한다.

     => JOIN을 이용해서 결합해서 읽어오게끔 할 수 있다.

## 테이블 분리하기