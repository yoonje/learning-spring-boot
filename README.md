# Spring Boot 정리 자료
스프링 부트 개념과 활용 정리 문서

Table of contents
=================
<!--ts-->
   * [Spring Boot Start](#Spring-Boot-Start)
   * [Spring Boot Principle](#Spring-Boot-Principle)
   * [Spring Boot Utilization](#Spring-Boot-Utilization)
   * [Spring Boot Operation](#Spring-Boot-Operation)
<!--te-->

Spring Boot Start
=======
### 스프링 부트
* 스프링 부트: 운영 수준의 스프링 기반의 독립적인 애플리케이션을 만드는데 도움을 주는 소프트웨어
  * 스프링 개발을 할 때 빠르고 폭 넓은 사용성을 목표로 함
  * 컨벤션으로 사용되는 기본 설정을 제공하고, 이를 쉽고 빠르게 수정할 수 있음
  * 전반적으로 대부분의 프로젝트에 흔하게 사용되는 유용한 비함수적 기능을 제공
  * XML 설정과 코드 제너레이션을 하지 않음
  * 자바 8 이상에서 사용 가능
* 스프링 부트 시작하기
  * `pox.xml`에 `spring-boot-starter-web`의존성과 `spring-boot-maven-plugin` 빌드 플러그인을 추가
  * `@SpringBootApplication` 어노테이션을 메인 함수를 가지고 있는 클래스에 추가
  * `$ mvn package`을 통해서 패키징하여 jar 파일 생성
  * `$ java -j target/{jar 파일}`을 통해서 jar 파일 실행하여 애플리케이션 실행  
  cf) `Spring Initializer`로 스프링 부트 프로젝트에 대한 설정을 간단히 완성할 수 있음

### 스프링 부트 프로젝트의 구조
* 자바 소스 코드(src/main/java)
* 리소스(src/main/resource)
  * 소스 코드 이외의 모든 파일들
  * 리소스 디렉토리가 classpath `루트`이고 그 뒤를 지정하여 파일들을 참조할 수 있음
* 테스트 코드(src/test/java)
  * 테스트와 관련된 코드
* 테스트 리소스(src/test/resource)
  * 테스트와 관련된 리소스

Spring Boot Principle
=======
* 스프링 부트의 의존성 관리 기능
  * 스프링 부트는 `pom.xml`에 정의된 의존성을 버전 명시 없이 가져오는데, 이는 `parent`의 각각의 `starter`에서 이미 등록된 spring boot dependencies를 찾아가서 자동으로 버전을 가져오기 때문에 편리하고 spring boot dependencies에서 관리 하지 않는 방법은 라이브러리는 추가 시에 반드시 버전을 명시해야함
  * 새로운 의존성을 `parent`를 통해서 의존성을 관리하는 것이 좋은 방법
  * 서드 파티 라이브러리는 버전 명시를 명시해서 `dependencis`로 추가하는 것이 좋음
* 스프링 부트 자동 설정
  * `@SpringBootApplication` 안에 있는 `@EnableAutoConfiguration`을 통해서 기본 설정들이 진행이 됨
  * 스프링 부트는 Bean을 `@ComponentScan`, `@EnableAutoConfiguration` 2단계를 통해서 빈으로 등록
  * `@ComponentScan`는 `@SpringBootApplication` 밑에 존재하는 `@Component`, `@Configuration`, `@Repository`, `@Service`, `@Controller`, `@RestController` 어노테이션들을 모두 빈으로 등록
  * `@EnableAutoConfiguration`는 `spring.factories` 패키지에 등록된 기본 빈 설정들을 `@ConditionalOn`으로 체크하고 빈으로 등록되어 있지 않은 경우 빈으로 등록하는데 `@ComponentScan`보다 `@EnableAutoConfiguration` 우선되므로 주의
* 스프링 부트 자동 설정 방법
  * Starter와 Autoconfigure: `spring-boot-autoconfigure`와 `spring-boot-autoconfigure-processor` 의존성 추가 -> @Configuration 파일 작성 -> src/main/resource/META-INF에 spring.factories 파일 만들기 -> spring.factories 안에 자동 설정 파일 추가 -> mvn install
  * 
* 내장 웹 서버
  * 서블릿: 자바를 사용하여 웹페이지를 동적으로 생성하는 서버측 프로그램
  * 톰캣: 서블릿 컨테이너만 있는 웹 애플리케이션 서버
  * 웹 서버 만들기: 톰캣 객체 생성 -> 포트 설정 -> 톰캣에 컨텍스트 추가 -> 서블릿 만들기 -> 톰캣에 서블릿 추가 -> 컨텍스트에 서블릿 맵핑 -> 톰캣 실행 및 대기
  * `서블릿 컨테이너`과 톰캣은 자동 설정을 통해서 스프링 부트 애플리케이션을 실행하면 실행
  * `ServletWebServerFactoryAutoConfiguration`을 통해서 서블릿을 만들게 되고 `DispatcherServletAutoConfiguration`을 통해서 서블릿이 등록
* 독립적으로 실행 가능한 Jar
  * spring-boot-maven 플러그인을 통해서 프로젝트를 패키징하면 target 안에 모든 라이브러리와 코드를 패키징하여 `jar`로 만듦
  * 내장 JAR : 패키징된 jar 안에 넣어진 jar들
  * org.springframework.boot.loader.jar.JarFile을 사용해서 내장 JAR를 읽음
  * org.springframework.boot.loader.Launcher를 사용해서 실행


Spring Boot Utilization
=======
### 스프링 부트 핵심 기능
* SpringApplication
* 외부 설정
* 프로파일
* 로깅
* 테스트

### 스프링 부트와 연동되는 기술
* 스프링 웹 MVC
* 스프링 데이터
* 스프링 시큐리티
* 스프링 REST API

### SpringApplication
* 기본 로그 레벨은 `INFO`
* 애플리케이션의 에러가 났을 때 이를 정제하려 출력해주는 `Failure Analyzer`가 여러 개 등록 되어 있고 이를 커스터마징 가능
* 배너
  * `banner.txt`나 다른 이미지 파일 정의을 정의하고 배너 변수를 활용해서 배너 커스터마이징이 가능하고 `application.properties`에 spring.banner.location 설정으로 파일 등록 가능
  * SpringApllication.setBanner()로 배너를 끄거나 킬 수 있음

### SpringApplication 고급
* 이벤트 리스너
  * ApplicationEvent를 등록하고 리스너를 개발하면 ApplicationContext를 만들고 나서 `SpringApplication.addListeners()`에서 @Bean으로 등록한 리스너에 대해 특정 이벤트에 대한 핸들링을 할 수 있음
* 애플리케이션 타입
  * `SpringApplication.setWebApplicationType()`에서 WebApplicationType을 Servlet(MVC)이나 Reactive(webflux)으로 설정 가능한데, 둘 중 하나만 존재하면 설정을 하지 않아도 타입이 결정 되지만 서블릿이 존재하면 무조건 서블릿으로 동작하므로 Reactive로 동작시키고 싶다면 설정을 해줘야함
* 애플리케이션 전달인자
  * IntelliJ를 통해서 VM과 애플리케이션에 전달인자를 줄 수 있는데, VM 옵션에 -D로 오는 것은 JVM 전달인자이고 프로그램 전달인자에 --로 오는 것은 애플리케이션 전달인자임
  * cli로도 VM과 애플리케이션에 전달인자를 줄 수 있는데 `java -jar {.jar 파일}` 명령 뒤에 -D로 오는 것은 JVM 전달인자이고 프로그램 전달인자에 --로 오는 것은 애플리케이션 전달인자임 
  * `ApplicationRunner`나 `CommandLineRunner`를 사용하면 애플리케이션 실행 뒤에 프로그램 전달인자를 이용할 수 있음

### 외부 설정
* `application.properties`이나 `application.yml`를 통해서 애플리케이션의 외부 설정을 저장할 수 있고 `@value("{설정값}")`을 통해서 코드에서 읽어올 수 있음
* 프로퍼티 설정의 우선 순위
  1. 유저 홈 디렉토리에 있는 spring-boot-dev-tools.properties
  2. 테스트에 있는 @TestPropertySource
  3. @SpringBootTest 애노테이션의 properties 애트리뷰트
  4. 커맨드 라인 아규먼트
  5. SPRING_APPLICATION_JSON(환경 변수 또는 시스템 프로티)에 들어있는 프로퍼티
  6. ServletConfig 파라미터
  7. ServletContext 파라미터
  8. java:comp/env JNDI 애트리뷰트
  9. System.getProperties() 자바 시스템 프로퍼티
  10. OS 환경 변수
  11. RandomValuePropertySource
  12. JAR 밖에 있는 특정 프로파일용 application properties
  13. JAR 안에 있는 특정 프로파일용 application properties
  14. JAR 밖에 있는 application properties
  15. `JAR 안에 있는 application properties`(기본)
  16. @PropertySource
  17. 기본 프로퍼티(SpringApplication.setDefaultProperties)
* 기본 프로퍼티에 외부 파일 추가하기
  * `classpath`를 `@TestPropertySource` 안에 locations 속성에 값을 넣어줘서 연결
* 프로퍼티 설정 파일 간의 우선 순위
  1. file:./config/: .은 프로젝트 홈 디렉토리
  2. file:./
  3. classpath:/config/: /는 프로젝트 src 안의 `resource` 디렉토리
  4. classpath:/

### 외부 설정 고급
* 프로퍼티 묶음 읽기 및 빈으로 등록 및 
  * 클래스를 정의하고 getter와 setter를 정의하고 그 클래스에 애노테이션으로 `@ConfigurationProperties("{속성}")`으로 프로퍼티를 묶어서 가져와서 `@Component`로 빈으로 등록 후 활용 가능
  * 케밥, 언더스코어, 캐멀케이스 모두 바인딩을 해줌
  * 값을 불러올 때 기본적인 타입 컨버전이 됨
  * `@Validated` 어노테이션으로 의존성을 주입해주면 `@NotNull`이나 `@Size`같은 검증을 할 수 있음
* 프로파일
  * `@Profile` 애노테이션을 통해서 특정 프로파일에만 빈을 사용할 수 있게 할 수 있음
  * 프로퍼티 파일 내에서 `spring.profiles.active`를 통해서 설정해서 특정 프로파일에만 활성화 시킬 수 있음
  * 프로퍼티 파일 내에서 `spring.profiles.include`를 통해서 설정해서 다른 프로파일을 추가할 수 있음
  * `application-{profile}.properties`방식으로 프로퍼티 파일을 더 추가할 수 만들 수 있음
* 로깅
  * slf4j: 로깅 퍼사드, 로거 API를 추상화한 인터페이스로 로거에 관계없이 애플리케이션 개발이 가능
  * log4j2, Logback: 로거, 로그를 찍는 라이브러리
* 로거 설정
  * 프로퍼티 파일 내에서 `spring.output.ansi.enable`를 통해서 컬러 출력을 지정할 수 있음
  * 프로퍼티 파일 내에서 `logging.file`이나 `logging.path`를 통해서 로그 파일들거나 로그 디렉토리를 지정할 수 있음
  * 프로퍼티 파일 내에서 `logging.level`을 통해서 로그 레벨을 지정할 수 있음
* 로깅 커스터마이징
  * `logback-spring.xml`이나 `log4j2-spring.xml`의 설정 파일을 통해서 로깅 설정을 full로 커스터마이징 할 수 있음
  * pom.xml에서 Logback을 exclusion하고 log4j2 의존성을 추가해서 log4j2로 설정을 변경할 수 있음

### 스프링 테스트


### 스프링 웹 MVC 기본
* `HttpMesssageConverter`
  * HTTP 요청 본문을 객체로 변경하거나, 객체를 HTTP 응답 본문으로 변경할 때 사용
  * `@RequestBody`(파라미터)나 `@ResponseBody`(리턴 값)와 같이 사용됨
  * `@Restcontroller`를 사용할 경우 `@RequestBody` 생략 가능
* `ViewReslove`
  * 뷰 이름으로부터 사용할 뷰 오브젝트를 매핑하는 객체
  * 들어오는 요청의 `accept header`에 따라 사용자가 원하는 뷰를 얻을 수 있으므로 그에 따라 응답을 다르게 할 수 있음
  * `accept header`에 정보를 담아주지 않을 경우 format이라는 경로 매개변수로 알 수 있음

### 스프링 웹 MVC 정적 리소스
  * 클라이언트의 요청에 이미 만들어져있는 리소스를 지원하는 것으로 기본적으로 URL의 루트에 매핑되어 있음
  * 기본 리소스 위치(/**)
    * classpath:/static
    * classpath:/public
    * classpath:/resources/
    * classpath:/META-INF/resources
  * 기본 리소스 위치 변경
    * spring.mvc.static-path-pattern: url 맵핑 경로 설정 변경 가능
    * spring.mvc.static-locations: 리소스 찾을 위치 변경 가능
* 웹 jar
  * css나 자바스크립트 라이브러리를 jar 파일로 추가한 것으로 메이븐으로 정적인 프론트 라이브러리를 추가할 수 있게 함
* index 페이지
  * 루트 url에 대한 웰컴 페이지는 `index.html`이 있으면 이를 제공
* 파비콘
  * 파비콘은 웹 페이지 옆에 아이콘을 의미
  * 리소스 디렉토리에 파비콘 아이콘을 두면 이를 제공

### 스프링 웹 MVC 동적 리소스
* Thymeleaf
  * 자바 템플릿 엔진으로 동적으로 컨텐츠를 만들 수 있어 주로 뷰를 만드는데 사용
  * JSP가 기존의 유행하는 템플릿 엔진은 JAR로 패키징할 때 동작하지 않고 WAR로 패키징해야함
  * `spring-boot-starter-thymeleaf`를 통해서 의존성 추가하고 html 파일에 `<html xmlns:th="http://www.thymeleaf.org">`을 삽입
  * `@Restcontroller`가 아닌 `@Controller` 애노테이션과 ui의 패키지의 `model`에 속성 이름과 값을 넣고 `/src/main/resources/template/`에 존재하는 템플릿 이름을 리턴하면 템플릿에 값을 넘겨줄 수 있음
  * 템플릿 파일에서는 속성 이름으로 `th:text="${속성이름}"`과 같은 방식으로 뷰를 만들 수 있음
* HtmlUnit
  * 템플릿 뷰를 전문적으로 테스트 할 수 있는 라이브러리

### 스프링 웹 MVC Exception Handling
* 스프링 @MVC 예외 처리 방법
  * @ExceptionHandler({예외클래스}.class) 애노테이션을 통해 해당 예외클래스에 대한 핸들러 함수를 매핑시킬 수 있음
* ErrorViewResolver의 구현을 통해서 커스터마이징 가능

### 스프링 데이터
* DataSource로 SQL(H2, MySQL, PostgreSQL) 이용하기
  * H2(인 메모리 디비), MySQL, PostgreSQL에 해당하는 라이브러리는 각각 pox.xml에 정의해야줘야 사용이 가능
  * 스프링은 아무런 디비 설정을 하지 않으면 인메모리 데이터베이스를 기본으로 데이터 소스로 설정
  * `DataSource` 빈을 의존성 주입 받고 이를 통해서 getConnection() 함수를 호출하면 connection을 얻을 수 있음 
  * connection을 통해서 createStatement() 함수를 호출하면 DataSrouce에 실행할 수 있는 statement를 얻을 수 있고 이게 해당하는 sql을 인자로 넘겨 statement.execUpdate(sql)을 호출하면 쿼리가 실행 가능
* Jdbc로 SQL(H2, MySQL, PostgreSQL) 이용하기
  `JdbcTemplate` 빈을 의존성 주입 받고 이를 통해서 excute() 함수를 호출하면 간단하게 sql 쿼리가 실행 가능
* DBCP
  * DBCP는 디비 커넥션 풀의 약자로, 디비와 커넥션을 만드는 과정이 비용이 크기 때문에 미리 커넥션을 만들어 둘 수 있는 기능
  * 스프링은 Hikari를 기본으로 사용
  * 프로퍼티 파일에서 DBCP에 관련 설정을 할 수 있음
* 스프링 데이터 JPA
  * ORM: 객체와 릴레이션을 맵핑할 때 발생하는 개념적 불일치를 해결하는 프레임워크
  * Spring Data JPA: 스프링에서 JPA를 쉽게 사용하기 위해 만든 모듈
  * JPA: Java Persistence API의 약자로, 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스 표준
  * Hibernate: JPA의 구현체 표준
  * Jdbc: Hibernate를 구현할 때 쓰는 자바 디비 라이브러리
  * Spring Data JPA -> JPA -> Hibernate -> Jdbd
* 스프링 데이터 JPA 이용
  * 모델 클래스에 `@Enity`를 통해서 매핑을 하고 `@Id`와 `@GeneratedValue`를 통해서 id라는 필드를 설정
  * 레포지토리를 인터페이스로 만들고 `JpaRepository< {모델}, {모델 Id의 자료형}>`를 extends하여 설정
  * 예를 들어, username이라는 필드가 있는 경우 FindByUsername 메소드를 레포지토리에 등록만 하여도 실제 구현체를 Spring Data JPA가 충족시켜줌
  * 레포지토리에 등록하는 메소드 위에 `@Query({JPQL})`을 통해서 jpql을 통해서 쿼리를 메소드와 매핑시킬 수도 있음
  * 레포지토리에 등록하는 메소드 위에 `@Query(nativeQuery=true, value={실제 SQL 쿼리})`를 통해서 실행하고 싶은 네이티브 SQL을 메소드와 매핑시킬 수도 있음
  * 레포지토리에 등록된 find 관련 메소드는 Optional을 리턴하게 할 수도 있는데, 이를 통해 결과가 비어 있는지 확인하고 사용할 수 있음
* 스프링 데이터 JPA 초기화
  * `spring.jpa.hibernate.ddl-auto=update`, `spring.jpa.hibernate.ddl-auto=create`, `spring.jpa.hibernate.ddl-auto=create-drop`, `spring.jpa.generate-dll=true`를 통해서 Entity를 보고 자동으로 스키마가 생성되도록 할 수 있음
  * schema.sql, schema-{$platform}.sql 이나 data.sql, data-{$platform}.sql을 통해서 SQL 스크립트로 초기화할 수 있고 ${platform} 값은 spring.datasource.platform 으로 설정 가능
* 스프링 데이터 JPA 마이그레이션
  * 스프링 마이그레이션 툴로는 `Flyway`와 `Liquibase`이 존재
  * 툴과 관련된 의존성을 추가하면 사용이 가능
  * db/migration 또는 db/migration/{vendor}로 마이그레이션 sql 파일 디렉토리르 설정할 수 있고 spring.flyway.locations로 변경 가능
  * 마이그레이션 파일 컨벤션
    * V숫자__이름.sql
    * V는 꼭 대문자로
    * 숫자는 순차적으로 (타임스탬프 권장)
    * 숫자와 이름 사이에 언더바 두 개
    * 이름은 가능한 서술적으로

### 스프링 시큐티리
* 

### REST Client
* REST Client
  * `RestTemplate`: Blocking I/O 기반의 동기적 API 클라이언트로 spring-web 모듈을 통해서 빌더가 빈으로 등록되어 있음
  * `WebClient`: Non-Blocking I/O 기반의 비동기적 API클라이언트로 spring-webflux 모듈을 통해서 빌더가 빈으로 등록되어 있음
* REST Clinet 커스터마이징
  * 로컬 커스터마이징은 지역 변수로 빌더를 만들 때 빌더에 추가 함수를 호출해서 지정할 수 있음
  * 글로벌 커스터마이징은 빈으로 클라이언트의 종류에 맞는 Customizer를 빈으로 등록하고 customize 함수를 오버라이딩하면서 내부에서 빌더에 추가 함수를 호출해서 지정할 수 있음

Spring Boot Operation
=======
### Acuator

### 보충할 것
* Spring Boot Principle - 내장 웹 서버 응용
* Spring Boot Utilization - 테스트
* Spring Boot Utilization - Web Mvc HATEOAS / CORS
* Spring Boot Operation