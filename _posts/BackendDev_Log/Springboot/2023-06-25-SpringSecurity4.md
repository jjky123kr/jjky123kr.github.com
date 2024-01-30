---
layout: single
title:  Spring Security-OAuth 구글 , 페이스북 로그인
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

# 구글 로그인

### User 모델 

```java
package com.cos.security1.Model;

import java.sql.Timestamp;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

import org.hibernate.annotations.CreationTimestamp;

import lombok.Data;
@Data
@Entity
public class User {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private int id;
	private String username;
	private String password;
	private String eamil;
	private String role;
	@CreationTimestamp
	private Timestamp createDate;
	
}
```


### Config 

**_WebMvcConfig_** 

* configureViewResolvers 가 view 페이지의 설정 하게 해준다. 

```java
package com.cos.security1.config;
import org.springframework.boot.web.servlet.view.MustacheViewResolver;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.ViewResolverRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer{  

  @Override
  public void configureViewResolvers(ViewResolverRegistry registry) {
      MustacheViewResolver resolver = new MustacheViewResolver();
//configureViewResolvers 가 view 페이지의 설정 하게 해준다. 
      resolver.setCharset("UTF-8"); // 인코딩 설정 
      resolver.setContentType("text/html;charset=UTF-8");// 데이터 보내는 페이지 html,utf-8
      resolver.setPrefix("classpath:/templates/"); // 위치 view 
      resolver.setSuffix(".html");// 머스테치 .html 

      registry.viewResolver(resolver); // 등록한다. 
  }
}
```

### SecurityConfig 환경설정

* **@Configuration 는 설정파일을 만들기 위한 애노테이션 or Bean을 등록하기 위한 애노테이션이다.**
* **SecurityFilterChain** 인터페이스며, 이름 그대로 보안과 관련된 여러 필터들로 구성된 체인을 의미한다.

```java
package com.cos.security1.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity // 스프링 시큐리티 필터가 스프링 필터체인에 등록
public class SecurityConfig{
//SecurityConfig 필터 
	
	// password 암호화 
	
	@Bean
	public BCryptPasswordEncoder encodePwd() {
		return new BCryptPasswordEncoder();
	}
	
	
	@Bean
	
	public SecurityFilterChain filterCahin(HttpSecurity http)throws Exception{
		http.csrf().disable(); //비활성화
		http.authorizeRequests()
		.antMatchers("/user/**").authenticated() // 인증만 되는 들어가는 주소
		.antMatchers("/manger/**").access("hasRole('ROLE_ADMIN')or hasRole('ROLE_MANGER')")
		.antMatchers("/admin/**").access("hasRole('ROLE_ADMIN')")
		.anyRequest().permitAll()
		.and()
		.formLogin()
		.loginPage("/loginForm")
		.loginProcessingUrl("/loginProc") // /login 주소가 호출 되면, 시큐리티가 낚아서 자동 로그인
		.defaultSuccessUrl("/");
		return http.build();
	}
}
```

## 구글 로그인 설정 

1. **maven 라이브러기 설정 Oauth**

```java

 <!-- oauth -->

	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-oauth2-client</artifactId>
	</dependency>
```

2. **구글 API 콘솔** 
<https://console.cloud.google.com/>

3. **프로젝트 생성**

<img width="1228" alt="구글 로그인" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/102a0c60-5705-4edd-a140-7a67a5597d46">
<br><br>

<img width="568" alt="구글 로그인2" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/1c237c39-5600-4522-b388-2bc9b342a89b">
<br><br>

<img width="452" alt="구글 로그인3" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/fd388923-27bb-442c-918c-2ffb49883ca5">
<br><br>

4. **API 및 서비스 => OAuth 동의 화면**

<img width="1117" alt="구글 로그인4" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/f618b983-da46-4bae-bc9d-11f1b61c4cee">
<br><br>

<img width="565" alt="구글 로그인5" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/4df867d7-5661-46ca-a2de-629f8ecdc607">
<br><br>

<img width="724" alt="구글 로그인6" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/d4ef1bc6-f997-408e-a460-e2cf3e8691e9">
<br><br>

5. **사용자 인증 정보 : 승인된 리다렉션 URI** 

<img width="801" alt="구글 로그인7" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/ca1ce351-b507-4d63-a273-d6055e3ea848">
<br><br>

<img width="647" alt="y" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/844b474f-a61f-4fb3-839c-f8619fe5ac10">


### <span style="background-color:#fffd91">application.yml 구글 설정</span>


<img width="636" alt="e" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/b40c3b5a-04d2-45fd-9724-f5ac8c103b85">

### Security Config 구글 추가

<img width="795" alt="n" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/fd47e989-af7c-4569-95f2-f5824569babc">



**_로그인 페이지_**

* <span style="color:red">구글 링크 연동 은 고정이다. (Oauth 라이브러리 때문에)</span><br>

<img width="565" alt="g1" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/a4823d12-0a33-4bce-9dee-4b180a969dd1">


```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>로그인 페이지</title>
</head>
<body>

<h1>로그인 페이지 입니다.</h1>
<hr>
<form action="/loginProc" method="POST">
<input type="text" name="username" id="username" placeholder="username"><br/>
<input type="password" name="password" id="password" placeholder="password"><br/>
<button>로그인</button>
</form>
<a href="/oauth2/authorization/google" >구글 로그인</a>
<a href="/joinForm">회원가입</a>

</body>
</html>
```

<img width="375" alt="b" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/5829b827-da3e-4879-9c4d-34e8e421a8f1">


### PrincipalOauth2UserService  토큰 과 사용자 정보

1. **<span style="background-color:#fff5b1">DefaultOAuth2UserService 를 상속한다.</span>**
2. **loadUser** 함수를 이용하여, "access token을 이용해 서버로부터 사용자 정보를 받아온다" 기능을 넣을 것입니다.
3. 어떤 값들이 있는지  System.out.println 콘솔로  확인한다. 

>**ClientRegistration는 클라이언트 서버의 기본정보(어떤 OAuth 로 로그인 했는지 확인가능)**

<img width="830" alt="i" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/e8881713-ea5c-4746-9acc-7ff07f86bf99">


**User 에 필드 추가한다.**

<img width="491" alt="z" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/8a3cfa3f-848f-4115-848b-0cf2af74956c">

**PrincipalDetails에 UserDetails 와, OAuth2User를 implement한다.**

```java
package com.cos.shop.config.auth;

import java.util.ArrayList;
import java.util.Collection;
import java.util.Map;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.oauth2.core.user.OAuth2User;

import com.cos.shop.model.User;

public class PrincipalDetails implements UserDetails,OAuth2User {

	private User user;

	private Map<String, Object> attributes;

	// 일반로그인 생성자
	public PrincipalDetails(User user) {
		this.user = user;
	}

	// OAuth2.0 로그인
	public PrincipalDetails(User user, Map<String, Object> attributes) {
		this.user = user;
		this.attributes = attributes;
	}

	

	@Override
	public String getPassword() {
		// TODO Auto-generated method stub
		return user.getPassword();
	}

	@Override
	public String getUsername() {
		// TODO Auto-generated method stub
		return user.getUsername();
	}

	@Override
	public boolean isAccountNonExpired() {
		// TODO Auto-generated method stub
		return true;
	}

	@Override
	public boolean isAccountNonLocked() {
		// TODO Auto-generated method stub
		return true;
	}

	@Override
	public boolean isCredentialsNonExpired() {
		// TODO Auto-generated method stub
		return true;
	}

	@Override
	public boolean isEnabled() {
		// TODO Auto-generated method stub
		return true;
	}

	
	// 계정이 갖고있는 권한 목록을 리턴한다.(권한이 여러개 있을 수 있어서 루프를 들어야 하는데 우리는 한개만)
		@Override
		public Collection<? extends GrantedAuthority> getAuthorities() {

			Collection<GrantedAuthority> collectors = new ArrayList<>();
//				collectors.add(new GrantedAuthority() {
//					
//					@Override
//					public String getAuthority() {
//						// TODO Auto-generated method stub
//						return "ROLE_"+user.getRole(); //ROLE_USER
//					}
//				});

			// 람다식 사용
			collectors.add(() -> {
				return "ROLE_" + user.getRole();
			});
			return collectors;
		}
		
		//OAuth2User 오버라이딩
		@Override
		public Map<String, Object> getAttributes() {
			// TODO Auto-generated method stub
			return attributes;
		}

		@Override
		public String getName() {
			// TODO Auto-generated method stub
			return null;
		}

}
```


**getAttributes 값으로 강제 회원가입 후 로그인 진행한다.**
1. userEntity 가 없으면 강제로 로그인 진행 

<img width="718" alt="n" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/e2f63c8f-98aa-4b4f-be71-b068636f8c47">


### 컨트롤 에서 일반 로그인 후 세션을 확인 해 본다. (로그인을 진행후 "test/login")

1. **<span style="background-color:#fff5b1">Authentication객체</span>내부에는 Principal있다.<br>**
**Principal은 리턴 타입이 Object 라서 principalDetails 다운 캐스팅을 한다.**<br>
**로그인한 세션의 정보를 principalDetails.getUser() 가져올수 있다.**<br>
**System.out.println("authentication:"+principalDetails.getUser());**

2. **<span style="background-color:#fff5b1">@AuthenticationPrincipal 주입한다.</span>**<br>
**PrincipalDetails userDetails 으로 세션정보를 가져올수 있다.**<br>
**System.out.println("userDetails"+userDetails.getUser());**

**첫번재 방법**  

<img width="576" alt="l" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/220841c6-0338-49d1-bfb4-1d20e493d2bf">

**두번쨰 방법**

<img width="799" alt="u" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/b8340ef5-01bc-40c9-9041-679b93ff2e4a">

3. 결과확인

<img width="6000" alt="nc" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/f25fd6aa-77f3-4458-ac4c-649c1e277720">


###  컨트롤 에서 구글 로그인 하여 세션 확인 

<img width="754" alt="z" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/1203afcd-d1ed-46eb-aa76-cadcac82341e">

1. **<span style="color:red">PrincipalDetails로 캐스팅이 되지 않된다.</span>**
2. **<span style="background-color:#fff5b1">OAuth2User</span>**으로 다운 캐스팅한다. 

### 정리 

<img width="583" alt="ㅗ" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/b2d2379d-9646-4d40-8f7b-10319137bb25">

**결론: PrincipalDetails 에 UserDetails 와 OAuth2User implement 하면 PrincipalDetails 클래스가 부모가 되어서 Authentication 안에 PrincipalDetails 를 담는다.**

### PrincipalDetails 을 만든이유??

시큐리티로 로그인을 할때 PrincipalDetails 에 UserDatails 를 implement 하여  같은 타입으로 묶어서 ,User 오브젝트를 품었다. 

 Authentication  안에 PrincipalDetails 사용할 수 있다. 그래서 OAuth2User도 PrincipalDetails 에 implement 하여  같은 타입으로 묶는다. 

##  페이스북 로그인 

1. **페이스북 API 콘솔**
<https://developers.facebook.com/?locale=ko_KR>

2. 페이스북 로그인 후 앱 만들기 

<img width="1231" alt="f1" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/47707d2d-d9b1-4504-bec0-b1e3361518ef">

<img width="1187" alt="f2" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/9609fcba-052f-4408-ad7c-c415987cdd26">

3. 페이스북 로그인 앱 설정 

<img width="1280" alt="f3" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/e49863b3-a76c-4b32-bb1a-f4b4fce4d894">

<img width="1000" alt="f4" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/b02caf04-6dcf-4278-8386-8e84b5bf43e7">

4. 웹 애플리케이션 선택 

<img width="1020" alt="f5" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/84c21946-3fd2-457c-bb60-dd207731e7c5">

5. 도메인 설정한다. 

<img width="940" alt="f6" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/61e389c2-da56-42e4-8876-7b37df9820b2">

### <span style="background-color:#fffd91">application.yml 페이스북 설정</span>

<img width="545" alt="f7" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/79af44b1-30ac-443d-9e20-22f56456e3e1">

### 로그인 폼에 페이스북 추가


### 페이스북 로그인 오류 발생

<img width="912" alt="0" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/736de3f9-8717-4451-a05b-07e57dede297">


**해결 방법**
1. 페이스북 API 콘솔 -> 내 앱 -> 이용사례 -> 인증 및 계정 만들기 수정한다. 

**인증 및 계정 만들기 에서 확인 해 보면 email 이 없다. 그래서 수정한다.**

<img width="1259" alt="02" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/f13c8e9b-33c6-40f2-9f52-a163ec3b969c">

<img width="1245" alt="03" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/3a27febb-af2f-4059-b795-5a094b6e9720">

<img width="1246" alt="04" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/95ab3647-a841-4d6a-a106-b6f22847840b">

2. 수정 후 로그인 과 자동 회원 가입 확인 

<img width="994" alt="f8" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/7d6f412d-54e1-4bb5-8289-42a53d0ad3fb">

### DB User확인

<img width="627" alt="f9" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/5621d6e8-334e-4e1e-89d4-f87f995954c4">

**문제점**

**facebook 회원가입 할 경우 에 provider_id 와 username 값 의 fackbook_null 이 들어간다.**
**이떄 유지보수가 어렵다.** 

### 문제점을 해결을 위해서 코드를 수정한다. 

1. Config 패키지 -> oauth 패키지->provider 패키지 설정 한다.

2. provider 패키지 -> OAuthUserinfo (interface 클래스 생성한다.) 부모클래스 생성한다. 

**OAuthUserinfo**

<img width="473" alt="f10" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/4f63ed55-4b13-4bde-9441-29e5c6be2434">

3. provider 패키지 ->  GoogleUserInfo , FacebookUserInfo 자식클래스 생성 

* OAuthUserinfo 를 implements 한다. 
* 생성자 주입한다.

**GoogleUserInfo**

<img width="616" alt="f11" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/f7fecbd0-d13e-48a4-a16a-82f97428e0e7">

**FacebookUserInfo**
<img width="613" alt="f12" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/57da5349-034b-47f3-9445-92d831ffbd98">

4. **PrincipalOauth2UserService 수정**

> google 로그인,facebook 로그인을 if 조건문을 사용하여, google 경우 와 facebook 경우를 설정하여 
회원가입을 진행한다.

<img width="743" alt="f13" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/081aa9c9-36a1-4942-a0d2-dc21074a9db1">

**결과DB**

<img width="628" alt="f14" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/87ea8148-484d-4e75-863e-58d0ee11c471">

## 네이버 로그인 

1. **네이버 개발센터 API**
<https://developers.naver.com/main/>

2. 네이버 로그인 

3. 내 애플리케이션 등록 

<img width="1080" alt="n1" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/2051df36-209e-4366-822d-b3b56c987f1b">

<img width="1055" alt="n2" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/5539b4d1-2213-4718-9591-cb583b51b15a">

<img width="1049" alt="n3" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/8f50f7bb-1403-430c-8bda-56f6f27b4357">


### appliation.yml 네이버 추가 

<img width="682" alt="n4" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/fd97df3e-f96d-4acb-827e-488f314a9e61">

### **네이버 등록시 오류 발생**

**org.springframework.security.oauth2.client.registration.InMemoryClientRegistrationRepository: Factory method 'clientRegistrationRepository' threw exception; nested exception is java.lang.IllegalStateException: Provider ID must be specified for client registration 'naver'**

<span style="background-color:#fffd91"> clientRegistrationRepository클래스 있는데 여기 네이버를 저장이 불가능 하다. 
이유는 네이버는 provider가 제공 해주지 않는다.</span>

**해결방법: provider 등록** 

<img width="674" alt="n5" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/dac36bcb-d1db-4d0a-bf66-bac946dd5bb2">

**NaverUserInfo 클래스 생성**

<img width="433" alt="n16" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/574ed079-5142-4ed9-bb29-7422a34adc9d">

**오류 발생**

<img width="887" alt="n7" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/38ff1d5f-9ea4-495c-81e2-9eb741d8dee3">

원인 : 사용자 정보가 response 라는 곳에 저장이 되어있다. 

<img width="690" alt="z1" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/dd155a88-9c43-4273-b12a-d1baa0f91bc1">


**해결방법**

* Map 로 response를 추출한다.

NaverUserInfo 클래스 생성

<img width="619" alt="n15" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/5709afdf-9623-4ecf-bf7c-2c67e1626af7">

**결과 DB**

<img width="829" alt="n12" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/eed94dde-78ba-4d37-8b08-da38557a9e98">

## 카카오 로그인 

1. 카카오 개발 API 
<https://developers.kakao.com/>

2. 내어플리케이션 등록 


3. Web 플랫폼 등록 

<img width="959" alt="k1" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/f83e5d68-f9ed-49d3-9de6-391aa569da12">

4. Redirect URI 등록 

<img width="502" alt="k2" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/151afa4c-473d-4651-8eeb-6d58a705925b">

5. 동의 항목

<img width="1057" alt="k3" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/88a7508e-bd2a-41c1-9f74-c8fe93391765">

### appliation.yml 카카오 정보 추가

*  사용자 정보가 id 라는 곳에 저장이 되어있다. 

<img width="563" alt="k5" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/bbc8766e-2018-4926-a426-25c578155117">

* <span style="background-color:#fffd91">oAuth2.0 라이브러리 provider 에서 카카오는 지원하지 않는다. 
  그래서 provider를 네이버 처럼 등록해야 한다.</span>

  <img width="527" alt="k6" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/a0ff960a-81ca-4196-b1bd-eacd0f3a1f03">

**KakaoUserInfo** 클래스 생성 

### 카카오 오류 발생했다. 

<img width="830" alt="스크린샷 2023-06-28 082528" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/2af3182e-42ab-465b-ab28-dde4203e7c6c">

**원인:카카오의 id 값이 데이터 타입이 Long** 
**해결:to Stirng 으로변경 한다.**

* 카카오에는 닉네임이 properties 안에 있어서 추출해야 한다. 

* 카카오에서는 이메일이 kakao_account 안에 있어서 추출해야 한다.

>**Map로 추출해야한다.**

<img width="662" alt="k4" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/bd25650d-8339-4fa1-9862-84adb78b47c1">

**PrincipalOauth2UserService**

* 카카오 로직을 추가한다. 

<img width="536" alt="k7" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/ecdec124-dbe7-44ff-87dd-b395b30ff8c5">

### **결과DB**

<img width="894" alt="k8" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/f1c256e7-51bb-4926-9f57-a941a15d119b">






