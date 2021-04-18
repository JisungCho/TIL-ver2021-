## Spring Message(다국어) 설정 +  ControllerAdvice 사용법 + 간편한 데이터 검증/예외처리

### WebConfiguration ( Spring Message 설정)

```java
package kr.co.jisung.configuration;

import java.util.Locale;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.support.ReloadableResourceBundleMessageSource;
/*
 * 다국어 지원 설정
 */
@Configuration
public class WebConfiguration {
	/*
	 * 메세지 소스를 생성한다.
	 */
	@Bean
	public ReloadableResourceBundleMessageSource messageSource() {
		ReloadableResourceBundleMessageSource source = new ReloadableResourceBundleMessageSource();
		//메세지 프로퍼티 파일의 위치와 이름을 지정
		source.setBasename("classpath:/messages/message");
		//기본 인코딩을 지정
		source.setDefaultEncoding("UTF-8");
		//프로퍼티 파일의 변경을 감지할 시간 간격을 지정
		source.setCacheSeconds(60);
		//기본값
		source.setDefaultLocale(Locale.KOREAN);
		//없는 메세지일 경우 예외를 발생시키는 대신 코드를 기본 메세지로 한다.
		source.setUseCodeAsDefaultMessage(true);
		return source;
	}
}

```

### 커스텀 예외처리 추상 클래스 AbstractBaseException 

```java
package kr.co.jisung.configuration.exception;

import kr.co.jisung.configuration.http.BaseResponseCode;

/*
 * 커스텀 예외처리 추상 클래스
 */
public abstract class AbstractBaseException extends RuntimeException {
	private static final long serialVersionUID = 8342235231880246631L;

	protected BaseResponseCode responseCode;
	protected Object[] args;

	public AbstractBaseException() {
	}

	public AbstractBaseException(BaseResponseCode responseCode) {
		this.responseCode = responseCode;
	}

	public BaseResponseCode getResponseCode() {
		return responseCode;
	}

	public Object[] getArgs() {
		return args;
	}
}

```

----

### 공통 예외처리 클래스 BaseException

```java
package kr.co.jisung.configuration.exception;

import kr.co.jisung.configuration.http.BaseResponseCode;
/*
 * 공통 예외처리 클래스
 */
public class BaseException extends AbstractBaseException {
	private static final long serialVersionUID = 8342235231880246631L;
	
	public BaseException() {
	}
	
	public BaseException(BaseResponseCode responseCode) {
		this.responseCode = responseCode;
	}

	public BaseException(BaseResponseCode responseCode, String[] args) {
		this.responseCode = responseCode;
		this.args = args;
	}

}

```

---

### BaseControllerAdvice

```java
package kr.co.jisung.configuration.web.bind.annotation;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.MessageSource;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.context.request.WebRequest;

import kr.co.jisung.configuration.exception.BaseException;
import kr.co.jisung.configuration.http.BaseResponse;

/*
 * @ControllerAdvice의 경우에는 모든 @Controller,@RestController에서 발생하는 예외를 핸들링
 */
@ControllerAdvice
public class BaseControllerAdvice {
	
	@Autowired
	private MessageSource messageSource; //WebConfiguration의 bean 자동 주입
	/*
	 * @ExceptionHandler
	 * @Controller, @RestController가 적용된 Bean내에서 발생하는 예외를 잡아서 하나의 메서드에서 처리해주는 기능을 한다.
	 */
	@ExceptionHandler(value = { BaseException.class })
	@ResponseStatus(HttpStatus.OK)
	@ResponseBody
	private BaseResponse<String> handleBaseException(BaseException e, WebRequest request) {
		return new BaseResponse<String>(e.getResponseCode(),messageSource.getMessage(e.getResponseCode().name(), e.getArgs(), null));
	}
}

```

```java
package kr.co.jisung.configuration.http;

import lombok.Data;

@Data
public class BaseResponse<T>{
	private BaseResponseCode code;
	private String message;
	private T data;
	
	public BaseResponse(T data) {
		this.code = BaseResponseCode.SUCCESS;
		this.data = data;
	}

    //code와 message
	public BaseResponse(BaseResponseCode code, String message) {
		this.code = code;
		this.message = message;
	}
}

```

------

### message_ko.properties

```properties
SUCESS = 정상적으로 처리되었습니다.
ERROR = 오류가 발생했습니다.
DATA_IS_NULL = 요청하신 {0} 데이터는 NULL입니다.
VALIDATE_REQUIRED = {0}({1}) 필드는 필수로 입력하셔야 합니다.
```

-----

### BaseResponseCode

```java
package kr.co.jisung.configuration.http;

/*
 * enum이란 enumerated type의 줄임말로
 * 멤버라 불리는 명명된 값의 집합을 이루는 자료형이다.
 * 즉, 열거형에 사용될 수 있는 특정한 값들을 정의해서 해당 값들만 사용할 수 있게 한다.
 * 참고로, 열거자 이름들은 일반적으로 해당 언어의 상수 역할을 하는 식별자이다.
 */

public enum BaseResponseCode {
	SUCCESS,
	ERROR,
	DATA_IS_NULL,
	VALIDATE_REQUIRED,
	;
}

```



----

### BoardController

```java
package kr.co.jisung.mvc.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.util.StringUtils;
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
import kr.co.jisung.configuration.exception.BaseException;
import kr.co.jisung.configuration.http.BaseResponse;
import kr.co.jisung.configuration.http.BaseResponseCode;
import kr.co.jisung.mvc.domain.Board;
import kr.co.jisung.mvc.parameter.BoardParameter;
import kr.co.jisung.mvc.service.BoardService;

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
@RestController
@RequestMapping("/board")
@Api(tags = "게시판 API")
public class BoardController {
	
	@Autowired
	private BoardService boardService;
	
	@GetMapping
	@ApiOperation(value = "목록 조회",notes = "목록 정보를 조회할 수 있습니다.")
	public BaseResponse<List<Board>> getList(){
		return new BaseResponse<List<Board>> (boardService.getList());
	}
	
	
	@GetMapping("/{boardSeq}")
	@ApiOperation(value = "상세 조회",notes = "게시물 번호에 해당하는 상세 정보를 조회할 수 있습니다.")
	@ApiImplicitParams({
		@ApiImplicitParam(name = "boardSeq" ,value = "게시물 번호", example = "1")
	})
	public BaseResponse<Board> get(@PathVariable int boardSeq) {
		Board board = boardService.get(boardSeq);
		//가져온게 없으면 예외처리
		if(board == null) {
			throw new BaseException(BaseResponseCode.DATA_IS_NULL, new String[] {"게시물"});
		}
		return new BaseResponse<Board>(board);
	}
	
	@PutMapping("/save")
	@ApiOperation(value = "등록/수정 처리",notes = "신규 게시물 등록 및 기준 게시물 수정처리")
	@ApiImplicitParams({
		@ApiImplicitParam(name = "boardSeq" ,value = "게시물 번호", example = "1"),
		@ApiImplicitParam(name = "title" ,value = "제목", example = "spring"),
		@ApiImplicitParam(name = "contents" ,value = "내용", example = "spring 강좌")
	})
	public BaseResponse<Integer> save(BoardParameter parameter) {
		//제목 필수 체크
		//StringUtils.isEmpty : java 문자 혹은 문자열 빈값 체크하는 방법
		if(StringUtils.isEmpty(parameter.getTitle())) {
			throw new BaseException(BaseResponseCode.VALIDATE_REQUIRED, new String[] {"title","제목" });
		}
		//내용 필수 체크
		if(StringUtils.isEmpty(parameter.getContents())) {
			throw new BaseException(BaseResponseCode.VALIDATE_REQUIRED, new String[] {"contents","내용" });
		}
		boardService.save(parameter);
		return new BaseResponse<Integer>(parameter.getBoardSeq());
	}
	
	@DeleteMapping("/{boardSeq}")
	@ApiOperation(value = "삭제 처리",notes = "게시물 번호에 해당하는 게시물 삭제처리")
	@ApiImplicitParams({
		@ApiImplicitParam(name = "boardSeq" ,value = "게시물 번호", example = "1"),
	})
	public BaseResponse<Boolean> delete(@PathVariable int boardSeq) {
		Board board = boardService.get(boardSeq);
		if(board == null) {
			return new BaseResponse<Boolean>(false);
		}
		boardService.delete(boardSeq);
		return new BaseResponse<Boolean>(true);
	}
}

```

