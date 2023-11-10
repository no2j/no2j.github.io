---
title : SpringBoot - Practice2
date : 2023-11-10 01:24:43 +09:00
categories : [Study,SpringBoot]
tags : [SpringBoot,Practice]
---

## 게시판 CRUD 처리
- CRUD란? : Created, Read, Update, Delete를 의미한다.

<br>

### 게시글 요청(Request) 클래스 생성
<hr>

- 이전 스프링 레거시에서는 테이블을 구조화한 클래스의 접미사로 VO 또는 DTO를 붙여 요청과 응답에 공통으로 사용하였었다.
- 이번에는 요청과 응답용 클래스를 분리해서 작업해보고자 한다.

<br>

```java
package com.study.board.domain.post;

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class PostRequest { // 게시글 생성 및 수정에 사용할 요청 클래스
	
	private long id;			// 게시글 번호
	private String title;		// 제목
	private String content;		// 내용
	private String writer;		// 작성자
	private boolean noticeYn;	// 공지글 여부
	
}
```

- 이전에는 Lombok 라이브러리의 @Data 어노테이션을 사용하였었다. <br> @Data에는 @ToString, @EqualsAndHashCode, @Getter, @Setter, @RequiredArgsConstructor를 같이 사용하게 되는 것이며 문제가 발생할 수 있는 단점이 있다. <br>

  1. 무분별한 Setter 남용
     - Setter는 그 의도가 분명하지 않고 객체를 언제든지 변경할 수 있는 상태가 되어서 객체의 안전성이 보장받기 힘들다.<br> 불필요한 변경 포인트를 제공하지 않음으로써 안정성을 취할 수 있다.
  2. @ToString : 양방향 연관관계 시 순환 참조
     - Member 와 Coupon 이 1:N 양방향으로 매핑되어 있는 상황을 가정할 수 있다. <br> 이때, ToString을 호출하면 무한 순환 참조가 발생한다. <br> 이러한 문제를 해결하기 위해서는 @ToString(exclude = "coupons") 처럼 어노테이션을 사용해서 특정 항목을 제외시키는 방법을 사용할 수 있다.
  3. @EqualsAndHashCode
     - @EqualsAndHashCode는 상당히 고품질의 euqals()와 hashCode() 메소드를 만들어준다. 따라서 잘 사용하면 좋지만, 남발하면 심각한 문제가 생긴다. <br> 특히 문제가 되는 점은 Mutable 객체에 아무런 파라미터 없이 그냥 사용하는 경우이다. <br>동일한 객체여도 필드 값을 변경시키면 hashCode가 변경되면서 찾을 수 없는 값이 되어버리기 때문이다.

<br>

### 게시글 응답(Response) 클래스 생성
<hr>

```java
package com.study.board.domain.post;

import java.time.LocalDateTime;

import lombok.Getter;

@Getter
public class PostResponse { // 사용자에게 보여줄 게시글 데이터를 처리할 응답용 클래스
	
  private Long id;                         // 게시글 번호
    private String title;                  // 제목
    private String content;                // 내용
    private String writer;                 // 작성자
    private int viewCount;                 // 조회 수
    private Boolean noticeYn;              // 공지글 여부
    private Boolean deleteYn;              // 삭제 여부
    private LocalDateTime createdDate;     // 생성일시
    private LocalDateTime modifiedDate;    // 최종 수정일시

	
}
```
<br>

### Mapper 인터페이스 생성
<hr>

- 요청,응답 클래스와 동일한 패키지에 생성.

```java
package com.study.board.domain.post;

import java.util.List;

import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface PostMapper {

	// 게시글 저장
	void save(PostRequest params);
	
	// 게시글 상세정보 조회
	PostResponse findById(Long id);
	
	// 게시글 수정
	void update(PostRequest params);
	
	// 게시글 삭제
	void deleteById(Long id);
	
	// 게시글 리스트 조회
	List<PostResponse> findAll();
	
	// 게시글 조회수
	int count();
}

```

`@Mapper` <br>
- Mapper에는 @Mapper 어노테이션을 필수적으로 선언해야 한다.
- MyBatis는 Mapper(Java 인터페이스)와 XML Mapper(실제로 DB에 접근해서 호출할 SQL 쿼리를 작성하는 파일)를 통해 DB와 통신한다.
  - 즉, XML Mapper에 SQL 쿼리를 선언해 두고 Mapper를 통해 SQL 쿼리를 호출하는 방식.
  - Mapper는 XML Mapper에 선언된 SQL 중에서 메서드 명과 동일한 id를 가진 SQL 쿼리를 찾아 실행한다. <br> 예로, 메서드명이 "savePost( )"라고 가정했을 때 SQL id는 "savePost"가 되어야 한다.
- Mapper와 XML Mapper는 XML Mapper의 namespace라는 속성을 통해 연결된다.

<br><br>

### SQL 쿼리 작성
<hr>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.study.board.domain.post.PostMapper">

	<!--
		<mapper> 태그
		- Mapper 인터페이스는 XML Mapper에서 메서드명과 동일한 id를 가진 SQL 쿼리를 찾아 실행
		
		<sql> 태그 & <include> 태그
		- 공통으로 사용되거나 반복적으로 사용되는 쿼리를 처리할 수 있다.
		- Java로 비유하자면, 변수로 선언해 두고 필요한 상황에 호출해서 사용하는 개념과 유사
		- 두 태그의 포인트는 중복 제거.
		- 동일한 XML Mapper뿐만 아니라, 다른 XML Mapper에 선언된 SQL 조각도 인클루드(Include) 할 수 있다.
		
		parameterType
		- SQL 쿼리 실행에 필요한 파라미터의 타입을 의미한다.
		
		resultType
		- SQL 쿼리의 실행 결과를 매핑할 결과 타입을 의미한다.
		- Mapper 인터페이스에 선언한 메서드의 리턴 타입과 동일한 타입으로 선언하면 된다.
		
		#{} 표현식
		- MyBatis는 #{ 변수명 } 표현식을 이용해서 전달받은 파라미터를 기준으로 쿼리를 실행한다.
		
	 -->

	<!-- BOARD 테이블 전체 컬럼 -->
	<sql id="postColumns">
		id,
		title,
		content,
		writer,
		view_count,
		notice_yn,
		delete_yn,
		created_date,
		modified_date
	</sql>
	
	<!-- 게시글 작성 -->
	<insert id="save" parameterType="com.study.domain.post.PostRequest">
		INSERT INTO BOARD(
			<include refid="postColumns"/>
		) VALUES(
			#{id},
			#{title},
			#{content},
			#{writer},
			0,
			#{noticeYn},
			0,
			NOW(),
			NULL
		)
	</insert>
	
	<!-- 게시글 상세 조회 -->
	<select id="findById" parameterType="long" resultType="com.study.domain.post.PostResponse">
		SELECT
			<include refid="postColumns"/>
		FROM
			BOARD
		WHERE
			id = #{value}
	</select>
	
	<!-- 게시글 수정 -->
	<update id="update" parameterType="com.study.domain.post.PostRequest">
		UPDATE BOARD
		SET 
			modified_date = NOW(),
			title = #{title},
			content = #{content},
			writer = #{writer},
			notice_yn = #{noticeYn}
		WHERE 
			id = #{id}
	</update>
	
	<!-- 게시글 삭제 -->
	<delete id="deleteById" parameterType="long">
		UPDATE BOARD
		SET
			delete_yn = 1
		WHERE
			id = #{id}
	</delete>
	
	<!-- 게시글 전체 조회 -->
	<select id="findAll" parameterType="com.study.domain.post.PostResponse">
		SELECT
			<include refid="postColumns"/>
		FROM
			BOARD
		WHERE
			delete_yn = 0
		ORDER BY
			id DESC
	</select>
	
</mapper>
```

<br><br>

### SELECT 컬럼과 멤버 변수 매핑(바인딩)
<hr>

- 대표적으로 세 가지의 방법이 있다.
  1. SELECT 하는 칼럼마다 별칭(Alias)을 지정한다.
  2. MyBatis의 ResultMap을 이용한다.
  3. properties(application.properties) 또는 mybatis-config 파일에 설정을 추가한다.
    <br><br>
  - 1번은 효율적인 방법이 아니며
  - 이전에 스프링 레거시에서 사용하였던 2번은 XML Mapper가 지저분해지는 느낌이므로
  - 단 한 줄의 코드로 SELECT 하는 모든 컬럼과 멤버 변수를 매핑해 주는 3번 방법을 사용할 것이다.

<br><br>

`application.properties` <br>
![4](https://github.com/no2j/no2j.github.io/assets/106552182/12a7f35e-1eb6-4639-a53f-0539cb026700)

<br><br>

`DatabaseConfig 클래스` <br>
![5](https://github.com/no2j/no2j.github.io/assets/106552182/a24a3c20-c49e-4259-b729-7177502cbcfe)

![6](https://github.com/no2j/no2j.github.io/assets/106552182/292f5e27-8163-4b15-be14-52ed77578632)

<br><br>

### Test 코드 작성
<hr>

```java
package com.study.board.post;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.datatype.jsr310.JavaTimeModule;
import com.study.board.domain.post.PostMapper;
import com.study.board.domain.post.PostRequest;
import com.study.board.domain.post.PostResponse;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

import java.util.List;

@SpringBootTest
public class PostMapperTest {

	@Autowired
    PostMapper postMapper;

    @Test // 게시글 등록
    void save() {
        PostRequest params = new PostRequest();
        params.setTitle("1번 게시글 제목");
        params.setContent("1번 게시글 내용");
        params.setWriter("테스터");
        params.setNoticeYn(false);
        postMapper.save(params);

        List<PostResponse> posts = postMapper.findAll();
        System.out.println("전체 게시글 개수는 : " + posts.size() + "개 입니다.");
    }

    @Test // 게시글 상세 조회
    void findById() throws JsonProcessingException {
    	PostResponse post = postMapper.findById(1L);
    	
    	String postJson = new ObjectMapper().registerModule(new JavaTimeModule()).writeValueAsString(post);
    	System.out.println(postJson);

    	/*
    	 * ObjcetMapper란?
    	 * - json 데이터를 파싱하거나 object를 json 형태의 string으로 변환할 때 자주 사용되는 라이브러리이다.
    	 * - 위 라이브러리는 스프링 부트에 기본으로 내장되어 있는 Jackson 라이브러리이다.
    	 * 
    	 * ObjectMapper().registerModule(new JavaTimeModule())
    	 * - ObjectMapper에 Java 8의 java.time API를 지원하는 모듈을 등록한다.
    	 * - 즉, Java 8의 java.time API를 사용하는 날짜 및 시간 필드가 있다면 registerModule(new JavaTimeModule())를 사용하여 처리해야한다.
    	 * 
    	 * .writeValueAsString(post)
    	 * - post 객체를 JSON 문자열로 직렬화한다.
    	 * 
    	 * 정리하면, postJson 객체를 ObjectMapper를 통해서 Json 문자열로 변환하여 표현하는 것이다.
    	 */
    }
    
    @Test // 게시글 수정
    void update() {
    	PostRequest params = new PostRequest();
    	
    	params.setId(1L);
    	params.setTitle("1번 게시글 수정");
    	params.setContent("1번 게시글내용 수정");
    	params.setWriter("수정자");
    	params.setNoticeYn(true);
    	
    	postMapper.update(params);
    }
    
    @Test // 게시글 삭제
    void delete() {
    	postMapper.deleteById(1L);
    }
}
```