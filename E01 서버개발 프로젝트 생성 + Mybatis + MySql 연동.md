## E01 서버개발 프로젝트 생성 + Mybatis + MySql 연동
### 스프링부트 시작 설정
1. spring starter project 실행
2. 디펜던시 설정
	- Lombok
    - Mybatis Framework
    - Mysql Driver
    - Spring Boot Dev Tools => 코드 수정 시 자동 실행
    - Spring configuration Processor => Configuration metadata file를 생 성 할 수 있도록 함
    - Spring web => mvc패턴
   
--------

### 스프링부트 어노테이션

```java
package kr.co.jisung;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ExampleSpringApplication {

	public static void main(String[] args) {
		SpringApplication.run(ExampleSpringApplication.class, args);
	}

}

```

- **@SpringBootApplication = @SpringBootConfiguration + @ComponentScan + @EnableAutoConfiguration**
  1. **@SpringBootConfiguration (== @Configuration)**
     설정을 위한 어노테이션으로 개발자가 생성한 class를
     Bean으로 생성 할 때 Single Tone으로 한번만 생성
  2. **@ComponentScan**
     annotation이 지정된 class의 package 밑으로 component scan을 진행하여 @Component 계열 annotation(@Controller, @RestController, @Configuration, @Repository, @Service)과 @Bean annotaion이 붙은 method return 객체를 모두 bean으로 등록합니다.
  3. **@EnableAutoConfiguration**
     application context에 ServletWebServerFactory bean을 등록하여 web application으로 만들어 줍니다.

>   참고) @Bean은 개발자가 제어할 수 없는 외부 라이브러리를 bean으로 등록하는 경우 사용하고 @Component는 개발자가 제어할 수 있는(직접 작성한) 클래스를 bean으로 등록하는 경우 사용

----

### DatabaseConfiguration

```java
package kr.co.jisung.configuration;

import javax.sql.DataSource;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class DatabaseConfiguration {
	
	@Bean
	@ConfigurationProperties(prefix = "spring.datasource")
	public DataSource dataSource() {
		return DataSourceBuilder.create().build();
	}
}

```

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/[DB이름]/serverTimeZone=UTC&CharacterEncoding=UTF-8allowMultiQueries=true&serverTimeZone=UTC&CharacterEncoding=UTF-8
spring.datasource.username=tester
spring.datasource.password=1234
```

1. @ConfigurationProperties
   - *.properties , *.yml 파일에 있는 property를 자바 클래스에 값을 가져와서(바인딩) 사용할 수 있게 해주는 어노테이션

### 

