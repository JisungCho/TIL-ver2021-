# E11 공통기능 - 게시물 조회시 목록 페이징 처리방법(HandleMethodArgumentResolver 활용)

## MySQLPageRequest

```java
package kr.co.jisung.framework.data.domain;

import com.fasterxml.jackson.annotation.JsonIgnore;

import io.swagger.annotations.ApiModelProperty;
import lombok.Data;

/**
 * MySQL 페이지 요청 정보 및 계산된 값
 * @author jisun
 *
 */

@Data
public class MySQLPageRequest {
	
	//페이지
	private int page;
	//페이지당 보여줄 갯수
	private int size;
	
	/*
	 * limit , offset 2개 변수는 page, size 정보로,  자동으로 계산된 값이 들어감
	 * @JsonIgnore를 뭍이면 데이터를 주고 받을 때 해당데이터는 ignore되어서 직렬화 되지 않음
	 * @ApiModelProperty : 모델의 요소에 설명을 추가합니다.
	 * 										hidden true시 샘플 모델에서 자동으로 제외
	 */
	
	@JsonIgnore
	@ApiModelProperty(hidden = true)
	private int limit;
	
	@JsonIgnore
	@ApiModelProperty(hidden = true)
	private int offset;
	
	public MySQLPageRequest(int page, int size, int limit, int offset) {
		this.page = page;
		this.size = size;
		this.limit = limit;
		this.offset = offset;
	}
	
	
}

```

## PageRequestParameter

```java
package kr.co.jisung.framework.data.domain;

import lombok.Data;

/**
 * 페이지 요정정보와 파라미터 정보
 * @author jisun
 * @param <T>
 */

@Data
public class PageRequestParameter<T> {
	private MySQLPageRequest pageRequest;
	private T parameter;
	
	public PageRequestParameter(MySQLPageRequest pageRequest, T parameter) {
		this.pageRequest = pageRequest;
		this.parameter = parameter;
	}
	
}

```

## MySQLPageRequestHandleMethodArgumentResolver

```java
package kr.co.jisung.framework.data.web;

import javax.servlet.http.HttpServletRequest;

import org.apache.commons.lang3.math.NumberUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.MethodParameter;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;

import kr.co.jisung.framework.data.domain.MySQLPageRequest;

/**
 * MySQL 쿼리 페이징 limit, offset 값을 자동계산하여 MySQLPageRequest 클래스에 담아서 컨트롤러에서 받을 수 있게함
 * HandlerMethodArgumentResolver : 컨트롤러 메서드에서 특정 조건에 맞는 파라미터가 있을 때 원하는 값을 바인딩해주는 인터페이스
 * @author jisun
 *
 */
public class MySQLPageRequestHandleMethodArgumentResolver implements HandlerMethodArgumentResolver {

	final Logger logger = LoggerFactory.getLogger(getClass());
	
	private static final String DEFAULT_PARAMETER_PAGE = "page";
	private static final String DEFAULT_PARAMETER_SIZE = "size";
	private static final int DEFAULT_SIZE = 20;
	
	/*
	 * 바인딩할 객체를 조작할 수 있는 메서드
	 */
	@Override
	public Object resolveArgument(MethodParameter parameter, ModelAndViewContainer mavContainer, NativeWebRequest webRequest, WebDataBinderFactory binderFactory) throws Exception {
		HttpServletRequest request = (HttpServletRequest) webRequest.getNativeRequest();
		
		
		// 현재 페이지 
		int page = NumberUtils.toInt(request.getParameter(DEFAULT_PARAMETER_PAGE), 1);
		// 리스트 갯수
		int offset = NumberUtils.toInt(request.getParameter(DEFAULT_PARAMETER_SIZE), DEFAULT_SIZE);
		
		// 시작지점
		int limit = (offset * page) - offset;
		
		logger.info("page : {}", page);
		logger.info("limit : {}, offset : {}", limit, offset);
		
		return new MySQLPageRequest(page, offset, limit, offset);
	}

	/*
	 * 바인딩할 클래스를 지정
	 */
	@Override
	public boolean supportsParameter(MethodParameter methodParameter) {
		return MySQLPageRequest.class.isAssignableFrom(methodParameter.getParameterType());
	}
	
}

```

## WebConfiguration

```java
	/*
	 * MySQLPageRequestHandleMethodArgumentResolver 등록
	 */
	@Override
	public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
		// 페이지 리졸버 등록
		resolvers.add(new MySQLPageRequestHandleMethodArgumentResolver());
	}
```

## BoardController , Service , Repository

```java
	/*
	 * @ApiParam : 요청 Parameter에 대한 설명 및 필수여부, 예제값 설정
	 */
	@GetMapping
	@ApiOperation(value = "목록 조회", notes = "게시물 목록 정보를 조회할 수 있습니다.")
	public BaseResponse<List<Board>> getList(@ApiParam BoardSearchParameter parameter,@ApiParam MySQLPageRequest pageRequest) {
		logger.info("pageRequest : {}", pageRequest);
		PageRequestParameter<BoardSearchParameter> pageRequestParameter = new PageRequestParameter<BoardSearchParameter>(pageRequest, parameter);
		return new BaseResponse<List<Board>>(boardService.getList(pageRequestParameter));
	}
```

```java
	public List<Board> getList(PageRequestParameter<BoardSearchParameter> pageRequestParameter){
		return repository.getList(pageRequestParameter);
	}
```

```java
List<Board> getList(PageRequestParameter<BoardSearchParameter> pageRequestParameter);
```

## Board.xml

```sql
		<!-- 변수는 pageRequst , parameter -->
		<select id="getList" parameterType="kr.co.jisung.framework.data.domain.PageRequestParameter" resultType="kr.co.jisung.mvc.domain.Board">
		SELECT
			B.BOARD_SEQ,
			B.TITLE,
			B.CONTENTS,
			B.REG_DATE
		FROM T_BOARD B
		<!--Mybatis에서 제공하는 where문법  -->
		<where>
		<!-- @를 사용하면 java static method 접근 가능 -->
		<!-- @패키지 + 클래스명@메소드명(파라메터 변수명)  -->
		<!-- null과 공백문자이면 false 나머지 true-->
		<!-- 컨텐츠가 리턴되면 단순히 “WHERE”만을 추가한다. 게다가 컨텐츠가 “AND”나 “OR”로 시작한다면 그 “AND”나 “OR”를 지워버린다 -->
			<if test="@org.apache.commons.lang3.StringUtils@isNotEmpty(parameter.keyword)">
				AND B.TITLE LIKE CONCAT ('%' , #{parameter.keyword} , '%')
			</if>
			<!-- null 체크 , 배열의 길이 체크 -->
			<!-- WHERE 일치하길 원하는 칼럼명 IN ( 조건1 ,조건2...) -->
			<!-- separator : 반복 되는 사이에 출력할 문자열 -->
			<if test="@org.apache.commons.lang3.ObjectUtils@isNotEmpty(parameter.boardTypes)">
				AND B.BOARD_TYPE IN (
					<foreach collection="parameter.boardTypes" item="value" separator=",">
						#{value}
					</foreach>
				)
			</if>
		</where>
		ORDER BY B.REG_DATE DESC
		<!-- 페이징 처리 -->
		LIMIT #{pageRequest.limit} , #{pageRequest.offset}
	</select>
```

