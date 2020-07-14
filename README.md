# Spring Boot 정리 자료
인프런 백기선 강사님 스프링 부트 개념과 활용 정리 문서

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
* 스프링 부트 프로젝트의 구조
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


Spring Boot Utilization
=======


Spring Boot Operation
=======

### 보충할 것
* Spring Boot Principle
