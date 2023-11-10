---
title : SpringBoot - Practice
date : 2023-11-08 01:47:43 +09:00
categories : [Study,SpringBoot]
tags : [SpringBoot,Practice]
---

## Spring Boot와 친해지기

- Spring Legacy Project의 단점
  - 개발 초기 단계의 XML 설정...
  - Maven 특유의 라이브러리 의존(버전,충돌) 문제...

이러한 이유로 Spring Boot를 통해 비즈니스 로직 작성에 더욱 집중해보자 한다.
<br><br>

### 스프링 이니셜라이저(Spring Initializr)
<hr>

[https://start.spring.io/](https://start.spring.io/) 를 통해 밑의 설정대로 프로젝트 설정 <br>
또는, STS의 Spring Starter Project를 통해 생성가능 <br>

<br>

`스프링 이니셜라이저 설정` <br>
![스프링이니셜라이저](https://github.com/no2j/no2j.github.io/assets/106552182/947bacac-8c82-43e2-8e31-71fdcbd48146) <br>
참고로 Tool은 STS를 사용하여 진행
<br><br><br>

## 스프링 부트 프로젝트 구조
<br>

`스프링 부트 프로젝트 구조` <br>
![프로젝트_구조](https://github.com/no2j/no2j.github.io/assets/106552182/77f70ce3-ff3b-4138-a31b-64f17c3bdf7a) <br>

<br>

### src/main/java 디렉토리
<hr>

- 스프링 레거시와 마찬가지로 클래스, 인터페이스 등 Java 관련 파일이 위치하는 디렉토리
<br>

#### xxxApplication 클래스
- 프로젝트 생성 시 BoardApplication 클래스가 자동으로 생성되어있음.
  - 해당 클래스를 열어보면 선언부에 main()메서드 하나만 선언되어 있음.
  - 선언된 main() 메서드는 **SpringApplication.run()을 호출해서 웹 어플리케이션을 실행**하는 역할을 함.
  
- **@SpringBootApplication 어노테이션**
  - @SpringBootApplication 어노테이션은 세 가지의 어노테이션으로 구성되어 있다. <br><br>
    - **@EnableAutoConfiguration** : 스프링 부트는 개발에 필요한 몇 가지 필수적인 설정들의 처리가 되어 있으며, 해당 어노테이션에 의해 다양한 설정들의 일부가 자동으로 완료 됨. <br><br>
    - **@ComponentScan** : XML 설정 방식을 이용하는 스프링 레거시는 빈(Bean)의 등록 및 스캔을 위해, 수동으로 ComponentScan을 여러 개 선언하는 방식을 사용했었다. 스프링 부트는 해당 어노테이션에 의해 자동으로 컴포넌트 클래스를 검색하고, 스프링 어플리케이션 컨텍스트(IoC 컨테이너)에 빈(Bean)으로 등록한다. 쉽게 말해, 의존성 주입(DI) 과정이 더욱 간편해졌다고 생각할 수 있다. <br> <br>
    - **@Configuration** : 해당 어노테이션이 선언된 클래스는 Java 기반의 설정 파일로 인식된다. 스프링 4 버전부터 Java 기반의 설정이 가능하게 되었으며, XML 설정에 큰 시간을 소모하지 않아도 된다.

<br><br>

### src/main/resources 디렉토리
<hr>

- 스프링 레거시는 프로젝트가 생성되었을 때 해당 디렉토리에 log4.xml 파일만 생성되었었다.
- 스프링 부트는 templates 폴더, static 폴더, application.properties 파일이 기본적으로 생성된다.

<br>

#### 폴더 및 파일 상세 설명
- **templates 폴더** <br> <hr>
스프링 레거시는 HTML 내에 Java 코드를 삽입하는 방식인 JSP를 주로 사용했었다. 하지만, 스프링 공식 문서에서는 View(화면) 영역에서 JSP가 아닌 **타임리프(Thymeleaf) 템플릿 엔진의 사용을 권장**하고 있다. 타임리프는 JSP와 마찬가지로 HTML 내에서 Java 영역의 데이터를 처리하는 데 사용되는 템플릿 엔진이다. 문법 또한 JSTL과 유사하기에 JSP를 사용해 본 경험이 있다면 쉽게 접근이 가능하다.  <br>
결론적으로 해당 디렉터리에는 타임리프 관련 파일이 위치하게 되고, 타임리프는 HTML5 기반이기 때문에 HTML 파일로 화면을 구성한다.
<br>

- **static 폴더** <br> <hr>
해당 폴더에는 css, fonts, images, plugin, scripts 등의 **정적 리소스 파일**이 위치한다.

<br>

- **application.properties 파일** <br> <hr>
해당 파일은 웹 어플리케이션을 실행하면서 자동으로 로딩되는 파일이다. 예를 들어, 부트에 내장된 톰캣의 포트 번호, 컨텍스트 패스(Context Path) 설정이나, 데이터베이스 관련 정보 등 어플리케이션에서 사용하는 여러가지 설정을 해당 파일에 **Key - Value 형식으로 선언**해서 사용할 수 있다. 선언한 속성은 일반적으로 설정(Configuration) 파일에서 사용한다.

<br><br>

### src/test/java 디렉토리
<hr>

- 해당 디렉토리의 com.study.board 패키지에는 **BoardApplicationTests** 클래스가 생성되어 있다.
- 해당 클래스를 이용해서 개발 단계별로 단위 테스트를 진행하며, 스프링 레거시와는 달리 복잡한 설정 없이 곧바로 테스트가 가능하다.

<br><br>

### build.gradle
<hr>

- 프로젝트의 빌드 도구
- 기존의 스프링은 pom.xml에 dependency를 추가해서 라이브러리를 관리하는 방식의 메이븐(Maven)을 이용하였었다. 메이븐을 이용하면서, 라이브러리의 버전 문제, 충돌 문제, 종속적인 문제 등 골치 아픈 상황을 겪어보았다. 이러한 문제를 해결해줄 수 있으며 간략한 코드로 라이브러리를 추가할 수 있다.

<br><br>

## 데이터 소스 설정(Data Source Configuration)

<br>

`패키지와 클래스가 추가된 디렉토리 구조` <br>
![1](https://github.com/no2j/no2j.github.io/assets/106552182/dd285f9a-aa67-45db-82db-91d13ebe1816)

<br><br>

`application.properties` <br>
![3](https://github.com/no2j/no2j.github.io/assets/106552182/f4d2bbec-87d1-49e0-aa3e-74fd6cb0fa98)

<br><br>

`소스 코드 작성` <br>
![2](https://github.com/no2j/no2j.github.io/assets/106552182/371ef4d0-adaa-4497-ac5a-4409e686b675)
<br><br>

### 소스 코드 설명
<hr>

- @Configuration
  - 스프링은 @Configuration이 선언된 클래스를 자바(Java) 기반의 설정 파일로 인식한다.
  - 스프링 레거시의 XML 설정 방식을 Java 클래스로 대체한 것으로 생각.
  
- @PropertySource
  - 해당 클래스에서 참조할 properties의 경로를 선언(지정)한다.
  
- @Autowired
  - 빈(Bean)으로 등록된 인스턴스(이하 객체)를 클래스에 주입하는 데 사용한다.
  - 의존성 주입(DI)의 필드 주입 방식

- ApplicationContext
  - 스프링 컨테이너(Spring Container) 중 하나이다. (Bean Factory를 상속받는다.)
  - 

- @Bean
  - Configuration 클래스의 메서드 레벨에만 선언 가능
  - @Bean이 선언된 객체는 스프링 컨테이너에 의해 관리되는 빈(Bean)으로 등록된다.

- @ConfigurationProperties
  - 해당 어노테이션은 인자에 prefix 속성을 선언(지정)할 수 있다.
  - @PropertySource에 선언된 파일(application.properties)에서 prefix에 해당하는 spring.datasource로 시작하는 설정을 모두 읽어 들여 해당 메서드에 매핑(바인딩)하는 개념

- hikariConfig
  - 히카리CP(Connection Pool) 객체를 생성한다.
  - **히카리CP는 커넥션 풀(Connection Pool) 라이브러리** 중 하나이다.

- dataSource
  - 데이터 소스 객체를 생성한다.
  - 순수 JDBC는 SQL을 실행할 때마다 커넥션을 맺고 끊는 I/O 작업을 하는데, 이 작업은 상당한 양의 리소스를 잡아먹는다.
  - 이 문제의 해결책으로 커넥션 풀이 등장하게 되었다.
  - **데이터 소스는 커넥션 풀을 지원하기 위한 인터페이스**이다.

- sqlSessionFactory
  - SqlSessionFactory 객체를 생성한다.
  - SqlSessionFactory는 DB 커넥션과 SQL 실행에 대한 모든 것을 갖는 객체이다.
  - SqlSessionFactoryBean은 FactoryBean 인터페이스의 구현 클래스로, 마이바티스와 스프링의 연동 모듈로 사용된다.
  - 쉽게 말해 factoryBean 객체는 데이터 소스를 참조하며, XML Mapper(SQL 쿼리 작성 파일)의 경로와 설정 파일 경로 등의 정보를 갖는 객체이다.

- sqlSession
  - SqlSessionTemplate은 SqlSessionFactory를 통해 생성된다.
  - SqlSessionTemplate은 DB의 커밋, 롤백 등 SQL의 실행에 필요한 모든 메서드를 갖는 객체이다.