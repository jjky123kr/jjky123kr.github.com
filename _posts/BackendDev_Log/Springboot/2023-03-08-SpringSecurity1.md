---
layout: single
title:  Spring boot Security 회원 수정 과 session변경  
categories: [Springboot,Security]           
tag:      [blog,Springboot]
author_profile: true
sidebar:
    nav: "main"

toc: ture
toc_alble: 목록
toc_icon: "bars"
toc_sticky: true
---

# Spring Security 회원 수정 과 session변경

### UpdateForm 회원 정보 수정 

* principal은 user 의 정보가 담겨 있다. 해당 user의 정보를 보여준다. 
* id 값을 hidden 으로 들고 간다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<%@ include file="../layout/header.jsp"%>
<main>

	<form action="/user/join" method="post">
     <input type="hidden" id="id" value="${principal.user.id}"/>
		<div class="mb-3 mt-3">
			<label for="username" class="form-label">Username </label> 
			<input type="text"  value="${principal.user.username}"class="form-control" id="username" placeholder="Username" name="username" readonly="readonly">
		</div>
		<div class="mb-3">
			<label for="password" class="form-label">Password </label> 
			<input type="password"value="${principal.user.password}" class="form-control" id="password" placeholder="Password" name="password">
		</div>

		<div class="mb-3">
			<label for="email" class="form-label">Email </label> 
			<input type="email" value="${principal.user.email}" class="form-control" id="email" placeholder="Email" name="email">
		</div>
	</form>
	<!-- 회원가입 시 버튼으로 js 실행 id="btn-save" 호출-->
		<button id="btn-update" class="btn btn-primary">회원수정 완료</button>
</main>
<!-- user.js 실행 코드 -->
<script src="/js/user/user.js">

</script>

<%@ include file="../layout/footer.jsp"%>
```

### 회원수정 json

```js

let index={
		init:function(){
			$("#btn-update").on("click",()=>{ // function(){},()=>{} this 를 바인딩하기
				   this.update();
			});
  }, 
    update:function(){
    	// alert("user의 save함수 호출됨");
    	let data={
    			id:$("#id").val(),
    			username:$("#username").val(),
    			password:$("#password").val(),
    			email:$("#email").val()
    	};
    	
    	$.ajax({
    		 type:"PUT",
    		 url:"/user" ,// 컨트롤 주소 값
    		 data:JSON.stringify(data), // http body 데이터
    		 contentType:"application/json;charset=utf-8", // body 데이터가 어떤
															// 타입인지(MIME)
    		 dataType:"json"// 요청을 서버로 해서 응답이 왔을때 기본으로 모든적이 문자열(생긴것이
							// json)이라며=>javascript 오브젝트로변경
    		// .을 통해서 사용 => 회원가입 수행 요청
    	}).done(function(resp){
    		alert("회원수정이 완료되었습니다.");    		
    		location.href="/"; 
    		
    	}).fail(function(error){ // 에러
            alert(JSON.stringify(error));    		
    	}); 
    },   
    
       
}

index.init(); // init 함수 호출
```