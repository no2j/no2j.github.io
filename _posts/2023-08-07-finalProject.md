---
title : 파이널프로젝트
date : 2023-08-07 15:24:37 +09:00
categories : [Project,FinalProject]
tags : [finalproject]
---

## 프로젝트 명 : FEASIBLE UNIVERSITY
<hr>

### 개발목표
- 기존의 대학교 홈페이지를 사용했던 경험을 바탕으로 너무 많은 정보로 인해 찾고자 하는 정보를 찾기가 어려웠던 경험이 있었습니다.
불편했던 경험을 보완하여 사용자 중심의 홈페이지와 필요로 하는 정보를 쉽게 얻을 수 있도록 하기 위해서 직관적인 카테고리와 정보에 대한 우선순위를 정하여 중요한 정보를 더 눈에 띄게 배치하는 것을 중점으로 개발하였습니다.

### 사용기술 및 개발환경
- 운영체제 : Window OS
- 사용언어
   - Front : JavaScript, HTML5, CSS3
   - Back : Java 1.8
- FrameWork / Library : Spring, Mybatis, Maven, Project Lombok, AJAX
- DB : Oracle SQL Developer 21c
- Tool : STS(Spring Tool Suite), Visual Studio Code
- WAS : Apache Tomcat 9.0
- Collaboration : Github

### 구현기능

#### ※ 담당기능 √ 표시
- [ ] 메인페이지 : 학교 소개, 학사 소개, 학사일정, 공지 게시판, 종합정보시스템
- [x] 종합정보시스템 메인페이지 : [**√**] 로그인 및 아이디 찾기 / 비밀번호 초기화, [**√**] 날씨 및 미세먼지 정보, 공지사항(미리보기), FAQ(미리보기)
- [x] 종합정보시스템 (학생 전용) : 학사관리 ( [**√**] 졸업사정표 ), [**√**] 수강 신청, [**√**] 챗봇, 등록/장학, 상담관리, 수업 관리
- [ ] 종합정보시스템 (교직원 전용) : 급여 관리, 학사관리, 상담관리, 강의 관리, 수업 관리
- [ ] 종합정보시스템 (임직원 전용) : 금전 관리, 학사관리, 강의 관리

### 상세 구현 기능
- 로그인 관련 기능구현
  - 아이디 저장 - (Cookie 사용)
  - 자동로그인 - (Cookie 사용)
  - 비밀번호 암호화 판별 - (BCryptPasswordEncoder)
- 아이디 찾기 및 비밀번호 초기화
  - E-mail방식 - (Javax.mail.API)
  - SMS방식 - (Naver SENS API)
- 졸업 사정표 구현
- 수강 신청 관련 기능 구현
  - Synchronized 사용 : 수강인원 초과를 방지하기 위해서 사용
  - Transactional 사용 : 수강 신청 과정 중 에러 발생 시 복구를 위해서 사용
- 챗봇 기능 구현
  - 2중 for 문 사용 : 사용자 입력 값 경우의 수 처리를 위해 사용
  - HashSet 사용 : 중복 값을 처리하기 위해 사용
- 날씨 및 미세먼지 정보 기능 구현 : 공공데이터 API
  - Cache 사용 : 트래픽을 최소화하며 동일한 데이터를 받아볼 수 있도록 하기 위해 사용
  - Scheduled 사용 : 정해진 시간마다 데이터의 재갱신을 위해서 사용
  - Synchronized 사용 : 두 곳에서 호출하기 때문에 반복 요청 방지를 위해서 사용
- 페이지 interceptor 처리
  - 권한 관리와 보안 강화를 위해서 사용
  
### ERD 설계
![전체ERD](https://github.com/no2j/no2j.github.io/assets/106552182/85c864ae-0db6-42ed-86fb-f35b712313ad){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

<hr>

![123](https://github.com/no2j/no2j.github.io/assets/106552182/9431ddaa-7ad6-4b9b-b69d-6975be7f4e3d){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

#### ERD 설명
- 관리자와 교수의 중복되는 컬럼이 많아 PROFESSOR 테이블의 ADMIN 컬럼으로 식별
- PROFESSOR, STUDENT, CLASS 테이블에 STATUS 컬럼으로 상태 값 변경에 대한 상황 고려
- 수강 신청 및 시간표 중복 방지를 위해 CLASS 테이블에 CLASS_HOUR 컬럼 사용하여 추가데이터 고려
- CALENDAR 테이블을 사용하여 수강 신청 기간에만 신청할 수 있도록 설정
<br><br>

## 기능구현
### 로그인 기능
---
<img src="https://github.com/no2j/no2j.github.io/assets/106552182/c516202a-9504-4102-9365-57c2f7a23233" width="500" height="500" style="border:1px solid #606060; border-radius: 7px; padding : 0px;">
1. 아이디 및 비밀번호 빈값 처리
2. 아이디 저장 기능
3. 로그인 상태 유지(자동 로그인) 기능
4. 아이디 찾기 및 비밀번호 초기화

#### SMS인증을 이용한 ID찾기
---
<img src="https://github.com/no2j/no2j.github.io/assets/106552182/cfa5c1e9-9993-4342-b49c-650e3110811f"  width="500" height="350" style="border:1px solid #606060; border-radius: 7px; padding : 0px;">

- ID 찾기 및 비밀번호 초기화 버튼을 클릭 시 나오게 되는 페이지
- SMS 인증 또는 이메일 인증 중 선택하여 인증 진행
<br><br>

<img src="https://github.com/no2j/no2j.github.io/assets/106552182/e27317ea-be36-4a2d-9ed8-3a18756eb2d3" width="500" height="350" style="border:1px solid #606060; border-radius: 7px; padding : 0px;">

![id찾기_인증번호](https://github.com/no2j/no2j.github.io/assets/106552182/da037354-b194-4109-98b1-67475b1dbc7c){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
<img src="https://github.com/no2j/no2j.github.io/assets/106552182/49e03053-dfa4-4464-ab43-33c0524ed542" width="500" height="200" style="border:1px solid #606060; border-radius: 7px; padding : 0px;">

- 학생 및 임직원의 이름과 등록된 휴대폰 번호를 입력받아 학교에 등록된 사용자인지 판별
- 판별 후 해당 사용자의 휴대폰 번호 또는 이메일로 인증번호를 발송하여 진행
  - 인증번호 유효시간은 총 5분이며 유효시간 초과 시 인증번호 입력영역과 인증 완료 버튼이 사라지며 다시 인증요청 버튼으로 변경됨
- 인증이 완료되었다면 해당 사용자의 ID를 표시

#### 이메일인증을 이용한 비밀번호 초기화
---
<img src="https://github.com/no2j/no2j.github.io/assets/106552182/43ee05b8-01b1-43c3-8716-81d8b6fa70e0" width="600" style="border:1px solid #606060; border-radius: 7px; padding : 0px;">
<img src="https://github.com/no2j/no2j.github.io/assets/106552182/a44a4af9-cd94-476a-a3ae-1457bda72b03" width="600" style="border:1px solid #606060; border-radius: 7px; padding : 0px;">

- ID 찾기와 달리 사용자의 아이디와 휴대폰 번호를 입력받은 후 인증을 진행
- 총 5분의 인증 유효시간이 존재하며 시간 연장 버튼을 클릭 시 인증 유효시간을 다시 5분으로 설정
<br><br>

<img src="https://github.com/no2j/no2j.github.io/assets/106552182/01812c06-7718-4fb3-aaf4-e337cef3243c" width="700" style="border:1px solid #606060; border-radius: 7px; padding : 0px;">
- 사용자의 등록된 이메일주소로 인증 번호를 발송
<br><br>

<img src="https://github.com/no2j/no2j.github.io/assets/106552182/32f430eb-20a6-4144-be7d-dbe7d32df686" width="850" style="border:1px solid #606060; border-radius: 7px; padding : 0px;">

- 인증 완료 후 비밀번호를 재설정하는 사진 
- onkeyup 키보드 이벤트를 사용하여 첫 입력영역과 두 번째 입력영역이 다를 경우 일치하지 않는다는 안내 문자가 나오게 되며 비밀번호 변경 버튼이 비활성화
- 두 개의 입력영역이 일치할 경우 비밀번호 변경 버튼이 활성화되며 비밀번호 재설정이 가능하게 됨

### 날씨 및 미세먼지 정보
---
![Weather](https://github.com/no2j/no2j.github.io/assets/106552182/016d57e8-b8cd-4443-ad4c-57962f72e135){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 사용 API 정보
  - 공공데이터 기상청_단기예보(구 동네예보) API
  - 공공데이터 한국환경공단_에어코리아_대기 오염정보 API

- 제공하는 데이터 정보
  1. 초단기 온도
  2. 날씨 아이콘 및 하늘 상태
  3. 최저온도 및 최고온도
  4. 미세먼지 및 초미세먼지
<br><br>

- 총 4개의 API를 호출 : 네 가지의 데이터를 빠르게 요청하고 받아오기 위해 AJAX를 사용하여 비동기식 처리 진행
- 캐시(Cache) 사용 : 모든 사용자가 동일한 데이터를 받아볼 수 있고, 트래픽을 최소화하기 위해서 캐시 사용
- Scheduled(스케쥴러) 사용 : 매 10분, 매 30분, 23시마다 해당하는 메소드를 스케쥴러를 사용하여 데이터 재갱신
- 사용자에게 더 원활한 데이터 제공을 위해 메인 홈페이지에 날씨 및 미세먼지 API를 미리 호출하였으며 API 호출은 메인페이지와 종합정보시스템 메인페이지 총 두 곳에서 호출(총 두 번의 요청 발생 방지를 위해 Synchronized 사용)
<br><br>

![Weather2](https://github.com/no2j/no2j.github.io/assets/106552182/259eeb94-e651-4896-9487-a4fb1dc91e4c){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 데이터 요청 오류 및 데이터 재요청 횟수 도달 시 GIF 형식의 이미지를 사용하여 사용자에게 다른 페이지로 이동 및 새로고침으로 데이터 재요청 유도

### 졸업사정표
---
![졸업사정표](https://github.com/no2j/no2j.github.io/assets/106552182/bc6c78d0-6175-4e8b-9933-75f8dcc24be8){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 페이지 설명
  1. 로그인 한 학생의 졸업 관련 정보
  2. 전체 이수 현황으로 기준학점에 따라 이수학점과 비교하여 판정이 결정되고 최종적으로 판정 결과를 통해 최종 졸업 판정을 확인 가능
  3. 요약 코멘트가 들어가는 영역이며 교양(교공, 교일) 및 전공 기준학점 미달사항에 대해서 요약 확인 가능
  4. 이수 현황을 상세 조회할 수 있는 버튼이며 해당 버튼 클릭 시 모달을 이용하여 상세 조회 가능
<br><br>

#### 최종 판정 결과 도출 과정
![졸업사정표_최종판별](https://github.com/no2j/no2j.github.io/assets/106552182/8177534a-e7e9-4802-b116-049430dd8de1){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 전체 이수 현황표의 판정 중 불합격이 하나라도 있다면 최종 판정 결과에 불합격이 들어가도록 설정하였음
<br><br>

#### 이수 현황 상세 조회
![졸업사정표_상세조회](https://github.com/no2j/no2j.github.io/assets/106552182/7bf1e395-dd95-4098-9a6d-b1b6a70394e6){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 상세 조회 버튼 클릭 시 이수 및 이수하지 못한 과목들도 조회할 수 있으며 총계를 통해 이수학점을 확인할 수 있는 modal 창
<br><br>

### 수강신청
---
![수강신청](https://github.com/no2j/no2j.github.io/assets/106552182/e6cd823c-9ce1-4227-98b2-ba3adc74ac2e){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 페이지 설명
  1. 현재 연도와 학기의 정보를 알 수 있는 영역
  2. 학생의 필요에 따라 선택하여 수강 신청할 수 있도록 수강 신청방식을 5가지로 구분
  3. 수강신청 영역
     - 선택한 수강 신청방식에 따라 카테고리 선택 및 검색 영역이 나오고 선택 및 검색을 사용하게 되면 선택한 값에 따라 해당 과목들이 조회가 가능
     - 이미 수강 신청한 과목은 조회가 되지 않도록 설정
     - 수강인원이 최대로 찼다면 수강인원 영역의 신청 수가 빨간색으로 표시되어 식별이 가능하고 수강 신청 버튼 클릭 시 수강 신청할 수 없다는 alert 메세지가 나오게 됨
     - Synchronized 어노테이션을 사용하여 수강인원 초과를 방지하였음
     - 한 학기당 신청 학점 합계가 20학점을 초과할 수 없도록 설정하였음
     - **수강 신청 버튼 클릭 시(내부 작동 순서)**
        1. 해당 강의 조회
        2. 수강인원 체크
        3. 신청하려는 강의가 현재 학생이 신청한 강의 중에 시간이 겹치는 강의가 있는지 체크
        4. 데이터 등록 ( Transactional 어노테이션을 사용하여 밑 과정 중 실패 시 rollback 시키도록 설정 )  
             1. 해당 강의 시작 시간의 데이터를 추가
             2. 신청한 과목이 장바구니에 있으면 장바구니에서 데이터를 삭제
             3. 2시간짜리 강의일 경우 시작 시간 +1한 데이터를 추가
  4. 수강 신청 내역
      - 신청한 과목들의 학점 합계를 확인 가능
      - 수강취소 버튼 클릭 시 AJAX를 통해 페이지 새로고침 없이 취소 가능
<br><br>

### 수강취소
---
![수강취소](https://github.com/no2j/no2j.github.io/assets/106552182/c4068688-3158-4ee8-bf51-316865e00e9b){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

 - 페이지 설명
   - 수강 신청 기간이 아닐 때 수강 신청 탭을 이용할 수 없으므로 해당 페이지에서 수강을 취소할 수 있음
   - 현재 연도, 학기에 해당하는 수강 신청 내역을 조회가 가능한 페이지이며 신청 학점 합계를 확인할 수 있고 수강취소 버튼을 클릭하여 해당 과목을 수강 철회 및 취소 가능
<br><br>

### 수강 신청 내역조회
---
![수강신청_내역조회](https://github.com/no2j/no2j.github.io/assets/106552182/dbc14110-e1ab-4cd2-8a5f-efd845aa6336){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 페이지 설명
  - 수강 신청 내역이 존재하는 연도들만 조회할 수 있도록 하였으며 연도와 학기를 설정하여 검색하는 방식과 이전, 다음 학기 버튼을 사용하여 검색하는 방식이 있다.
  - 설정한 연도와 학기에 해당하는 총 학점을 조회할 수 있다.
<br><br>

### 예비수강신청
---
![예비수강신청](https://github.com/no2j/no2j.github.io/assets/106552182/d2831561-6b36-4587-8173-060e142eed4c){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 페이지 설명
  - 해당 페이지는 수강 신청 전 미리 강의를 담아 놓을 수 있는 페이지이며 수강 신청 시 장바구니 카테고리를 사용하여 담아놓았던 강의를 조회할 수 있도록 하는 페이지입니다.
  - 수강 신청 페이지와 같은 기능을 하지만 수강 신청 조건인 수강인원 초과, 강의 시간 중복, 신청 최대 학점 초과를 요구하지 않는다는 것이 다른 점입니다.
<br><br>

### 챗봇
---
![챗봇](https://github.com/no2j/no2j.github.io/assets/106552182/a3855c72-d423-4b01-95b3-01e2ae7e91f4){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![챗봇2](https://github.com/no2j/no2j.github.io/assets/106552182/1c827360-de8c-4659-b259-9310ea9de46e){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 종이비행기를 클릭 시 챗봇을 시작할 수 있는 영역이 오픈되며 아래의 사진과 같이 사용자의 질문에 따라 질문내용과 유사하거나 같은 값을 가지고 있는 버튼들을 보여주며 버튼을 클릭하게 되면 버튼에 해당하는 페이지로 이동하게 되는 기능

![챗봇_결과](https://github.com/no2j/no2j.github.io/assets/106552182/67e66494-6242-48bf-b270-f447269f87de){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

1. 챗봇 시작하기를 클릭 시 나오게 되는 영역이며 큰 카테고리들을 예시로 보여줌
2. 예로 "휴학은 어디서 해야 하나요?" 라고 질문하였을 경우
3. "휴학"이라는 키워드를 가지고 있는 버튼들을 보여주며 버튼 클릭 시 해당하는 페이지로 이동

- **내부 작동 순서**
  - replace를 사용하여 공백을 제거
  - 이중 for 문을 사용하여 사용자 입력값의 경우의 수들을 처리
  - contains를 사용하여 경우의 수 처리한 값 중에 사용자가 입력한 값에 대한 관련 값 처리
  - HashSet을 사용하여 중복되는 값 처리
<br><br>

## 주요 기능 코드 상세 설명
---

### 날씨 및 미세먼지 정보
---
![1](https://github.com/no2j/no2j.github.io/assets/106552182/11646ca6-8f87-4030-92be-fa035f87c6e4){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![2](https://github.com/no2j/no2j.github.io/assets/106552182/a0490d84-6231-4696-bc61-1408756f2705){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![3](https://github.com/no2j/no2j.github.io/assets/106552182/7cd129c3-d643-4824-a010-4f4b9e412fdf){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![4](https://github.com/no2j/no2j.github.io/assets/106552182/070672a2-fb31-4626-a1c0-3c39d05241a6){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![5](https://github.com/no2j/no2j.github.io/assets/106552182/66b2c774-60df-447d-8b2c-7687823a923b){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![6](https://github.com/no2j/no2j.github.io/assets/106552182/6a8584fd-bf15-445b-ac77-a114e77dc7e1){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
<br><br>

### 수강신청
---

#### 수강신청 - 강의 목록 조회 (Controller)
![수강신청_코드상세1](https://github.com/no2j/no2j.github.io/assets/106552182/6d53b556-ce62-4ebe-9795-463d1fce5042){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
  - AJAX를 사용하여 값을 받아오기 위해 ResponseBody 어노테이션을 사용
  - RequestParam 어노테이션을 사용하여 departmentName의 기본값을 "교양"으로 설정

#### 수강신청 - 강의 목록 조회 (Service)
![수강신청_코드상세2](https://github.com/no2j/no2j.github.io/assets/106552182/518ed76a-5752-4080-a155-0ca6c0510af9){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
#### 수강신청 - 강의 목록 조회 (Dao)
![수강신청_코드상세3](https://github.com/no2j/no2j.github.io/assets/106552182/849dce9c-379c-426f-9a5a-6b3ba2024a37){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
#### 수강신청 - 강의 목록 조회 (mapper)
![수강신청_코드상세4](https://github.com/no2j/no2j.github.io/assets/106552182/20534192-3d7a-4458-9a76-9b7e0fbe0400){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
- 값을 DB에서 정리하여 처리
  - "시간/학점"으로 값을 보내주기 위해 두 개의 값을 CONCAT으로 처리
  - DECODE를 사용하여 조건처리 후 설정한 값으로 변경하였으며 공백도 값 사이사이에 추가하여 요일, 강의 시간, 강의실을 합치도록 하였음
- 쿼리문 조건
  1. 강의가 활성화되어 있는지 판별
  2. 해당 학생이 이미 신청한 강의인지 판별
  3. 현재 연도와 학기 일치 판별
  4. F 학점 받은 강의는 재수강할 수 있지만 F 학점 이상을 받은 강의일 경우 재수강을 할 수 없게 설정
  5. Mybatis 동적쿼리 사용하여 판별
       - 교수 이름
       - 강의 이름
       - 전공 이름으로 검색
- 수강 대상 오름차순, 강의번호 오름차순 정렬
<br><br>

#### 수강신청 - 수강신청 버튼 클릭 시 (Controller)
![수강신청_코드상세5](https://github.com/no2j/no2j.github.io/assets/106552182/e15b9906-18f3-4eec-95d5-c5bc08243106){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
- Synchronized 어노테이션을 사용하여 수강인원 초과 신청 방지
- AJAX를 사용하여 값을 받아오기 위해 ResponseBody 어노테이션을 사용
- 작동 순서
  1. 해당 강의 수강 신청 인원 체크
  2. 신청내역 중 해당 강의 시간과 겹치는 강의 유무 체크
  3. 최종 수강 신청 가능 여부 판별
<br><br>

#### 수강신청 - 수강신청 버튼 클릭 시 (Service)
![수강신청_코드상세6](https://github.com/no2j/no2j.github.io/assets/106552182/f0f374f5-eaad-487c-83ae-4c14018ceb0d){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
- Transactional 어노테이션을 사용하여 수강 신청 과정 중 하나라도 실패 시 rollback 시키도록 설정
- 작동 순서
  1. 해당 강의 시작 시간의 데이터를 추가
  2. 신청한 과목을 장바구니에서 삭제
  3. 2시간짜리 강의일 경우 시작 시간 +1한 데이터를 추가
<br><br>

#### 수강신청 - 수강신청 버튼 클릭 시 (Dao)
![수강신청_코드상세7](https://github.com/no2j/no2j.github.io/assets/106552182/65010163-959a-4bbb-baf5-4a4b01d4e73c){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
#### 수강신청 - 수강신청 버튼 클릭 시 (mapper)
![수강신청_코드상세8](https://github.com/no2j/no2j.github.io/assets/106552182/b6902d57-d79a-4072-9c5a-39ed541bf596){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
- 수강 신청 - 수강 신청 (강의 시간 체크)의 동적쿼리에서 AND가 OR보다 우선순위가 높으므로 괄호를 사용하여 OR 연산자를 먼저 처리
<br><br>