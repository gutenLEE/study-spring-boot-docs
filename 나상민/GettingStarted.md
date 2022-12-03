# 4. 시작하기
만약에 `Spring Boot` 나 일반적인 `Spring`을 시작한다면, 이 섹션을 읽고 시작 해야 합니다.
이 문서는 `무엇을`, `어떻게`, `왜`와 같은 질문에 대한 기본 답변입니다. 또한, 설치 방법을 포함한 SpringBoot에 대한 소개가 포함되어 있습니다.
그런 다음 당신의 첫 Spring Boot 어플리케이션을 빌드하는 과정을 안내하고 몇 가지 핵심 원칙에 대해 논의 합니다.

## 4.1 SpringBoot 소개
Spring Boot는 독립적으로 배포 실행할 수 있는 Spring 기반의 애플리케이션을 만드는데 도움이 됩니다.
최소한의 비용으로 시작할 수 있도록 Spring플랫폼 및 타사 라이브러리에 대해 지향하는 견해를 갖습니다.
대부분의 Spring Boot 어플리케이션은 최소한의 Spring 설정이 필요합니다.

Spring Boot를 사용하여 java -jar 또는 보다 전통적인 war 배포를 사용하여 시작할 수 있는 Java 애플리케이션을 만들 수 있습니다.
`Spring Script`를 실행하는 명령줄 도구 또한 제공합니다.

우리의 주요 목표는 :
- 모든 Spring 개발을 위해 더 빠르고 광범위하게 시작하는 경험을 제공합니다.
- 추가적인 설정없이 사용하는 것을 지향하지만, 기본 설정과 다른 요구사항이 있으면 바로 변경이 가능합니다.
- 임베디드 서버, 보안, 메트릭, 상태 확인 및 외부화된 구성과 같은 대규모 프로젝트 클래스에 공통적인 다양한 비기능 기능을 제공합니다.
- 코드 생성 및 XML 구성에 대한 요구 사항이 전혀 없습니다.

## 4.2 시스템 요구사항
Spring Boot 2.6.11-SNAPSHOT에는 Java 8이 필요하며 Java 18까지 호환됩니다. Spring Framework 5.3.22 이상이 필요합니다.

다음 빌드 도구에 대한 명시적 빌드 지원이 제공됩니다:

| 빌드 툴  | 버전 |
|-------|--------------|
| Maven | 3.5+         |
| Gradle | 6.8.x, 6.9.x, and 7.x |

### 4.2.1. Servlet Containers
Spring Boot는 다음 임베디드 서블릿 컨테이너를 지원합니다:

| 이름  | 서블릿 버전 |
|-------|--------|
| Tomcat 9.0 | 4.0    |
| Jetty 9.4 | 3.1    |
| Jetty 10.0 | 4.0    |
| Undertow 2.0 | 4.0    |
모든 서블릿 3.1+ 호환 컨테이너에 Spring Boot 애플리케이션을 배포할 수도 있습니다.

## 4.3 Spring Boot 설치
SpringBoot 고전적인 자바 개발 도구와 함께 사용하거나 커멘드 라인으로 설치할 수 있습니다.
어떤 방법이든 Java SDK v1.8이상이 필요합니다. 시작하기 전에 다음 명령을 사용하여 현재 자바가 설치되어있는지 확인해야 합니다.
```shell
$ java -version
```
자바 개발이 처음이거나 Spring Boot를 경험하고 싶다면 먼저 Spring Boot CLI를 사용해 볼수 있습니다. 그렇지 않으면 고전적인 설치 지침을 읽으세요.

### 4.3.1 자바 개발자를 위한 설치 지침
표준 자바 라이브러리와 동일한 방식으로 스프링 부트를 사용할 수 있습니다. 진행할려면, 클래스 경로에 적절한 `spring-boot-*.jar`파일을 포함하세요.
스프링 부트에는 특별한 도구 통합이 필요하지 않으므로 모든 IDE 또는 텍스트 편집기를 사용할 수 있습니다.
또한 스프링 부트 애플리케이션에는 특별한 것이 없으므로 다른 자바 프로그램과 마찬가지로 스프링 부트 애플리케이션을 실행하고 디버그할 수 있습니다.

`Spring Boot jars`를 복사할 수 있지만, 일반적으로 종속성 관리를 지원하는 빌드 도구(예: Maven 또는 Gradle)를 사용하는 것이 좋습니다.

### Maven 설치
스프링 부트는 Apache Maven 3.3 이상과 호환됩니다. Maven을 아직 설치하지 않은 경우 [maven.apache.org](https://maven.apache.org/)의 지침을 따를 수 있습니다.
> TIP
>  
> 많은 운영 체제에서 Maven은 패키지 관리자와 함께 설치할 수 있습니다. 
 OSX Homebrew를 사용하는 경우 brew install maven을 사용해 보세요.
 Ubuntu 사용자는 sudo apt-get install maven을 실행할 수 있습니다.
 Chocolatey를 사용하는 Windows 사용자는 상승된(관리자) 프롬프트에서 choco install maven을 실행할 수 있습니다.

스프링부트 디펜더시는 `org.springframework.boot` `groupId`를 사용합니다.
일반적으로 Maven POM 파일은 `spring-boot-starter-parent` 프로젝트에서 상속되며 하나 이상의 "Starters"에 대한 종속성을 선언합니다.
Spring Boot는 또한 실행 가능한 jar를 생성하기 위한 선택적 Maven 플러그인을 제공합니다.

Spring Boot 및 Maven을 시작하는 방법에 대한 자세한 내용은 Maven 플러그인 참조 가이드의 시작하기 섹션에서 찾을 수 있습니다.

### Gradle 설치
Spring Boot는 Gradle 6.8, 6.9 및 7.x와 호환됩니다. Gradle을 아직 설치하지 않은 경우 gradle.org의 지침을 따를 수 있습니다.

Spring Boot 종속성은 `org.springframework.boot` 그룹을 사용하여 선언할 수 있습니다. 일반적으로 프로젝트는 하나 이상의 "스타터"에 대한 종속성을 선언합니다.
Spring Boot는 종속성 선언을 단순화하고 실행 가능한 jar를 생성하는 데 사용할 수 있는 유용한 Gradle 플러그인을 제공합니다.

>Gradle Wrapper
> 
> Gradle Wrapper는 프로젝트를 빌드해야 할 때 Gradle을 얻기위한 좋은 방법을 제공합니다.
> 빌드 프로세스를 부트스트랩하기 위해 코드와 함께 커밋하는 작은 스크립트 및 라이브러리입니다.
> 자세한 내용은 docs.gradle.org/current/userguide/gradle_wrapper.html을 참조하세요.

Spring Boot 및 Gradle을 시작하는 방법에 대한 자세한 내용은 Gradle 플러그인 참조 가이드의 시작하기 섹션에서 확인할 수 있습니다.

### 4.3.2 Spring Boot CLI로 설치
Spring Boot CLI(명령줄 인터페이스)는 Spring으로 신속하게 프로토타입을 만드는 데 사용할 수 있는 명령줄 도구입니다
이를 통해 Groovy 스크립트를 실행할 수 있습니다. 즉, boilerplate 코드가 많이 없어도 친숙한 Java 유사 구문을 사용할 수 있습니다

CLI를 사용하여 Spring Boot를 사용할 필요는 없지만 IDE 없이 Spring 애플리케이션을 시작하는 빠른 방법입니다.

### 수동설치
Spring 소프트웨어 리포지토리에서 Spring CLI 배포버전을 다운로드할 수 있습니다.
- spring-boot-cli-2.6.11-SNAPSHOT-bin.zip
- spring-boot-cli-2.6.11-SNAPSHOT-bin.tar.gz

최첨단 스냅샷 배포도 가능합니다.

다운로드가 완료되면 압축을 푼 아카이브의 `INSTALL.txt` 지침을 따르십시오. 요약하면 `.zip` 파일의 `bin/` 디렉토리에 스프링 스크립트(Windows용 spring.bat)가 있습니다.
또는 .jar 파일과 함께 `java -jar`를 사용할 수 있습니다(스크립트는 클래스 경로가 올바르게 설정되었는지 확인하는 데 도움이 됨).

### SDKMAN!으로 설치
SDKMAN! (Software Development Kit Manager)는 Groovy 및 Spring Boot CLI를 포함하여 다양한 바이너리 SDK의 여러 버전을 관리하는 데 사용할 수 있습니다
SDKMAN!을 받으세요! sdkman.io에서 다음 명령을 사용하여 Spring Boot를 설치합니다

```shell
$ sdk install springboot
$ spring --version
Spring CLI v2.6.11-SNAPSHOT
```

CLI용 기능을 개발하고 빌드한 버전에 액세스하려면 다음 명령을 사용하십시오.

```shell
$ sdk install springboot dev /path/to/spring-boot/spring-boot-cli/target/spring-boot-cli-2.6.11-SNAPSHOT-bin/spring-2.6.11-SNAPSHOT/
$ sdk default springboot dev
$ spring --version
Spring CLI v2.6.11-SNAPSHOT
```

앞의 지침은 dev 인스턴스라는 스프링의 로컬 인스턴스를 설치합니다. 타겟 빌드 위치를 가리키므로 Spring Boot를 다시 빌드할 때마다 Spring이 최신 상태입니다.

다음 명령을 실행하여 볼 수 있습니다.

```shell
$ sdk ls springboot

================================================================================
Available Springboot Versions
================================================================================
> + dev
* 2.6.11-SNAPSHOT

================================================================================
+ - local version
* - installed
> - currently in use
================================================================================
```

### OSX Homebrew 설치
Mac을 사용 중이고 Homebrew를 사용하는 경우 다음 명령을 사용하여 Spring Boot CLI를 설치할 수 있습니다.

```shell
$ brew tap spring-io/tap
$ brew install spring-boot
```

Homebrew는 스프링을 `/usr/local/bin`에 설치합니다.

> 노트
> 
> 공식이 표시되지 않으면 추출 설치가 오래된 것일 수 있습니다. 이 경우 brew update를 실행하고 다시 시도하십시오.

### MacPorts 설치
Mac을 사용 중이고 MacPorts를 사용하는 경우 다음 명령을 사용하여 Spring Boot CLI를 설치할 수 있습니다.

```shell
$ sudo port install spring-boot-cli
```

### Command-line 완성
Spring Boot CLI에는 `BASH` 및 `zsh 셸`에 대한 명령 완성을 제공하는 스크립트가 포함되어 있습니다.
모든 셸에서 스크립트(spring이라고도 함)를 소싱하거나 개인 또는 시스템 전체 bash 완료 초기화에 넣을 수 있습니다.
Debian 시스템에서 시스템 전체 스크립트는 `/shell-completion/bash`에 있으며 해당 디렉토리의 모든 스크립트는 새 셸이 시작될 때 실행됩니다.
예를 들어 SDKMAN!을 사용하여 설치한 경우 스크립트를 수동으로 실행하려면 다음 명령을 사용하십시오.

```shell
$ . ~/.sdkman/candidates/springboot/current/shell-completion/bash/spring
$ spring <HIT TAB HERE>
  grab  help  jar  run  test  version
```

> 노트
> 
> Homebrew 또는 MacPorts를 사용하여 Spring Boot CLI를 설치하면 명령줄 완성 스크립트가 자동으로 셸에 등록됩니다.

### Windows Scoop 설치
Windows를 사용 중이고 Scoop을 사용하는 경우 다음 명령을 사용하여 Spring Boot CLI를 설치할 수 있습니다.

```shell
> scoop bucket add extras
> scoop install springboot
```

Scoop은 `~/scoop/apps/springboot/current/bin`에 spring을 설치합니다.

> 노트
> 
> 앱 manifest가 표시되지 않으면 스쿠프 설치가 오래된 것일 수 있습니다. 이 경우 `scoop update`를 실행하고 다시 시도하십시오.

### Quick-start Spring CLI 예제
다음 웹 응용 프로그램을 사용하여 설치를 테스트할 수 있습니다. 시작하려면 다음과 같이 `app.groovy`라는 파일을 만듭니다.

```groovy
@RestController
class ThisWillActuallyRun {

    @RequestMapping("/")
    String home() {
        "Hello World!"
    }

}
```

그런 다음 다음과 같이 셸에서 실행합니다.

```shell
$ spring run app.groovy
```

> 노트
> 
> 디펜더시가 다운로드되기 때문에 애플리케이션의 첫 번째 실행이 느립니다. 후속 실행이 훨씬 빠릅니다.

선호하는 웹 브라우저에서 `localhost:8080`을 엽니다. 다음 출력이 표시되어야 합니다.

```text
Hello World!
```

## 4.4 첫 번째 Spring Boot 애플리케이션 개발
이 섹션에서는 작은 "Hello World!"를 개발하는 방법에 대해 설명합니다. Spring Boot의 주요 기능 중 일부를 강조하는 웹 애플리케이션입니다.
대부분의 IDE가 Maven을 지원하므로 Maven을 사용하여 이 프로젝트를 빌드합니다.

> TIP
> 
> spring.io 웹 사이트에는 Spring Boot를 사용하는 많은 "시작하기" 가이드가 포함되어 있습니다. 특정 문제를 해결해야 하는 경우 먼저 확인하십시오.
> 
> start.spring.io로 이동하고 디펜더시 검색기에서 web starter를 선택하여 아래 단계를 바로 실행할 수 있습니다.
> 그렇게 하면 바로 코딩을 시작할 수 있도록 새로운 프로젝트 구조가 생성됩니다. 자세한 내용은 start.spring.io 사용자 가이드를 확인하세요.

시작하기 전에 터미널을 열고 다음 명령을 실행하여 유효한 버전의 Java 및 Maven이 설치되어 있는지 확인하십시오

```shell
$ java -version
java version "1.8.0_102"
Java(TM) SE Runtime Environment (build 1.8.0_102-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.102-b14, mixed mode)
```
```shell
$ mvn -v
Apache Maven 3.5.4 (1edded0938998edf8bf061f1ceb3cfdeccf443fe; 2018-06-17T14:33:14-04:00)
Maven home: /usr/local/Cellar/maven/3.3.9/libexec
Java version: 1.8.0_102, vendor: Oracle Corporation
```

> Note
> 
> 이 샘플은 자신의 디렉터리에 만들어야 합니다. 후속 지침에서는 적절한 디렉토리를 생성했으며 현재 디렉토리라고 가정합니다.

### 4.4.1 POM 생성
Maven pom.xml 파일을 생성하여 시작해야 합니다. pom.xml은 프로젝트를 빌드하는 데 사용되는 재료입니다. 즐겨 사용하는 텍스트 편집기를 열고 다음을 추가합니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>myproject</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.11-SNAPSHOT</version>
    </parent>

    <!-- Additional lines to be added here... -->

    <!-- (you do not need this if you are using a .RELEASE version) -->
    <repositories>
        <repository>
            <id>spring-snapshots</id>
            <url>https://repo.spring.io/snapshot</url>
            <snapshots><enabled>true</enabled></snapshots>
        </repository>
        <repository>
            <id>spring-milestones</id>
            <url>https://repo.spring.io/milestone</url>
        </repository>
    </repositories>
    <pluginRepositories>
        <pluginRepository>
            <id>spring-snapshots</id>
            <url>https://repo.spring.io/snapshot</url>
        </pluginRepository>
        <pluginRepository>
            <id>spring-milestones</id>
            <url>https://repo.spring.io/milestone</url>
        </pluginRepository>
    </pluginRepositories>
</project>
```

앞의 목록은 작동하는 빌드를 제공해야 합니다. mvn 패키지를 실행하여 테스트할 수 있습니다(지금은 "jar가 비어 있을 것입니다. 포함할 콘텐츠가 표시되지 않았습니다!" 경고를 무시할 수 있습니다).

> Note
> 
> 이 시점에서 프로젝트를 IDE로 가져올 수 있습니다(대부분의 최신 Java IDE에는 Maven에 대한 기본 제공 지원이 포함됨). 단순화를 위해 이 예제에서는 일반 텍스트 편집기를 계속 사용합니다

### 4.2.2 Classpath Dependencies 추가
Spring Boot는 클래스 경로에 jar를 추가할 수 있는 여러 "스타터"를 제공합니다. 스모크 테스트용 애플리케이션은 POM의 상위 섹션에서 `spring-boot-starter-parent`를 사용합니다.
spring-boot-starter-parent는 유용한 Maven 기본값을 제공하는 특수 스타터입니다. 또한 "blessed" 의존성에 대한 버전 태그를 생략할 수 있도록 의존성 관리 섹션을 제공합니다.

다른 "스타터"는 특정 유형의 애플리케이션을 개발할 때 필요할 가능성이 있는 dependencies을 제공합니다.
웹 애플리케이션을 개발 중이므로 `spring-boot-starter-web` 의존성을 추가합니다. 그 전에 다음 명령을 실행하여 현재 가지고 있는 것을 볼 수 있습니다.

```shell
$ mvn dependency:tree

[INFO] com.example:myproject:jar:0.0.1-SNAPSHOT
```

`mvn dependency:tree` 명령은 프로젝트 종속성의 트리 표현을 인쇄합니다.
`spring-boot-starter-parent` 자체적으로 종속성을 제공하지 않는 것을 볼 수 있습니다.
필요한 종속성을 추가하려면 `pom.xml`을 편집하고 상위 섹션 바로 아래에 `spring-boot-starter-web` 종속성을 추가하십시오.

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

`mvn dependency:tree`를 다시 실행하면 이제 Tomcat 웹 서버 및 Spring Boot 자체를 포함하여 많은 추가 종속성이 있음을 알 수 있습니다.

### 4.4.3. 코드 작성
애플리케이션을 완료하려면 단일 Java 파일을 생성해야 합니다. 기본적으로 Maven은 `src/main/java`에서 소스를 컴파일하므로 해당 디렉터리 구조를 만든 다음 `src/main/java/MyApplication`이라는 파일을 추가해야 합니다.
JAVA로 구성한 코드는 다음과 같습니다.

```java
@RestController
@EnableAutoConfiguration
public class MyApplication {

    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

}
```

여기에는 많은 코드가 없지만 상당히 많은 작업이 진행됩니다. 다음 몇 섹션에서 중요한 부분을 단계별로 살펴봅니다.

### @RestController 와 @RequestMapping 어노테이션
MyApplication 클래스의 첫 번째 어노테이션은 @RestController입니다. 이를 스테레오타입 어노테이션이라고 합니다
코드를 읽는 사람들과 클래스가 특정 역할을 수행하는 Spring에 대한 힌트를 제공합니다. 이 경우 우리 클래스는 웹 @Controller이므로 Spring은 들어오는 웹 요청을 처리할 때 이를 고려합니다.

@RequestMapping 어노테이션은 "라우팅" 정보를 제공합니다.
이것은 / 경로가 있는 모든 HTTP 요청이 홈 메소드에 매핑되어야 함을 Spring에 알립니다. @RestController 어노테이션은 결과 문자열을 호출자에게 직접 다시 렌더링하도록 Spring에 지시합니다.

> TIP
> 
> @RestController 및 @RequestMapping 어노테이선은 Spring MVC 어노테이션입니다(Spring Boot에만 해당되지 않음). 자세한 내용은 Spring 참조 문서의 MVC 섹션을 참조하세요.

### @EnableAutoConfiguration 어노테이션
두 번째 클래스 레벨 어노테이션은 @EnableAutoConfiguration입니다.
이 어노테이션은 추가한 jar 종속성을 기반으로 Spring 구성 방법을 "추측"하도록 Spring Boot에 지시합니다.
`spring-boot-starter-web`이 Tomcat 및 Spring MVC를 추가했기 때문에 자동 구성은 웹 애플리케이션을 개발하고 그에 따라 Spring을 설정한다고 가정합니다.

> 스타터 및 자동 구성
> 
> 자동 구성은 "스타터"와 잘 작동하도록 설계되었지만 두 개념이 직접 연결되지는 않습니다.
> 스타터 외부에서 jar 종속성을 자유롭게 선택하고 선택할 수 있습니다. Spring Boot는 여전히 애플리케이션을 자동 구성하기 위해 최선을 다하고 있습니다.

### 메인 메소드
애플리케이션의 마지막 부분은 기본 메서드입니다. 애플리케이션 진입점에 대한 Java 규칙을 따르는 표준 방법입니다.
메인 메서드는 `run`을 호출하여 Spring Boot의 `SpringApplication 클래스`에 위임합니다.
SpringApplication은 애플리케이션을 부트스트랩하여 Spring을 시작하고, Spring은 자동 구성된 Tomcat 웹 서버를 시작합니다.
우리는 `MyApplication.class`를 run 메서드에 대한 인수로 전달하여 SpringApplication에 기본 Spring 구성 요소를 알려야 합니다.
args 배열도 전달되어 모든 명령줄 인수를 노출합니다.

### 예제 실행
이 시점에서 애플리케이션이 작동해야 합니다. `spring-boot-starter-parent` POM을 사용했으므로 애플리케이션을 시작하는 데 사용할 수 있는 유용한 실행 목표가 있습니다.
루트 프로젝트 디렉토리에서 mvn spring-boot:run을 입력하여 애플리케이션을 시작합니다. 다음과 유사한 출력이 표시되어야 합니다.

```shell
$ mvn spring-boot:run

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::  (v2.6.11-SNAPSHOT)
....... . . .
....... . . . (log output here)
....... . . .
........ Started MyApplication in 2.222 seconds (JVM running for 6.514)

```

localhost:8080에 대한 웹 브라우저를 열면 다음 출력이 표시되어야 합니다.

```text
Hello World!
```

애플리케이션을 정상적으로 종료하려면 ctrl-c를 누르십시오.

### 실행 가능한 jar 만들기
프로덕션 환경에서 실행할 수 있는 완전히 독립적인 실행 가능한 jar 파일을 생성하여 예제를 마무리합니다.
실행 가능한 jar("fat jar"라고도 함)는 코드를 실행하는 데 필요한 모든 jar 종속성과 함께 컴파일된 클래스를 포함하는 아카이브입니다.

> 실행 가능한 jar 및 Java
> 
> Java는 중첩된 jar 파일(jar 내에 포함된 jar 파일)을 로드하는 표준 방법을 제공하지 않습니다. 독립형 애플리케이션을 배포하려는 경우 문제가 될 수 있습니다.
> 
> 이 문제를 해결하기 위해 많은 개발자들이 "uber" jars를 사용합니다. uber jar는 모든 애플리케이션 종속성의 모든 클래스를 단일 아카이브로 패키징합니다.
> 이 접근 방식의 문제점은 애플리케이션에 어떤 라이브러리가 있는지 확인하기 어렵다는 것입니다. 또한 여러 jar에서 동일한 파일 이름이 사용되지만 내용이 다른 경우에도 문제가 될 수 있습니다.
> 
> Spring Boot는 다른 접근 방식을 취하며 실제로 jar를 직접 중첩할 수 있습니다.

실행 가능한 jar를 생성하려면 pom.xml에 `spring-boot-maven-plugin`을 추가해야 합니다. 이렇게 하려면 종속성 섹션 바로 아래에 다음 줄을 삽입합니다.

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

> 노트
> 
> `spring-boot-starter-parent` POM에는 리패키지 목표를 바인딩하는 `<executions>` 구성이 포함되어 있습니다. 상위 POM을 사용하지 않는 경우 이 구성을 직접 선언해야 합니다. 자세한 내용은 플러그인 설명서를 참조하십시오.

다음과 같이 pom.xml을 저장하고 명령줄에서 mvn 패키지를 실행합니다.

```shell
$ mvn package

[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building myproject 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] .... ..
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ myproject ---
[INFO] Building jar: /Users/developer/example/spring-boot-example/target/myproject-0.0.1-SNAPSHOT.jar
[INFO]
[INFO] --- spring-boot-maven-plugin:2.6.11-SNAPSHOT:repackage (default) @ myproject ---
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------

```

대상 디렉토리를 보면 `myproject-0.0.1-SNAPSHOT.jar`이 표시되어야 합니다. 파일 크기는 약 10MB여야 합니다. 내부를 들여다 보려면 다음과 같이 `jar tvf`를 사용할 수 있습니다.

```shell
$ jar tvf target/myproject-0.0.1-SNAPSHOT.jar
```

또한 대상 디렉토리에 `myproject-0.0.1-SNAPSHOT.jar.original`이라는 훨씬 작은 파일이 표시되어야 합니다. 이것은 Spring Boot에 의해 리패키징되기 전에 Maven이 생성한 원래 jar 파일입니다.

해당 애플리케이션을 실행하려면 다음과 같이 java -jar 명령을 사용하십시오.

```shell
$ java -jar target/myproject-0.0.1-SNAPSHOT.jar

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::  (v2.6.11-SNAPSHOT)
....... . . .
....... . . . (log output here)
....... . . .
........ Started MyApplication in 2.536 seconds (JVM running for 2.864)

```

이전과 마찬가지로 응용 프로그램을 종료하려면 ctrl-c를 누릅니다.

## 4.5. 다음에 읽을 내용
바라건대, 이 섹션에서는 몇 가지 Spring Boot 기본 사항을 제공하고 자신의 애플리케이션을 작성하는 방법에 대해 설명했습니다.
작업 지향적인 유형의 개발자라면 spring.io로 이동하여 특정 "Spring으로 어떻게 해야 합니까?"라는 문제를 해결하는 시작 가이드를 따를 수 있습니다.
또한 Spring Boot 관련 "How-to" 참조 문서도 있습니다.

그렇지 않으면 다음 논리적 단계는 Spring Boot로 개발을 읽는 것입니다. 정말 참을성이 없다면 Spring Boot 기능에 대해 읽어볼 수도 있습니다.