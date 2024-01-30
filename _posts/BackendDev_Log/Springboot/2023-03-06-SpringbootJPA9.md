---
layout: post
related_posts:
  - /backdev-log/springboot/springboot_10/
title: Spring boot JPA 2탄
  - backdev-log
  - springboot
---
# Spring boot JPA 2탄

### 매인 페이지 

* layout 페이지 설정(header.jsp , footer.jsp)
* heard.jsp 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Bootstrap Example</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/css/bootstrap.min.css" rel="stylesheet">
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.3/dist/js/bootstrap.bundle.min.js"></script>
	<script src="http://code.jquery.com/jquery-latest.js"></script>
</head>
<body>

<nav class="navbar navbar-expand-sm bg-dark navbar-dark">
  <div class="container-fluid">
    <a class="navbar-brand" href="/blog">HOME</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#collapsibleNavbar">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="collapsibleNavbar">
      <ul class="navbar-nav">
        <li class="nav-item">
          <a class="nav-link" href="/blog/user/loginForm">로그인</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="/blog/user/joinForm">회원가입</a>
        
      </ul>
    </div>
  </div>
</nav>
 <br>
```

* footer.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

 <br>
	<footer>
		<div class="mt-5 p-2 bg-dark text-white text-center" style="margin-bottom: 0;">
			<p>Created by Home</p>
			<p>010-1234-5678</p>
			<p>주소지: 서울 동대문구 장안동</p>
			<p>이메일: jjky123kr@naver.com</p>
		</div>
	</footer>

</body>
</html>
```
* index.jsp

* layout (header.jsp , footer.jsp) 파일 include 설정 한다. 

> <%@ include file="layout/header.jsp" %> , <%@ include file="layout/footer.jsp" %>

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<%@ include file="layout/header.jsp" %>
<main>

	<div class="card m-5">
		<div class="card-body">
			<h4 class="card-title">제목</h4>
			<p class="card-text">내용</p>
			<a href="#" class="btn btn-primary">상세보기</a>
		</div>
	</div>


	<div class="card m-5">
		<div class="card-body">
			<h4 class="card-title">제목</h4>
			<p class="card-text">내용</p>
			<a href="#" class="btn btn-primary">상세보기</a>
		</div>
	</div>


	<div class="card m-5">
		<div class="card-body">
			<h4 class="card-title">제목</h4>
			<p class="card-text">내용</p>
			<a href="#" class="btn btn-primary">상세보기</a>
		</div>
	</div>

</main>
<br/>


<%@ include file="layout/footer.jsp" %>
```



* joinForm.jsp

```jsp

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<%@ include file="../layout/header.jsp"%>
<main>

	<form action="/user/join" method="post">

		<div class="mb-3 mt-3">
			<label for="username" class="form-label">Username </label> 
			<input type="text" class="form-control" id="username" placeholder="Username" name="username">
		</div>
		<div class="mb-3">
			<label for="password" class="form-label">Password </label> 
			<input type="password" class="form-control" id="password" placeholder="Password" name="password">
		</div>

		<div class="mb-3">
			<label for="email" class="form-label">Email </label> 
			<input type="email" class="form-control" id="email" placeholder="Email" name="email">
		</div>
	</form>
  <!-- 회원가입 시 버튼으로 js 실행 id="btn-save" 호출--> 
		<button id="btn-save" class="btn btn-primary">회원가입</button>
</main>
<!-- user.js 실행 코드 -->
<script src="/blog/js/user/user.js">

</script>
<%@ include file="../layout/footer.jsp"%>

```


# Controller

### BoardController
* 홈페이지 설정 (index.jsp 페이지)

```java
package com.cos.blog.Controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class BoardController {

	@GetMapping("/")
	public String index() {
		// /WEB-INF/views/index.jsp
		return"index";
	}
}
```

### UserController

* 로그인 폼 설정
* 회원가입 폼 설정 

```java
package com.cos.blog.Controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class UserController {

	//회원가입 폼
	@GetMapping("/user/joinForm")
	public String joinForm() {
		return"user/joinForm";
	}
	// 로그인 폼
	@GetMapping("/user/loginForm")
	public String loginForm() {
		return"user/loginForm";
	}

	
}

```
### UserApiController (ajax)

```java
package com.cos.blog.Controller;

import javax.net.ssl.HttpsURLConnection;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import com.cos.blog.DTO.ResponseDto;
import com.cos.blog.Service.UserService;
import com.cos.blog.model.RoleType;
import com.cos.blog.model.User;

@RestController
public class UserApiController {
  @Autowired
  private UserService userService;
	
	@PostMapping("/api/user")
	public ResponseDto<Integer> save(@RequestBody User user) {
		System.out.println("UserApiController:save 호출");
		// 실제로 DB에 insert 실행 하고 아래에서 return 한다. 
          user.setRole(RoleType.USER);
		int result=userService.회원가입(user);
		return new ResponseDto<Integer>(HttpStatus.OK,result);
	}
}
```
### DTO
### ResponseDto 
* HTTP Status Code
* HTTP Status Code(HTTP 상태 코드)는 클라이언트가 보낸 HTTP 요청에 대한 서버의 응답을 코드로 표현한 것으로 해당 코드로 요청의 성공 / 실패 / 실패요인등을 알 수 있다.

```java
package com.cos.blog.DTO;

import org.springframework.http.HttpStatus;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;

@Data
@NoArgsConstructor
@AllArgsConstructor
@Builder
public class ResponseDto<T> {

	 int  status; 
	 T data;
}
```


### 회원가입 ajax 방식 

```js

let index={
		init:function(){
			$("#btn-save").on("click",()=>{ // function(){},()=>{} this 를 바인딩하기 위해서
				this.save();
			}); // btn-save 클릭시 아래가 실행 된다. 
		},
    save:function(){
    	//alert("user의 save함수 호출됨");
    	let data={
    			username:$("#username").val(),
    			password:$("#password").val(),
    			email:$("#email").val()
    	};
    	
    	//console.log(data);   	
    	
    	// ajax 호출시 default 가 비동기 호출
    	// ajax 통신을 이용해서 3개의 데이터를 json으로 변경하여  insert 요청한다. 
    	// ajax 통신을 성공하고 서버가 json을 리턴해주면 자동으로 자바 오브젝트 변환 가능하다.
    	$.ajax({
    		 type:"POST",
    		 url:"/blog/api/user" ,// 컨트롤 주소 값
    		 data:JSON.stringify(data), //http body 데이터
    		 contentType:"application/json;charset=utf-8", // body 데이터가 어떤 타입인지(MIME)
    		 dataType:"json"// 요청을 서버로 해서 응답이 왔을때 기본으로 모든적이 문자열(생긴것이 json)이라며=>javascript 오브젝트로변경
    		// .을 통해서 사용 => 회원가입 수행 요청
    	}).done(function(resp){
    		alert("회원가입이 완료되었습니다.");    		
    		location.href="/blog";
    		
    	}).fail(function(error){ // 에러
            alert(JSON.stringify(error));    		
    	}); 
    }

}

index.init(); // init 함수 호출 
```
# repository 

### userRepository
* JpaRepository 상속 
* UseruserRepository 인터페이스 생성한다. 
* DAO 페이지 이자 자동으로 bean등록이 된다.
* @Repository 생략이 가능하다.

```java
package com.cos.blog.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.cos.blog.model.User;

//DAO
// 자동으로 bean 등록이 된다. 
//@Repository 생략 가능
public interface UserRepository extends JpaRepository<User, Integer>{

}
```


# Service 

### UserService 회원가입 실행

* userRepository 어노테이션 설정

```java
package com.cos.blog.Service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
// 스프링이 컴포넌트 스캔을 통해서 Bean 에 등록

import com.cos.blog.model.User;
import com.cos.blog.repository.UserRepository;

@Service
public class UserService {

	@Autowired
	private UserRepository userRepository;

	public int 회원가입(User user) {
		try {
			userRepository.save(user);
			return 1;
		} catch (Exception e) {
           e.printStackTrace();
           System.out.println("UserRepository:회원가입():"+e.getMessage());
		}
		return -1;
	}

}
```

<img width="600" alt="회원등록" src="https://user-images.githubusercontent.com/107549149/223093800-5a6068f5-3c04-4e4c-b5f9-12d3ffcb8030.png">









