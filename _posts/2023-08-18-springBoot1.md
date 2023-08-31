---
title : Spring - 스프링 웹 개발 기초
date : 2023-08-18 15:39:43 +09:00
categories : [Study,Spring]
tags : [Spring]
---

## 라이브러리 살펴보기

### Gradle은 의존관계가 있는 라이브러리를 함께 다운로드 한다.

 - `Gradle의 특징`
   - 간결한 스크립트
     - Gradle은 Groovy를 사용하기 때문에 동적인 빌드는 Groovy 스크립트로 플러그인을 호출하거나 직접 코드를 짜면 된다.
   - 빌드 속도
     - Maven보다 최대 100배 빠르다.

### 스프링 부트 라이브러리

- spring-boot-starter-web
  - spring-boot-starter-tomcat : 톰캣(웹서버(WAS))
  - spring-webmvc : 스프링 웹 MVC
  
- spring-boot-starter-thymeleaf : 타임리프 템플릿 엔진(View)
- spring-boot-starter(공통) : 스프링부트 + 스프링 코어 + 로깅
  - spring-boot
    - spring-core
  - spring-boot-starter-logging
    - logback,slf4j

### 테스트 라이브러리
- spring-boot-starter-test
  - junit : 테스트 프레임워크
  - mockito : 목 라이브러리
  - assertj : 테스트 코드를 조금 더 편하게 작성하게 도와주는 라이브러리
  - spring-test : 스프링 통합 테스트 지원

## View 환경설정

### Welcome Page 만들기
- Welcome Page : 도메인을 눌렀을 때 나오는 첫 페이지를 의미

<br>

`resources/static/index.html`

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Hello</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
</head>
<body>
    Hello
    <a href="/hello">hello</a>
</body>
</html>
```

- 스프링 부트가 제공하는 Welcome Page 기능
  - `static/index.html`을 올려두면 Welcome Page 기능을 제공한다.
  
### thymeleaf 템플릿 엔진
- thymeleaf 공식 사이트 : https://www.thymeleaf.org/
- 스프링 공식 튜토리얼 : https://spring.io/guides/gs/serving-web-content/
- 스프링부트 메뉴얼 : https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-template-engines

### thymeleaf 템플릿 엔진 동작 환경
![thymeleaf_템플릿엔진_동작환경](https://github.com/no2j/no2j.github.io/assets/106552182/0b95bb4d-488f-4a62-bfea-c627c5a4031b)

- 동작 환경 순서
  1. 웹 브라우저에서 스프링에 주소를 보내준다.
  2. 스프링부트는 톰켓(웹서버)을 내장하고 있기 때문에 톰켓이 먼저 요청받은 후 스프링에 요청을 보낸다.
  3. 스프링에서 컨트롤러에 해당하는 매핑 주소를 찾고 해당 매핑주소의 메소드를 실행시킨다.
  4. model에 데이터를 넣어준 후 해당 메소드의 return 값과 일치하는 view 페이지로 넘어가게 된다.
  5. 페이지 이동 후 model의 키값과 동일한 타임리프 변수에 데이터를 넣어준다.
  6. 웹 브라우저에 반환

<br>

- 컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(`viewResolver`)가 화면을 찾아서 처리한다.
  - 스프링 부트 템플릿엔진 기본 viewName 매핑
  - `resources:templates/` + {ViewName} + `.html`

참고 : `spring-boot-devtools` 라이브러리를 추가하면, `html`파일을 컴파일만 해주면 서버 재시작없이 View 파일 변경이 가능하다.

## 스프링 웹 개발 기초
- 정적 컨텐츠
- MVC와 템플릿 엔진
- API

### 정적 컨텐츠
- 스프링 부트 정적 컨텐츠 기능

`resources/static/hello-static.html`
```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	정적 컨텐츠 입니다.
</body>
</html>
```
<br>

`정적 컨텐츠 동작 원리`
<br>

![정적컨텐츠](https://github.com/no2j/no2j.github.io/assets/106552182/c27b708b-32ac-4db4-b90a-ad8171402e0e)

1. 웹 브라우저에서 주소를 보내준다.
2. 톰켓이 요청받고 스프링에 넘겨준다.
3. 스프링은 첫 번째로 해당 매핑주소가 있는지 controller에서 찾아본다. (컨트롤러가 우선순위를 가진다는 것을 의미), 해당하는 컨트롤러가 없으면 resources:static/뷰 이름.html을 찾아서 있다면 반환을 해준다.

### MVC와 템플릿 엔진
- MVC : Model, View, Controller

`Controller`
```java
@Controller
public class HelloController{

    @GetMapping("hello-mvc")
    public String helloMvc(@RequestParam("name") String name, Model model){
        model.addAttribute("name", name);
        return "hello-template";
    }
}
```
<br>

`View`
`resources/template/hello-template.html`
```html
<!DOCTYPE html>
<html xmlns:th = "http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```
<br>

`MVC, 템플릿 엔진 이미지`
![MVC_템플릿 엔진](https://github.com/no2j/no2j.github.io/assets/106552182/91f877d6-1889-4aaa-aed7-873cb9857038)
1. 웹 브라우저에서 주소를 넘겨준다.
2. 톰켓에서 요청을 받고(hello-mvc) 스프링에 넘겨준다.
3. 스프링은 컨트롤러에서 해당 매핑주소가 있는지 찾고 해당 메소드를 실행시키고 return 값과 model 값을 스프링에 넘겨준다.
4. 스프링은 viewResolver에 해당 return 값과 일치하는 뷰를 찾아서 Thymeleaf 템플릿 엔진에 처리 요청을 보낸다.
   - viewResolver는 뷰를 찾아주고, 템플릿 엔진을 연결시켜준다. 
5. 템플릿 엔진이 렌더링해서 변환된 HTML을 웹 브라우저에 반환한다.

`정적 컨텐츠와의 차이점 : 변환 유무의 차이`

### API
 `@ResponseBody 문자 반환`
 ```java
 @Controller
 public class HelloController{

	@ResponseBody
	@GetMapping("hello-string")
	public String helloString(@RequestParam("name") String name) {
		return "hello " + name;
	}

 }
 ```

 - `@ResponseBody`를 사용하면 뷰 리졸버(`viewResolver`)를 사용하지 않음
 - 대신 HTTP의 BODY에 문자 내용을 직접 반환

<br>

`@ResponseBody 객체 반환`
```java
    @Controller
    public class HelloController{

        static class Hello {
            private String name;

            public String getName() {
                return name;
            }

            public void setName(String name) {
                this.name = name;
            }
        }

        @ResponseBody
        @GetMapping("hello-api")
        public Hello helloApi(@RequestParam("name") String name) {
            Hello hello = new Hello();
            hello.setName(name);

            return hello; 
        }
	
    }
```
`@ResponseBody`를 사용하고, 객체를 반환하면 객체가 JSON으로 반환됨
<br>

`@ResponseBody 사용 원리`
![ResponseBody_사용원리](https://github.com/no2j/no2j.github.io/assets/106552182/05803216-0874-4afc-836d-6221a8edfbf4)

- < 작동 순서 >
  1. 웹 브라우저가 요청을 보내면 톰켓이 요청받고 톰켓은 스프링에 넘겨주며 해당 매핑 주소를 찾는다.
  2. 컨트롤러에 해당 매핑 주소가 있고 @ResponseBody의 어노테이션이 붙어있다면 HttpMessageConvertert가 동작한다.
  3. return 값이 문자라면 StringConverter가 동작하고, 객체라면 JsonConverter가 동작한다.
  4. 컨버터에서 변환 후 웹 브라우저로 보내준다.
<br><br>

- `@ResponseBody`를 사용
  - HTTP의 BODY에 문자 내용을 직접 반환
  - `viewResolver`대신에 `HttpMessageConverter`가 동작
  - 기본 문자처리 : `StringHttpMessageConverter`
  - 기본 객체처리 : `MappingJackson2HttpMessageConverter`
  - byte 처리 등등 기타 여러 HttpMessageConverter가 기본으로 등록되어 있음

`Gson과 Jackson의 차이점은?`
- Jackson은 Spring 프레임워크에 내장되어 있으며, 고용량의 JSON 데이터를 처리할 때 성능이 좋다.
- Gson은 라이브러리를 따로 추가해 줘야 하며, 가벼운 JSON 데이터를 처리할 때 성능이 좋다.
<br><br>

참고 : 클라이언트의 HTTP Accept 해더와 서버의 컨트롤러 반환 타입 정보를 조합해서 `HttpMessageConverter`가 선택된다.
