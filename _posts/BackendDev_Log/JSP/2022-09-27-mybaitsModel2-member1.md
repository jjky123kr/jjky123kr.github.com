---
layout: single
title : Mybaits와 Model2 회원가입
folder: "spring"
categories: 
      Spring
tags: [blog,Spring]

author_profile: true
sidebar:
  nav: "main"

toc: ture
toc_alble: 목록
toc_icon: "bars"
toc_sticky: true
---

### Mybatis 데이터 액세스 계층 흐름

```
MemberDAO -> mybatis-config.xml(mybaits환경설정 파일) ->member.xml (mapper파일)


SqlSession(SQL문을 실행시키는 메소드를 제공하는 인터페이스)

SQL문   -   메소드
-----------------------------
insert  -  int insert()
update  -  int update()
delete  -  int delete()
select  - Objsect select() :  검색한 결과가 1개일떄 (DTO객체)
           List   select()  : 검색한 결과가 2개 이상일떄 사용(List 객체)
```
### MVC 흐름
![웹 캡처_18-9-2022_15241_](https://user-images.githubusercontent.com/107549149/190888145-dc4fc3bb-8f71-4796-b024-92cebf544f5c.jpeg){: width="500" height="500"}

* 클라이언트의 요청 
* Controller(servlet)에서 어느 service 로 갈지 결정된다.
* service 클래스는 -> DAO ->DB(데이터 정보)
* DB->DAO->service클래스로 간다.
* service-> serviet로 간다. 이때 service 클래스에서 페이지 이동시 공유설정한다.(forward)
* forward 해서, view 페이지에서 정보 전달 된다. 

### 프로그램 구성
```
mybatismember-src-main-java- controller - MemberController.java
                             DTO클래스: model - MemberDTO.java (DTO 클래스)
                             DAO클래스: dao - MemberDAO.java (DAO 클래스)
                             Action 인터페이스  : service - Action.java 
                             ActionForward클래스 : service - ActionForward.java

                        Service 클래스 : service - MemberInsert.java (회원가입)
		                    IdCheck.java(ID중복검사)
		                    Login.java(로그인)
		                    Logout.java(로그아웃)
		                    UpdateMember.java(정보수정폼)
		                    Update.java(정보수정)	
		                    DeleteMember.java(회원탈퇴폼)
		                    Delete.java(회원탈퇴)

resources -db.properties(DB연동)
          -member.xml(mapper 파일=SQL문)
          -mybatis-config.xml(환경설정 파일)

webapp(view 페이지)-member폴더-회원가입폼 : memberform.jsp --> member.jsp
                      ID중복검사 : idcheck.jsp
                      로그인 폼 : loginform.jsp --> login.jsp --> main.jsp
                      수정 폼 : updateform.jsp --> update.jsp
                      삭제 폼 : deleteform.jsp  --> delete.jsp
                      로그아웃 : logout.jsp

             프로젝트 -pom.xml (라이브러리 관리)
```
<img width="224" alt="1" src="https://user-images.githubusercontent.com/107549149/192406835-6c1da104-b69f-4ff2-abc2-1fc397e40ee0.png">

### pox.xml
* 가장 먼저 설정을 해야한다. (라이브러리 설정)
* 의존 라이브러리 관리 (오라클, Mysql, iBatis,Mybatis,jstl,cos) 설정
* 라이브러리를 <dependencies  안쪽에 라이브러를 작성한다. 
* 주의사항 : 오라클 라이브러리가 잘 동작 되지 않을때, 다운로드 되어있는 오라클 넣어서 설정한다. 

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.myhome</groupId>
	<artifactId>mybatismember</artifactId>
	<packaging>war</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>mybatismember Maven Webapp</name>
	<url>http://maven.apache.org</url>

	<!-- 복사 시작 -->
	<repositories>
		<repository>
			<id>codelds</id>
			<url>https://code.lds.org/nexus/content/groups/main-repo</url>
		</repository>
	</repositories>

	<dependencies>

		<!-- 오라클 JDBC Library -->
		<dependency>
			<groupId>com.oracle</groupId>
			<artifactId>ojdbc6</artifactId>
			<version>11.2.0.3</version>
		</dependency>

		<!-- MySQL -->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.47</version>
		</dependency>

		<!-- iBatis -->
		<dependency>
			<groupId>org.apache.ibatis</groupId>
			<artifactId>ibatis-sqlmap</artifactId>
			<version>2.3.4.726</version>
		</dependency>

		<!-- mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.5.3</version>
		</dependency>		

		<!-- jstl -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		<dependency>
			<groupId>org.apache.taglibs</groupId>
			<artifactId>taglibs-standard-impl</artifactId>
			<version>1.2.5</version>
		</dependency>

		<!-- cos -->
		<dependency>
			<groupId>servlets.com</groupId>
			<artifactId>cos</artifactId>
			<version>05Nov2002</version>
		</dependency>

		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
	</dependencies>
	<build>
		<finalName>mybatismember</finalName>
		<plugins>
	<plugin>
		<artifactId>maven-war-plugin</artifactId>
		<version>3.2.2</version>
  </plugin>
</plugins>	
		
	</build>
</project>
```
### db.properties (오라클 연동)

```

jdbc.driverClassName=oracle.jdbc.driver.OracleDriver
jdbc.url=jdbc:oracle:thin:@localhost:1521:xe
jdbc.username=totoro
jdbc.password=totoro123
```
### mybatis-config.xml (mybatis 환경설정)
1. DTO 클래스 : 팩토리.클래스 명을 치환하여 새로운 이름으로 생성한다. 

```xml
<typeAliases>
		<typeAlias type="model.MemberDTO" alias="member"></typeAlias>
</typeAliases>
```
2. DB연동 
* db.properties(오라클)파일 연동

```xml

<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driverClassName}" />
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jdbc.username}" />
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
	</environments>

```
3. mapper 파일

```
<mappers>
	<mapper resource="member.xml" />
</mappers>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource="db.properties" />
	<typeAliases>
		<typeAlias type="model.MemberDTO" alias="member"></typeAlias>
	</typeAliases>
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${jdbc.driverClassName}" />
				<property name="url" value="${jdbc.url}" />
				<property name="username" value="${jdbc.username}" />
				<property name="password" value="${jdbc.password}" />
			</dataSource>
		</environment>
	</environments>
	<mappers>
		<mapper resource="member.xml" />
	</mappers>
</configuration>
```
### 테이블 생성
```sql
-- 회원관리
select * from tab;
select * from member0609;

create table member0609(
	id varchar2(20) primary key,
	passwd  varchar2(20) not null,
	name varchar2(20) not null,
	jumin1 varchar2(6) not null,
	jumin2 varchar2(7) not null,
	mailid varchar2(30), 
	domain varchar2(30), 
	tel1 varchar2(5),
	tel2 varchar2(5),
	tel3 varchar2(5),
	phone1 varchar2(5),
	phone2 varchar2(5),
	phone3 varchar2(5),
	post varchar2(10),
	address varchar2(200),
	gender varchar2(20),
	hobby varchar2(50),
	intro varchar2(2000),
	register timestamp );
```
*  자동으로 mapping으로 하게 설정한다. 
> 테이블 컬럼 명과DTO 필드 명과 입력양식의 name의 명을 동일하게 해주어야 mapping이 된다.

```java
package controller;

import java.io.IOException;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import service.Action;
import service.ActionForward;
import service.Delete;
import service.Idcheck;
import service.Login;
import service.MemberInsert;
import service.Update;
import service.UpdateMember;

/**
 * Servlet implementation class MemberController
 */
@WebServlet("*.do")
public class MemberController extends HttpServlet {
	private static final long serialVersionUID = 1L;

	public void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		String requestURI = request.getRequestURI();
		String contextPath = request.getContextPath();               
		String command = requestURI.substring(contextPath.length());  
		
		System.out.println("requestURI:"+requestURI);    // requestURI: /model2member/MemberInsert.do
		System.out.println("contextPath:"+contextPath);  // contextPath: /model2member
		System.out.println("command:"+command);          // command: /MemberInsert.do
		
		Action action = null;
		ActionForward forward = null;



 // 포워딩 처리
		if(forward != null) {
			if(forward.isRedirect()) {		// redirect 방식으로 포워딩
				response.sendRedirect(forward.getPath());
			}else {							// dispatcher 방식으로 포워딩
				RequestDispatcher dispatcher = 
						request.getRequestDispatcher(forward.getPath());
				dispatcher.forward(request, response);
			}
		}		
		
	}// doProcess() end	
```

### memberform.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원 가입 폼</title>
<script src="http://code.jquery.com/jquery-latest.js"></script>

<script src="http://dmaps.daum.net/map_js_init/postcode.v2.js"></script>
<script>
	function openDaumPostcode() {
		new daum.Postcode({
			oncomplete : function(data) {
				// 팝업에서 검색결과 항목을 클릭했을때 실행할 코드를 작성하는 부분.
				// 우편번호와 주소 정보를 해당 필드에 넣고, 커서를 상세주소 필드로 이동한다.
//				document.getElementById('join_zip1').value = data.postcode1;
//				document.getElementById('join_zip2').value = data.postcode2;
				document.getElementById('post').value = data.zonecode;
				document.getElementById('address').value = data.address;
				
			}
		}).open();
	}
</script>


<!-- 외부 자바스크립트 파일 불러오기 -->
<script src="<%=request.getContextPath() %>/member/member.js"></script>

</head>
<body>

<form method="post" action="<%=request.getContextPath() %>/MemberInsert.do"> 
<table border=1 width=500 align=center>
	<caption>회원 가입</caption>
	<tr><td>ID</td>
		<td><input type=text autofocus="autofocus" id="id" name="id">
			<input type=button value="ID중복검사" id="idcheck">
			<div id="myid"></div>
		</td>
	</tr>
	<tr><td>비밀번호</td>
		<td><input type=password id="passwd" name="passwd"></td>
	</tr>
	<tr><td>성명</td>
		<td><input type=text id="name" name="name"></td>
	</tr>
	<tr><td>주민번호</td>
		<td><input type=text size=6 maxlength=6 id="jumin1" name="jumin1">-
			<input type=text size=7 maxlength=7 id="jumin2" name="jumin2">
		</td>
	</tr>
	<tr><td>E-Mail</td>
		<td><input type=text size=10 id="mailid" name="mailid">@
		    <input type=text size=10 id="domain" name="domain">
		    <select id="email">
		    	<option value="">직접입력</option>
		    	<option value="naver.com">네이버</option>
		    	<option value="daum.net">다음</option>
		    	<option value="nate.com">네이트</option>
		    	<option value="gmail.com">gmail</option>
		    </select>		    
		 </td>
	</tr>
	<tr><td>전화번호</td>
		<td><input type=text size=4 id="tel1" name="tel1" maxlength=4>-
			<input type=text size=4 id="tel2" name="tel2" maxlength=4>-
			<input type=text size=4 id="tel3" name="tel3" maxlength=4>
		</td>
	</tr>
	<tr><td>핸드폰</td>
		<td><select id="phone1" name="phone1">
				<option value="">번호선택</option>
				<option value="010">010</option>
				<option value="011">011</option>
				<option value="016">016</option>
				<option value="018">018</option>
				<option value="019">019</option>
			</select>-
			<input type=text size=4 id="phone2" name="phone2" maxlength=4>-
			<input type=text size=4 id="phone3" name="phone3" maxlength=4>
		</td>
	</tr>
	<tr><td>우편번호</td>
		<td><input type=text size=5 id="post" name="post">
			<input type=button value="우편번호검색" 
			       onClick="openDaumPostcode()">
		</td>
	</tr>
	<tr><td>주소</td>
		<td><input type=text size=45 id="address" name="address"></td>
	</tr>
	<tr><td>성별</td>
		<td>
			<input type=radio id="male" name="gender" value="남자">남자
			<input type=radio id="female" name="gender" value="여자">여자
		</td>
	</tr>
	<tr><td>취미</td>
		<td>
			<input type="checkbox" id="h1" name="hobby" value="공부" checked>공부
			<input type="checkbox" id="h2" name="hobby" value="게임">게임
			<input type="checkbox" id="h3" name="hobby" value="등산">등산
			<input type="checkbox" id="h4" name="hobby" value="낚시">낚시
			<input type="checkbox" id="h5" name="hobby" value="쇼핑">쇼핑
		</td>
	</tr>	
	<tr><td>자기소개</td>
		<td>
			<textarea id="intro" name="intro" rows="5" cols="50" placeholder="자기소개를 100자 이내로 입력하세요"></textarea>
		</td>
	</tr>
	<tr><td colspan=2 align=center>
			<input type=submit value="회원가입">
			<input type=reset value="취소">
		</td>
	</tr>		
</table>
</form>
</body>
</html>
```
### MemberInsert.java

```java
package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.MemberDAO;
import model.MemberDTO;

public class MemberInsert implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("MemberInsert");
		
		request.setCharacterEncoding("utf-8");
		
		MemberDTO member = new MemberDTO();
		member.setId(request.getParameter("id"));
		member.setPasswd(request.getParameter("passwd"));
		member.setName(request.getParameter("name"));
		member.setJumin1(request.getParameter("jumin1"));
		member.setJumin2(request.getParameter("jumin2"));
		member.setMailid(request.getParameter("mailid"));
		member.setDomain(request.getParameter("domain"));
		member.setTel1(request.getParameter("tel1"));
		member.setTel2(request.getParameter("tel2"));
		member.setTel3(request.getParameter("tel3"));
		member.setPhone1(request.getParameter("phone1"));
		member.setPhone2(request.getParameter("phone2"));
		member.setPhone3(request.getParameter("phone3"));
		member.setPost(request.getParameter("post"));
		member.setAddress(request.getParameter("address"));
		member.setGender(request.getParameter("gender"));
		
		String h = "";
		String[] h1 = request.getParameterValues("hobby");
		for(String h2 : h1) {
			h += h2+"-";			// 공부-게임-
		}
		member.setHobby(h);
		
		member.setIntro(request.getParameter("intro"));
		
		MemberDAO dao = MemberDAO.getInstance();
		int result = dao.insert(member);
		if(result == 1) {
			System.out.println("회원가입 성공");
		}
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("/member/loginform.jsp");
		
		return forward;
	}

}
```
### controller.java(회원가입)

```java
// 회원 가입
		if(command.equals("/MemberInsert.do")) {
			try {
				action = new MemberInsert();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
			
		// 회원가입 폼	
		}else if(command.equals("/MemberForm.do")) {
			forward = new ActionForward();
			forward.setRedirect(true);
			forward.setPath("./member/memberform.jsp");
```
### mapper파일 생성할떄 <mapper></mapper>안에 작성해야한다.
* 태그의 ID값으로 구분해준다. 
* paramteerType
* resultType 
* namespace  
> 테이블 갯수가 늘어나며,mapper파일도 증가된다. 
 이때 ID값이 중복될 수 있어서 충돌이 발생한다. 
 그래서 각 각의 mapper파일의 namespace 이름 중복되지 않게 작성한다.
 * 표기법: member.getID() = #{id}으로 작성한다.

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="mymember">

	<!-- 회원가입 -->
	<insert id="insert" parameterType="member">
	  insert into member0609 values(#{id},#{passwd},#{name},#{jumin1},#{jumin2},
	  #{mailid},#{domain},#{tel1},#{tel2},#{tel3},#{phone1},#{phone2},#{phone3},
	  #{post},#{address},#{gender},#{hobby},#{intro},sysdate)
    </insert>
</mapper>
```

### MemberDAO
```java
// DAO(Data Access Object)

package dao;

import java.io.Reader;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import model.MemberDTO;

public class MemberDAO {
	
	private static MemberDAO instance = new MemberDAO();
	
	public static MemberDAO getInstance() {
		return instance;
	}
	
	public SqlSession getSession() { // 환경설정을 구해옴
		SqlSession session=null;
		Reader reader=null;			
		try {
			reader = Resources.getResourceAsReader("mybatis-config.xml"); //DB연동
			SqlSessionFactory sf = new SqlSessionFactoryBuilder().build(reader);
			session = sf.openSession(true);	 // auto commit	
		}catch(Exception e) {
			e.printStackTrace();
		}		
		return session;
	}
		
		
	// 회원가입 = mapper 파일
	public int insert(MemberDTO member) throws Exception{
		int result=0;
		SqlSession session = getSession();
		result = session.insert("insert", member);		
		System.out.println("result:"+result);
		
		return result;
	}
```
### ID중복검사 (유효성 검사)

```js
$(document).ready(function(){	
	
	// ID 중복검사
	$("#idcheck").click(function(){
		if($("#id").val()==""){
			alert("ID를 입력하세요");
			$("#id").focus();
			return false;
		}else{
			
			var id = $("#id").val(); // 사용자가 입력한 ID	
			
			$.ajax({
				type:"post",
				url:"/mybatismember/Idcheck.do", 
				data:{"id":id}, //json 타입
				datatype:"text",
				success:function(data){
//					alert(data); 
					
					if(data==1){	// 중복 ID
						$("#myid").text("중복ID"); //div= id값
						$("#id").val("").focus();
					}else{			// 사용 가능한 ID
						$("#myid").text("사용 가능한 ID");
						$("#passwd").focus();
					}					
				}
			});			
		}		
	});
```
### Idcheck.java(Id중복검사)

```java
package service;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import dao.MemberDAO;

public class Login implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("Login");
		
		response.setContentType("text/html; charset=utf-8");
		request.setCharacterEncoding("utf-8");	
		
		HttpSession session = request.getSession();
		PrintWriter out = response.getWriter();
		
		String id = request.getParameter("id");
		String passwd = request.getParameter("passwd");
		
		MemberDAO dao = MemberDAO.getInstance();
		int result = dao.memberAuth(id, passwd);
		if(result == 1) System.out.println("회원인증 성공");
		
		if(result == 1) {			// 회원인증 성공
			session.setAttribute("id", id);
		}else {						// 회원인증 실패
			out.println("<script>");
			out.println("alert('로그인 실패');");
			out.println("history.go(-1)");
			out.println("</script>");
			out.close();	
			
			return null;
		}
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("/member/main.jsp");
		
		return forward;
	}

}
```
### controller(Id중복검사)

```java
// ID중복 검사(ajax)	
		}else if(command.equals("/Idcheck.do")) {
			try {
				action = new Idcheck();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
```
### member.xml
```xml
<!-- ID중복검사, 회원인증 -->
	<select id="idcheck" parameterType="String" resultType="member">
	 select * from member0609 where id = #{id}
	</select>
```
### Member.DAO(ID중복 검사)
```java
// ID중복검사
	public int idcheck(String id) throws Exception{
		int result = 0;
		SqlSession session = getSession();
		MemberDTO member = session.selectOne("idcheck", id);
		if(member != null) {	// 중복 ID	
			result = 1;			
		}else {					// 사용가능한 ID
			result = -1;
		}
		
		return result;
	}
```
### main.jsp (메인 페이지)
* jstl의 cos라이브러리 사용 
>
* session이 있는 경우와 없는 경우로 나누어서 설정한다.
* session 있으면, 로그아웃, 회원정보 수정, 회원탈퇴
* session 없으면, 회원가입,로그인
* 경로를 잘 찾아가기 위해서 <%=request.getContextPath() %> 설정한다. 

### controller 클래스(회원정보 수정 폼)

```java
// 회원정보 수정폼	
		}else if(command.equals("/UpdateMember.do")) {
			try {
				action = new UpdateMember();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
```
### updateMember.java( 회원정보 수정 폼)

```java
package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import dao.MemberDAO;
import model.MemberDTO;

public class UpdateMember implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("UpdateMember");
		
		HttpSession session = request.getSession(); 
		String id = (String)session.getAttribute("id"); // session 공유된 id를 값을 받는다. 
		System.out.println("id:"+id);
		
		MemberDAO dao = MemberDAO.getInstance();
		MemberDTO member = dao.getMember(id);
		System.out.println("member:"+member);
		
		String hobby = member.getHobby();	// "공부-게임-등산-"
		String[] h = hobby.split("-");
		
		request.setAttribute("member", member);				
		request.setAttribute("h", h);				
		
		ActionForward forward = new ActionForward();	
		forward.setRedirect(false);        // dispatcher 방식으로 포워딩
		forward.setPath("/member/updateform.jsp");
		
		return forward;
	}

}
```
### member.xml(1명의 상세정보)

```xml

	<!-- ID중복검사, 회원인증 , 1명의 ㄴ상세정보-->
	<select id="idcheck" parameterType="String" resultType="member">
	 select * from member0609 where id = #{id}
	</select>
```	

### MemberDAO(회원1명 정보) : 수정폼,수정, 삭제

```java
public MemberDTO getMember(String id) throws Exception{
		SqlSession session = getSession();
		MemberDTO member = session.selectOne("idcheck", id);
		
		return member;
	}
````
### main.jsp
```jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%-- <%@ taglib prefix="c" uri="http://java.sun.com/jstl/core" %> --%> <!-- 안됨 -->


<!--  세션이 있으면 -->
<%-- <c:if test="${not empty sessionScope.id}"> --%>	<!-- 2개 모두 잘됨 -->
<c:if test="${sessionScope.id != null }">
	${sessionScope.id} 님 환영 합니다. <br><br>
	
	<a href="<%=request.getContextPath() %>/UpdateMember.do">회원정보 수정</a>  <br><br>
	
	<a href="<%=request.getContextPath() %>/Logout.do">로그아웃</a>  <br><br>
	
	<a href="<%=request.getContextPath() %>/DeleteMember.do">회원탈퇴</a> <br><br>
</c:if>

<!-- 세션이 없으면 -->
<%-- <c:if test="${empty sessionScope.id}"> --%>
<c:if test="${sessionScope.id == null}">
	<a href="<%=request.getContextPath() %>/MemberForm.do">회원가입</a> <br><br>
	<a href="<%=request.getContextPath() %>/LoginForm.do">로그인</a> <br><br>
</c:if>
````
### controller(로그아웃)

```java

// 로그 아웃	
		}else if(command.equals("/Logout.do")) {
			forward = new ActionForward();
			forward.setRedirect(false);
			forward.setPath("/member/logout.jsp");
```
### logout.jsp(로그아웃 페이지)
* 로그아웃은 service 클래스가 필요없다.
> service클래스는 DB연동이 필요한 경우에만 사용된다. 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>

<%	// 세션 삭제
	session.invalidate();
%>

<script>
	alert("로그아웃");
	location.href="./LoginForm.do"; // 로그인 페이지 이동
</script>
````
### controller (회원수정 폼)

* main.jsp 에서 회원수정 클릭 했을 떄 updateMember.do 
설정

```java
// 회원정보 수정폼	
		}else if(command.equals("/UpdateMember.do")) {
			try {
				action = new UpdateMember();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
```
### updateMember.java(회원 수정 폼)

```java
package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import dao.MemberDAO;
import model.MemberDTO;

public class UpdateMember implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("UpdateMember"); 
		
		HttpSession session = request.getSession(); 
		String id = (String)session.getAttribute("id"); // session 공유된 id를 값을 받는다. 
		System.out.println("id:"+id);
		
		MemberDAO dao = MemberDAO.getInstance();
		MemberDTO member = dao.getMember(id);
		System.out.println("member:"+member);
		
		String hobby = member.getHobby();	// "공부-게임-등산-"
		String[] h = hobby.split("-");
		
		request.setAttribute("member", member); // 공유 설정	
		request.setAttribute("h", h);				
		
		ActionForward forward = new ActionForward();	
		forward.setRedirect(false);        // dispatcher 방식으로 포워딩
		forward.setPath("/member/updateform.jsp");
		
		return forward;
	}

}
```
### updateform.jsp (회원정보 수정)
* jstl 를 사용한다. 
* if 태그와 forEach  태그 사용 intems 속성를 사용한다. 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %> 

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원 수정 폼</title>
<script src="http://code.jquery.com/jquery-latest.js"></script>

<script src="http://dmaps.daum.net/map_js_init/postcode.v2.js"></script>
<script>
	function openDaumPostcode() {
		new daum.Postcode({
			oncomplete : function(data) {
				// 팝업에서 검색결과 항목을 클릭했을때 실행할 코드를 작성하는 부분.
				// 우편번호와 주소 정보를 해당 필드에 넣고, 커서를 상세주소 필드로 이동한다.
//				document.getElementById('join_zip1').value = data.postcode1;
//				document.getElementById('join_zip2').value = data.postcode2;
				document.getElementById('post').value = data.zonecode;
				document.getElementById('address').value = data.address;
				
			}
		}).open();
	}
</script>


<!-- 외부 자바스크립트 파일 불러오기 -->
<script src="<%=request.getContextPath() %>/member/member.js"></script>

</head>
<body>

<form method="post" action="<%=request.getContextPath() %>/Update.do"> 
<input type="hidden" name="id" value="${member.id}">
<table border=1 width=500 align=center>
	<caption>회원 수정</caption>
	<tr><td>ID</td>
		<td>${member.id}</td>
	</tr>
	<tr><td>비밀번호</td>
		<td><input type=password id="passwd" name="passwd"></td>
	</tr>
	<tr><td>성명</td>
		<td><input type=text id="name" name="name" value="${member.name}"></td>
	</tr>
	<tr><td>주민번호</td>
		<td><input type=text size=6 maxlength=6 id="jumin1" name="jumin1" value="${member.jumin1}">-
			<input type=text size=7 maxlength=7 id="jumin2" name="jumin2" value="${member.jumin2}">
		</td>
	</tr>
	<tr><td>E-Mail</td>
		<td><input type=text size=10 id="mailid" name="mailid" value="${member.mailid}">@
		    <input type=text size=10 id="domain" name="domain" value="${member.domain}">
		    <select id="email">
		    	<option value="">직접입력</option>
		    	<option value="naver.com">네이버</option>
		    	<option value="daum.net">다음</option>
		    	<option value="nate.com">네이트</option>
		    	<option value="gmail.com">gmail</option>
		    </select>		    
		 </td>
	</tr>
	<tr><td>전화번호</td>
		<td><input type=text size=4 id="tel1" name="tel1" maxlength=4 value="${member.tel1 }">-
			<input type=text size=4 id="tel2" name="tel2" maxlength=4 value="${member.tel2 }">-
			<input type=text size=4 id="tel3" name="tel3" maxlength=4 value="${member.tel2 }">
		</td>
	</tr>
	<tr><td>핸드폰</td>
		<td><select id="phone1" name="phone1">
				<option value="">번호선택</option>
				<option value="010" <c:if test="${member.phone1 == '010' }">${'selected'}</c:if> >010</option>
				<option value="011" <c:if test="${member.phone1 == '011' }">${'selected'}</c:if> >011</option>
				<option value="016" <c:if test="${member.phone1 == '016' }">${'selected'}</c:if> >016</option>
				<option value="018" <c:if test="${member.phone1 == '018' }">${'selected'}</c:if> >018</option>
				<option value="019" <c:if test="${member.phone1 == '019' }">${'selected'}</c:if> >019</option>
			</select>-
			<input type=text size=4 id="phone2" name="phone2" maxlength=4 value="${member.phone2 }">-
			<input type=text size=4 id="phone3" name="phone3" maxlength=4 value="${member.phone3 }">
		</td>
	</tr>
	<tr><td>우편번호</td>
		<td><input type=text size=5 id="post" name="post" value="${member.post }">
			<input type=button value="우편번호검색" 
			       onClick="openDaumPostcode()">
		</td>
	</tr>
	<tr><td>주소</td>
		<td><input type=text size=45 id="address" name="address" value="${member.address }"></td>
	</tr>
	<tr><td>성별</td>
		<td>
		
		<c:if test="${member.gender == '남자' }">
			<input type=radio id="male" name="gender" value="남자" checked="checked">남자
			<input type=radio id="female" name="gender" value="여자">여자
		</c:if>
		<c:if test="${member.gender == '여자' }">
			<input type=radio id="male" name="gender" value="남자">남자
			<input type=radio id="female" name="gender" value="여자" checked="checked">여자
		</c:if>			
			
		</td>
	</tr>
	<tr><td>취미</td>
		<td>// 배열이 공유가 되면 intems 값으로 배열이름이 들어간다. 
			<input type="checkbox" id="h1" name="hobby" value="공부"
		<c:forEach var="i" items="${h}">
			<c:if test="${i=='공부'}">${'checked'}</c:if>
		</c:forEach> >공부
		
			<input type="checkbox" id="h2" name="hobby" value="게임"
		<c:forEach var="i" items="${h}">	
			<c:if test="${i=='게임'}">${'checked'}</c:if>
		</c:forEach> >게임
			
			<input type="checkbox" id="h3" name="hobby" value="등산"
		<c:forEach var="i" items="${h}">	
			<c:if test="${i=='등산'}">${'checked'}</c:if>
		</c:forEach> >등산
			
			<input type="checkbox" id="h4" name="hobby" value="낚시"
		<c:forEach var="i" items="${h}">	
			<c:if test="${i=='낚시'}">${'checked'}</c:if>
		</c:forEach> >낚시
			
			<input type="checkbox" id="h5" name="hobby" value="쇼핑"
		<c:forEach var="i" items="${h}">	
			<c:if test="${i=='쇼핑'}">${'checked'}</c:if>
		</c:forEach> >쇼핑
			
		</td>
	</tr>	
	<tr><td>자기소개</td>
		<td>
			<textarea id="intro" name="intro" rows="5" cols="50" placeholder="자기소개를 100자 이내로 입력하세요">${member.intro }</textarea>
		</td>
	</tr>
	<tr><td colspan=2 align=center>
			<input type=submit value="회원수정">
			<input type=reset value="취소">
		</td>
	</tr>		
</table>
</form>
</body>
</html>
```
### controller (회원정보 수정)
```java
}else if(command.equals("/Update.do")) {
			try {
				action = new Update();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
```
### update.java
```java
package service;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.MemberDAO;
import model.MemberDTO;

public class Update implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("Update");
		
		response.setContentType("text/html; charset=utf-8"); // 현재 페이지 인코딩
		request.setCharacterEncoding("utf-8"); 
		
		PrintWriter out = response.getWriter(); // 출력스트럼
		
		MemberDTO member = new MemberDTO();
		member.setId(request.getParameter("id"));
		member.setPasswd(request.getParameter("passwd"));
		member.setName(request.getParameter("name"));
		member.setJumin1(request.getParameter("jumin1"));
		member.setJumin2(request.getParameter("jumin2"));
		member.setMailid(request.getParameter("mailid"));
		member.setDomain(request.getParameter("domain"));
		member.setTel1(request.getParameter("tel1"));
		member.setTel2(request.getParameter("tel2"));
		member.setTel3(request.getParameter("tel3"));
		member.setPhone1(request.getParameter("phone1"));
		member.setPhone2(request.getParameter("phone2"));
		member.setPhone3(request.getParameter("phone3"));
		member.setPost(request.getParameter("post"));
		member.setAddress(request.getParameter("address"));
		member.setGender(request.getParameter("gender"));
		
		String h="";
		String[] h1 = request.getParameterValues("hobby");
		for(String h2 : h1) {
			h += h2+"-";			// h = "공부-게임-등산-"
		}
		member.setHobby(h);
		member.setIntro(request.getParameter("intro"));
		
		MemberDAO dao = MemberDAO.getInstance(); //DAO 객체 생성
		MemberDTO old = dao.getMember(member.getId());
		
		// 비번 비교
		if(old.getPasswd().equals(member.getPasswd())) { //비번 일치시
			int result = dao.update(member);
			if(result==1) System.out.println("회원수정 성공");
			
		}else {											 //비번 불일치시
			out.println("<script>");
			out.println("alert('비번이 일치하지 않습니다.');");
			out.println("history.go(-1);");
			out.println("</script>");
			out.close();
			
			return null;
		}	
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("/member/main.jsp"); // 페이지 이동
		
		return forward;
	}

}
````
### MemberDAO(회원수정)
```java

// 회원정보 수정
	public int update(MemberDTO member) throws Exception{
		int result = 0;
		SqlSession session = getSession();
		result = session.update("update", member);

		return result;
	}
```
### member.xml(회원수정)
```mxl
<!-- 회원정보 수정 -->
	<update id="update" parameterType="member">
	  update member0609 set name=#{name}, jumin1=#{jumin1}, jumin2=#{jumin2}, 
	  mailid=#{mailid}, domain=#{domain}, tel1=#{tel1}, tel2=#{tel2}, tel3=#{tel3},
	  phone1=#{phone1}, phone2=#{phone2}, phone3=#{phone3}, post=#{post}, address=#{address},
	  gender=#{gender}, hobby=#{hobby}, intro=#{intro} where id = #{id}
	</update>
```
### controller(회원탈퇴)
```java
// 회원탈퇴	
		}else if(command.equals("/Delete.do")) {
			try {
				action = new Delete();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
		}
```
### deleteform.jsp
* 세션 값이 공유가 되어서, value="${sessionScope.id}" 설정
><input type="hidden" name="id" value="${sessionScope.id}">
ID을 값을  hidden으로 전달하는 이유는 => Id 값으로, 회원탈퇴를 한다. 
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>    

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>회원 탈퇴</title>
<script src="http://code.jquery.com/jquery-latest.js"></script>

<!-- 외부 자바스크립트 파일 불러오기 -->
<script src="<%=request.getContextPath() %>/member/member.js"></script>

</head>
<body>

<form method="post" action="<%=request.getContextPath() %>/Delete.do"> 
<input type="hidden" name="id" value="${sessionScope.id}">
<table border=1 width=500 align=center>
	<caption>회원 탈퇴</caption>	
	<tr><td>비밀번호</td>
		<td><input type=password id="passwd" name="passwd"></td>
	</tr>	
	<tr><td colspan=2 align=center>
			<input type=submit value="회원탈퇴">
			<input type=reset value="취소">
		</td>
	</tr>		
</table>
</form>


</body>
</html>
```
### Delete.java (회원 탈퇴)
* hidden으로 넘어온 ID와 passwd를 값을 받는다. 
* return null의 역할은 아래쪽 내용이 실행되지 않는다. 
> return null 이 없으면, 비밀번호 일치하지 않았을때 history.go(-1)로 이동되는데 
  forward.setPath("/LoginForm.do");로이동이 되어서 충돌이 발생된다.  
```java

package service;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import dao.MemberDAO;
import model.MemberDTO;

public class Delete implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("Delete");
		
		response.setContentType("text/html; charset=utf-8");
		request.setCharacterEncoding("utf-8");
		
		PrintWriter out = response.getWriter(); // 출력 객체
		HttpSession session = request.getSession(); // 세션공유 받는다. 
		
		String id = request.getParameter("id");    
		String passwd = request.getParameter("passwd");
		
		MemberDAO dao = MemberDAO.getInstance();
		MemberDTO old = dao.getMember(id);  
		
		// 비번 비교
		if(old.getPasswd().equals(passwd)) {	// 비번 일치시
			int result = dao.delete(id);
			if(result == 1) System.out.println("회원삭제 성공");
			
			session.invalidate();               // 세션 삭제
			
		}else {									// 비번 불일치시
			out.println("<script>");	
			out.println("alert('비번이 일치하지 않습니다.');");	
			out.println("history.go(-1);");	
			out.println("</script>");	
			out.close();
			
			return null;			
		}
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("/LoginForm.do");
		
		return forward;
	}

}
```
### memberDAO(회원삭제)

```java

// 회원 탈퇴
	public int delete(String id) throws Exception{
		int result = 0;
		SqlSession session = getSession();
		result = session.delete("delete", id);		
		
		return result;
	}	
```
### member.xml (회원삭제)

```xml
<!-- 회원 삭제 -->
	<delete id="delete" parameterType="String">
	  delete from member0609 where id = #{id}
	</delete> 
```






		