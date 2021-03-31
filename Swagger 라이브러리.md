## Swagger 라이브러리

### Swagger 라이브러리 추가

```xml
		<!-- swagger설정1 -->
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger2</artifactId>
			<version>${swagger-version}</version>
		</dependency>
		<!-- swagger설정2 -->
		<dependency>
			<groupId>io.springfox</groupId>
			<artifactId>springfox-swagger-ui</artifactId>
			<version>${swagger-version}</version>
```

----

### SwaggerConfiguration

```java
package kr.co.jisung.configuration;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.ApiSelectorBuilder;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;
/*
 * Swagger설정
 */
@Configuration
@EnableSwagger2
public class SwaggerConfiguration {
/*
 * ApiInfo는 Swagger-ui에서 메인으로 보여질 정보를 설정
 * Docket은 api의 그룹명이나 이동경로, 보여질 api가 속한 패키지 등의 자세한 정보를 담음
 */
	@Bean
	public Docket docket() {
		ApiInfoBuilder apiInfo = new ApiInfoBuilder();
		apiInfo.title("API 서버 문서");
		apiInfo.description("API 서버 문서 입니다.");
		
		Docket docket = new Docket(DocumentationType.SWAGGER_2);
		
		// Swagger API 문서에 대한 설명을 표기하는 메소드입니다. (선택)
		docket.apiInfo(apiInfo.build());
		
		//Swagger API 문서로 만들기 원하는 basePackage 경로입니다. (필수)
		ApiSelectorBuilder apis = docket.select().apis(RequestHandlerSelectors.basePackage("kr.co.jisung.mvc.controller"));
		
		//URL 경로를 지정하여 해당 URL에 해당하는 요청만 Swagger API 문서로 만듭니
		apis.paths(PathSelectors.ant("/**")); 
		
		return apis.build();
	}
}

```

----

### BoardController에 swagger 적용

```java
package kr.co.jisung.mvc.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import io.swagger.annotations.Api;
import io.swagger.annotations.ApiImplicitParam;
import io.swagger.annotations.ApiImplicitParams;
import io.swagger.annotations.ApiOperation;
import kr.co.jisung.mvc.domain.Board;
import kr.co.jisung.mvc.parameter.BoardParameter;
import kr.co.jisung.mvc.service.BoardService;

@RestController
@RequestMapping("/board")
/*
 * @Api : 해당 클래스가 Swagger리소스라는 것을 명시
 * @ApiOperation : 한 개의 operation을 선언
 * 								VALUE : API에 대한 간략한 설명
 * 								NOTES : 자세한 설명
 * @ApiParam : 파라미터에 대한 정보를 명시합니다.
 * 						value : 파라미터 정보를 작성합니다.
 * 						required : 필수 파라미터이면 true, 아니면 false를 작성합니다.
 * 						example :  테스트를 할 때 보여줄 예시를 작성합니다
*/	
@Api(tags = "게시판 API")
public class BoardController {
	
	@Autowired
	private BoardService boardService;
	
	@GetMapping("/list")
	@ApiOperation(value = "목록 조회",notes = "목록 정보를 조회할 수 있습니다.")
	public List<Board> getList(){
		return boardService.getList();
	}
	
	
	@GetMapping("/{boardSeq}")
	@ApiOperation(value = "상세 조회",notes = "게시물 번호에 해당하는 상세 정보를 조회할 수 있습니다.")
	@ApiImplicitParams({
		@ApiImplicitParam(name = "boardSeq" ,value = "게시물 번호", example = "1")
	})
	public Board get(@PathVariable int boardSeq) {
			return boardService.get(boardSeq);
	}
	
	@PutMapping("/save")
	@ApiOperation(value = "등록/수정 처리",notes = "신규 게시물 등록 및 기준 게시물 수정처리")
	@ApiImplicitParams({
		@ApiImplicitParam(name = "boardSeq" ,value = "게시물 번호", example = "1"),
		@ApiImplicitParam(name = "title" ,value = "제목", example = "spring"),
		@ApiImplicitParam(name = "contents" ,value = "내용", example = "spring 강좌")
	})
	public int save(BoardParameter parameter) {
		boardService.save(parameter);
		return parameter.getBoardSeq();
	}
	
	@DeleteMapping("/{boardSeq}")
	@ApiOperation(value = "삭제 처리",notes = "게시물 번호에 해당하는 게시물 삭제처리")
	@ApiImplicitParams({
		@ApiImplicitParam(name = "boardSeq" ,value = "게시물 번호", example = "1"),
	})
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

---

### 파라미터로 쓰일 BoardParameter 클래스 작성 및 BoardService , BoardRepository , Board.xml 수정

```java
package kr.co.jisung.mvc.parameter;

import java.sql.Date;

import lombok.Data;

//파라미터와 Response는 클래스로 구별하는게 좋다
//getter , setter
@Data
public class BoardParameter {
	private int boardSeq;
	private String title;
	private String contents;
}

```

```java
package kr.co.jisung.mvc.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import kr.co.jisung.mvc.domain.Board;
import kr.co.jisung.mvc.parameter.BoardParameter;
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
	public void save(BoardParameter parameter) {
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

```java
package kr.co.jisung.mvc.repository;

import java.util.List;

import org.springframework.stereotype.Repository;

import kr.co.jisung.mvc.domain.Board;
import kr.co.jisung.mvc.parameter.BoardParameter;
/*
 * 게시물 레퍼지토리
 */
@Repository
public interface BoardRepository {
	List<Board> getList();
	
	Board get(int boardSeq);
	
	void save(BoardParameter board);
	
	void update(BoardParameter board);
	
	void delete(int boardSeq);
	
}

```

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
	<insert id="save" parameterType="kr.co.jisung.mvc.parameter.BoardParameter" useGeneratedKeys="true" keyProperty="boardSeq">
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
	
	<update id="update" parameterType="kr.co.jisung.mvc.parameter.BoardParameter">
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

