### Spring boot 소개
- 최소한의 스프링 설정으로 스프링 어플리케이션을 만들 수 있음.
- XML 설정을 위한 코드나 필수 요건 없이 쉽고 빠르게 스프링 어플리케이션을 개발할 수 있음.

### 시스템 요건
- java8 ~ java 18
- spring framework 5.3.22 ~ 이상
- Maven 3.5+
- Gradle 6.8.x, 6.9.x, and 7.x

### Servlet Containers
- 서블릿 컨테이너가 내장되어 있음

|  Name | Servlet Version|
|---|----------------|
|  Tomcat 9.0 | 4.0            |
|Jetty 9.4   | 3.1            |

### Installation

- 표준 자바 라이브러리처럼 **spring-boot-*.jar* 패턴에 일치하는 파일이 classpath 경로에있어야 한다.
- 의존성 관리를 지원하는 빌트 툴 사용을 권한다.

### Maven Installation
- 스프링 부트는 아파치 메이븐 3.3 이상 버전과 호환성이 있음.
- 스프링 부트 의존성은 *org.springframework.boot groupId* 을 이용한다. 
- 메이븐 POM 파일은 [spring-boot-starter-parent 프로젝트](https://www.baeldung.com/spring-boot-starter-parent) 를 상속한다..

### Gradle Installation
- 스프링 부트는 Gradle 6.8, 6.9, and 7.x. 버전과 호환성이 있음.
- 스프링 부트 의존성은 *org.springframework.boot group* 을 이용한다. 

스프링 부트는 Gradle, maven 모두 의존성 선언 플러그인을 제공한다.

### Installing the Spring Boot CLI
1. 수동
2. SDKMAN
3. Homebrew
4. MacPorts
