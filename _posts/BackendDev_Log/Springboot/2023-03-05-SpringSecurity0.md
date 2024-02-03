---
layout: single
title:  Spring Security 설정 권한 설정
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

# Spring Security 설정 권한 설정

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
## Spring Security

* **_개념_** : Spring 기반의 애플리케이션의 보안(인증과 권한, 인가 등)을 담당하는 스프링 하위 프레임워크이다 

* **_특징_** : Spring Security는 '인증'과 '권한'에 대한 부분을 Filter 흐름에 따라 처리하고 있다.

*  **_장점_** : Spring Security는 보안과 관련해서 체계적으로 많은 옵션을 제공하여 일일이 보안관련 로직을 작성하지 않아도 된다.

### **_Spring Security 처리 과정_** 

![image](https://github.com/jjky123kr/jjky123kr/assets/107549149/06a94f1e-9b92-484b-ab77-2a5419820d41)

1. 사용자가 아이디 비밀번호로 로그인을 요청함
2. AuthenticationFilter에서 UsernamePasswordAuthenticationToken을 생성하여 AuthenticaionManager에게 전달
3. AuthenticaionManager는 등록된 AuthenticaionProvider(들)을 조회하여 인증을 요구함
4. AuthenticaionProvider는 UserDetailsService를 통해 입력받은 아이디에 대한 사용자 정보를 DB에서 조회함
5. 입력받은 비밀번호를 암호화하여 DB의 비밀번호화 매칭되는 경우 인증이 성공된 UsernameAuthenticationToken을 생성하여 AuthenticaionManager로 반환함
6. AuthenticaionManager는 UsernameAuthenticaionToken을 AuthenticaionFilter로 전달함
7. AuthenticationFilter는 전달받은 UsernameAuthenticationToken을 LoginSuccessHandler로 전송하고, SecurityContextHolder에 저장함

### 인증(Authorizatoin)과 인가(Authentication)

* 인증(Authentication): 해당 사용자가 본인이 맞는지를 확인하는 절차

* 인가(Authorization): 인증된 사용자가 요청한 자원에 접근 가능한지를 결정하는 절차 

<br>
![image](https://github.com/jjky123kr/jjky123kr/assets/107549149/02eb8f23-a54f-4a58-8600-c4042caab601)


**Spring Security는 기본적으로 인증 절차를 거친 후에 인가 절차를 진행하게 되며, 인가 과젱에서 해당 리소스에 대한 접근 권한이 있는지 확인을 하게 된다. Spring Security에서는 이러한 인증과 인가를 위해 <span style="color:red">Principal을 아이디로</span>, <span style="color:red"> Credential을 비밀번호</span>로 사용하는 <span sytle="color:red">Credential</span> 기반의 인증 방식을 사용한다.**

**[ SecurityContextHolder ]**

* SecurityContextHolder란 Authentication을 담고 있는 Holder 이다. 
* Authentication 자체는 인증된 정보이기에 SecurityContextHolder 가 가지고 있는 값을 통해 <br>
  인증이 되었는지 아닌지 확인할 수있다. 

**접근 주체의 정보와 권한을 담는 Authentication**

**Authentication 구조**

* principal : 접근 주체의 아이디 혹은 User 객체를 저장합니다.
* credentials : 접근 주체의 비밀번호를 저장합니다.
* authorities : 인증된 접근 주체자의 권한 목록을 저장합니다.
* details : 인증에 대한 부가 정보를 저장합니다.
* authenticated : boolean 타입의 인증 여부를 저장합니다.

**SecurityContext**

* Authentication 객체가 보관되는 저장소 역할을 합니다.
* 필요시 SecurityContext로부터 Authentication 객체를 꺼내서 사용할 수 있습니다.
* 서버에 접속해서 생성되는 각 Thread는 각각의 ThreadLocal에 SecurityContext를 가지고 있는 것입니다.<br>
(SecurityContext는 ThreadLocal에 저장되어 있으면서 동시에 HttpSession에도 저장되어 있습니다.)


### SecurityConfig 환경설정

* **@Configuration 는 설정파일을 만들기 위한 애노테이션 or Bean을 등록하기 위한 애노테이션이다.**
* **SecurityFilterChain** 인터페이스며, 이름 그대로 보안과 관련된 여러 필터들로 구성된 체인을 의미한다.


![image](https://github.com/jjky123kr/jjky123kr/assets/107549149/eceb5f89-b7c4-4177-89f3-5dcfd2297601)

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

### <span style="background-color:#C0FFFF">스프링시큐리티의 각종 설정 HttpSecurity</span>
<img width="656" alt="x" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/8cd6b81f-06bb-4358-9118-50b11a687eb3">

**HttpSecurity**
>* 스프링시큐리티의 각종 설정은 HttpSecurity로 대부분 하게 됩니다.
>* 리소스(URL) 접근 권한 설정
>* 인증 전체 흐름에 필요한 Login, Logout 페이지 인증완료 후 페이지 인증 실패 시 이동페이지 등등 설정
>* 인증 로직을 커스텀하기위한 커스텀 필터 설정
>* 기타 csrf, 강제 https 호출 등등 거의 모든 스프링시큐리티의 설정


### 시큐리티를 적용하면 보통 configure() 메서드에는 아래와 같이 csrf().disable()로 적용을 하는데요, 이러한 CSRF란 무엇인지 알아보겠습니다.

<img width="644" alt="a" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/5d89f41f-5b7b-4ae1-b5ca-0494aa1fddab">

### CSRF 란
<h3>사이트 간 요청 위조(Cross-Site Request Forgery)</h3>
CSRF란 웹 애플리케이션의 취약점 중 하나로, 이용자가 의도하지 않은 요청을 통한 공격을 의미합니다.<br>
즉 CSRF 공격이란, 인터넷 사용자(희생자)가 자신의 의지와는 무관하게 공격자가 의도한 행위(등록, 수정, 삭제 등)를 특정 웹사이트에 요청하도록 만드는 공격 입니다. 

**@EnableWebSecurity는 어노테이션은 기본적으로 CSRF 공격을 방지하는 기능을 지원하고 있습니다.**
>http.csrf().disable()에서 csrf은 무엇이고, disable() 하는 이유가 무엇일까?

**rest api에서 client는 권한이 필요한 요청을 하기 위해서는 요청에 필요한 인증 정보를(OAuth2, jwt토큰 등)을 포함시켜야 한다. 따라서 서버에 인증정보를 저장하지 않기 때문에 굳이 불필요한 csrf 코드들을 작성할 필요가 없다.**

**Cross site Request forgery로 사이즈간 위조 요청인데, 즉 정상적인 사용자가 의도치 않은 위조요청을 보내는 것을 의미한다.**


### 권한 설정 하는 방법 1

* 리소스(URL)의 권한한 설정 
* antMatchers 특정 리소스에 대해서 권한을 설정합니다.
* permitAll는 antMatchers 설정한 리소스의 접근을 인증절차 없이 허용한다는 의미 입니다.
* anyRequest 는 **모든 리소스를 의미하며 접근허용 리소스 및 인증후 특정 레벨의 권한을 가진 사용자만 접근가능한 리소스를 설정하고 그외 나머지 리소스들은 무조건 인증을 완료해야 접근이 가능하다는 의미입니다.**

### 시큐리티 로그인 

* **formLogin** 로그인 페이지 로그인 처리 및 성공 실패 처리를 사용하겠다는 의미 입니다.
* **loginPage** 로그인 페이지 URI 
* **loginProcessingUrl("loginProc)** form 에서 post 방식으로 요청할떄 로그인 즉 인증 처리를 하는 URL을 설정합니다. <br> **/loginProc 호출**하면 시큐리티가 낚아서 자동으로 로그인 (인증처리를 수행하는 필터가 호출됩니다.)
* **defaultSuccessUrl** 로그인 성공후 이동할 페이지

### 비밀번호 암호화 

* Spring Security 는  회원가입 할 때 보안을 위해서 비밀번호 암호화를 해야한다. 

* BCryptPasswordEncoder 클래스를 @Bean으로 등록한다. : 비밀번호 암호화 

<span style="background-color:#fff5b1">**회원가입 할떄 비밀번호 암호화 setting한다.**</span>

<img width="469" alt="w" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/14a83e62-4862-45fe-9b0a-6cbd3a9fca5e">

**DB 확인**

<img width="562" alt="qw" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/19f4622a-072e-451a-992d-e1ba09d0a884">




### 시큐리티 로그인을 진행이 완료가 되면 시큐리티 session을 만들어 준다. 

* 로그인을 진행이 완료가 되면 시큐리티 session을 만들어 준다.(Security ContextHolder) 
* 시큐리티가 가지고 있는 <span style="color:red">session에 오브젝트가 정해져 있다.</span>
* <span style="background-color:#fff5b1">시큐리티의 session 오브젝트 => Authentication 타입 객체 </span>
* Authentication 안에 User 정보가 있어야 한다. 
* User 오브젝트 타입=> UserDetails 타입 객체 <br><br>
**<span style="background-color:#fff5b1">Security Session => Authentication 객체 정해짐 => UserDetails(PrincipalDetails)</span>**

**PrincipalDetails**

```java
package com.cos.security1.auth;

import java.util.ArrayList;
import java.util.Collection;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;

import com.cos.security1.Model.User;

// Security Session => Authentication 객체 정해짐 => UserDetails(PrincipalDetails)
public class PrincipalDetails implements UserDetails {

	private User user;
	
	public PrincipalDetails(User user) {
		this.user=user;
	}
	
	
	// 해당 User 의 권한을 리턴하는 곳
	@Override
	public Collection<? extends GrantedAuthority> getAuthorities() {
	Collection<GrantedAuthority>collect = new ArrayList<>();
	collect.add(new GrantedAuthority() {
		
		@Override
		public String getAuthority() {
			return user.getRole();
		}
	});
		return collect;
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
		// 1년동안 회원이 로그인을 안하면 !! 휴먼계정으로 하기
		// 현재 시간 - 로그인 시간 => 1년을 초과 하면 false한다.
		
		return true;
	}

}
```

**PrincipalDetailsService**

* 시큐리티 설정에서 .loginProcessingUrl("/login");
* login 요청이 오면 자동으로 UserDetailsService 타입으로 loC되어 있는 loadUserByUsername 함수 실행

```java
package com.cos.security1.auth;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import com.cos.security1.Model.User;
import com.cos.security1.Repository.UserRepository;

@Service
public class PrincipalDetailsService implements UserDetailsService{

	
	@Autowired
	private UserRepository userRepository;

	
	@Override
	public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
		User userEntity = userRepository.findByUsername(username);
		if(userEntity !=null) {
			return new PrincipalDetails(userEntity);
		}
		return null;
	}

}
```
### 시큐리티 권한 설정 방법 2

**<span style="background-color:#fff5b1;">@EnableGlobalMethodSecurity(securedEnabled = true,prePostEnabled = true) <br>시큐어 어노테이션 활성화,preAutorize 어노테이션 활성화</span>**

1. @Secured("ROLE_ADMIM") 특정 한 부분에만 권한을 부여할 경우

2. @PreAuthorize("hasRole('ROLE_MANGER')or hasRole('ROLE_ADMIN')") 권한을 설정할 떄 2개의 권한을 설정한다. 

**IndexController**

<img width="403" alt="q" src="https://github.com/jjky123kr/jjky123kr/assets/107549149/4aca7df5-c5fc-47e2-988d-167995cdb711">







