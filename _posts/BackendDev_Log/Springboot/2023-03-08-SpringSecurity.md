---
layout: post
related_posts:
  - /backdev-log/springboot/springboot_13/
title: Spring Secutiry 비밀해쉬화 로그인 기능 구현
categories: 
  - backdev-log
  - springboot
---

# Spring Secutiry 비밀해쉬화 로그인 기능 구현


### pox.xml 에 Security 등록

```xml
	<!-- 시큐리티 태그 라이브러리 -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-taglibs</artifactId>
		</dependency>

    <!-- Secrity -->
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency> 
```

### UserApiController 

* 기존 로그인 방식을 주석 한다.

```java
package com.cos.blog.Controller;

import javax.net.ssl.HttpsURLConnection;
import javax.servlet.http.HttpSession;

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
	   userService.회원가입(user);
		return new ResponseDto<Integer>(HttpStatus.OK.value(),1); //자바 오브젝트를 JSON으로 변환해서 리턴 (Jackson)
	}
	
	// 기존 로그인 방식 
	/*
	 * @PostMapping("/api/user/login") public ResponseDto<Integer>login(@RequestBody
	 * User user,HttpSession session){ //session
	 * System.out.println("UserApiController:login 호출"); User
	 * principal=userService.로그인(user); //principal (접근주체)
	 * 
	 * if(principal !=null) { session.setAttribute("principal", principal); } return
	 * new ResponseDto<Integer>(HttpStatus.OK.value(),1); }
	 */
	
}

```
### url 주소에 http://localhost:8000/

* 자동으로 security 로그인 페이지 
* id = user
* security 비번 

<img width="871" alt="시큐리트2" src="https://user-images.githubusercontent.com/107549149/223672810-67c13a0d-dbb7-4f58-ba63-07e65bb6593c.png">

<img width="631" alt="시큐리트1" src="https://user-images.githubusercontent.com/107549149/223672550-ec29a15d-1023-403b-9442-4ab81484dcaa.png">


### 로그인 시 보여지는 페이지 설정 

### header.jsp 페이지
* security-taglibs 사용 
* <%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %>  

* principal => session 역할

#### Spring Security _ sec 태그의 표현식들

![image](https://user-images.githubusercontent.com/107549149/223675421-39d06e29-b23e-465d-ab36-9d3f8767ef5a.png)

출처<https://velog.io/@nugoory20/Spring-Security-sec-%ED%83%9C%EA%B7%B8%EC%9D%98-%ED%91%9C%ED%98%84%EC%8B%9D%EB%93%A4>

* hasRole은 권한을 검사 할 경우
* hasAnyRole은 원하는 권한 중 뭐든 통과 할 경우
* principal은 현재 접속 사용자의 정보
* pemitAll 권한이 없이도 모두 허용
* denyAll은 테스트 코드 등에 접속을 막고 싶은 경우

```java 
<sec:authorize access="isAuthenticated()">
<sec:authentication property="principal" var="principal"/>
</sec:authorize>
```

```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="sec" uri="http://www.springframework.org/security/tags" %> 

<sec:authorize access="isAuthenticated()">
<sec:authentication property="principal" var="principal"/>
</sec:authorize>

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
			<a class="navbar-brand" href="/">HOME</a>
			<button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#collapsibleNavbar">
				<span class="navbar-toggler-icon"></span>
			</button>
			<div class="collapse navbar-collapse" id="collapsibleNavbar"></div>
	

	<c:choose>
		<c:when test="${empty principal}">
			<ul class="navbar-nav">
				<li class="nav-item"><a class="nav-link" href="/loginForm">로그인</a></li>
				<li class="nav-item"><a class="nav-link" href="/joinForm">회원가입</a>
			</ul>
		</c:when>

		<c:otherwise>
			<ul class="navbar-nav">
				<li class="nav-item"><a class="nav-link" href="/board/Form">글작성</a></li>
				<li class="nav-item"><a class="nav-link" href="/user/Form">회원정보</a></li>
				<li class="nav-item"><a class="nav-link" href="/logout">로그아웃</a></li>
			</ul>
		</c:otherwise>  
	</c:choose>
		</div>
		</nav>
```

### Config (Secutiry 커스터마이징)
* BCryptPasswordEncoder (비밀번호 해쉬)
* 프레임워크에서 제공하는 클래스 중 하나로 비밀번호를 암호화하는 데 사용할 수 있는 메서드를 가진 클래스입니다.
* DefaultSecurityFilterChain 
* RequestMatcher 내용이 일치하는지 판단하며 일치할경우 해당 FilterChain에 등록되어있는 Filter들을  반환하고 이를 doFilter를 통해 필터링을 진행한다


### Secutiry Config
* HttpSecurity 객체를 파라미터로 받는 configure 메소드 오버라이드
* authroizeRequest() : http요청을 위한 웹 기반 보안을 설정하겠다는 의미
* antMatchers() : ant pattern으로 URL지정
* permitAll() : 해당 URL에 모든 사용자 접근을 허용
* authenticated() : 해당 URL에 인증된 사용자 접근만을 허용
* and() / formLogin() / httpBasic()으로 어떤 인증을 사용할지 설정
* loginProcessingUrl()  스프링 시큐리티가 해당 주소로 요청이 들어오면, 로그인을 가로채서 대신  로그인 인증 역할

```java
package com.cos.blog.Config;

import org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.DefaultSecurityFilterChain;

// 빈 등록: 스프링 컨테이너에서 객체를 관리할 수 있게 하는것
@Configuration // loC
public class SecurityConfig {

//	 프레임워크에서 제공하는 클래스 중 하나로 비밀번호를 암호화하는 데 사용할 수 있는 메서드를 가진 클래스입니다.
	@Bean
BCryptPasswordEncoder encode() {
		return new  BCryptPasswordEncoder();
	}
// 시큐리티 필터를 자동등록
	@Bean
	DefaultSecurityFilterChain confiaure (HttpSecurity http) throws Exception{ 
		http
		     .csrf().disable()// csrf 토큰 비활성화 (테스트시 걸어두는 게 좋음)
		     .authorizeRequests()
		     .antMatchers("/","/auth/**","/js/**","/css/**","/image/**") //요청이 들러오면 허용
		     .permitAll()  해당 url에 모든 사용자 접근을 허용
		     .anyRequest() // 다른 모든 요청은 
		     .authenticated() // 인증이 되어야한다.
		   .and()
		      .formLogin() 
		      .loginPage("/auth/loginForm")
          .loginProcessingUrl("/auth/loginProc"); // 스프링 시큐리티가 해당 주소로 요청이 오는 로그인을 가로채서 대신 로그인 해준다. 
		return http.build();
	}

}

```
* http://localhost:8000/ 메인 페이지 -> http://localhost:8000/auth/loginForm  요청이 들어오면, 로그인 폼으로 이용

<img width="458" alt="tlzn" src="https://user-images.githubusercontent.com/107549149/223933027-e7040fce-be56-423c-8a11-517f460a6d7c.png">

### 비밀번호 해쉬 : 고정길이의 16진수 값으로 변경된다.
* 시큐리티-> 로그인 요청 -> 시큐리티가 요청을 가로채다.(username, password)
* 로그인 할떄 비밀번호 1234 정보 시큐리티에 들어오면, 로그인이 안된다. 
* 시큐리티 -> 로그인 요청 - 시큐리티가 요청을 가로채다.(username, password) -> 내부적으로 로그인  진행완료-> 시큐리티 센션 으로 user정보를 등록한다.(IOC) 

* user정보를 DI로 필요할때 가져온다. 
* user정보를 등록할떄 직접 만든 user 오브젝트 가 아니라 타입이 UserDetails 을사용한다.
* user 오브젝트와 등록된 user 타입이 달라서 extends 해준다.(상속)


### UserService 
* 회원가입
* private BCryptPasswordEncoder encoder 설정

```java
package com.cos.blog.Service;


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Service;
// 스프링이 컴포넌트 스캔을 통해서 Bean 에 등록
import org.springframework.transaction.annotation.Transactional;

import com.cos.blog.model.RoleType;
import com.cos.blog.model.User;
import com.cos.blog.repository.UserRepository;

@Service
public class UserService {

	@Autowired
	private UserRepository userRepository;
	
	@Autowired
	  private BCryptPasswordEncoder encoder;
		

	@Transactional
	public void 회원가입(User user) {
		
		String rawPassword = user.getPassword();// 1234원문
		String encPassword = encoder.encode(rawPassword); // 해쉬	
		user.setPassword(encPassword); // 해쉬 변경
		user.setRole(RoleType.USER); 
		userRepository.save(user);
	}
	/*
	 * @Transactional(readOnly = true) //select 할때 트랜잭션이 시작, 서비스 종료시 트랜잭션 종료(정합성)
	 * public User 로그인(User user) { return
	 * userRepository.findByUsernameAndPassword(user.getUsername(),
	 * user.getPassword()); }
	 */

}
```
<img width="502" alt="해쉬" src="https://user-images.githubusercontent.com/107549149/223965088-8d503e69-b313-4df8-b7c3-b3a63fe887fe.png">


### 시큐리티는 로그아웃은 디폴트 이다.
* 로그아웃 로직 하지 않아도, 자동으로 시큐리티가 디폴트이라서 된다.

### Security 로그인 인증

### UserDetails  

Spring Security 에서 사용자의 정보를 담는 인터페이스이다.
Spring Security에서 사용자의 정보를 불러오기 위해서 구현해야 하는 인터페이스로 기본 오버라이드 메서드들은 아래와 같다.


<img width="1000" alt="a" src="https://user-images.githubusercontent.com/107549149/229665716-17cb2d0a-3fc9-44cf-bace-c72868bd849a.png" >

* GrantedAuthority 메소드가 한개이라서 람다형식으로 작성가능하다.


**PrincipalDetail**

```java
package com.Animal.blog.Config.auth;

import java.util.ArrayList;
import java.util.Collection;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import com.Animal.blog.Model.Member;

public class PrincipalDetail implements UserDetails {

	private Member member;  // 객체를 품다(콤포지션)


	@Override
	public String getPassword() {
		// TODO Auto-generated method stub
		return member.getPassword();
	}

	@Override
	public String getUsername() {
		// TODO Auto-generated method stub
		return member.getUsername();
	}

	// 계정이 만료되지 않았는지 리턴한다. (treu=만료안됨)
	@Override
	public boolean isAccountNonExpired() {
	
		return true;
	}
	// 계정이 만료되지 않았는지 리턴 한다.(true :잠기지 않음)
	@Override
	public boolean isAccountNonLocked() {
		
		return true;
	}

	// 비밀번호가 만료되지 않았는지 리턴한다.(true:만료안됨)
	@Override
	public boolean isCredentialsNonExpired() {
		
		return false;
	}
	
	// 계정 활성화(사용가능)인지 리턴(true="만료안됨")
	@Override
	public boolean isEnabled() {
		
		return false;
	}
	
	
	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
		Collection<GrantedAuthority>collectors=new ArrayList<>();
		collectors.add(new GrantedAuthority() {
			
			@Override
			public String getAuthority() {
				// TODO Auto-generated method stub
				return "ROLE_"+member.getRole();
			}
		});
	collectors.add(()->{return"ROLE_"+member.getRole();});
		return null;
	}
	
	
}
```

### UserDetailsService 

* Spring Security에서 유저의 정보를 가져오는 인터페이스이다.

* Spring Security에서 유저의 정보를 불러오기 위해서 구현해야하는 인터페이스로 기본 오버라이드 메서드는 아래와 같다.

<img width="900" alt="z" src="https://user-images.githubusercontent.com/107549149/229667309-b9addd2d-d549-4159-b4f2-46d43395db06.png">


자료 출처 <https://programmer93.tistory.com/68>


