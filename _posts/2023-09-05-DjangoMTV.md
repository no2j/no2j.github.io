---
title : Django - MVC 패턴 & MTV 패턴
date : 2023-09-05 21:40:43 +09:00
categories : [Study,Django]
tags : [Django]
---

## MVC (Model, View, Controller)
<hr>

- Model : 데이터를 처리하며 데이터베이스와 상호작용하는 인터페이스 역할

- View : 브라우저에서 실제 사용자에게 표시되는 로직을 처리하여 UI로 나타냄

- Controller : View에서 흐름을 처리하거나 Model의 데이터 업데이트를 처리하는 로직을 제공

- `정리` : Model을 통해 데이터를 처리하고 View를 통해서 사용자의 데이터를 얻으며 Model을 통해 View를 변경하거나 데이터를 업데이트 하여 로직을 구현하는 것

<br><br>

## MTV (Model, Template, View)
<hr>

- Model : 사용자가 사용할 데이터를 정의하고 관리하는데 DB에 저장되는 데이터를 의미한다.
    - MVC 패턴의 Model에 대응한다.

<br>

- Template : 사용자에게 보여지는 화면(User Interface)에 해당한다.
  - MVC 패턴의 View에 대응한다.

<br>

- View : 요청에 따라 적절한 로직을 수행하여 결과를 Template으로 렌더링하며 응답한다.
  - MVC 패턴의 Controller에 대응한다.

<br>

`MVC 패턴과 MTV 패턴 비교`

![MVT](https://github.com/no2j/no2j.github.io/assets/106552182/b9e9cdc0-94cb-4ef6-b96d-73870565fd7f)

<br><br>

`MTV 패턴 동작 방식`
![MTV2](https://github.com/no2j/no2j.github.io/assets/106552182/a989c428-92d2-4406-84a6-fe0c534d2128)
1. 클라언트로부터 요청을 받으면 URLconf 모듈을 이용하여 URL을 분석하고 그 결과를 통해 해당 URL에 매칭되는 view 를 실행한다. 
2. view는 자신의 로직을 실행하고, 데이터베이스 처리가 필요하면 model을 호출한다.
3. model은 호출받으면 데이터베이스에서 필요한 값을 받아오고 그 결과를 view에 반환받는다.
4. view는 자신의 로직 처리가 끝나면 template을 사용하여 클라이언트에 전송할 HTML 파일을 생성한다
    - View에서 로직을 처리한 후 HTML 파일을 context와 함께 렌더링하는데 이 때의 HTML 파일을 Template이라 칭한다.
5. view는 최종 결과로 HTML 파일을 클라이언트에게 보낸다

<br><br>

`URLconf 란`
![urlconf](https://github.com/no2j/no2j.github.io/assets/106552182/161d182f-5330-4215-bb3e-01d8e58902e8)

- 클라이언트가 페이지를 요청하기 위해 서버에 URL을 보내면 서버는 URLconf 모듈을 이용해 URL을 분석한다.
- URL/View 맵핑을 위해 사용하는 path 함수는 함수 내 변수로 주어진 URL과 요청받은 URL이 일치하면 특정 View를 호출한다.

<br><br>

## MVC 패턴과 MTV 패턴의 차이점
<hr>
<br>

`MVC 패턴의 상호작용`
![MVC상호작용](https://github.com/no2j/no2j.github.io/assets/106552182/2650f8ae-06d7-410a-826f-fc39a1fd348e)

- MVC 패턴에서는 실제 사용자(Browser)와 Controller가 상호작용을 한다.

<br>

`MTV 패턴의 상호작용`
![MTV상호작용](https://github.com/no2j/no2j.github.io/assets/106552182/beb4cb53-e63e-4793-a165-d9ceae8c5b97)

- MTV 패턴에서는 실제 사용자가 Django와 상호작용을 한다.