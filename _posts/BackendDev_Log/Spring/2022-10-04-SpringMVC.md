---
layout: post
related_posts:
  - /backdev-log/spring/spring_4/
title: Spring MVC 패턴 1탄 
categories: 
  - backdev-log
  - spring
---
# Spring MVC 패턴

### Spring 흐름
![image](https://user-images.githubusercontent.com/107549149/192942110-db86f88d-6278-47a0-9118-c41ab32d2160.png){: width="500" height="500"}
 
- 요청 처리 순서
1. 사용자가 요청하면 아파치 톰캣이 실행이 되면서 web.xml (환경설정) 파일을 읽어온다.
web.xml 파일에 DispatcherServlet(front controller servlet) 가장 먼저 찾아간다. 이름 과 위치가 등록 되어있어서 읽어온다. 
2. servlet-mapping은 어떤 확장자가 요청 하는지 판별을 위해서  패턴을 지정하여 요청할때 어디로 찾아갈 지 결정한다. 
-모든 요청을 찾아간다.(/)
### 프로그램 구조
<img width="209" alt="5" src="https://user-images.githubusercontent.com/107549149/193738905-88c82df8-a824-47bd-aad9-2695fee43bd4.png">



### web.xml 기본 세팅 
1. DispatcherServlet 와 servlet-mapping 세팅
2. servlet-context.xml(Spring 환경설정 파일) 등록한다.
3. root-context.xml(Spring 환경설정 파일) 등록한다. 
> Spring의 환경 설정 파일은 자동으로 실행이 안되어, web.xml등록해야한다. 
* 만약 환경설정 파일을 resources에 저장을 하면, 이때 classpath: 붙여서 등록한다.
4. Filter 한글 인코딩 등록 => 한글 값이 post방식으로 전송 될떄 utf-8로 인코딩을 시켜주는 역할

```xml
	<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
	<!-- 한글 입력 -->
	<filter>
		<filter-name>CharacterEncodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>CharacterEncodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	<!-- The definition of the Root Spring Container shared by all Servlets 
		and Filters -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>/WEB-INF/spring/root-context.xml</param-value>
	</context-param>
	<!-- Creates the Spring Container shared by all Servlets and Filters -->
	<listener>
		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
	</listener>
	<!-- Processes application requests -->
	<servlet>
		<servlet-name>appServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>/WEB-INF/spring/appServlet/servlet-context.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
</web-app>
```
servlet-context.xml 
1. 어노테이션으로 bean 을 사용 
> context:component-scan base-package="com.ch.hello" />

base-package 의 디렉토리 안에 있는 파일을 불러온다. 
2. ViewResolver 등록 => set 메소드 주입(set DI)
> beans:property name="prefix" value="/WEB-INF/views/" />  
  beans:property name="suffix" value=".jsp" />
  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
	<!-- DispatcherServlet Context: defines this servlet's request-processing infrastructure -->
	<!-- Enables the Spring MVC @Controller programming model -->
	<annotation-driven />
	<!-- Handles HTTP GET requests for /resources/** by efficiently serving up static resources in the ${webappRoot}/resources directory -->
	<resources mapping="/resources/**" location="/resources/" />
	<resources mapping="/css/**" location="/WEB-INF/css/" />
	<resources mapping="/fonts/**" location="/WEB-INF/fonts/" />
	<resources mapping="/js/**" location="/WEB-INF/js/" />
	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" /><!-- view 파일이 저장된 최상위 디렉토리  -->
		<beans:property name="suffix" value=".jsp" /><!--view 파일의 확장자  -->
	</beans:bean>	
	<context:component-scan base-package="com.ch.hello" />
</beans:beans>
```
root-context.xml 
1. DB연동 
2. 객체 생성 
* setter DI 메소드를 이용해서 주입

idex.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	function chk() {
		var url = document.getElementById("url").value;
		location.href = url;
	}
</script>
</head>
<body>
	<h2>원하는 작업을 선택하세요</h2>
	<!-- <script type="text/javascript">
			location.href="hello";
		</script> -->
	<select id="url" onchange="chk()">
		<option value="hello">hello</option>
		<option value="color">배경색</option>
		<option value="gugu">구구단</option>
		<option value="joinForm">회원가입</option>
	</select>
</body>
</html>
```
### 예제1

HomeController.java
### @RequestMapping

1. 클라이언트 요청을 받을 떄 <span style="background-color:#fffd91">@RequestMapping 어노테이션이다.</span>   
@RequestMapping(value = "/", method = RequestMethod.GET  
method: 전송 방식 => GET ,POST (대문자)  
value :요청 이름 값 (요청이름값 서로 다르게 설정 한다.)
> 요청 값이 같게 되면, 충돌이 발생된다. 

2. 컨트롤러에서 view 페이지로 값을 가지고 갈 경우에는 Model and View 클래스에 값을  
저장해서 이동한다.
* public String hello(Locale locale, Model model) { //  Model 객체가 생성된다.
* model.addAttribute 로 가져간다. (key, value) 값을 가져간다.
3. 접근 제어자 : public  사용
   기본자료형  
  <span style="background-color:#fffd91"> String 클래스=> view 페이지에 출력 할떄 ${}
   DTO 클래스 => view 페이지에서 출력 할떄 ${DTO.필드명}
   list 클래스 => view 페이지에서 출력 할떄 forEach 태그 사용한다.</spn> 
    
4. return 할 떄 prefix와 suffix 를 제외한 나머지 경로 파일명만 기술한다.     
   return "home"; // view 페이지 경로 값 
  

```java
package com.ch.hello;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class HomeController {
	private static final Logger logger = LoggerFactory.getLogger(HomeController.class);

	@RequestMapping(value = "/hello", method = RequestMethod.GET) 
	public String hello(Locale locale, Model model) { // Model로 변수명을 model 객체생성 없이 사용가능.
  // public 과 자료형은 String 클래스 사용
		logger.info("Welcome home! The client locale is {}.", locale);
		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
		String formattedDate = dateFormat.format(date);
    // model.addAttribute 로 가져간다. (key, value)
		model.addAttribute("serverTime", formattedDate);
		return "home"; // 뷰 페이지 경로 값
	}

	@RequestMapping("/color")
	public String color(Model model) {
		String[] color = { "red", "orange", "yellow", "green", "blue", "navy", "violet" };
		int num = (int) (Math.random() * 7);
		model.addAttribute("color", color[num]);
		return "color"; // view 페이지 경로
	}

	@RequestMapping("/gugu")
	public String gugu(Model model) {
		int num = (int) (Math.random() * 8) + 2;
//		int a = num / 0;
		model.addAttribute("num", num);
		return "gugu"; //view 페이지 경로
	}
	/*
	 * @ExceptionHandler(ArithmeticException.class) 
	 * public String err() { 
	 * return
	 * "arr-err"; }
	 */
}
```

home.jsp (view 페이지)

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<%@ page session="false" %>

<!DOCTYPE html>
<html>
<head>
	<title>Home</title>
</head>
<body>
<h1>
	Hello world!  
</h1>

<P>  The time on the server is ${serverTime}. </P>
</body>
</html>
```
gugu.jsp
1. core 라이브러리 사용
2. forEach 태그로 루프 돌린다. 
> c:forEach var="i" begin="1" end="9">
3. 기본 자료형이 int 형이라서 EL로 출력한다.
> ${num } * ${i } = ${num*i }

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>	
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<h2>구구단 ${num }단</h2>
	<c:forEach var="i" begin="1" end="9">
		${num } * ${i } = ${num*i }<br>
	</c:forEach>
</body>
</html>
```

### 예제2(회원가입)
#### @ModelAttribute
Join.Controller.java

* <span style="background-color:#fffd91">@ModelAttribute</span>
 어노테이션은 DTO클래스로 값을 받을때 사용하는 어노테이션이다.
* @ModelAttribute Member member 어노테이션이 생략 되어 있다.
* 가입한 name값이 DTO객체의 필드 값과 테이블 의 value 값을 일치시킨다. (mapping) 
* join(Member member, Model model) DTO 객체 생성 
* 만약 session 객체를 생성 하고 싶으며 Model 객체 대신 설정 하면 session이 생성된다.
* public String join(Member member, Session session)
* View 페이지 출력 할떄 ${member.필드} (값이 Model model 공유가 되어서 EL로 출력한다.)

```java
package com.ch.hello;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;


@Controller
public class JoinController {
	@RequestMapping("/joinForm")
	public String joinForm() {
		return "joinForm"; // 뷰페이지 경로 값
	}

	@RequestMapping("/join")
	public String join(Member member, Model model) {
		model.addAttribute("member", member);
		return "joinResult"; // 뷰페이지 경로 값
	}
}
```

joinForm.jsp(view 페이지)

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div class="container" align="center">
		<form action="join" method="post">
			<h2 class="text-primary">회원가입</h2>
			<table class="table table-bordered">
				<tr>
					<th>아이디</th>
					<td><input type="text" name="id" required="required"></td>
				</tr>
				<tr>
					<th>암호</th>
					<td><input type="password" name="pass" required="required"></td>
				</tr>
				<tr>
					<th>이름</th>
					<td><input type="text" name="name" required="required"></td>
				</tr>
				<tr>
					<th>나이</th>
					<td><input type="number" name="age" required="required"></td>
				</tr>
				<tr>
					<th>주소</th>
					<td><input type="text" name="addr" required="required"></td>
				</tr>
				<tr>
					<th>이메일</th>
					<td><input type="email" name="email" required="required"></td>
				</tr>
				<tr>
					<th colspan="2"><input type="submit" value="확인"></th>
				</tr>
			</table>
		</form>
	</div>
</body>
</html>
```
joinResult.jsp(view )
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div class="container" align="center">
		<h2 class="text-primary">회원가입 결과</h2>
		<table class="table table-bordered">
			<tr>
				<th>아이디</th>
				<td>${member.id }</td>
			</tr>
			<tr>
				<th>암호</th>
				<td>${member.pass }</td>
			</tr>
			<tr>
				<th>이름</th>
				<td>${member.name }</td>
			</tr>
			<tr>
				<th>나이</th>
				<td>${member.age }</td>
			</tr>
			<tr>
				<th>주소</th>
				<td>${member.addr }</td>
			</tr>
			<tr>
				<th>이메일</th>
				<td>${member.email }</td>
			</tr>
			<tr>
				<th colspan="2"><a href="joinForm">회원가입 창</a>
			</tr>
		</table>
	</div>
</body>
</html>
```
### 예제 3
Dispatcher Servlet     
### @RequestParam 
PersonController.java
* <span style="background-color:#fffd91">@RequestParam 어노테이션 </span> 
> 개별적으로 값을 받을 떄 ==> @RequestParam 어노테이션 
* @RequestParam("name")  String name : 전달 되는 이름을  String name 변수에 저장한다.
> 전달 되는 변수명 과 값을 받는 변수명 이  @RequestParam("name") 생략가능 
* @RequestParam("name") 어노테이션은 requset.getParameter("name")와 같은 역할
> 값을 한번에 많이 받을 떄는 model.addAttribute("name", name);
* @RequestParam("num") int num : 전달 되는 번호 값 int num 변수에 저장한다. 
> 이때 형변화 없이 설정이 가능하다. 

```java
package com.ch.hello;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

@Controller
public class PersonController {
  // String name = requset.getParamter("name");
	//@RequestParam("name")  String name
	@RequestMapping("/addr") 
	public String addr(@RequestParam("name") String name,
			                             @RequestParam("name")  String addr, 
			                              Model model) {
		model.addAttribute("name", name);
		model.addAttribute("addr", addr);
		return "addr";
	}

	@RequestMapping("/addr2")
	public String addr2(Person p, Model model) {
		model.addAttribute("person", p);
		return "addr2";
	}
}
```
person.jsp (view )
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
	<title>Insert title here</title>
</head>
<body>
	<h2>이름과 주소</h2>
	<form action="addr"> 
	<!-- <form action="addr2"> -->
		이름 : <input type="text" name="name">
		<p>
			주소 : <input type="text" name="addr">
		<p>
			<input type="submit" value="확인">
	</form>
</body>
</html>
```
person.java (DTO)
```java
package com.ch.hello;

public class Person {
	private String name;
	private String addr;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getAddr() {
		return addr;
	}

	public void setAddr(String addr) {
		this.addr = addr;
	}
}
```
 addr.jsp (view)
* 개별적으로 값을 전달 한다. 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>${name }님 ${addr }에 사시는 군요
</body>
</html>
```
addr2.jsp
* Model  DTO 객체로 생성하여 값을 전달 ${person.필드} 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>${person.name }님 ${person.addr }에 사시는 군요
</body>
</html>
```
### 예제4
sample.jsp 
* list 와 DTO 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<script type="text/javascript">
		// location.href="sample";
		location.href = "list";
	</script>
</body>
</html>
```
SampleVo.java

```java
package com.ch.hello;

public class SampleVo {
	private Integer mno;
	private String firstName;
	private String lastName;

	public Integer getMno() {
		return mno;
	}

	public void setMno(Integer mno) {
		this.mno = mno;
	}

	public String getFirstName() {
		return firstName;
	}

	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	/*
	 * public String toString() { return
	 * "SampleVosuper[나이:"+mno+", 성:"+firstName+ ", 이름:"+lastName+"]"; }
	 */
}
```
Dispatcher Servlet
* 자료형이 DTO or list 
* @RestController = @ Controller + @ResponseBody 결과를 callback 함수로 돌려준다. 
* public SampleVo sample(){  DTO를 객체를 생성한다.
  > SampleVo은 DTO 클래스이다.
* @ResponseBody는 json 형태로 값을 돌려준다. 그래서  json 라이브러리가 있어야 한다.
> 비동기적 처리 할떄 사용 된다. @ResponseBody
* list를 객체를 생성한다.

```java
package com.ch.hello;

import java.util.ArrayList;
import java.util.List;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
//@Controller
public class SampleController {
	@RequestMapping("/sample")
  @ResponseBody 
	public SampleVo sample() {
		SampleVo sv = new SampleVo();
		sv.setMno(23);
		sv.setFirstName("홍");
		sv.setLastName("길동");
		return sv; // json 형태로 요청한 곳에 값을 돌려준다.
	}

	@RequestMapping("/list")
	public List<SampleVo> list() {
		List<SampleVo> list = new ArrayList<SampleVo>();
		for (int i = 1; i <= 10; i++) {
			SampleVo sv = new SampleVo();
			sv.setMno(i);
			sv.setFirstName("홍");
			sv.setLastName("길동" + i);
			list.add(sv);
		}
		return list;
	}
}
```



