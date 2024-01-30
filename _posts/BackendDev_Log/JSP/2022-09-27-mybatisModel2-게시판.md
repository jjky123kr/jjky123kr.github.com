---
layout: single
title : Mybaits와 Model2 게시판
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

### MVC 흐름
![웹 캡처_18-9-2022_15241_](https://user-images.githubusercontent.com/107549149/190888145-dc4fc3bb-8f71-4796-b024-92cebf544f5c.jpeg){: width="500" height="500"}

* 클라이언트의 요청 
* Controller(servlet)에서 어느 service 로 갈지 결정된다.
* service 클래스는 -> DAO ->DB(데이터 정보)
* DB->DAO->service클래스로 간다.
* service-> serviet로 간다. 이때 service 클래스에서 페이지 이동시 공유설정한다.(forward)
* forward 해서, view 페이지에서 정보 전달 된다. 

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
### 프로그램 구성

```
mybatisboard- src - controller - BoardFrontController.java (Controller)

	             model - BoardBean.java(DTO)

	             dao - BoardDAO.java(DAO)

	            service -  Action.java(인터페이스)		       		       
		                     ActionForward.java				       	       
		                     BoardAddAction.java(글작성)
		                     BoardListAction.java(글목록)
		                     BoardDetailAction.java(글내용)
		                     BoardReplyAction.java(댓글폼)
		                     BoardReply.java(댓글)
		                     BoardModifyAction.java(글수정폼)
		                     BoardModify.java(글수정)
		                     BoardDeleteAction.java(글삭제폼)  (x)
		                     BoardDelete.java(글삭제)     	


	WebContent - board - qna_board_write.jsp(글작성 폼)
			     qna_board_list.jsp(글목록 출력)
			     qna_board_view.jsp(글내용 출력)
			     qna_board_reply.jsp(답변글)
			     qna_board_modify.jsp(글수정)
			     qna_board_delete.jsp(글삭제)
			     file_down.jsp


	             boardupload (자료실 임시폴더)

	
			WEB-INF - web.xml(프로젝트의 환경설정파일)
                index.jsp
                pox.xml
```
### pox.mxl(라이브러리 저장한다.)
[라이브러리]<http://mvnrepository.com>


1. 오라클 , MySQL, cos, jstl , MyBatis ,iBatis 설정한다.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.myhome</groupId>
	<artifactId>mybatisboard</artifactId>
	<packaging>war</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>ibatisboard Maven Webapp</name>
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
		<finalName>mybatisboard</finalName>
		<plugins>
	<plugin>
		<artifactId>maven-war-plugin</artifactId>
		<version>3.2.2</version>
  </plugin>
</plugins>	
	</build>
</project>
```
### db.properties (데이터 베이스 계정)

```xml
jdbc.driverClassName=oracle.jdbc.driver.OracleDriver
jdbc.url=jdbc:oracle:thin:@localhost:1521:xe
jdbc.username=totoro
jdbc.password=totoro123
```
### mybaits-config.xml (환경설정)

1. typeAlias = DTO객체 를 치환한다. alias ="board"
2. DB 연동 할때 name= 변수 명 , value=${왼쪽은 값} 
3. mapper 파일 설정

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource="db.properties" />
	<typeAliases>
		<typeAlias type="model.BoardBean" alias="board"></typeAlias>
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
		<mapper resource="board.xml" />
	</mappers>
</configuration>
````
### 테이블 생성
```sql
select* from tab;
select*from seq;
select* from model2;

create table model2(
board_num number primary key,
board_name varchar2(20),
board_pass varchar2(15),
board_subject varchar2(50),
board_content varchar2(2000),
board_file varchar2(50),  -- 첨부 파일명
board_re_ref number,      -- 글 그룹번호 
board_re_lev number,      -- 댓글 깊이 : 원문(0), 1, 2...
board_re_seq number,      -- 댓글 출력 순서 : 원문(0) 오름차순 정렬
board_readcount number,
board_date timestamp );

create sequence model2_seq
start with 1
increment by 1
nocache;
```
### 인터페이스 파일 생성 Action (부모)

```java
package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public interface Action {

	//추상 메소드 
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response)throws Exception;
	
}
````
### ActionForward .java (포워딩)

````java
package service;

public class ActionForward {
 
	private boolean redirect; //포워딩 방법 설정
	private String path;      // 포워딩 파일명 설정
	
	public boolean isRedirect() {
		return redirect;
	}
	public void setRedirect(boolean redirect) {
		this.redirect = redirect;
	}
	public String getPath() {
		return path;
	}
	public void setPath(String path) {
		this.path = path;
	}
	
}
````

### Cotroller 클래스

```java
package controller;

import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class BoardFrontController
 */
//("/")아무이름이나 요청을 받는다. 
@WebServlet("*.do")  //do확장자로 요청하는 요청을 받는다는 의미
public class BoardFrontController extends HttpServlet {
	private static final long serialVersionUID = 1L;
	//doGet(),doPost()메소드에서 공통적인 작업을 처리하는 메소드
	protected void doProcess(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		
		String requestURI = request.getRequestURI();
		String contextPath = request.getContextPath();
		String command = requestURI.substring(contextPath.length());
		
		System.out.println("requestURI:" + requestURI);  
		System.out.println("contextPath:" + contextPath);
		System.out.println("command:" + command);        
		
	}//doProcess()end
	
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		System.out.println("get");
	   doProcess(request,response); //doProcess()호출
	}

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		System.out.println("post");		
	  doProcess(request,response); //doProcess()호출
	}

}
````
### qna_board_write.jsp (글작성 폼)
* action = 요청명과 동일 하다.
* post방식
* 파일 전송 enctype="multipart/form-data" 사용된다.
* 파일 전송 하지 않을때 enctype="multipart/form-data" 사용하면 안된다. 

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8"%>

<html>
<head>
	<title>MVC 게시판</title>
	<script src="http://code.jquery.com/jquery-latest.js"></script>
	<script src="<%=request.getContextPath() %>/board/script.js"></script>
</head>
<body>

<form action="<%=request.getContextPath() %>/BoardAddAction.do" method="post" 
	  enctype="multipart/form-data">
<table cellpadding="0" cellspacing="0" align=center border=1>
	<tr align="center" valign="middle">
		<td colspan="5">MVC 게시판</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">글쓴이</div>
		</td>
		<td>
			<input name="board_name" id="board_name" type="text" size="10" maxlength="10" 
				value=""/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">비밀번호</div>
		</td>
		<td>
			<input name="board_pass" id="board_pass" type="password" size="10" maxlength="10" 
				value=""/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">제 목</div>
		</td>
		<td>
			<input name="board_subject" id="board_subject" type="text" size="50" maxlength="100" 
				value=""/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12">
			<div align="center">내 용</div>
		</td>
		<td>
			<textarea name="board_content" id="board_content" cols="67" rows="15"></textarea>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12">
			<div align="center">파일 첨부</div>
		</td>
		<td>
			<input name="board_file" type="file"/>
		</td>
	</tr>
	<tr bgcolor="cccccc">
		<td colspan="2" style="height:1px;">
		</td>
	</tr>
	<tr><td colspan="2">&nbsp;</td></tr>
	<tr align="center" valign="middle">
		<td colspan="5">			
			<input type=submit value="등록">
			<input type=reset value="취소">
		</td>
	</tr>
</table>
</form>

</body>
</html>
````
<img width="359" alt="게시판 양식" src="https://user-images.githubusercontent.com/107549149/192144609-bf331ed7-103d-4ef3-9e5f-5d906725d45c.png">

### BoardAddAction.java( service 글작성)
* 첨부파일은 MultipartRequest 클래스로 객체를 생성해서 파일 업로드가 수행된다.
* 공유한 값을 받을 때 multi로 받는다. 

```java
BoardBean board = new BoardBean();
	 board.setBoard_name(multi.getParameter("board_name"));
	 board.setBoard_pass(multi.getParameter("board_pass"));
	 board.setBoard_subject(multi.getParameter("board_subject"));
	 board.setBoard_content(multi.getParameter("board_content"));
	 board.setBoard_file(multi.getFilesystemName("board_file"));
````

```java
package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import com.oreilly.servlet.MultipartRequest;
import com.oreilly.servlet.multipart.DefaultFileRenamePolicy;

import dao.BoardDAO;
import model.BoardBean;

public class BoardAddAction implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("BoardAddAction");
		
		
		String path = request.getRealPath("boardupload");
		System.out.println("path:"+path);
		
		int size = 1024*1024;
	// 첨부파일은 MultipartRequest 클래스로 객체를 생성하면서  파일 업로드가 수행된다.	
	 MultipartRequest multi = 
        new MultipartRequest(request, 
						        path,   // 업로드 디렉토리
						        size,   // 업로드 파일 크기(1MB)
						     "utf-8",   // 한글 인코딩
	    new DefaultFileRenamePolicy()); //중복파일 문제해결
	 
	 BoardBean board = new BoardBean();
	 board.setBoard_name(multi.getParameter("board_name"));
	 board.setBoard_pass(multi.getParameter("board_pass"));
	 board.setBoard_subject(multi.getParameter("board_subject"));
	 board.setBoard_content(multi.getParameter("board_content"));
	 board.setBoard_file(multi.getFilesystemName("board_file"));
	 
	 BoardDAO dao = BoardDAO.getInstance();
	 int result = dao.insert(board);  //원문 글 작성
	 if(result==1)System.out.println("글작성 성공");
	 
		
		ActionForward forward = new ActionForward();		
        forward.setRedirect(false);
        forward.setPath("/BoardListAction.do"); //목록을 불러오는 요청해야 한다. 
		return forward;
	}

}
````
### BoardFrontController.java (controller)

글작성(원문작성)
```java
  
		if(command.equals("/BoardAddAction.do")) {
			try {
				action = new BoardAddAction();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();				
			}
// 글작성 폼	
		}else if(command.equals("/BoardForm.do")) {	
			forward = new ActionForward();
			forward.setRedirect(false);
			forward.setPath("/board/qna_board_write.jsp");

````
### BoardDAO(DAO) 

```java
// 글작성 : 원문작성
package dao;

import java.io.Reader;
import java.util.ArrayList;
import java.util.List;
import java.util.Map;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import model.BoardBean;

public class BoardDAO {

	private static BoardDAO instance = new BoardDAO();
  
	public static BoardDAO getInstance() {
		return instance;
	}
		
	public SqlSession getSession() {
		SqlSession session=null;
		Reader reader=null;			
		try {
			reader = Resources.getResourceAsReader("mybatis-config.xml");//DB연동
			SqlSessionFactory sf = new SqlSessionFactoryBuilder().build(reader);
			session = sf.openSession(true);	 // auto commit	
		}catch(Exception e) {
			e.printStackTrace();
		}		
		return session;
	}
	//글작성(원문작성)
	public int insert(BoardBean board) throws Exception {
		int result=0;
		SqlSession session = getSession();
		result = session.insert("board_insert", board);		
		System.out.println("result:"+result);
		
		return result;
`````
### board.xml(글작성 원문)
* jdbcType=VARCHAR 는 NULL값을 허용한다.

```sql
<mapper namespace="myboard">

	<!-- 글작성(원문) -->
	<insert id="board_insert" parameterType="board">
	 insert into model22 values(model22_seq.nextval,#{board_name},
	 #{board_pass},#{board_subject},#{board_content},
	 #{board_file,jdbcType=VARCHAR},model22_seq.nextval,0,0,0,sysdate)
	 <!--jdbcType=VARCHAR null 값을 허용한다.   -->
	</insert>
```
### BoardListAction.java (serivce 글목록)

* 공유되는 값이 설정 
>request.setAttribute
* 공유되는 값이 request라서 dispatch방식 으로 포워딩
1.page 값이 전달 되는 것이 2개 이상이라서 
DAO에서 값을 처리할 수가 없어서,page값 만(DAO)전달해서 SQL문에서 따로 처리한다.(start,end)  
2. Map 객체로 생성해서 (key값 , value값 ) 으로 처리한다.
3. DTO 객체에서 필드명에 startRow ,endRow을 추가하여 사용한다. 
> 검색기능을 구현한 페이지 처리에서는 DTO객체 사용한다. 

```java
package service;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;

public class BoardListAction implements Action{
	
	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("BoardListAction");
		
		int page=1;      // 페이지 번호
		int limit=10;    // 한 화면에 출력 데이터 갯수
		
		if(request.getParameter("page")!=null) {
			page = Integer.parseInt(request.getParameter("page"));					
		}
		
		int startRow = (page-1) * limit + 1;  
		int endRow = page * limit;
		
		List boardlist = null; // 총데이터 갯수
		BoardDAO dao = BoardDAO.getInstance(); 
		int listcount = dao.getCount(); 
//		boardlist = dao.getList(startRow, endRow);
//		boardlist = dao.getList(page);
//      mybatis 에서는 값 전달이 1개만 전달한다.
//    1.DAO에서  값을 처리할 수가 없어서,page값을(DAO)전달해서 SQL문에서 따로 처리한다.(start,end)   
       // 검색 기능을 사용할때는  사용할 수 없다.
//    2.Map 처리	----------------------------------	
		Map map = new HashMap();
		map.put("start", startRow); //(키("start") ,value(startRow) )
		map.put("end", endRow);     //(키("end"), value(endRow))
		
//		boardlist = dao.getList(page);		
		boardlist = dao.getList(map);		
//      Map은 속도가 떨어지는 단점이 있다. 
//-------------------------------------------------	
//    3. DTO클래스 안에 start,end 를 필드 생성후 DTO객체를 생성한다.(검색기능구현) 		
		System.out.println("listcount:"+listcount); 
		System.out.println("boardlist:"+boardlist);		
		
		int pageCount = listcount/limit + ((listcount%limit==0) ? 0:1); // 총 페이지  
		
		int startPage = ((page-1)/10) * limit + 1; //첫번째 블럭 페이지
		int endPage = startPage + 10 - 1;          //마지막 페이지
		
		if(endPage > pageCount) endPage = pageCount; //실제 존하는 페이지 설정
		
		// 값을 공유설정 한다.
		request.setAttribute("page", page);
		request.setAttribute("listcount", listcount);
		request.setAttribute("boardlist", boardlist);
		request.setAttribute("pageCount", pageCount);
		request.setAttribute("startPage", startPage);
		request.setAttribute("endPage", endPage);		
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("./board/qna_board_list.jsp"); //뷰페이지 
		
		return forward;
	}

}
````
controller(글 목록)
```
// 글목록	
		}else if(command.equals("/BoardListAction.do")) {
			try {
				action = new BoardListAction();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
```
### BoardDAO(DAO)

```java
// 총 데이터 갯수 구하기
// 총데이터 갯수 구하기
	public int getCount() throws Exception{
		int result=0;
		SqlSession session=getSession();
		result = (Integer)session.selectOne("board_count");
		
		return result;
	}
	// 데이터 목록
//	public List getList(int page) throws Exception {
	public List getList(Map map) throws Exception {
		List list = new ArrayList();
		SqlSession session=getSession();
		list = session.selectList("board_list", map);//page 번호  
		
		return list;
	}   
```
### board.xml (SQL문)

```sql
<!-- 글갯수 -->
	<select id="board_count" resultType="int">
	 select count(*) from model22
	</select>

<!-- 글목록 -->
	<!-- page 전달-->
	<!-- <select id="board_list" parameterType="int" resultType="board">
	 select * from (select rownum rnum, board.* from ( 
	  select * from model22 order by board_re_ref desc,board_re_seq asc) board )
	  where rnum &gt;= (#{page}-1) * 10 + 1   and rnum &lt;= #{page}*10
	         start= (#{page}-1) * 10 + 1, end= #{page}*10
	        &gt(<),&lt(>) 부등호 기호를 인식이 되지 않는다.특수 문자로 작성
	</select>  -->
	
	<!-- Map 전달 -->
	<select id="board_list" parameterType="Map" resultType="board">
	 select * from (select rownum rnum, board.* from (
	  select * from model22 order by board_re_ref desc,board_re_seq asc) board )
	  where rnum &gt;= #{start} and rnum &lt;= #{end}<!--service 에서 value값-->
	</select> 
```

### qna_board_list.jsp (글 목록 페이지)
* EL 표현언어 + JSTL 사용
* 국제화 라이브러리 frm => 날짜 시간 폼 
* 공유되는 값의 자료형이 다르면,View페이지에서 출력하는 방법이 다르다. 
1. 공유되는 값이 int 형 
>int 형과 String 형 은 바로 EL 표현언어${} 작성
2. 공유되는 값이 list 출력 방법
> forEach 태그의 items 값에 설정한다. 
3. 공유되는 값이 DTO 일때 출력 방법 
DTO 의 이름 값=board
> EL =${board.board_num}으로 사용된다. 

```jsp
 <c:forEach var="b" items="${boardlist}"> 
```
* 댓글 제목 앞에 여백 처리 
>if태그와 forEach태그 사용

```jsp

<c:if test="${b.board_re_lev >0}">
	 <c:forEach var="i" begin="0" end="${b.board_re_lev}">
   &nbsp;
   </c:forEach>
```

* EL에서는 증감 연산자가 지원되지 않는다. 
* set태그로 변수 정의

```jsp
${num}<c:set var="num" value="${num-1}" />
```

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<a href="./BoardForm.do">글작성</a><br>
글갯수:${listcount}개<br> 

<%
int count =((Integer)request.getAttribute("listcount")).intValue(); 
%>
글갯수 : <%=count%>개<br>
	<table border="1" width="700" align="center">
		<tr>
			<td>번호</td>
			<td>제목</td>
			<td>작성자</td>
			<td>날짜</td>
			<td>조회수</td>
		</tr>
		<!-- 각 페이지에 출력될 시작 번호  -->
		<c:set var="num" value="${listcount-(page-1)*10}" />
		<c:forEach var="b" items="${boardlist}">
			<tr>
				<td>${num}<c:set var="num" value="${num-1}" />
				</td>
				<td>
					<!--댓글 제목 앞에 여백 처리  --> 
					<c:if test="${b.board_re_lev >0}">
					<c:forEach var="i" begin="0" end="${b.board_re_lev}">
                      &nbsp;
                    </c:forEach>
					</c:if> 
	   <a href="./BoardDetailAcion.do?board_num=${b.board_num}&page=${page}">				
					${b.board_subject}
	   </a>			
				</td>
				<td>${b.board_name}</td>
				<td><fmt:formatDate value="${b.board_date}"
						pattern="yyyy-MM-dd HH:mm:ss EEEE" /></td>
				<td>${b.board_readcount}</td>
			</tr>
		</c:forEach>
	</table><br><br>

<!--페이지처리-->
<center>
<c:if test="${listcount > 0 }">

<!-- 1page로 이동 -->
<a href="./BoardListAction.do?page=1" style="text-decoration:none"> << </a>

<!-- 이전 블럭으로 이동 -->
<c:if test="${startPage>10}">
<a href="./BoardListAction.do?page=${startPage-10}">[이전]</a>
</c:if>

<!-- 각 블럭에 10개의 페이지 출력 -->
<c:forEach var="i" begin="${startPage}" end="${endPage}">
	<c:if test="${i == page}">		<!-- 현재 페이지 -->
		[${i}]
	</c:if>
	<c:if test="${i != page}">		<!-- 현재 페이지가 아닌 경우 -->
		<a href="./BoardListAction.do?page=${i}">[${i}]</a>
	</c:if>
</c:forEach>

<!-- 다음 블럭으로 이동 -->
<c:if test="${endPage<pageCount}">
<a href="./BoardListAction.do?page=${startPage+10}">[다음]</a>
</c:if>

<!-- 마지막 페이지 이동 -->
<a href="./BoardListAction.do?page=${pageCount}"
  style="text-decoration:none"> >> </a>
</c:if>

</center>
</body>
</html>
````
<img width="632" alt="상세페이지" src="https://user-images.githubusercontent.com/107549149/192144675-68514fa9-feca-4ca9-9ddd-fc195adac750.png">

### BoardDetailAction.java( service 글 내용-상세페이지 )

```java

package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import model.BoardBean;

public class BoardDetailAction implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("BoardDetailAction");
		
		int board_num = Integer.parseInt(request.getParameter("board_num")); 
		String page = request.getParameter("page");
		
		BoardDAO dao = BoardDAO.getInstance();
//		BoardBean board = dao.updateContent(board_num);
		
		//조회수 증가
		dao.updateCount(board_num);   
		BoardBean board = dao.getContent(board_num); // 조회수 증가
		
		request.setAttribute("board", board);
		request.setAttribute("page", page);
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("/board/qna_board_view.jsp");
		
		return forward;
	}

}

```
### BoardFrontController (Controller)

상세페이지

```java

		}else if(command.equals("/BoardDetailAction.do")) {
		  try {
			  action = new BoardDetailAction();
			  forward = action.execute(request, response);
		  }catch(Exception e) {
			  e.printStackTrace();
		  }
		}
```
### DAO
```
	// 조회수 증가
	public void updateCount(int board_num) throws Exception{
		SqlSession session = getSession();
		session.update("board_updatecount", board_num);
	}
```
### board.xml (조회수 증가 SQL)
```
<!-- 조회수 증가 -->
	<update id="board_updatecount" parameterType="int">
     update model22 set board_readcount=board_readcount+1 where board_num = #{board_num}	
	</update>
```
### DAO(상세 페이지)
```
// 상세 페이지, 수정 폼
	public BoardBean getContent(int board_num) throws Exception {
		BoardBean board = new BoardBean();

		SqlSession session=getSession();
		board  = (BoardBean)session.selectOne("board_content", board_num);
		
		return board;
	}
```
### board.xml(상세페이지)
```
<!-- 상세페이지, 수정폼 -->
	<select id="board_content" parameterType="int" resultType="board">
	 select * from model22 where board_num = #{board_num}
	</select>
```
### qna_board_view.jsp(상세페이지)
* core , fmt 사용
*  댓글 클릭 할때  board_num 값 과, page 번호값 가져간다. 
* 목록을 클릭 했을때 목록페이지 이동할때 
page 값을 가져간다. 
* 공유된 값이 board = DTO  출력방식
>${board.board_num}

```jsp
00000<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %> 
    
<table border=1 width=600 align=center>
	<caption>상세 페이지</caption>
	<tr>
		<td>작성자</td>
		<td>${board.board_name}</td>
	</tr>
	<tr>
		<td>조회수</td>
		<td>${board.board_readcount}</td>
	</tr>
	<tr>
		<td>날짜</td>
		<td>
			<fmt:formatDate value="${board.board_date}"
							pattern="yyyy-MM-dd H:mm"/>		
		</td>
	</tr>
	<tr>
		<td>제목</td>
		<td>${board.board_subject}</td>
	</tr>
	<tr>
		<td>내용</td>
		<td><pre>${board.board_content}</pre></td>
	</tr>
	<tr>
		<td>첨부 파일</td>
		<td>
			<c:if test="${board.board_file !=  null }">
				<a href="./board/file_down.jsp?file_name=${board.board_file}">${board.board_file}</a>
			</c:if>
		</td>
	</tr>
	<tr>
		<td colspan=2 align=center>
			<input type="button" value="댓글" 
				   onClick="location.href='./BoardReplyAction.do?board_num=${board.board_num}&page=${page}&board_re_ref=${board.board_re_ref}&board_re_lev=${board.board_re_lev}&board_re_seq=${board.board_re_seq}'">
			<input type="button" value="수정" 
				   onClick="location.href='./BoardModifyAction.do?board_num=${board.board_num}&page=${page}'">
			<input type="button" value="삭제" 
				   onClick="location.href='./BoardDeleteAction.do?board_num=${board.board_num}&page=${page}'">
			<input type="button" value="목록" 
			       onClick="location.href='./BoardListAction.do?page=${page}'">
		</td>
	</tr>
</table>    

````

<img width="375" alt="상세 페잊" src="https://user-images.githubusercontent.com/107549149/192144701-f666b287-9830-4663-a220-bf39d0d6a5e0.png">



### controller (댓글 폼)
```java
// 댓글 폼	
		}else if(command.equals("/BoardReplyAction.do")) {
			forward = new ActionForward();
			forward.setRedirect(false);
			forward.setPath("/board/qna_board_reply.jsp");
```

### BoardReplyAction.java ( service 댓글 폼)
* 글내용 출력(qna_board_view.jsp)에서 넘어온 page 값과, board_num 값을 받는다.
```java
int board_num= Integer.parseInt(request.getParameter("board_num"));
String page = request.getParameter("page");
`````
* 공유 설정 (boare_num , page)

```java
 request.setAttribute("board", board);
 request.setAttribute("page", page);
```

```java
package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import model.BoardBean;

public class BoardReplyAction implements Action{

	@Override
public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		
		System.out.println("BoardReplyAction");
		// board_num 값과 , page  값 받는다.
		int board_num= Integer.parseInt(request.getParameter("board_num"));
		String page = request.getParameter("page");
		
		BoardDAO dao = BoardDAO.getInstance();
		
		//부모글의 상세정보 구하기 
	    BoardBean board = dao.getDetail(board_num);
	    
	    //공유설정 (페이지, board(DTO)공유)
	    request.setAttribute("board", board);
	    request.setAttribute("page", page);
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("./board/qna_board_reply.jsp");
		
		
		return forward;
	}

}
```
### qna_board_reply.jsp (댓글 폼)

* hidden으로 page , board_num, re_lev , re_ref ,re_sqf 설정한다. 
* 만약 부모 댓글을 값을 넘기지 않으면, 서비스 클래스로 부모글을 구하고, 공유설정후
  댓글 작성하는 곳에 hidden으로 값을 받는다. 

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8"%>

<html>
<head>
	<title>댓글 게시판</title>
	<script src="http://code.jquery.com/jquery-latest.js"></script>
	<script src="<%=request.getContextPath() %>/board/script.js"></script>
</head>
<body>

<form action="<%=request.getContextPath() %>/BoardReply.do" method="post" >
<input type="hidden" name="board_num" value="${board.board_num}"> 
<input type="hidden" name="page" value="${page}"> 
<input type="hidden" name="board_re_ref" value="${board.board_re_ref}"> 
<input type="hidden" name="board_re_lev" value="${board.board_re_lev}"> 
<input type="hidden" name="board_re_seq" value="${board.board_re_seq}"> 
<table cellpadding="0" cellspacing="0" align=center border=1>
	<tr align="center" valign="middle">
		<td colspan="5">댓글 게시판</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">글쓴이</div>
		</td>
		<td>
			<input name="board_name" id="board_name" type="text" size="10" maxlength="10" 
				value=""/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">비밀번호</div>
		</td>
		<td>
			<input name="board_pass" id="board_pass" type="password" size="10" maxlength="10" 
				value=""/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">제 목</div>
		</td>
		<td>
			<input name="board_subject" id="board_subject" type="text" size="50" maxlength="100" 
				value="re."/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12">
			<div align="center">내 용</div>
		</td>
		<td>
			<textarea name="board_content" id="board_content" cols="67" rows="15"></textarea>
		</td>
	</tr>
	<tr bgcolor="cccccc">
		<td colspan="2" style="height:1px;">
		</td>
	</tr>
	<tr><td colspan="2">&nbsp;</td></tr>
	<tr align="center" valign="middle">
		<td colspan="5">			
			<input type=submit value="댓글">
			<input type=reset value="취소">
		</td>
	</tr>
</table>
</form>

</body>
</html>
````
<img width="361" alt="댓글 게시판" src="https://user-images.githubusercontent.com/107549149/192144741-20e0f01b-e0bc-4c73-bbe9-ab418b8195a9.png">

### BoardReply.java(serivce 댓글작성)
* 댓글 폼에서 hindden으로 넘어온 5가지 정보를 받는다. 
* DTO객체로 부모글 의 re_ref , re_seq 값을 저장한다. 
 
board.setBoard_re_ref(board_re_ref);
board.setBoard_re_seq(board_re_seq);
		 
```java
package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import model.BoardBean;

public class BoardReply implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("BoardReply");
		
		request.setCharacterEncoding("utf-8");
		
		int board_num = Integer.parseInt(request.getParameter("board_num"));
		int board_re_ref = Integer.parseInt(request.getParameter("board_re_ref"));
		int board_re_lev = Integer.parseInt(request.getParameter("board_re_lev"));
		int board_re_seq = Integer.parseInt(request.getParameter("board_re_seq"));
		String page = request.getParameter("page");
		
		BoardBean board = new BoardBean();
		board.setBoard_re_ref(board_re_ref);
		board.setBoard_re_seq(board_re_seq);
		
		BoardDAO dao = BoardDAO.getInstance();
		dao.updateSeq(board); 	// board_re_seq값 증가 		
		
		board.setBoard_re_seq(board_re_seq+1);
		board.setBoard_re_lev(board_re_lev+1);		
		board.setBoard_name(request.getParameter("board_name"));
		board.setBoard_pass(request.getParameter("board_pass"));
		board.setBoard_subject(request.getParameter("board_subject"));
		board.setBoard_content(request.getParameter("board_content"));
					
		dao.boardReply(board);		
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("/BoardDetailAction.do?board_num="+board_num+"&page="+page);
		
		return forward;
	}

}

````

### BoardFrontController(controller )
댓글작성
```java
}else if(command.equals("/BoardReply.do")) {
			try {
				action = new BoardReply();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
```
### DAO (댓글 출력 , 댓글작성)
```java
// 댓글 출력 순서 (board_re_seq값 증가)
	public void updateSeq(BoardBean board) {
		SqlSession session = getSession();
		session.update("board_updateseq", board);
	}
		
// 댓글작성
	public void boardReply(BoardBean board) {
		SqlSession session = getSession();
		session.insert("board_reply", board);
	}
```	
### board.xml(댓글 출력 순서 , 댓글 작성)
```xml
<!-- 댓글 출력순서 -->
	<update id="board_updateseq" parameterType="board">
	 update model22 set board_re_seq=board_re_seq+1 
	  where board_re_ref = #{board_re_ref} and board_re_seq &gt; #{board_re_seq}
	</update>
``` 
```xml

	<!-- 댓글 작성 -->
	<insert id="board_reply" parameterType="board">
	 insert into model22 values(model22_seq.nextval,#{board_name},
	 #{board_pass},#{board_subject},#{board_content},
	 #{board_file,jdbcType=VARCHAR},#{board_re_ref},#{board_re_lev},#{board_re_seq},0,sysdate)
	</insert>
```
### BoardFrontController(controller)
수정폼

```java
}else if(command.equals("/BoardModifyAction.do")) {
			try {
				action = new BoardModifyAction();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
````
### BoardModifyAction.java(service 수정폼 )
* page , num 값을 받는다. 
* DTO 객체를 생성해서 상세 정보를 구한다. 
````java
package service;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import model.BoardBean;

public class BoardModifyAction implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("BoardModifyAction");
		
		int board_num = Integer.parseInt(request.getParameter("board_num"));
		String page = request.getParameter("page");
		
		BoardDAO dao = BoardDAO.getInstance();
		//상세 정보 구하기
		BoardBean board = dao.getDetail(board_num);
		
		//공유 설정(상세정보 , page)
		request.setAttribute("board", board);
		request.setAttribute("page", page);
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);   //dispatcher 방식으로 포워딩
		forward.setPath("./board/qna_board_modify.jsp");
		
		return forward;
	}

}
````

### qna_board_modify.jsp(글 수정 폼)
* 상세정보 공유값을 설정한다. ${board.board_name}으로 작성한다. 
```jsp

<%@ page language="java" contentType="text/html; charset=utf-8"%>

<html>
<head>
	<title>게시판 수정</title>
	<script src="http://code.jquery.com/jquery-latest.js"></script>
	<script src="<%=request.getContextPath() %>/board/script.js"></script>
</head>
<body>

<form action="<%=request.getContextPath() %>/BoardModify.do" method="post" >
<input type="hidden" name="board_num" value="${board.board_num}"> 
<input type="hidden" name="page" value="${page}"> 
<table cellpadding="0" cellspacing="0" align=center border=1>
	<tr align="center" valign="middle">
		<td colspan="5">게시판 수정</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">글쓴이</div>
		</td>
		<td>
			<input name="board_name" id="board_name" type="text" size="10" maxlength="10" 
				value="${board.board_name }"/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">비밀번호</div>
		</td>
		<td>
			<input name="board_pass" id="board_pass" type="password" size="10" maxlength="10" 
				value=""/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">제 목</div>
		</td>
		<td>
			<input name="board_subject" id="board_subject" type="text" size="50" maxlength="100" 
				value="${board.board_subject}"/>
		</td>
	</tr>
	<tr>
		<td style="font-family:돋음; font-size:12">
			<div align="center">내 용</div>
		</td>
		<td>
			<textarea name="board_content" id="board_content" cols="67" rows="15">${board.board_content}</textarea>
		</td>
	</tr>
	<tr bgcolor="cccccc">
		<td colspan="2" style="height:1px;">
		</td>
	</tr>
	<tr><td colspan="2">&nbsp;</td></tr>
	<tr align="center" valign="middle">
		<td colspan="5">			
			<input type=submit value="수정">
			<input type=reset value="취소">
		</td>
	</tr>
</table>
</form>

</body>
</html>
`````
<img width="355" alt="게시판 수정" src="https://user-images.githubusercontent.com/107549149/192144754-91a9b071-94f9-4693-931a-9afc329ffd27.png">

### BoardModify.java(service 글수정)

```java
package service;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import model.BoardBean;

public class BoardModify implements Action{

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("BoardModify");
				
		response.setContentType("text/html; charset=utf-8");// 현재문서 인코딩
		request.setCharacterEncoding("utf-8");// 한글 인코딩
		
		PrintWriter out = response.getWriter(); //출력스트림
		
		int board_num=Integer.parseInt(request.getParameter("board_num"));
		String page = request.getParameter("page");
		String board_pass = request.getParameter("board_pass");// 사용자 입력 비번
		
		BoardBean board = new BoardBean(); // 수정에 필요한 값을 set으로 저장한다.
		
		board.setBoard_num(board_num);
		board.setBoard_name(request.getParameter("board_name")); 
		board.setBoard_subject(request.getParameter("board_subject")); 
		board.setBoard_content(request.getParameter("board_content")); 
		
		BoardDAO dao = BoardDAO.getInstance();
		BoardBean old = dao.getDetail(board_num);// DTO 에 저장된 비번 구하기 
		//비번 비교
		if(old.getBoard_pass().equals(board_pass)) {//비번 일치시
			dao.update(board);
			
		}else {	// 비번 불일치시
			
			out.println("<script>");
			out.println("alert('비밀번호가 일치하지 않습니다.');");
			out.println("history.go(-1);");
			out.println("</script>");
			out.close();
			
			return null;
		}		
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("/BoardDetailAction.do?board_num="+board_num+"&page="+page);
		
		return forward;
	}

}
````
### BoardFrontCotroller(controller)
글 수정
```java
// 글 수정
		}else if(command.equals("/BoardModify.do")) {
			try {
				action = new BoardModify();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
		}
````
### DAO 글수정

````java
 
// 글수정
	public void update(BoardBean board) throws Exception {
		SqlSession session=getSession();
		session.update("board_update", board);
	}
````
### board.xml (글수정 SQL)
```sql
<!-- 글수정 -->
	<update id="board_update" parameterType="board">
	 update model22 set board_name=#{board_name}, board_subject=#{board_subject},
	 board_content=#{board_content} where board_num=#{board_num}
	</update>
```
### controller(삭제)
```java
// 삭제 폼(service 클래스가 없어도된다. => 데이터와 연결하지 않아서)
		}else if(command.equals("/BoardDeleteAction.do")) {
			forward = new ActionForward();
			forward.setRedirect(false);
			forward.setPath("/board/qna_board_delete.jsp");
```
### BoardFrontController.java
글 삭제 폼 

```java

		}else if(command.equals("/BoardDeleteAction.do")) { 
			forward = new ActionForward();
			forward.setRedirect(false);     //dispatcher
			forward.setPath("./board/qna_board_delete.jsp");
````
### qna_board_delete.jsp
* get방식으로 넘어온 값을 param으로 사용한다.  
> value = "${param.board_num}"을 사용한다. 
````
<input type="hidden" name="board_num" value="${param.board_num}">
// DB에서 비번을 가져오기 위해서 전달해야한다. 
<input type="hidden" name="page" value="${param.page}">
// page는 클릭 했던 페이지로 다시 돌아가기 위해서 전달해야 한다.  
````

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8"%>

<html>
<head>
	<title>게시판 삭제</title>
	<script src="http://code.jquery.com/jquery-latest.js"></script>
	<script src="<%=request.getContextPath() %>/board/script.js"></script>
</head>
<body>

board_num1:<%=request.getParameter("board_num") %><br>
page1:<%=request.getParameter("page") %><br>

board_num1 :${param.board_num}<br>
page2:${param.page}<br>



<form action="<%=request.getContextPath() %>/BoardDelete.do" method="post" >
<!--공유되는 값이 없어서, param 으로 값을 받는다. hidden의 value 값으로 설정   -->
<input type="hidden" name="board_num" value="${param.board_num}"> 
<input type="hidden" name="page" value="${param.page}"> 
<table cellpadding="0" cellspacing="0" align=center border=1>
	<tr align="center" valign="middle">
		<td colspan="5">게시판 삭제</td>
	</tr>
	
	<tr>
		<td style="font-family:돋음; font-size:12" height="16">
			<div align="center">비밀번호</div>
		</td>
		<td>
			<input name="board_pass" id="board_pass" type="password" size="10" maxlength="10" 
				value=""/>
		</td>
	</tr>
	
	</tr>
	<tr bgcolor="cccccc">
		<td colspan="2" style="height:1px;">
		</td>
	</tr>
	<tr><td colspan="2">&nbsp;</td></tr>
	<tr align="center" valign="middle">
		<td colspan="5">			
			<input type=submit value="삭제">
			<input type=reset value="취소">
		</td>
	</tr>
</table>
</form>

</body>
</html>
````
<img width="107" alt="삭제 폼" src="https://user-images.githubusercontent.com/107549149/192144788-b4e7ff61-3361-475a-bfcf-7512afc37df9.png">


### controller(글 삭제)
```java
// 삭제	
		}else if(command.equals("/BoardDelete.do")) {
			try {
				action = new BoardDelete();
				forward = action.execute(request, response);
			}catch(Exception e) {
				e.printStackTrace();
			}
    }
```


### BoardDelete.java
* 비번을 비교해서, 삭제 한다. 
* 폴더 안에있는 이미지도 삭제 
>File file = new File(path);
* 폴더 안에 이미지를 불러올때 배열을 사용


```java
package service;

import java.io.File;
import java.io.PrintWriter;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import dao.BoardDAO;
import model.BoardBean;

public class BoardDelete implements Action {

	@Override
	public ActionForward execute(HttpServletRequest request, HttpServletResponse response) throws Exception {
		// TODO Auto-generated method stub
		System.out.println("BoardDelete");
		
		response.setContentType("text/html; charset=utf-8"); 
		request.setCharacterEncoding("utf-8");
		
		PrintWriter out = response.getWriter();
		
		String path = request.getRealPath("boardupload"); //패스값 첨부파일
		
		int board_num = Integer.parseInt(request.getParameter("board_num"));
		String page = request.getParameter("page");
		String board_pass  = request.getParameter("board_pass"); // 사용자 입력한 비번
				
		BoardDAO dao = BoardDAO.getInstance(); 
		BoardBean old = dao.getContent(board_num); // 상세정보 구하기
		
		if(old.getBoard_pass().equals(board_pass)) { //비번 일치시
			dao.delete(board_num);
			
			if(old.getBoard_file() != null) { // 첨부파일 있을면 
				
				File file = new File(path); // 첨부 파일 삭제
				file.mkdirs();
				
				File[] f = file.listFiles();
				for(int i=0; i<f.length; i++) {
					if(f[i].getName().equals(old.getBoard_file())) {
						f[i].delete();
					}
				}				
			}
			
		}else {	// 비번 불일치시
	
			out.println("<script>");
			out.println("alert('비밀번호가 일치하지 않습니다.');");
			out.println("history.go(-1);");
			out.println("</script>");
			out.close();

			return null;			
		}
		
		ActionForward forward = new ActionForward();
		forward.setRedirect(false);
		forward.setPath("/BoardListAction.do?page="+page);
		
		return forward;
	}

}

````
### DAO 삭제 

```java
// 글삭제
	public void delete(int board_num) {
		SqlSession session=getSession();
		session.delete("board_delete", board_num);
	}

}
```
### board.xml (삭제 SQL) 

```sql

<!-- 글삭제 -->
	<delete id="board_delete" parameterType="int">
	 delete from model22 where board_num=#{board_num}
	</delete>
```
















