---
title : 세미프로젝트
date : 2023-08-02 10:45:17 +09:00
categories : [project,semiproject]
tags : [semiproject]
---

## 프로젝트 명 : 파랑서점
<hr>

### 개발 목표
- 사용자들의 편리한 이용을 위한 도서 쇼핑몰 사이트 구축
- 화면구현부터 백엔드까지 전반적인 구현 경험
- 게시판 CRUD 구현 및 AJAX 비동기 방식으로 데이터 갱신

### 사용기술 및 개발 환경
- 운영체제 : Window OS
- 사용언어 
  - Front :  javascript, jQuery, HTML5, CSS3, AJAX
  - Back : JAVA 1.8, JSP & Servlet
- DB : Oracle SQL Developer 21c
- Tool : Eclipse, Visual Studio Code
- WAS : Apache Tomcat 9.0
- Collaboration : Github

### 구현기능

#### ※ 담당기능 √ 표시
- [ ] 메인페이지 : 회원가입 및 로그인 기능, 통합검색기능
- [ ] 도서 구매페이지 : 전체도서, 베스트 셀러, 신간 도서, 결제 기능
- [ ] 관련상품 구매페이지
- [ ] 출석 체크 페이지
- [ ] 고객센터 페이지
- [x] 관리자 페이지 : 회원관리, 상품관리, 주문관리, 게시판 관리

### 담당 역할
- 팀 조장
- 관리자 페이지 구현
  - 각 페이지별 키워드 검색 기능
  - 회원 관리 페이지(회원 조회, 수정, 삭제)
  - 상품관리 페이지(입출고 조회, 전체상품 조회, 수정, 삭제)
  - 주문관리 페이지(주문내역 조회)
  - 게시판 관리 페이지(공지사항 작성, 수정, 삭제(사진 업로드 가능), 1:1 문의 조회, 답변, 수정, FAQ 작성, 수정, 삭제)

### ERD 설계
![semiProject_ERD](https://github.com/no2j/no2j.github.io/assets/106552182/e23b428b-1a47-4421-b905-d1b53819eb7f){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

#### ERD 설명
1. 회원 관리
   - 회원 등급을 통한 혜택 설정
   - 상태 값 컬럼으로 재가입에 대한 상황도 고려
2. 게시판 관리
   - 답변 여부 컬럼으로 1:1문의 답변상태 체크 가능
   - 삭제 여부 컬럼으로 게시글 복구에 대한 상황도 고려
   - 게시판과 리뷰를 1:N 관계로 릴레이션을 맺음
3. 상품관리
   - 장바구니 테이블과 주문 테이블을 나누어 구매 확정 구분
<br><br>

## 기능구현

### 회원관리 페이지
---
![member_category_blankSearch](https://github.com/no2j/no2j.github.io/assets/106552182/c734da52-90e0-47af-b03b-2493d128036c){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

1. 검색 카테고리
   - 카테고리를 선택하여 세부적으로 검색할 수 있는 기능
2. 검색 빈값 처리
   - onsubmit을 사용하여 빈값 처리를 front 단에서 처리
<br><br>

![member_detail](https://github.com/no2j/no2j.github.io/assets/106552182/2aed8643-dc03-4604-8990-8dd70c273183){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![member_update](https://github.com/no2j/no2j.github.io/assets/106552182/d3d20607-7bcd-4dff-9641-a045affc4cc3){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 행 클릭 시 해당 회원의 상세 정보를 modal 창으로 확인 할 수 있는 기능
<br>

3. 회원 수정
   - 해당 회원의 정보를 수정할 수 있는 기능
4. 회원 삭제
   - 해당 회원 삭제 클릭 시 삭제 재확인 요청 후 삭제하는 기능
<br><br>

### 상품관리 페이지
---
![입고조회](https://github.com/no2j/no2j.github.io/assets/106552182/16177321-3e04-4c9c-b840-a9e2c1a35090){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 상품등록을 하면 해당 페이지에서 입고내역을 확인할 수 있으며 검색 카테고리를 사용하여 세부적으로 입고내역을 조회할 수 있는 페이지
<br><br>

![출고조회](https://github.com/no2j/no2j.github.io/assets/106552182/199b0218-46f5-4831-9999-132e0cbad1f4){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 회원이 상품을 구매할 경우 해당 페이지에서 출고내역에 대한 정보를 확인할 수 있으며<br>
  검색카테고리를 사용하여 세부적으로 출고내역을 조회할 수 있는 페이지
<br><br>

![상품관리](https://github.com/no2j/no2j.github.io/assets/106552182/891b8e69-f6f0-4d0e-aff8-3d73e208960c){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 전체 상품들의 정보들을 간략하게 제공하며 검색 카테고리를 사용하여 세부적으로 상품을 조회할 수 있고, 검색어에 대한 페이징 처리를 하여 페이징바를 이용할 수 있는 페이지
<br><br>

![상품상세조회](https://github.com/no2j/no2j.github.io/assets/106552182/dee57ba4-eefb-4de5-878c-b59b62631afa){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 게시글의 한 행을 클릭 시 해당 상품에 대한 상세 정보를 보여주며 자세한 상품정보와 <br>
  상품등록 시 사용하였던 이미지들도 확인할 수 있고 상품수정과 삭제를 할 수 있는 modal 창
<br><br>

![상품수정](https://github.com/no2j/no2j.github.io/assets/106552182/367866af-4452-4196-8467-c83646970bc6){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 상품수정 버튼을 클릭 시 이동되는 페이지이며 해당 상품에 대한 정보들을 변경할 수 있는 페이지
<br><br>

![상품등록](https://github.com/no2j/no2j.github.io/assets/106552182/dc0d54b7-5b76-4528-9e0a-cb785d5f15cc){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 두 개의 카테고리(도서 & 상품)로 나누어져 있으며 대표이미지(썸네일)와 상세 이미지를 설정할 수 있고 해당 상품의 정보를 입력한 후 상품을 등록할 수 있는 페이지
<br><br>


### 주문관리 페이지
---

![주문관리](https://github.com/no2j/no2j.github.io/assets/106552182/16f9a131-43d2-42c1-8084-5fe48c08a890){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

1. 검색카테고리
   - 카테고리를 선택하여 세부적으로 주문내역을 검색할 수 있는 기능
2. 결제상태
   - 총 4개의 결제상태가 존재하며 주문자의 결제요청에 따라 관리자가 체크 후 확인할 수 있는 기능
<br><br>

![주문내역상세조회](https://github.com/no2j/no2j.github.io/assets/106552182/22e6e653-cbf5-4058-9ef8-c2c5f8406ba9){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 주문내역의 한 행을 클릭 시 해당 주문내역을 상세 조회할 수 있는 기능
<br><br>

### 게시판관리 페이지
---
![게시판관리_공지사항](https://github.com/no2j/no2j.github.io/assets/106552182/2365b263-359a-4b74-b43c-dbee719b413a){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

1. 검색카테고리
   - 카테고리를 선택하여 세부적으로 공지사항을 검색할 수 있는 기능
2. 공지사항 등록
   - 관리자가 공지사항 등록할 수 있는 버튼
3. 공지사항 조회
   - 작성한 공지사항들을 간략하게 조회할 수 있으며, 한 행을 클릭 시 해당 게시글의 상세 정보를 조회할 수 있는 기능
<br><br>

![게시판관리_공지사항등록](https://github.com/no2j/no2j.github.io/assets/106552182/aee8bf1e-a03b-41e8-af2c-f64cb6c8ab67){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 공지사항등록 버튼을 클릭 시 해당 페이지로 이동되며 제목, 내용, 이미지를 넣어 작성할 수 있는 페이지
<br><br>

![게시판관리_공지사항상세조회](https://github.com/no2j/no2j.github.io/assets/106552182/e38a54e2-2ec5-45f3-b5d7-1566c6b16563){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 한 행을 클릭 시 해당 공지사항의 내용을 상세 조회할 수 있으며 수정 및 삭제를 할 수 있는 modal 창
<br><br>

![게시판관리_문의fix](https://github.com/no2j/no2j.github.io/assets/106552182/4872c0cb-bf4b-4ab1-b080-cc1325349955){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

1. 검색카테고리
   - 카테고리를 선택하여 세부적으로 문의 사항을 검색할 수 있는 기능
2. 답변 상태
   - 관리자의 답변내용 유무에 따른 답변 상태 표시가 다르게 나타나는 기능
<br><br>

![게시판관리_문의답변](https://github.com/no2j/no2j.github.io/assets/106552182/7265cf19-f834-4a21-99ab-38a275f58a73){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
- 해당 문의 클릭 시 답변 페이지로 이동되며 문의에 대한 답변 및 답변 수정을 할 수 있는 페이지
<br><br>

![게시판관리_FAQ](https://github.com/no2j/no2j.github.io/assets/106552182/74680f61-7292-4fe0-ad93-ed3242e0a93f){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

1. 검색카테고리
   - 카테고리를 선택하여 세부적으로 FAQ를 검색할 수 있는 기능
2. FAQ 등록
   - 해당 버튼을 클릭하여 FAQ를 등록할 수 있는 기능
3. 조회 및 수정, 삭제
   - 작성하였던 FAQ를 조회할 수 있으며 수정 및 삭제할 수 있는 기능
<br><br>

![게시판관리_FAQ등록](https://github.com/no2j/no2j.github.io/assets/106552182/cbae0c5e-df67-4322-a1d8-c82788007e9a){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
- FAQ 등록 버튼을 클릭 시 해당 페이지로 이동되며 제목 및 내용을 입력하여 FAQ를 등록할 수 있는 페이지
<br><br>

## 주요 기능 코드 상세 설명
---

### 검색기능을 사용하여 검색할 경우 해당 검색어에 대한 페이징바 처리

![주요코드리뷰](https://github.com/no2j/no2j.github.io/assets/106552182/4e5ef466-e9f8-4058-94f5-e4b06dfd24a2){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![주요코드리뷰2](https://github.com/no2j/no2j.github.io/assets/106552182/c8a13b0c-0b49-4a7d-ac7a-e79b5a0b4ce3){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

  - 검색기능사용 후 페이징바를 사용했을 경우 검색한 값들에 대한 판별 진행.
  - 검색한 값에 대한 페이징 처리와 데이터 조회 후 일반조회 서블릿으로 값 위임.
<br><br>

![주요코드리뷰3](https://github.com/no2j/no2j.github.io/assets/106552182/332d4a9a-4a06-4f41-9010-b7da5356659b){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}
![주요코드리뷰4](https://github.com/no2j/no2j.github.io/assets/106552182/1ff911ee-5d4e-467f-8f71-f4578c20264d){:style="border:1px solid #606060; border-radius: 7px; padding: 0px;"}

- 일반조회 서블릿에서의 조건문에  “list”의 값이 null이 아니므로 else 문으로 들어오게 되며 bar라는 변수를 1로 초기화 시켜주고 위임받은 값들을 받아주고 다시 담아서 JSP로 넘겨주기.

- 조건문에 변수 bar 가 1이므로 else 문으로 들어오게 되며 검색기능의 페이징바를 생성해 주고, 쿼리스트링으로 검색했던 값들을 다시 담아줌.