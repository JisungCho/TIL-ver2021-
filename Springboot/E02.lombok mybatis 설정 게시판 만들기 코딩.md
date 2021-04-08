# lombok mybatis 설정 게시판 만들기 코딩

### Board 클래스

```java
package kr.co.jisung.mvc.domain;

import java.sql.Date;

import lombok.Data;

//getter , setter
@Data
public class Board {
	private int boardSeq;
	private String title;
	private String contents;
	private Date regDate;
}

```

----

### BoardRepository 인터페이스

```java
package kr.co.jisung.mvc.repository;

import java.util.List;

import org.springframework.stereotype.Repository;

import kr.co.jisung.mvc.domain.Board;
/*
 * 게시물 레퍼지토리
 */
@Repository
public interface BoardRepository {
	List<Board> getList();
	
	Board get(int boardSeq);
	
	void save(Board board);
	
	void update(Board board);
	
	void delete(int boardSeq);
	
}

```

---

### BoardService

```java
package kr.co.jisung.mvc.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import kr.co.jisung.mvc.domain.Board;
import kr.co.jisung.mvc.repository.BoardRepository;

/*
 * 게시물 서비스
 */
@Service
public class BoardService {
	
	@Autowired
	private BoardRepository repository;
	
	public List<Board> getList(){
		return repository.getList();
	}
	
	public Board get(int boardSeq) {
			return repository.get(boardSeq);
	}
	
	/*
	 *  등록/수정처리
	 */
	public void save(Board parameter) {
		Board board = repository.get(parameter.getBoardSeq());
		if(board == null) {
			repository.save(parameter);
		}else {
			repository.update(parameter);
		}
	}
	
	public void delete(int boardSeq) {
		repository.delete(boardSeq);
	}
}

```

---

### BoardController

```java
package kr.co.jisung.mvc.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import kr.co.jisung.mvc.domain.Board;
import kr.co.jisung.mvc.service.BoardService;

@RestController
@RequestMapping("/board")
public class BoardController {
	
	@Autowired
	private BoardService boardService;
	
	@GetMapping("/list")
	public List<Board> getList(){
		return boardService.getList();
	}
	
	@GetMapping("/{boardSeq}")
	public Board get(@PathVariable int boardSeq) {
			return boardService.get(boardSeq);
	}
	
	@GetMapping("/save")
	public int save(Board parameter) {
		boardService.save(parameter);
		return parameter.getBoardSeq();
	}
	
	@GetMapping("/delete/{boardSeq}")
	public boolean delete(@PathVariable int boardSeq) {
		Board board = boardService.get(boardSeq);
		if(board == null) {
			return false;
		}
		boardService.delete(boardSeq);
		return true;
	}
}

```

-----

### Board.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace: Repository 위치 -->
<mapper namespace="kr.co.jisung.mvc.repository.BoardRepository">
		<select id="getList" parameterType="kr.co.jisung.mvc.domain.Board" resultType="kr.co.jisung.mvc.domain.Board">
		SELECT
			B.BOARD_SEQ,
			B.TITLE,
			B.CONTENTS,
			B.REG_DATE
		FROM T_BOARD B
		ORDER BY B.REG_DATE DESC
	</select>
	
	<select id="get" parameterType="int" resultType="kr.co.jisung.mvc.domain.Board">
		SELECT
			B.BOARD_SEQ,
			B.TITLE,
			B.CONTENTS,
			B.REG_DATE
		FROM T_BOARD B
		WHERE B.BOARD_SEQ = #{boardSeq}
	</select>	
	
	<!-- 
	useGeneratedKeys : (insert, update에만 적용) 자동생성 키를 받을때 true로 설정한다. (default: false)
	keyProperty : 리턴 될 key property 설정. 여러개를 사용한다면 ,(콤마)를 구분자로 나열한다.
 	-->
	<insert id="save" parameterType="kr.co.jisung.mvc.domain.Board" useGeneratedKeys="true" keyProperty="boardSeq">
		INSERT INTO T_BOARD
		(
			TITLE,
			CONTENTS,
			REG_DATE
		)
		VALUES
		(
			#{title},
			#{contents},
			NOW()
		)
	</insert>	
	
	<update id="update" parameterType="kr.co.jisung.mvc.domain.Board">
		UPDATE T_BOARD
		SET
			TITLE = #{title},
			CONTENTS = #{contents}
		WHERE BOARD_SEQ = #{boardSeq}
	</update>		
	
	<delete id="delete" parameterType="int">
		DELETE FROM T_BOARD
		WHERE BOARD_SEQ = #{boardSeq}
	</delete>	
</mapper>
```

----

### MybatisConfiguration

```java
package kr.co.jisung.configuration;

import javax.sql.DataSource;

import org.apache.ibatis.session.SqlSessionFactory;
import org.mybatis.spring.SqlSessionFactoryBean;
import org.mybatis.spring.SqlSessionTemplate;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/*
 * mybatis 는 application.properties 에 설정 불가)
 * Mybatis 설정
 */
@Configuration
//@MapperScan 아노테이션은 지정된 베이스 패키지에서 DAO(Mapper) 를 검색하여 등록합니다
@MapperScan(basePackages = "kr.co.jisung.mvc.repository")
public class MybatisConfiguration {
	
	/*
	 * 마이바티스에서는 SqlSession를 생성하기 위해 SqlSessionFactory를 사용한다.
	 * SqlSessionFactory는 데이터베이스와의 연결과 SQL의 실행에 대한 모든 것을 가진 가장 중요한 객체이다.
	 * SqlSessionFactory를 생성해주는 특별한 객체를 먼저 설정해주어야하는데, 
	 * 이때 스프링-마이바티스에서는 SqlSessionFactoryBean이라는 클래스를 사용한다
	 */
	@Bean																/*DatabaseConfig에서 만든 DataSource주입*/
	public SqlSessionFactory sqlSessionFactory(@Autowired DataSource dataSource,ApplicationContext applicationContext) throws Exception{
		SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
		factoryBean.setDataSource(dataSource);
		/*마이바티스 XML 파일의 위치*/
		factoryBean.setMapperLocations(applicationContext.getResources("classpath:mybatis/sql/*.xml"));
		SqlSessionFactory factory = factoryBean.getObject();
		factory.getConfiguration().setMapUnderscoreToCamelCase(true);
		return factoryBean.getObject();
	}
	
	//SqlSessionTemplate은 SqlSession을 구현하고 코드에서 SqlSession을 대체하는 역활
	@Bean
	public SqlSessionTemplate sqlSessionTemplate(@Autowired SqlSessionFactory sqlSessionFactory) {
		return new SqlSessionTemplate(sqlSessionFactory);
	}
}

```

