# 6. 스프링부트로 개발하기

이 섹션에서는 Spring Boot를 사용하는 방법에 대해 자세히 설명합니다. 빌드 시스템, 자동 구성 및 애플리케이션 실행 방법과 같은 주제를 다룹니다. 또한 몇 가지 Spring Boot 모범 사례도 다룹니다.
Spring Boot에 대해 특별히 특별한 것은 없지만(사용할 수 있는 또 다른 라이브러리일 뿐임) 따를 때 개발 프로세스를 좀 더 쉽게 만들어 주는 몇 가지 권장 사항이 있습니다.

Spring Boot를 시작하는 경우 이 섹션을 시작하기 전에 시작하기 가이드를 읽어야 합니다.

## 6.1 빌드 시스템들

종속성 관리를 지원하고 "Maven Central" 리포지토리에 게시된 아티팩트를 사용할 수 있는 빌드 시스템을 선택하는 것이 좋습니다.
Maven 또는 Gradle을 선택하는 것이 좋습니다. Spring Boot가 다른 빌드 시스템(예: Ant)과 함께 작동하도록 할 수 있지만 특별히 잘 지원되지는 않습니다.

### 6.1.1 종속성 관리

Spring Boot의 각 릴리스는 지원하는 선별된 종속성 목록을 제공합니다. 실제로 빌드 구성에서 이러한 종속성에 대한 버전을 제공할 필요가 없습니다. Spring Boot가 이를 관리하기 때문입니다.
Spring Boot 자체를 업그레이드하면 이러한 종속성도 일관된 방식으로 업그레이드됩니다.

> Note
>
> 필요한 경우 여전히 버전을 지정하고 Spring Boot의 권장 사항을 재정의할 수 있습니다.

선별된 목록에는 Spring Boot와 함께 사용할 수 있는 모든 Spring 모듈과 정제된 타사 라이브러리 목록이 포함되어 있습니다.
이 목록은 Maven 및 Gradle과 함께 사용할 수 있는 표준 BOM(`spring-boot-dependencies`)으로 제공됩니다.

> 경고
>
> Spring Boot의 각 릴리스는 Spring Framework의 기본 버전과 연결됩니다. 해당 버전을 지정하지 않는 것이 좋습니다.

### 6.1.2 Maven

Maven과 함께 Spring Boot를 사용하는 방법에 대해 알아보려면 Spring Boot의 Maven 플러그인에 대한 설명서를 참조하십시오.

- Reference (HTML and PDF)
- API

### 6.1.3. Gradle

Gradle과 함께 Spring Boot를 사용하는 방법에 대해 알아보려면 Spring Boot의 Gradle 플러그인 설명서를 참조하세요.

- Reference (HTML and PDF)
- API

### 6.1.4 Ant

Apache Ant+Ivy를 사용하여 Spring Boot 프로젝트를 빌드할 수 있습니다. `spring-boot-antlib` "AntLib" 모듈은 Ant가 실행 가능한 jar를 생성하는 데 도움이 되도록 사용할
수도 있습니다.

종속성을 선언하기 위한 일반적인 ivy.xml 파일은 다음 예제와 유사합니다.

```xml

<ivy-module version="2.0">
    <info organisation="org.springframework.boot" module="spring-boot-sample-ant"/>
    <configurations>
        <conf name="compile" description="everything needed to compile this module"/>
        <conf name="runtime" extends="compile" description="everything needed to run this module"/>
    </configurations>
    <dependencies>
        <dependency org="org.springframework.boot" name="spring-boot-starter"
                    rev="${spring-boot.version}" conf="compile"/>
    </dependencies>
</ivy-module>
```

일반적인 build.xml은 다음 예제와 같습니다.

```xml

<project
        xmlns:ivy="antlib:org.apache.ivy.ant"
        xmlns:spring-boot="antlib:org.springframework.boot.ant"
        name="myapp" default="build">

    <property name="spring-boot.version" value="2.6.11-SNAPSHOT"/>

    <target name="resolve" description="--> retrieve dependencies with ivy">
        <ivy:retrieve pattern="lib/[conf]/[artifact]-[type]-[revision].[ext]"/>
    </target>

    <target name="classpaths" depends="resolve">
        <path id="compile.classpath">
            <fileset dir="lib/compile" includes="*.jar"/>
        </path>
    </target>

    <target name="init" depends="classpaths">
        <mkdir dir="build/classes"/>
    </target>

    <target name="compile" depends="init" description="compile">
        <javac srcdir="src/main/java" destdir="build/classes" classpathref="compile.classpath"/>
    </target>

    <target name="build" depends="compile">
        <spring-boot:exejar destfile="build/myapp.jar" classes="build/classes">
            <spring-boot:lib>
                <fileset dir="lib/runtime"/>
            </spring-boot:lib>
        </spring-boot:exejar>
    </target>
</project>
```

> Tip
>
> spring-boot-antlib 모듈을 사용하지 않으려면 `build an Executable Archive from Ant without Using spring-boot-antlib` "How-to"를
> 참조하십시오.

### 6.1.5. Starters

스타터는 애플리케이션에 포함할 수 있는 편리한 종속성 설명자 집합입니다.
샘플 코드를 검색하고 많은 종속성 설명자를 복사하여 붙여넣지 않고도 필요한 모든 Spring 및 관련 기술에 대한 원스톱 상점을 얻을 수 있습니다.
예를 들어 데이터베이스 액세스를 위해 Spring 및 JPA를 사용하여 시작하려는 경우 프로젝트에 `spring-boot-starter-data-jpa` 종속성을 포함합니다.

스타터에는 일관되고 지원되는 관리 전이적 종속성 세트를 사용하여 프로젝트를 신속하게 시작하고 실행하는 데 필요한 많은 종속성이 포함되어 있습니다.

> Starters 이름의 뜻은?
>
> 모든 공식 스타터는 유사한 이름 지정 패턴을 따릅니다. `spring-boot-starter-*`, 여기서 *는 특정 유형의 애플리케이션입니다.
> 이 명명 구조는 스타터를 찾아야 할 때 도움을 주기 위한 것입니다. 많은 IDE의 Maven 통합을 통해 종속성을 이름으로 검색할 수 있습니다.
> 예를 들어 적절한 Eclipse 또는 Spring Tools 플러그인이 설치된 경우 POM 편집기에서 ctrl-space를 누르고 "`spring-boot-starter`"를 입력하면 전체 목록을 볼 수
> 있습니다.
>
> "자신만의 스타터 만들기" 섹션에서 설명한 것처럼 타사 스타터는 공식 Spring Boot 아티팩트용으로 예약되어 있으므로 spring-boot로 시작하면 안 됩니다. 오히려 타사 스타터는 일반적으로 프로젝트
> 이름으로 시작합니다.
> 예를 들어 thirdpartyproject라는 타사 스타터 프로젝트는 일반적으로 thirdpartyproject-spring-boot-starter로 이름이 지정됩니다.

다음 애플리케이션 스타터는 `org.springframework.boot` 그룹 아래의 Spring Boot에서 제공됩니다.

표 1. Spring Boot 애플리케이션 스타터

| 이름                           | 설명                                                                                                          |
|------------------------------|-------------------------------------------------------------------------------------------------------------|
| spring-boot-starter          | 자동 구성 지원, 로깅 및 YAML을 포함한 핵심 스타터                                                                             |
| spring-boot-starter-activemq | Apache ActiveMQ를 사용하는 JMS 메시징용 스타터                                                                          |
| spring-boot-starter-amqp     | Spring AMQP 및 Rabbit MQ 사용을 위한 스타터                                                                          |
| spring-boot-starter-aop | Spring AOP 및 AspectJ를 사용한 관점 지향 프로그래밍을 위한 스타터                                                               |
| spring-boot-starter-artemis | Apache Artemis를 사용하는 JMS 메시징용 스타터                                                                           |
| spring-boot-starter-batch | Spring Batch 사용을 위한 스타터                                                                                     |
| spring-boot-starter-cache | Spring Framework의 캐싱 지원을 사용하기 위한 스타터                                                                        |
| spring-boot-starter-data-cassandra | Cassandra 분산 데이터베이스 및 Spring Data Cassandra 사용을 위한 스타터                                                      |
| spring-boot-starter-data-cassandra-reactive | Cassandra 분산 데이터베이스 및 Spring Data Cassandra Reactive 사용을 위한 스타터                                             |
| spring-boot-starter-data-couchbase | Couchbase 문서 지향 데이터베이스 및 Spring Data Couchbase 사용을 위한 스타터                                                   |
| spring-boot-starter-data-couchbase-reactive | Couchbase 문서 지향 데이터베이스 및 Spring Data Couchbase Reactive 사용을 위한 스타터                                          |
| spring-boot-starter-data-elasticsearch | Elasticsearch 검색 및 분석 엔진과 Spring Data Elasticsearch를 사용하기 위한 스타터                                            |
| spring-boot-starter-data-jdbc | Spring Data JDBC를 사용하기 위한 스타터                                                                               |
| spring-boot-starter-data-jpa | Hibernate와 함께 Spring Data JPA를 사용하기 위한 스타터                                                                  |
| spring-boot-starter-data-ldap | Spring Data LDAP 사용을 위한 스타터                                                                                 |
| spring-boot-starter-data-mongodb | MongoDB 문서 지향 데이터베이스 및 Spring Data MongoDB 사용을 위한 스타터                                                       |
| spring-boot-starter-data-mongodb-reactive | MongoDB 문서 지향 데이터베이스 및 Spring Data MongoDB Reactive 사용을 위한 스타터                                              |
| spring-boot-starter-data-neo4j | Neo4j 그래프 데이터베이스 및 Spring Data Neo4j 사용을 위한 스타터                                                             |
| spring-boot-starter-data-r2dbc | Spring Data R2DBC를 사용하기 위한 스타터                                                                              |
| spring-boot-starter-data-redis | Spring Data Redis 및 Lettuce 클라이언트와 함께 Redis 키-값 데이터 저장소를 사용하기 위한 스타터                                        |
| spring-boot-starter-data-redis-reactive | Spring Data Redis 반응성 및 Lettuce 클라이언트와 함께 Redis 키-값 데이터 저장소를 사용하기 위한 스타터                                    |
| spring-boot-starter-data-rest | Spring Data REST를 사용하여 REST를 통해 Spring Data 리포지토리를 노출하기 위한 스타터                                              |
| spring-boot-starter-freemarker | FreeMarker 보기를 사용하여 MVC 웹 애플리케이션 구축을 위한 스타터                                                                 |
| spring-boot-starter-groovy-templates | Groovy 템플릿 뷰를 사용하여 MVC 웹 애플리케이션을 구축하기 위한 스타터                                                                |
| spring-boot-starter-hateoas | Spring MVC 및 Spring HATEOAS로 하이퍼미디어 기반 RESTful 웹 애플리케이션 구축을 위한 스타터                                          |
| spring-boot-starter-integration | Spring 통합 사용을 위한 스타터                                                                                        |
| spring-boot-starter-jdbc | HikariCP 연결 풀과 함께 JDBC를 사용하기 위한 스타터                                                                         |
| spring-boot-starter-jersey | JAX-RS 및 Jersey를 사용하여 RESTful 웹 애플리케이션을 구축하기 위한 스타터입니다. 대안`spring-boot-starter-web`                         |
| spring-boot-starter-jooq | jOOQ를 사용하여 JDBC로 SQL 데이터베이스에 액세스하기 위한 스타터입니다. 대안 `spring-boot-starter-data-jpa`또는`spring-boot-starter-jdbc` |
| spring-boot-starter-json | json 읽기 및 쓰기 스타터                                                                                            |
| spring-boot-starter-jta-atomikos |Atomikos를 사용하는 JTA 트랜잭션의 스타터|
| spring-boot-starter-mail |Java Mail 및 Spring Framework의 이메일 전송 지원을 사용하기 위한 스타터|
| spring-boot-starter-mustache |Mustache 뷰를 사용하여 웹 애플리케이션을 구축하기 위한 스타터|
| spring-boot-starter-oauth2-client |Spring Security의 OAuth2/OpenID Connect 클라이언트 기능을 사용하기 위한 스타터|
| spring-boot-starter-oauth2-resource-server |Spring Security의 OAuth2 리소스 서버 기능을 사용하기 위한 스타터|
| spring-boot-starter-quartz |Quartz 스케줄러 사용을 위한 스타터|
| spring-boot-starter-rsocket |RSocket 클라이언트 및 서버 구축을 위한 스타터|
| spring-boot-starter-security |Spring Security 사용을 위한 스타터|
| spring-boot-starter-test |JUnit Jupiter, Hamcrest 및 Mockito를 포함한 라이브러리로 Spring Boot 애플리케이션을 테스트하기 위한 스타터|
| spring-boot-starter-thymeleaf |Thymeleaf 뷰를 사용하여 MVC 웹 애플리케이션 구축을 위한 스타터|
| spring-boot-starter-validation |Hibernate Validator와 함께 Java Bean Validation을 사용하기 위한 스타터|
| spring-boot-starter-web |RESTful을 포함한 웹, Spring MVC를 사용하는 애플리케이션 구축을 위한 스타터. Tomcat을 기본 내장 컨테이너로 사용|
| spring-boot-starter-web-services |Spring Web Services 사용을 위한 스타터|
| spring-boot-starter-webflux |Spring Framework의 Reactive Web 지원을 사용하여 WebFlux 애플리케이션을 구축하기 위한 스타터|
| spring-boot-starter-websocket |Spring Framework의 WebSocket 지원을 사용하여 WebSocket 애플리케이션을 구축하기 위한 스타터|

애플리케이션 스타터 외에도 다음 스타터를 사용하여 프로덕션 준비 기능을 추가할 수 있습니다.

표 2. Spring Boot 생산 스타터

| 이름                           | 설명                                                                                            |
|------------------------------|-----------------------------------------------------------------------------------------------|
| spring-boot-starter-actuator         | 애플리케이션을 모니터링하고 관리하는 데 도움이 되는 프로덕션 준비 기능을 제공하는 Spring Boot의 Actuator를 사용하기 위한 스타터 |

마지막으로 Spring Boot에는 특정 기술 측면을 제외하거나 교체하려는 경우 사용할 수 있는 다음 스타터도 포함되어 있습니다.

표 3. Spring Boot 기술 스타터

| 이름                        | 설명                                                                                      |
|---------------------------|-----------------------------------------------------------------------------------------|
| spring-boot-starter-jetty | 포함된 서블릿 컨테이너로 Jetty를 사용하기 위한 스타터입니다. 대안 `spring-boot-starter-tomcat` |
| spring-boot-starter-log4j2| 로깅을 위해 Log4j2를 사용하기 위한 스타터. 대안 `spring-boot-starter-logging` |
| spring-boot-starter-logging | Logback을 사용하여 로깅하기 위한 스타터입니다. 기본 로깅 스타터 |
| spring-boot-starter-reactor-netty | 내장된 반응형 HTTP 서버로 Reactor Netty를 사용하기 위한 스타터입니다. |
| spring-boot-starter-tomcat | 포함된 서블릿 컨테이너로 Tomcat을 사용하기 위한 스타터입니다. 다음에서 사용하는 기본 서블릿 컨테이너 스타터 `spring-boot-starter-web` |
| spring-boot-starter-undertow | 내장된 서블릿 컨테이너로 Undertow를 사용하기 위한 스타터. 대안 `spring-boot-starter-tomcat` |

기술적 측면을 교환하는 방법에 대해 알아보려면 웹 서버 및 로깅 시스템 교환 방법 설명서를 참조하십시오.

> 팁
> 
> 추가 커뮤니티 기여 스타터 목록은 GitHub 의 모듈에 있는 README 파일 을 참조하십시오. `spring-boot-starters`

## 6.2 코드 구조화
Spring Boot는 작동하는 데 특정 코드 레이아웃이 필요하지 않습니다. 그러나 도움이 되는 몇 가지 모범 사례가 있습니다

### 6.2.1. 디폴트 패키지 사용
클래스가 패키지 선언을 포함하지 않으면 "디폴트 패키지"에 있는 것으로 간주됩니다. "디폴트 패키지"의 사용은 일반적으로 권장되지 않으며 피해야 합니다.
모든 jar의 모든 클래스를 읽기 때문에 @ComponentScan, @ConfigurationPropertiesScan, @EntityScan 또는 @SpringBootApplication 어노테이션을 사용하는 Spring Boot 애플리케이션에 특정 문제가 발생할 수 있습니다.

> 팁
> 
> Java의 권장 패키지 명명 규칙을 따르고 반전된 도메인 이름(예: com.example.project)을 사용하는 것이 좋습니다.

### 기본 애플리케이션 클래스 찾기
일반적으로 다른 클래스 위의 루트 패키지에서 기본 애플리케이션 클래스를 찾을 것을 권장합니다.
@SpringBootApplication 어노테이션은 종종 기본 클래스에 배치되며 특정 항목에 대한 기본 "검색 패키지"를 암시적으로 정의합니다.
예를 들어 JPA 애플리케이션을 작성하는 경우 @SpringBootApplication 어노테이션 클래스의 패키지를 사용하여 @Entity 항목을 검색합니다. 
루트 패키지를 사용하면 구성 요소 스캔을 프로젝트에만 적용할 수도 있습니다

> 팁
> 
> @SpringBootApplication을 사용하지 않으려면 가져오는 @EnableAutoConfiguration 및 @ComponentScan 어노테이션이 해당 동작을 정의하므로 대신 사용할 수도 있습니다.

다음 목록은 일반적인 레이아웃을 보여줍니다.

```text
com
 +- example
     +- myapplication
         +- MyApplication.java
         |
         +- customer
         |   +- Customer.java
         |   +- CustomerController.java
         |   +- CustomerService.java
         |   +- CustomerRepository.java
         |
         +- order
             +- Order.java
             +- OrderController.java
             +- OrderService.java
             +- OrderRepository.java
```

MyApplication.java 파일은 다음과 같이 기본 @SpringBootApplication과 함께 기본 메서드를 선언합니다

```java
@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

}
```

## 6.3. Configuration 클래스
Spring Boot는 Java 기반 구성을 선호합니다. XML 소스와 함께 SpringApplication을 사용하는 것이 가능하지만 일반적으로 기본 소스가 단일 @Configuration 클래스인 것을 권장합니다.

> 팁
> 
> XML 구성을 사용하는 많은 Spring 구성 예제가 인터넷에 게시되었습니다. 가능하면 항상 동등한 Java 기반 구성을 사용하십시오. Enable* 어노테이션을 검색하는 것이 좋은 출발점이 될 수 있습니다

### 6.3.1 추가 Configuration 클래스 가져오기
모든 @Configuration을 단일 클래스에 넣을 필요는 없습니다. @Import 어노테이션을 사용하여 추가 구성 클래스를 가져올 수 있습니다.
또는 @ComponentScan을 사용하여 @Configuration 클래스를 포함한 모든 Spring 구성 요소를 자동으로 선택할 수 있습니다.

### 6.3.2 XML Configuration 가져오기
XML 기반 구성을 반드시 사용해야 하는 경우 여전히 @Configuration 클래스로 시작하는 것이 좋습니다. 그런 다음 @ImportResource 어노테이션을 사용하여 XML 구성 파일을 로드할 수 있습니다.

## 6.4. Auto-configuration
Spring Boot Auto-configuration은 추가한 jar 디펜던시를 기반으로 Spring 애플리케이션을 자동으로 구성하려고 시도합니다.
예를 들어 `HSQLDB`가 클래스 경로에 있고 데이터베이스 연결 Bean을 수동으로 구성하지 않은 경우 Spring Boot는 메모리 내 데이터베이스를 자동 구성합니다.

@Configuration 클래스 중 하나에 @EnableAutoConfiguration 또는 @SpringBootApplication 어노테이션을 추가하여 자동 구성을 선택해야 합니다.

> 팁
> 
> @SpringBootApplication 또는 @EnableAutoConfiguration 어노테이션을 하나만 추가해야 합니다. 일반적으로 기본 @Configuration 클래스에만 둘 중 하나를 추가하는 것이 좋습니다.

### 6.4.1 점진적으로 Auto-configuration 교체
Auto-configuration은 비침투적입니다. 언제든지 자체 구성 정의를 시작하여 자동 구성의 특정 부분을 교체할 수 있습니다. 예를 들어, 자신의 DataSource bean을 추가하면 기본 임베디드 데이터베이스 지원이 뒤로 물러납니다.

현재 적용 중인 자동 구성 하는 것과 그 이유를 알아야 하는 경우 --debug 스위치를 사용하여 애플리케이션을 시작하십시오. 이렇게 하면 코어 로거 선택에 대한 디버그 로그가 활성화되고 조건 보고서가 콘솔에 기록됩니다.

### 6.4.2 특정 Auto-configuration 클래스 비활성화
원하지 않는 특정 자동 구성 클래스가 적용되고 있는 경우 다음 예제와 같이 @SpringBootApplication의 exclude 특성을 사용하여 비활성화할 수 있습니다

```java
@SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })
public class MyApplication {

}
```
클래스가 클래스 경로에 없으면 어노테이션의 excludeName 속성을 사용하고 대신 정규화된 이름을 지정할 수 있습니다.
@SpringBootApplication 대신 @EnableAutoConfiguration을 사용하려는 경우 exclude 및 excludeName도 사용할 수 있습니다.
마지막으로 `spring.autoconfigure.exclude` 속성을 사용하여 제외할 Auto-configuration 클래스 목록을 제어할 수도 있습니다.

> TIP
> 
> 어노테이션 수준과 속성을 사용하여 제외를 정의할 수 있습니다

> Note
> 
> `auto-configuration` 클래스가 public 이더라도 public API로 간주되는 클래스의 유일한 측면은 `auto-configuration`을 비활성화하는 데 사용할 수 있는 클래스의 이름입니다.
> 중첩된 구성 클래스 또는 Bean 메소드와 같은 해당 클래스의 실제 컨텐츠는 내부 전용이며 직접 사용하지 않는 것이 좋습니다.

## 6.5 스프링 빈과 의존성 주입
표준 Spring Framework 기술을 자유롭게 사용하여 빈과 삽입된 종속성을 정의할 수 있습니다. 일반적으로 생성자 주입을 사용하여 종속성을 연결하고 @ComponentScan을 사용하여 빈을 찾는 것이 좋습니다.
위에서 제안한 대로 코드를 구성하는 경우(최상위 패키지에서 애플리케이션 클래스 찾기) arguments 없이 @ComponentScan을 추가하거나 이를 암시적으로 포함하는 @SpringBootApplication 주석을 사용할 수 있습니다. 모든 애플리케이션 구성 요소(@Component, @Service, @Repository, @Controller 등)는 자동으로 Spring Beans로 등록됩니다.

다음 예제는 필요한 RiskAssessor bean을 얻기 위해 생성자 주입을 사용하는 @Service Bean을 보여줍니다.

```java
@Service
public class MyAccountService implements AccountService {

    private final RiskAssessor riskAssessor;

    public MyAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
    }

    // ...

}
```

bean이 하나 이상의 생성자를 가지고 있다면 Spring이 @Autowired와 함께 사용하기를 원하는 생성자를 표시해야 합니다.

```java
@Service
public class MyAccountService implements AccountService {

    private final RiskAssessor riskAssessor;

    private final PrintStream out;

    @Autowired
    public MyAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
        this.out = System.out;
    }

    public MyAccountService(RiskAssessor riskAssessor, PrintStream out) {
        this.riskAssessor = riskAssessor;
        this.out = out;
    }

    // ...

}
```

> Tip
> 
> 생성자 주입을 사용하면 riskAssessor 필드가 최종으로 표시되어 나중에 변경할 수 없음을 나타내는 방법에 유의하십시오.

## 6.6 @SpringBootApplication 어노테이션 사용
많은 Spring Boot 개발자는 앱이 자동 구성, 구성 요소 스캔을 사용하고 "애플리케이션 클래스"에서 추가 구성을 정의할 수 있는 것을 좋아합니다.
단일 @SpringBootApplication 어노테이션을 사용하여 다음과 같은 세 가지 기능을 활성화할 수 있습니다.
- @EnableAutoConfiguration: Spring Boot의 자동 구성 메커니즘을 활성화합니다.
- @ComponentScan: 애플리케이션이 있는 패키지에서 @Component 스캔을 활성화합니다(모범 사례 참조).
- @SpringBootConfiguration: 컨텍스트에서 추가 빈 등록 또는 추가 구성 클래스 가져오기를 활성화합니다. 통합 테스트에서 구성 감지를 지원하는 Spring의 표준 @Configuration에 대한 대안입니다.

```java
// Same as @SpringBootConfiguration @EnableAutoConfiguration @ComponentScan
@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

}
```

> Note
> 
> @SpringBootApplication은 또한 @EnableAutoConfiguration 및 @ComponentScan의 속성을 사용자 지정하기 위한 별칭을 제공합니다.
 
> Note
> 
> 이러한 기능 중 어느 것도 필수 기능이 아니며 활성화하는 기능으로 이 단일 어노테이션을 바꾸도록 선택할 수 있습니다. 예를 들어 애플리케이션에서 구성 요소 스캔 또는 구성 속성 스캔을 사용하고 싶지 않을 수 있습니다.
> ```java
> @SpringBootConfiguration(proxyBeanMethods = false)
> @EnableAutoConfiguration
> @Import({ SomeConfiguration.class, AnotherConfiguration.class })
> public class MyApplication {
>
>    public static void main(String[] args) {
>        SpringApplication.run(MyApplication.class, args);
>    }
> }
> ```
> 이 예제에서 MyApplication은 @Component 어노테이션이 달린 클래스와 @ConfigurationProperties 어노테이션이 달린 클래스가 자동으로 감지되지 않고 사용자 정의 빈을 명시적으로 가져온다는 점을 제외하면 다른 Spring Boot 애플리케이션과 같습니다(@Import 참조).

## 6.7 어플리케이션 실행
애플리케이션을 jar로 패키징하고 임베디드 HTTP 서버를 사용하는 것의 가장 큰 장점 중 하나는 애플리케이션을 다른 것과 마찬가지로 실행할 수 있다는 것입니다.
이 샘플은 Spring Boot 애플리케이션 디버깅에 적용됩니다. 특별한 IDE 플러그인이나 확장이 필요하지 않습니다.

> Note
> 
> 이 섹션에서는 jar 기반 패키징만 다룹니다. 응용 프로그램을 war 파일로 패키징하기로 선택한 경우 서버 및 IDE 설명서를 참조하십시오.

### 6.7.1 IDE에서 실행
IDE에서 Spring Boot 애플리케이션을 Java 애플리케이션으로 실행할 수 있습니다. 그러나 먼저 프로젝트를 가져와야 합니다.
그러나 먼저 프로젝트를 가져와야 합니다. 가져오기 단계는 IDE 및 빌드 시스템에 따라 다릅니다. 대부분의 IDE는 Maven 프로젝트를 직접 가져올 수 있습니다.
예를 들어 Eclipse 사용자는 파일 메뉴에서 가져오기… → 기존 Maven 프로젝트를 선택할 수 있습니다.

프로젝트를 IDE로 직접 가져올 수 없는 경우 빌드 플러그인을 사용하여 IDE 메타데이터를 생성할 수 있습니다. Maven에는 Eclipse 및 IDEA용 플러그인이 포함되어 있습니다. Gradle은 다양한 IDE용 플러그인을 제공합니다.

> Tip
> 
> 실수로 웹 애플리케이션을 두 번 실행하면 "이미 사용 중인 포트" 오류가 표시됩니다. Spring Tools 사용자는 Run 버튼이 아닌 Relaunch 버튼을 사용하여 기존 인스턴스가 닫혔는지 확인할 수 있습니다.

### 6.7.2 패키지 애플리케이션으로 실행
Spring Boot Maven 또는 Gradle 플러그인을 사용하여 실행 가능한 jar를 생성하는 경우 다음 예제와 같이 java -jar를 사용하여 애플리케이션을 실행할 수 있습니다.
```shell
java -jar target/myapplication-0.0.1-SNAPSHOT.jar
```
원격 디버깅 지원이 활성화된 패키지 애플리케이션을 실행할 수도 있습니다. 이렇게 하면 다음 예제와 같이 디버거를 패키징된 애플리케이션에 연결할 수 있습니다.
```shell
java -Xdebug -Xrunjdwp:server=y,transport=dt_socket,address=8000,suspend=n \
       -jar target/myapplication-0.0.1-SNAPSHOT.jar
```

### 6.7.3 메이븐 플러그인 사용
Spring Boot Maven 플러그인에는 애플리케이션을 빠르게 컴파일하고 실행하는 데 사용할 수 있는 실행 목표가 포함되어 있습니다.
응용 프로그램은 IDE에서처럼 분해된 형태로 실행됩니다. 다음 예제는 Spring Boot 애플리케이션을 실행하는 일반적인 Maven 명령을 보여줍니다.
```shell
mvn spring-boot:run
```
다음 예제와 같이 `MAVEN_OPTS` 운영 체제 환경 변수를 사용할 수도 있습니다.
```shell
export MAVEN_OPTS=-Xmx1024m
```
### 6.7.3 Gradle 플러그인 사용
Spring Boot Gradle 플러그인에는 분해된 형식으로 애플리케이션을 실행하는 데 사용할 수 있는 bootRun 작업도 포함되어 있습니다.
bootRun 작업은 org.springframework.boot 및 java 플러그인을 적용할 때마다 추가되며 다음 예제에 표시됩니다.
```shell
gradle bootRun
```
다음 예제와 같이 `JAVA_OPTS` 운영 체제 환경 변수를 사용할 수도 있습니다.
```shell
export JAVA_OPTS=-Xmx1024m
```
### 6.7.5 Hot Swapping
Spring Boot 애플리케이션은 일반 Java 애플리케이션이므로 JVM 핫스왑은 기본적으로 작동해야 합니다. JVM 핫 스와핑은 대체할 수 있는 바이트코드로 다소 제한됩니다. 보다 완벽한 솔루션을 위해 JRebel을 사용할 수 있습니다.

`spring-boot-devtools` 모듈에는 빠른 애플리케이션 재시작에 대한 지원도 포함되어 있습니다. 자세한 내용은 핫 스와핑 "방법"을 참조하십시오.

## 6.8 개발자 툴들
Spring Boot에는 애플리케이션 개발 경험을 좀 더 즐겁게 만들 수 있는 추가 도구 세트가 포함되어 있습니다.
`spring-boot-devtools` 모듈은 추가 개발 시간 기능을 제공하기 위해 모든 프로젝트에 포함될 수 있습니다.
devtools 지원을 포함하려면 Maven 및 Gradle에 대한 다음 목록에 표시된 대로 빌드에 모듈 종속성을 추가하십시오.

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

```gradle
dependencies {
    developmentOnly("org.springframework.boot:spring-boot-devtools")
}
```

> Caution
> 
> Devtools는 특히 다중 모듈 프로젝트에서 클래스 로딩 문제를 일으킬 수 있습니다. 클래스 로딩 문제 진단에서는 문제를 진단하고 해결하는 방법을 설명합니다.

> Note
> 
> 완전히 패키지된 애플리케이션을 실행할 때 개발자 도구는 자동으로 비활성화됩니다. 애플리케이션이 java -jar에서 시작되거나 특수 클래스 로더에서 시작되면 "프로덕션 애플리케이션"으로 간주됩니다.
> `spring.devtools.restart.enabled` 시스템 속성을 사용하여 이 동작을 제어할 수 있습니다.
> 애플리케이션을 시작하는 데 사용된 클래스 로더와 관계없이 devtools를 활성화하려면 `-Dspring.devtools.restart.enabled=true` 시스템 속성을 설정합니다.
> devtools를 실행하는 것이 보안 위험이 있는 프로덕션 환경에서는 이 작업을 수행해서는 안 됩니다. devtools를 비활성화하려면 종속성을 제외하거나 `-Dspring.devtools.restart.enabled=false` 시스템 속성을 설정합니다.

> Tip
>
> Maven에서 선택 사항으로 종속성을 표시하거나 Gradle에서 developmentOnly 구성을 사용하면(위에 표시된 대로) devtools가 프로젝트를 사용하는 다른 모듈에 전이적으로 적용되는 것을 방지할 수 있습니다.

> Tip
> 
> 리패키징된 아카이브에는 기본적으로 devtools가 포함되어 있지 않습니다. 특정 원격 devtools 기능을 사용하려면 포함해야 합니다. Maven 플러그인을 사용하는 경우 excludeDevtools 속성을 false로 설정합니다.
> Gradle 플러그인을 사용하는 경우, developmentOnly 구성을 포함하도록 작업의 클래스 경로를 구성합니다.

### 6.8.1 클래스로딩 문제 진단
restart 대 reload 섹션에서 설명한 대로 restart 기능은 두 개의 클래스 로더를 사용하여 구현됩니다. 대부분의 애플리케이션에서 이 접근 방식은 잘 작동합니다.
그러나 때로는 특히 다중 모듈 프로젝트에서 클래스 로딩 문제가 발생할 수 있습니다.

클래스 로딩 문제가 실제로 devtools와 두 클래스 로더로 인해 발생하는지 진단하려면 재시작을 비활성화하십시오. 이 방법으로 문제가 해결되면 전체 프로젝트를 포함하도록 다시 시작 클래스 로더를 사용자 정의하십시오.

### 6.8.2 속성 기본값
Spring Boot에서 지원하는 여러 라이브러리는 캐시를 사용하여 성능을 향상시킵니다. 예를 들어 템플릿 엔진은 컴파일된 템플릿을 캐시하여 반복적으로 템플릿 파일을 구문 분석하지 않도록 합니다.
또한 Spring MVC는 정적 리소스를 제공할 때 응답에 HTTP 캐싱 헤더를 추가할 수 있습니다.

캐싱은 프로덕션 환경에서 매우 유용하지만 개발 중에는 비생산적일 수 있으므로 방금 애플리케이션에서 변경한 내용을 볼 수 없습니다. 이러한 이유로 `spring-boot-devtools`는 기본적으로 캐싱 옵션을 비활성화합니다.

캐시 옵션은 일반적으로 `application.properties` 파일의 설정으로 구성됩니다. 예를 들어 Thymeleaf는 `spring.thymeleaf.cache` 속성을 제공합니다.이러한 속성을 수동으로 설정할 필요 없이 `spring-boot-devtools` 모듈은 합리적인 개발 시간 구성을 자동으로 적용합니다.

다음 표에는 적용되는 모든 속성이 나열되어 있습니다.

| 이름                                             | 기본값    |
|------------------------------------------------|--------|
| server.error.include-binding-errors            | always |
| server.error.include-message                   | always |
| server.error.include-stacktrace                | always |
| server.servlet.jsp.init-parameters.development | true   |
| server.servlet.session.persistent              | true   |
| spring.freemarker.cache                        | false  |
| spring.groovy.template.cache                   | false  |
| spring.h2.console.enabled                      | true   |
| spring.mustache.cache                          | false  |
| spring.mvc.log-resolved-exception              | true   |
| spring.reactor.debug            | true   |
| spring.template.provider.cache            | false  |
| spring.thymeleaf.cache            | false  |
| spring.web.resources.cache.period            | 0      |
| spring.web.resources.chain.cache            | false  |

> note
> 
> 속성 기본값을 적용하지 않으려면 `application.properties`에서 `spring.devtools.add-properties`를 false로 설정할 수 있습니다.

Spring MVC 및 Spring WebFlux 애플리케이션을 개발하는 동안 웹 요청에 대한 추가 정보가 필요하므로 개발자 도구는 웹 로깅 그룹에 대해 DEBUG 로깅을 활성화하도록 제안합니다.
그러면 들어오는 요청, 처리 중인 처리기, 응답 결과 및 기타 세부 정보에 대한 정보가 제공됩니다.
모든 요청 세부 정보(잠재적으로 민감한 정보 포함)를 기록하려면 `spring.mvc.log-request-details` 또는 `spring.codec.log-request-details` 구성 속성을 켤 수 있습니다.

### 6.8.3. 자동 재실행
spring-boot-devtools를 사용하는 애플리케이션은 클래스 경로의 파일이 변경될 때마다 자동으로 다시 시작됩니다.
이것은 코드 변경에 대한 매우 빠른 피드백 루프를 제공하므로 IDE에서 작업할 때 유용한 기능이 될 수 있습니다.
기본적으로 디렉토리를 가리키는 클래스 경로의 모든 항목은 변경 사항에 대해 모니터링됩니다.
static assets 및 뷰 템플릿과 같은 특정 리소스는 애플리케이션을 다시 시작할 필요가 없습니다.

> 다시 시작 트리거
> 
> DevTools가 클래스 경로 리소스를 모니터링하므로 재시작을 트리거하는 유일한 방법은 클래스 경로를 업데이트하는 것입니다. IDE를 사용하든 빌드 플러그인 중 하나를 사용하든 재시작을 트리거하려면 수정된 파일을 다시 컴파일해야 합니다.
> 클래스 경로를 업데이트하는 방법은 사용 중인 도구에 따라 다릅니다.
> 
> - Eclipse에서 수정된 파일을 저장하면 클래스 경로가 업데이트되고 다시 시작됩니다.
> - IntelliJ IDEA에서 프로젝트 빌드(Build +→+ Build Project)는 동일한 효과를 가집니다.
> - 빌드 플러그인을 사용하는 경우 Maven용 mvn compile 또는 Gradle용 gradle build를 실행하면 다시 시작됩니다.

> Note
> 
> 빌드 플러그인을 사용하여 Maven 또는 Gradle로 다시 시작하는 경우 forking set을 사용으로 설정해야 합니다. 
> forking set를 비활성화하면 devtools에서 사용하는 격리된 애플리케이션 클래스 로더가 생성되지 않고 재시작이 제대로 작동하지 않습니다.

> Tip
> 
> 자동 재시작은 LiveReload와 함께 사용할 때 매우 잘 작동합니다. 자세한 내용은 LiveReload 섹션을 참조하십시오.
> JRebel을 사용하는 경우 동적 클래스 다시 로드를 위해 자동 다시 시작이 비활성화됩니다. 다른 devtools 기능(예: LiveReload 및 속성 재정의)은 계속 사용할 수 있습니다

> Note
> 
> DevTools는 다시 시작하는 동안 응용 프로그램 컨텍스트의 종료 후크를 사용하여 닫습니다. 종료 후크(SpringApplication.setRegisterShutdownHook(false))를 비활성화한 경우 제대로 작동하지 않습니다.

> Note
> 
> DevTools는 ApplicationContext에서 사용하는 ResourceLoader를 사용자 지정해야 합니다. 
> 애플리케이션이 이미 제공하는 경우 래핑됩니다. ApplicationContext에서 getResource 메서드의 직접 재정의는 지원되지 않습니다.

> Caution
> 
> AspectJ 위빙을 사용할 때는 자동 재시작이 지원되지 않습니다.

> 재시작 vs 다시불러오기
> 
> Spring Boot에서 제공하는 재시작 기술은 두 개의 클래스로더를 사용하여 작동합니다.
> 변경되지 않는 클래스(예: 타사 jar의 클래스)는 기본 클래스 로더에 로드됩니다.
> 활발히 개발 중인 클래스는 재시작 클래스 로더에 로드됩니다.
> 응용 프로그램이 다시 시작되면 다시 시작 클래스 로더가 제거되고 새 클래스 로더가 생성됩니다.
> 이 접근 방식은 기본 클래스 로더가 이미 사용 가능하고 채워져 있기 때문에 애플리케이션 재시작이 일반적으로 "콜드 스타트"보다 훨씬 빠르다는 것을 의미합니다.
> 
> 애플리케이션의 재시작이 충분히 빠르지 않거나 클래스 로딩 문제가 발생하는 경우 ZeroTurnaround의 JRebel과 같은 리로딩 기술을 고려할 수 있습니다.
> 이들은 로드될 때 클래스를 다시 작성하여 다시 로드할 수 있도록 만드는 방식으로 작동합니다.

### 상태 평가의 변경 사항 로깅
기본적으로 애플리케이션을 다시 시작할 때마다 조건 평가 델타를 보여주는 보고서가 기록됩니다.
이 보고서는 Bean 추가 또는 제거 및 구성 속성 설정과 같은 변경 사항을 적용할 때 애플리케이션의 자동 구성에 대한 변경 사항을 보여줍니다.

보고서 로깅을 비활성화하려면 다음 속성을 설정합니다.

```properties
spring.devtools.restart.log-condition-evaluation-delta=false
```

```yaml
spring:
  devtools:
    restart:
      log-condition-evaluation-delta: false
```

### 리소스 제외
특정 리소스는 변경될 때 반드시 재시작을 트리거할 필요가 없습니다. 예를 들어 Thymeleaf 템플릿은 내부에서 편집할 수 있습니다.
본적으로 /META-INF/maven, /META-INF/resources, /resources, /static, /public 또는 /templates에서 리소스를 변경하면 재시작이 트리거되지 않지만 `live reload`가 트리거됩니다
이러한 제외를 사용자 지정하려면 `spring.devtools.restart.exclude` 속성을 사용할 수 있습니다.
예를 들어 /static 및 /public만 제외하려면 다음 속성을 설정합니다.

```properties
spring.devtools.restart.exclude=static/**,public/**
```

```yaml
spring:
  devtools:
    restart:
      exclude: "static/**,public/**"
```

> Tip
> 
> 이러한 기본값을 유지하고 추가 제외를 추가하려면 대신 `spring.devtools.restart.additional-exclude` 속성을 사용하십시오.
 
### 추가 경로 보기
클래스 경로에 없는 파일을 변경할 때 애플리케이션을 다시 시작하거나 다시 로드해야 할 수 있습니다
이렇게 하려면 `spring.devtools.restart.additional-paths` 속성을 사용하여 변경 사항을 감시할 추가 경로를 구성합니다.
앞에서 설명한 `spring.devtools.restart.exclude` 속성을 사용하여 추가 경로 아래의 변경 사항이 전체 다시 시작 또는 실시간 다시 로드를 트리거할지 여부를 제어할 수 있습니다.

### 다시 시작 비활성화
다시 시작 기능을 사용하지 않으려면 `spring.devtools.restart.enabled` 속성을 사용하여 비활성화할 수 있습니다.
대부분의 경우 application.properties에서 이 속성을 설정할 수 있습니다(이렇게 하면 여전히 재시작 클래스 로더가 초기화되지만 파일 변경 사항을 감시하지는 않음).

재시작 지원을 완전히 비활성화해야 하는 경우(예: 특정 라이브러리에서 작동하지 않기 때문에) SpringApplication.run(… )을 호출하기 전에 spring.devtools.restart.enabled 시스템 속성을 false로 설정해야 합니다. 다음 예에 나와 있습니다.

```java
@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        System.setProperty("spring.devtools.restart.enabled", "false");
        SpringApplication.run(MyApplication.class, args);
    }

}
```

### 트리거 파일 사용
변경된 파일을 지속적으로 컴파일하는 IDE로 작업하는 경우 특정 시간에만 다시 시작하도록 트리거하는 것이 좋습니다.
이렇게 하려면 실제로 다시 시작 확인을 트리거하려는 경우 수정해야 하는 특수 파일인 "트리거 파일"을 사용할 수 있습니다.

> Note
> 
> 파일에 대한 모든 업데이트는 검사를 트리거하지만 실제로 재시작은 Devtools가 할 일이 있음을 감지한 경우에만 발생합니다.

트리거 파일을 사용하려면 `spring.devtools.restart.trigger-file` 속성을 트리거 파일의 이름(경로 제외)으로 설정합니다. 트리거 파일은 클래스 경로 어딘가에 나타나야 합니다.

예를 들어 다음과 같은 구조의 프로젝트가 있는 경우

```text
src
+- main
   +- resources
      +- .reloadtrigger
```

그러면 트리거 파일 속성은 다음과 같습니다.

```properties
spring.devtools.restart.trigger-file=.reloadtrigger
```

```yaml
spring:
  devtools:
    restart:
      trigger-file: ".reloadtrigger"
```

다시 시작은 이제 `src/main/resources/.reloadtrigger`가 업데이트된 경우에만 발생합니다.

> Tip
> 
> 모든 프로젝트가 동일한 방식으로 작동하도록 `spring.devtools.restart.trigger-file`을 전역 설정으로 설정할 수 있습니다.

일부 IDE에는 트리거 파일을 수동으로 업데이트하지 않아도 되는 기능이 있습니다. Spring Tools for Eclipse 및 IntelliJ IDEA(Ultimate Edition) 모두 이러한 지원을 제공합니다
Spring Tools를 사용하면 콘솔 보기에서 "reload" 버튼을 사용할 수 있습니다(트리거 파일의 이름이 .reloadtrigger인 경우). IntelliJ IDEA의 경우 설명서의 지침을 따를 수 있습니다.

### 재시작 Classloader 커스터마이징
다시 시작과 다시 로드 섹션에서 앞서 설명한 것처럼 다시 시작 기능은 두 개의 클래스 로더를 사용하여 구현됩니다. 이로 인해 문제가 발생하면 어떤 클래스 로더에서 로드할 항목을 사용자 정의해야 할 수 있습니다.

기본적으로 IDE에서 열려 있는 모든 프로젝트는 "restart" 클래스 로더로 로드되고 일반 .jar 파일은 "기본" 클래스 로더로 로드됩니다.
`mvn spring-boot:run` 또는 `gradle bootRun`을 사용하는 경우에도 마찬가지입니다. @SpringBootApplication을 포함하는 프로젝트는 "restart" 클래스 로더로 로드되고 다른 모든 것은 "기본" 클래스 로더로 로드됩니다.

`META-INF/spring-devtools.properties` 파일을 생성하여 다른 클래스 로더로 프로젝트의 일부를 로드하도록 Spring Boot에 지시할 수 있습니다.
`spring-devtools.properties` 파일에는 `restart.exclude` 및 `restart.include` 접두사가 붙은 속성이 포함될 수 있습니다.
포함 요소는 "재시작" 클래스 로더로 끌어올려야 하는 항목이고 제외 요소는 "기본" 클래스 로더로 푸시다운해야 하는 항목입니다
속성 값은 다음 예제와 같이 클래스 경로에 적용되는 정규식 패턴입니다.

```properties
restart.exclude.companycommonlibs=/mycorp-common-[\\w\\d-\\.]+\\.jar
restart.include.projectcommon=/mycorp-myproj-[\\w\\d-\\.]+\\.jar
```

```yaml
restart:
  exclude:
    companycommonlibs: "/mycorp-common-[\\w\\d-\\.]+\\.jar"
  include:
    projectcommon: "/mycorp-myproj-[\\w\\d-\\.]+\\.jar"
```

> Note
>
>  속성이 `restart.include.`또는 `restart.exclude.` 시작하는 이상 모든 속성 키는 고유해야 합니다.

> Tip
> 
> 클래스 경로의 모든 META-INF/spring-devtools.properties가 로드됩니다. 프로젝트 내부 또는 프로젝트가 사용하는 라이브러리에서 파일을 패키징할 수 있습니다.

### 알려진 제한 사항
다시 시작 기능은 표준 ObjectInputStream을 사용하여 역직렬화된 개체에서 제대로 작동하지 않습니다.
데이터를 역직렬화해야 하는 경우 Spring의 ConfigurableObjectInputStream을 Thread.currentThread().getContextClassLoader()와 함께 사용해야 할 수 있습니다.

불행히도 여러 타사 라이브러리는 컨텍스트 클래스 로더를 고려하지 않고 역직렬화합니다. 이러한 문제를 발견하면 원 저작자에게 수정을 요청해야 합니다.

### 6.8.4 LiveReload
spring-boot-devtools 모듈에는 리소스가 변경될 때 브라우저 새로 고침을 트리거하는 데 사용할 수 있는 임베디드 LiveReload 서버가 포함되어 있습니다.
LiveReload 브라우저 확장은 livereload.com에서 Chrome, Firefox 및 Safari에서 무료로 사용할 수 있습니다.

애플리케이션이 실행될 때 LiveReload 서버를 시작하지 않으려면 spring.devtools.livereload.enabled 속성을 `false`로 설정할 수 있습니다.

> Note
> 
> 한 번에 하나의 LiveReload 서버만 실행할 수 있습니다. 애플리케이션을 시작하기 전에 다른 LiveReload 서버가 실행되고 있지 않은지 확인하십시오. 
> IDE에서 여러 애플리케이션을 시작하는 경우 첫 번째 애플리케이션만 LiveReload를 지원합니다.

> Warning
> 
> 파일이 변경될 때 LiveReload를 트리거하려면 자동 다시 시작을 활성화해야 합니다.
 
### 6.8.5. 전역 설정
다음 파일 중 하나를 $HOME/.config/spring-boot 디렉토리에 추가하여 전역 devtools 설정을 구성할 수 있습니다.

1. spring-boot-devtools.properties
2. spring-boot-devtools.yaml
3. spring-boot-devtools.yml

이 파일에 추가된 모든 속성은 devtools를 사용하는 머신의 모든 Spring Boot 애플리케이션에 적용됩니다. 예를 들어 항상 트리거 파일을 사용하도록 다시 시작을 구성하려면 spring-boot-devtools 파일에 다음 속성을 추가합니다.

```properties
spring.devtools.restart.trigger-file=.reloadtrigger
```

```yaml
spring:
  devtools:
    restart:
      trigger-file: ".reloadtrigger"
```

기본적으로 $HOME은 사용자의 홈 디렉토리입니다. 이 위치를 사용자 지정하려면 SPRING_DEVTOOLS_HOME 환경 변수 또는 spring.devtools.home 시스템 속성을 설정합니다.

> Note
> 
> $HOME/.config/spring-boot에서 devtools 구성 파일을 찾을 수 없으면 $HOME 디렉토리의 루트에서 .spring-boot-devtools.properties 파일이 있는지 검색합니다.
> 이를 통해 $HOME/.config/spring-boot 위치를 지원하지 않는 이전 버전의 Spring Boot에 있는 애플리케이션과 devtools 전역 구성을 공유할 수 있습니다.

> Note
> 
> 프로필은 devtools 속성/yaml 파일에서 지원되지 않습니다.
> 
> .spring-boot-devtools.properties에서 활성화된 프로파일은 프로파일별 구성 파일의 로딩에 영향을 미치지 않습니다.
> 프로필 특정 파일 이름(spring-boot-devtools-<profile>.properties 형식) 및 YAML 및 속성 파일 모두의 spring.config.activate.on-profile 문서는 지원되지 않습니다.
 
### FileSystemWatcher 구성
FileSystemWatcher는 특정 시간 간격으로 클래스 변경 사항을 폴링한 다음 더 이상 변경 사항이 없는지 확인하기 위해 미리 정의된 대기 기간 동안 대기하는 방식으로 작동합니다.
Spring Boot는 파일을 컴파일하고 Spring Boot가 파일을 읽을 수 있는 위치로 복사하는 데 전적으로 IDE에 의존하므로 devtools가 애플리케이션을 다시 시작할 때 특정 변경 사항이 반영되지 않는 경우가 있을 수 있습니다
이러한 문제가 지속적으로 관찰되면 `spring.devtools.restart.poll-interval` 및 `spring.devtools.restart.quiet-period` 매개변수를 개발 환경에 맞는 값으로 늘리십시오.

```properties
spring.devtools.restart.poll-interval=2s
spring.devtools.restart.quiet-period=1s
```

```yaml
spring:
  devtools:
    restart:
      poll-interval: "2s"
      quiet-period: "1s"
```

모니터링되는 클래스 경로 디렉터리는 이제 변경 사항에 대해 2초마다 폴링되며 추가 클래스 변경 사항이 없는지 확인하기 위해 1초의 대기 기간이 유지됩니다.

### 원격 어플리케이션
Spring Boot 개발자 도구는 로컬 개발에만 국한되지 않습니다. 응용 프로그램을 원격으로 실행할 때 여러 기능을 사용할 수도 있습니다.
원격 지원은 보안 위험이 있을 수 있으므로 선택합니다. 신뢰할 수 있는 네트워크에서 실행 중이거나 SSL로 보호된 경우에만 활성화해야 합니다.
이러한 옵션 중 어느 것도 사용할 수 없는 경우 DevTools의 원격 지원을 사용해서는 안 됩니다. 프로덕션 배포에 대한 지원을 활성화해서는 안 됩니다.

이를 활성화하려면 다음 목록과 같이 리패키징된 아카이브에 devtools가 포함되어 있는지 확인해야 합니다.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludeDevtools>false</excludeDevtools>
            </configuration>
        </plugin>
    </plugins>
</build>
```

그런 다음 `spring.devtools.remote.secret` 속성을 설정해야 합니다. 중요한 암호나 암호와 마찬가지로 값은 추측하거나 무차별 대입할 수 없도록 고유하고 강력해야 합니다.

원격 devtools 지원은 연결을 허용하는 서버 측 엔드포인트와 IDE에서 실행하는 클라이언트 애플리케이션의 두 부분으로 제공됩니다.
`spring.devtools.remote.secret` 속성이 설정되면 서버 구성 요소가 자동으로 활성화됩니다. 클라이언트 구성 요소는 수동으로 시작해야 합니다.

> Note
> 
> Spring WebFlux 애플리케이션에는 원격 devtools가 지원되지 않습니다.
 
### 원격 클라이언트 애플리케이션 실행
원격 클라이언트 애플리케이션은 IDE 내에서 실행되도록 설계되었습니다. org.springframework.boot.devtools를 실행해야 합니다.
연결할 원격 프로젝트와 동일한 클래스 경로가 있는 RemoteSpringApplication. 응용 프로그램의 단일 필수 인수는 연결되는 원격 URL입니다.

예를 들어 Eclipse 또는 Spring 도구를 사용 중이고 Cloud Foundry에 배포한 my-app이라는 프로젝트가 있는 경우 다음을 수행합니다.

- Run 메뉴에서 Run Configurations… 를 선택합니다. 
- 새 Java 응용 프로그램 "launch configuration"을 만듭니다.
- my-app 프로젝트를 찾습니다.
- org.springframework.boot.devtools.RemoteSpringApplication을 기본 클래스로 사용합니다. 
- 프로그램 인수(또는 원격 URL)에 https://myapp.cfapps.io를 추가합니다.

실행 중인 원격 클라이언트는 다음 목록과 유사할 수 있습니다.

```text
  .   ____          _                                              __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _          ___               _      \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` |        | _ \___ _ __  ___| |_ ___ \ \ \ \
 \\/  ___)| |_)| | | | | || (_| []::::::[]   / -_) '  \/ _ \  _/ -_) ) ) ) )
  '  |____| .__|_| |_|_| |_\__, |        |_|_\___|_|_|_\___/\__\___|/ / / /
 =========|_|==============|___/===================================/_/_/_/
 :: Spring Boot Remote ::  (v2.6.11-SNAPSHOT)

2022-08-17 19:56:56.950  INFO 1190 --- [           main] o.s.b.devtools.RemoteSpringApplication   : Starting RemoteSpringApplication v2.6.11-SNAPSHOT using Java 1.8.0_345 on myhost with PID 1190 (/Users/myuser/.m2/repository/org/springframework/boot/spring-boot-devtools/2.6.11-SNAPSHOT/spring-boot-devtools-2.6.11-SNAPSHOT.jar started by myuser in /opt/apps/)
2022-08-17 19:56:57.005  INFO 1190 --- [           main] o.s.b.devtools.RemoteSpringApplication   : No active profile set, falling back to 1 default profile: "default"
2022-08-17 19:56:57.721  INFO 1190 --- [           main] o.s.b.d.a.OptionalLiveReloadServer       : LiveReload server is running on port 35729
2022-08-17 19:56:57.742  INFO 1190 --- [           main] o.s.b.devtools.RemoteSpringApplication   : Started RemoteSpringApplication in 1.493 seconds (JVM running for 2.228)
```

> Note
> 
> 원격 클라이언트는 실제 애플리케이션과 동일한 클래스 경로를 사용하기 때문에 애플리케이션 속성을 직접 읽을 수 있습니다. 
> 이것이 spring.devtools.remote.secret 속성을 읽고 인증을 위해 서버로 전달하는 방법입니다.

> Tip
> 
> 트래픽이 암호화되고 암호를 가로챌 수 없도록 항상 https://를 연결 프로토콜로 사용하는 것이 좋습니다.

> Tip
> 
> 프록시를 사용하여 원격 애플리케이션에 액세스해야 하는 경우 `spring.devtools.remote.proxy.host` 및 `spring.devtools.remote.proxy.port` 속성을 구성합니다.

### 원격 업데이트
원격 클라이언트는 로컬 다시 시작과 동일한 방식으로 애플리케이션 클래스 경로의 변경 사항을 모니터링합니다. 업데이트된 모든 리소스는 원격 애플리케이션에 푸시되고 (필요한 경우) 재시작을 트리거합니다.
이는 로컬에 없는 클라우드 서비스를 사용하는 기능을 반복하는 경우에 유용할 수 있습니다. 일반적으로 원격 업데이트 및 다시 시작은 전체 재구축 및 배포 주기보다 훨씬 빠릅니다.

느린 개발 환경에서는 조용한 기간이 충분하지 않아 클래스의 변경 사항이 일괄 처리로 분할될 수 있습니다.
클래스 변경 사항의 첫 번째 배치가 업로드된 후 서버가 다시 시작됩니다. 서버가 다시 시작 중이므로 다음 배치를 애플리케이션으로 보낼 수 없습니다.

이는 일반적으로 RemoteSpringApplication 로그에 일부 클래스 업로드 실패에 대한 경고와 그에 따른 재시도로 나타납니다.
그러나 애플리케이션 코드 불일치 및 변경 사항의 첫 배치가 업로드된 후 재시작 실패로 이어질 수도 있습니다
이러한 문제가 지속적으로 관찰되면 `spring.devtools.restart.poll-interval` 및 `spring.devtools.restart.quiet-period` 매개변수를 개발 환경에 맞는 값으로 늘리십시오.
이러한 속성을 구성하려면 Configuring File System Watcher 섹션을 참조하십시오.

> Note
> 
> 파일은 원격 클라이언트가 실행 중일 때만 모니터링됩니다. 원격 클라이언트를 시작하기 전에 파일을 변경하면 원격 서버로 푸시되지 않습니다.
 
## 6.9 프로덕션용 애플리케이션 패키징
실행 가능한 jar는 프로덕션 배포에 사용할 수 있습니다. 독립형이므로 클라우드 기반 배포에도 적합합니다.

상태, 감사, 메트릭 REST 또는 JMX 엔드포인트와 같은 추가 "프로덕션 준비" 기능의 경우 spring-boot-actuator 추가를 고려하십시오. 자세한 내용은 프로덕션 준비 기능을 참조하십시오.

## 6.10 다음에 읽을 내용
이제 Spring Boot를 사용하는 방법과 따라야 할 몇 가지 모범 사례를 이해해야 합니다.
이제 특정 Spring Boot 기능에 대해 자세히 알아보거나 건너뛰고 Spring Boot의 "프로덕션 준비" 측면에 대해 읽을 수 있습니다.