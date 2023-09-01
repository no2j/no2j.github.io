---
title : Spring - MVC 패턴 & Spring Framework MVC
date : 2023-09-02 01:07:43 +09:00
categories : [Study,Spring]
tags : [Spring]
---

## MVC 란
![MVC](https://github.com/no2j/no2j.github.io/assets/106552182/c04b247c-fac5-49e8-a81a-2ee39c8b2c03)

- MVC는 Model - View - Controller의 약자이며, 애플리케이션을 구성하는 요소를 역할에 따라 세가지 모듈로 나누어 구분한 패턴이다.

1. Model(모델)
   - 애플리케이션의 데이터이며, 모든 데이터 정보를 가공하여 가지고 있는 컴포넌트이다.
   - 사용자가 이용하려는 모든 데이터를 가지고 있어야하며 View 또는 Controller에 대해 어떠한 정보도 알 수 없어야 한다.
   - Controller에서 받은 데이터를 저장하는 역할을 한다.
2. View(뷰)
   - 사용자에게 보여지는 화면, UI를 의미하며 Controller로 부터 받은 Model 데이터를 바탕으로 사용자에게 표현해준다.
3. Controller(컨트롤러)
   -  데이터와 비즈니스 로직 사이의 상호 동작을 관리한다. 즉, Model과 View를 통제한다는 의미.
   -  MVC 패턴에서 View와 Model이 직접적인 상호 소통을 하지 않도록 관리를 한다.

<br><br>

## MVC 패턴 Model1
![MVC1](https://github.com/no2j/no2j.github.io/assets/106552182/67f1ce15-c219-4a90-bfa6-e9961cdf2031)

- 특징 : View와 Controller를 모두 JSP가 담당하는 형태를 가진다.
- 장점 : JSP 하나로 사용자의 요청을 받고 응답을 처리하므로 구현 난이도는 쉽다.
- 단점 : 프로젝트 규모가 커질수록 코드가 복잡해져 유지보수가 힘들어진다.

<br><br>

## MVC 패턴 Model2
![MVC2](https://github.com/no2j/no2j.github.io/assets/106552182/33d05d26-470b-4698-aa2f-7b35abd8a72a)

- 특징 : Model1패턴과 달리 Controller, View가 분리되어 있다.
- 장점 : 유지보수에 용이하다.
- 단점 : Model1 보다 구조가 복잡해질 수 있다.

<br><br>

## Spring Framework의 MVC2 패턴
![springMVC](https://github.com/no2j/no2j.github.io/assets/106552182/26350082-3127-4956-b6a2-3a889df98cba)

Spring Framework에서 MVC2 모델을 조금 더 발전시켜 Spring MVC가 나왔으며 이는 MVC2 모델이 기반인 웹 모듈이다.

### 동작 순서
1. 사용자로부터 요청이 들어오면 DispatcherServlet (Front Controller)이 호출된다.
2. DispatcherServlet은 받은 요청을 HandlerMapping에게 던져준다. 요청받은 URL을 분석하여 적합한 Controller를 선택하여 반환한다.
3. DispatcherServlet은 HandlerAdapter를 호출한다. HandlerAdapter는 해당하는 Controller중 요청한 URL에 적합한 Method를 찾아준다.
4. Controller는 비즈니스 로직을 처리하고 Model 객체에게 요청에 맞는 View 정보를 담아 DispatcherServlet에게 전달한다.
5. DispatcherServlet은 ViewResolver를 호출하여 전달받은 정보를 기반으로 적합한 View를 찾아준다.
6. DispatcherServlet은 View객체에 처리결과를 넘겨 결과를 보여주도록 요청한다.
7. View 객체는 해당하는 View를 호출하고, 해당 View는 Model 객체에서 화면 표시에 필요한 객체를 가져와 화면 표시를 처리하며 사용자에게 응답하여 요청을 마친다.

<hr>

**HandlerMapping**
- 요청을 직접 처리할 컨트롤러를 탐색한다.

**HandlerAdapter**
- 매핑된 컨트롤러의 실행을 요청한다.

**Controller**
- 직접 요청을 처리하며 처리 결과를 반환한다.
- 결과가 반환되면 HandlerAdapter가 ModelAndView 객체로 변환되며 해당 객체에는 View Name과, 응답을 통해 보여줄 View에 대한 정보와 관련된 데이터가 포함되어 있다.

**View Resolver**
- View Name을 확인한 후 컨트롤러에게 받은 로직 처리 결과를 반영할 View 파일 (JSP)을 탐색한다.

**View**
- 로직 처리 결과를 반영한 최종 화면을 생성한다.

### 상세 설명

`web.xml`
![web_xml](https://github.com/no2j/no2j.github.io/assets/106552182/11d674cf-fe09-4362-8891-a73580c754b3)

1. Servlet은 웹 프로그래밍에서 Client의 요청을 처리하고 그 결과를 Client에게 전송하는 기술이다. Java를 이용하여 Web을 만들기 위해서 필요한 기술이다.
  - Servlet의 특징
        1. Client의 요청에 대해 동적으로 작동한다.
        2. HTML을 사용하여 요청에 응답한다.
        3. Java Thread를 이용하여 동작한다.
        4. MVC 패턴에서 Controller로 이용된다.
  - Spring Framework에서 Servlet은 DispatcherServlet을 이용한다. DispatcherServlet은 Front Controller를 담당하며 모든 HTTP 요청을 받아들여 다른 객체들 사이의 흐름을 제어한다.
  - Servlet의 요청에 관련된 객체를 정의하는 곳이 servlet-context.xml이다. 즉 Controller나 Annotation, ViewResolver 등을 설정해주는 곳이다.
2. Servlet 별칭을 통해 DispatcherServlet을 mapping 해준다.
    - url-pattern을 '/'로 설정하였기 때문에 Root 경로로 들어온 모든 요청을 처리할 수 있게 된다.
3. 모든 Servlet 및 Filter가 공유하는 Root Spring Container를 정의한다. ContextConfigLocation이라는 파라미터를 이용함으로써 ContextLoader가 호출할 수 있는 설정 파일을 여러 개 쓸 수 있다.
    - 여기서 사용되는 XML은 root-context.xml로 Root Context를 구성한다. 주로 Service나 Respository(DAO), DB 등 비즈니스 로직과 관련된 설정을 해준다.
4. 모든 Servlet 및 Filter가 공유하는 Spring Container를 생성한다.

<hr><br>
`servlet-context.xml`
![servletContext](https://github.com/no2j/no2j.github.io/assets/106552182/134edef5-b98f-4f5c-a879-5d2df933e213)

1. 적합한 Controller Annotation을 인식하도록 < annotation-driven/>을 사용한다. -> HandlerMapping 역할.
2. HTTP GET 요청을 통해 Spring에서 정적인 리소스인 CSS, HTML 등 파일들을 처리할 수 있도록 등록한다.
3. Controller가 반환한 View name을 기반으로 적합한 View(JSP)를 찾을 수 있도록 경로를 지정한다. ViewResolver 역할을 한다.
4. component-scan은 XML에 각 Bean을 일일이 지정하지 않고 @Component 어노테이션을 통해 자동으로 Bean을 등록시켜준다.<br>base-package 경로를 기반으로 Annotation을 식별하여 Bean을 생성한다.

![Component](https://github.com/no2j/no2j.github.io/assets/106552182/b91a1e1b-3d53-4c9b-a025-40c2be00cbd5)

<hr><br>

**`root-context와 servlet-context에 대해서`**

Spring은 계층구조를 가지는 multi context 환경을 구성할 수 있도록 해주며 이 spring의 context에 대한 설정을 크게 두가지로 나눈다면 root context와 servlet context가 있다.<br>
ContextLoaderListener와 DispatcherServlet은 각자 WebApplicationContext를 생성한다.<br>
ContextLoaderListener가 생성한 Context가 Root Context가 되고 DispatcherServlet이 생성한 인스턴스는 Root Context를 부모로 하는 자식 Context가 된다.<br>
자식 Context들은 Root Context의 설정 Bean을 사용할 수 있기 때문에 ContextLoaderListener를 공통으로 사용하는 Bean을 설정한다.

<br>

- ContextLoaderListener
  - Root Context
  - Web Application의 실제 비즈니스 혹은 목적을 위한 Service layer와 해당 service layer에서 조회 및 처리에 필요한 DataBase와 연결되는 repository layer를 구성하는 bean들에 대한 설정을 한다.
- DispatcherServlet
  - 자식 Context
  - Web Application의 Client의 요청을 받기 위한 entry point로서의 서블릿의 context 설정을 한다.
  - 즉 요청에 대한 처리를 해줄 Controller의 매핑설정(Handler Mapping)과 요청처리 후 View처리를 어떻게 할 것인가에 대한 설정(View Resolver)등이 존재한다.

<br>

이러한 Context의 특징을 알고 Bean을 설정할 때 component-scan을 통한 등록을 주의해야한다.<br>
양쪽 Context설정에서 component-scan을 통해 Bean을 등록할 때 Controller와 Service, repository의 각 Bean들이 등록되어야 하는데 Context에 맞게 등록이 되도록 해야한다.<br>
단순히 base-package에 대한 설정만으로 Scan을 하면 같은 Bean이 양쪽 Context에 모두 등록되어 불필요한 Bean등록이 발생한다.<br>
따라서 component-scan을 통한 각 context에 대한 Bean 등록 시 exclude, include를 적절히 사용하여 불필요한 중복 Bean이 등록되지 않도록 해야한다.

![multiContext](https://github.com/no2j/no2j.github.io/assets/106552182/dd915358-401f-4fc4-8b0f-df18c9e77288)