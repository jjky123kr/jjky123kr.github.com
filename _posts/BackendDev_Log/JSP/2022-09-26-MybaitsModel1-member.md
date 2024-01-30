---
layout: single
title : Mybaits와 Model1 회원가입
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
### Model 1 연동
### Mybaits 데이터 액세스 계층 흐름

Member DAO (DAO) ->mybatis -config.xml(mybatis환경설정파일)-> member.xml(mapper파일=SQL문)

1. member.xml 파일을 mybatis -config.xml 불러온다.
> DAO에서 mapper파일의 Id값을 불러온다.=> mapper파일이 SQL문을 불러와서 DAO에서 실행

````s
    <mappers>
	 <mapper resource="member.xml" />
</mappers>
````

2. MemberDAO(DAO)파일에서 mybatis -config.xml(mybaits 환경설정파일)을 불러온다.

```java

try {
			reader = Resources.getResourceAsReader("mybatis-config.xml");
			SqlSessionFactory sf = new SqlSessionFactoryBuilder().build(reader);
			session = sf.openSession(true);	// auto commit 설정
		} catch (IOException e) {
			System.out.println(e.getMessage());
		}

```
3. MemberDAO -> Sqlsession(SQL문을 실행시키는 메소드를 제공하는 인터페이스 ) 
> 메소드 실행  

```
SQL문  - 메소드  

-------------------
insert - int insert()  
            (mapper파일) id값을 불러와서 메소드 실행 
update - int update()  
delete - int delete()  
select - Object select(): 검색 결과가 1개인 경우  
       - List selectList(): 검색 결과가 2개 이상인 경우   
````

### DB  연동 (Mysql, oracle)

```
#mysql
#jdbc.driverClassName=com.mysql.jdbc.Driver
#jdbc.url=jdbc:mysql://localhost:3306/jsptest
#jdbc.username=jspid
#jdbc.password=jsppass

#oracle
jdbc.driverClassName=oracle.jdbc.driver.OracleDriver
jdbc.url=jdbc:oracle:thin:@localhost:1521:xe
jdbc.username=totoro
jdbc.password=totoro123
```

### mybatis-config.xml (mybatis 환경설정)
* resources  : DB 연동할 환경설정 =>db.properties
* DB 연동 할때 name= 변수 명 , value=${왼쪽은 값} 
* 테이블 기준으로 DTO , DAO , mapper 가 늘어난다.
* <mark style='background-color:#fff5b1'> DTO 객체 설정</mark> 
> typeAliases 안에서 DTO객체 설정,  
 typeAlias type=DTO명,alias="DTO객체 치환명"으로설정

```xml
<typeAliases>
    <typeAlias type="model.Member" alias="member"></typeAlias>
</typeAliases>
```
```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource="db.properties" />
	<typeAliases>
		<typeAlias type="model.Member" alias="member"></typeAlias>
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

### member.xml(mapper SQL문) 
```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="memberns">

	<insert id="insert" parameterType="member">
		insert into member22 values (#{id}, #{password})
	</insert>
	
	<select id="list" resultType="member">
		select * from member22
	</select>
	
	<select id="select" parameterType="String" resultType="member">
		select * from member22 where id = #{id}
	</select>	
	
	<update id="update" parameterType="member">
		update member22 set password = #{password} where id = #{id}
	</update>
	
	<delete id="delete" parameterType="String">
		delete from member22 where id = #{id}
	</delete>
	
</mapper>
````
설명
* DTO클래스 필드 명과, DB의 컬럼명을 동일하게 사용한다.    
* mapper 파일이 여러개 있을 경우  
>ID값이 중복 되지 않는다.       
그래서 중복으로 충돌 발생하여 namespace의 이름을 다르게 사용한다.    
그럼 ID값이 동일해도  충돌하지 않는다.     

#### 만약 mapper 파일이 여러 개 일 떄 DAO에서 insert를 설정 하는 방법
* id값을 불러올때 namespace.id 로 설정한다.                
result = session.insert(memberns.insert", member);
####  resultType="#package.#modelname" 
값을 돌려준다.        
돌려주는  값의 자료형 1개의 DTO 클래스 = (alise명)

돌려주는 자료형 : int,String,DTO 사용된다.      
DTO명과 DB의 컬럼명 동일해야 사용 가능하다.      
insert,update,delete 는 자동으로 값을 돌려주어서 사용하면 안된다.    
>select SQL문 만 사용할 수 있다.     
  
#### parameterType
값의 자료형  설정한다.     
=> 전달되는 값의 자료형을 쓴다.(String , int , DTO객체)    
만약 DTO클래스를 자료형을 설정하면, (alise명) 설정    
<mark style='background-color:#fff5b1'>그러나 alise 설정이 안되 있으며, my batis 환경설정의 #DTO클래스의 패키지 명,#해당 클래스 까지  경로 </mark>   

### Member Dao.java (DAO) 

```java
package dao;

import java.io.IOException;
import java.io.Reader;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

import model.Member;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

public class MemberDao {
	
	private SqlSession getSession() {
		SqlSession session=null;
		Reader reader=null;
		try {
			reader = Resources.getResourceAsReader("mybatis-config.xml");
			SqlSessionFactory sf = new SqlSessionFactoryBuilder().build(reader);
			session = sf.openSession(true);			// auto commit
		} catch (IOException e) {
			System.out.println(e.getMessage());
		}
		return session;
	}

	public int insert(Member member) {
		int result = 0;
		SqlSession session=null;
		try { 
			session = getSession(); 
			result = session.insert("insert", member);			
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return result;
	}

	public int chk(Member member) {
		int result = 0;
		SqlSession session=null;
		try { session = getSession(); 
			Member mem = (Member) session.selectOne("select", member.getId());
			if (mem.getId().equals(member.getId())) {
				result = -1;
				if (mem.getPassword().equals(member.getPassword())) {
					result = 1;
				}
			}
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return result;
	}

	public Member select(String id) throws SQLException {
		Member mem = null;
		SqlSession session=null;
		try { session = getSession(); 
		mem = (Member) session.selectOne("select", id);
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return mem;
	}

	public List<Member> list() {
		List<Member> list = new ArrayList<Member>();
		SqlSession session=null;
		try { session = getSession(); 
			list = session.selectList("list");
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return list;
	}

	public int delete(String id) {
		int result = 0;
		SqlSession session=null;
		try { session = getSession(); 
			result = session.delete("delete", id);
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return result;
	}

	public int update(Member mem) {
		int result = 0;
		SqlSession session=null;
		try { session = getSession(); 
			result = session.update("update", mem);
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return result;
	}
}
```
### getSession 설정
* DB 연동 
* SQL문 메소드 설정 
* session = sf.openSession(true);	// auto commit 설정
> insert,select,update,delete일때 commint을 사용하지 않아도 된다.

```java

private SqlSession getSession() {
		SqlSession session=null;
		Reader reader=null;
		try {
			reader = Resources.getResourceAsReader("mybatis-config.xml");// DB연동 수행 
			SqlSessionFactory sf = new SqlSessionFactoryBuilder().build(reader);
			session = sf.openSession(true);			// auto commit
		} catch (IOException e) {
			System.out.println(e.getMessage());
		}
		return session;
	}
```
### login.Form.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>

<body>
<h2>로그인</h2>
<form action="loginPro.jsp" >
	아이디 : <input type="text" name="id" required="required"><p>
	암호 : <input type="password" name="password" required="required"><p>
	<input type="submit" value="확인">
</form>
<p><a href="joinForm.jsp">회원가입</a>
</body>

</html>
```


### joinForm.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>

<body>
<h2>회원가입</h2>
<form action="joinPro.jsp">
	아이디 : <input type="text" name="id" required="required"><p>
	암호 : <input type="password" name="password" required="required"><p>
	<input type="submit" value="확인">
</form>
</body>
</html>
```
### loginPro

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@page import="dao.MemberDao"%>

<jsp:useBean id="mem" class="model.Member"/>
<jsp:setProperty property="*" name="mem"/>

<%
MemberDao md = new MemberDao();
int result = md.chk(mem); //회원 인증처리

if (result==1) {
	session.setAttribute("id",mem.getId());// 공유 설정
%> 
	<script type="text/javascript">
		alert("환영함");
		location.href="main.jsp";
	</script>
<%
} else if (result == -1) {
%> 
	<script type="text/javascript">
		alert("비번이 다르다");
		history.go(-1);
	</script>
<%
} else {
%> 
	<script type="text/javascript">
		alert("그런 ID가 없습니다.");
		history.go(-1);
	</script>
<%
}
%>
```
### insert (DAO)
insert()=> mapper.xml의 insert의 ID 값
> mapper에 insert의 SQL문 실행 해여 결과를 int형으로 돌려준다.

```java

public int insert(Member member) { //회원가입
		int result = 0;
		SqlSession session=null;
		try { 
			session = getSession(); 
			result = session.insert("insert", member);			
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return result;
	}
```
### select 
* select()의 ID값으로 설정한다. 
* 값을 돌려주는 것이 1개 라서 Object로 돌려주어야한다  
>Member mem = (Member) session.selectOne("select", member.getId());	

```java
public int chk(Member member) {  //회원 인증
		int result = 0;
		SqlSession session=null;
		try { session = getSession(); 
			Member mem = (Member) session.selectOne("select", member.getId()); //회원1명 검색
			if (mem.getId().equals(member.getId())) { // 아이디, 비밀번호 비교
				result = -1;
				if (mem.getPassword().equals(member.getPassword())) { 
					result = 1;  // 회원인증 성공 
				}
			}
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return result;
```
### main.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
	
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>

<%
 	String id = (String) session.getAttribute("id");//session 값을 받는다.  
if (id == null || id.equals("")) { // 비접속으로 했을때 main.jsp 단독 실행 했을떄 (session값이 없다.)
	response.sendRedirect("loginForm.jsp");
} else if (id.equals("master")) {
	out.println("관리자 모드!");	
} 

/* String id ="";
if ((String) session.getAttribute("id") != null){
	id = (String) session.getAttribute("id");
if (id.equals("master")) {
out.println("관리자 모드!");	
}
}
else if (session.getAttribute("id") == null) {
	response.sendRedirect("loginForm.jsp");
} */

%>

<body>
	로그인함
	<p>
		정보수정<br>
<%
		if (id != null) {
		if (id.equals("master")) {
%>
		<a href="list.jsp">회원명단</a><br>
<%
		}
	}
%>
		<a href="logout.jsp">로그아웃</a>
		
</body>
</html>
```
* 관리자 회원과 일반 회원이 보여주는 것이 다르게 화면 구성 

### logout.jsp
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
    
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

<%
session.invalidate(); //session 삭제 
%>

<script type="text/javascript">
	alert("로그아웃 됨");
	location.href="loginForm.jsp";
</script>

</body>
</html>
```
* 로그아웃 구성은 : 세션 삭제 와, 로그인 페이지 이동

### list.jsp 회원 명단

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@page import="java.util.List"%>
<%@page import="model.Member"%>
<%@page import="dao.MemberDao"%>

<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

회원 명단
<table border=1><tr><th>아이디<th>비밀번호</th><th>수정</th><th>삭제</th></tr>

<%
	MemberDao md = new MemberDao(); //DAO 객체 생성
	List<Member> list = md.list();  //list 메소드 
	
for (int i = 0;i<list.size();i++){  
%>
	<tr>
	<td><%=list.get(i).getId() %></td>
	<td><%=list.get(i).getPassword() %></td>
	<td><input type="button" value="수정" onclick='location.href="updateForm.jsp?id=<%=list.get(i).getId() %>"'></td>
	<td><input type="button" value="삭제" onclick='location.href="delete.jsp?id=<%=list.get(i).getId() %>"'></td>
	</tr>
<%
}
%>
</table>

<a href="main.jsp">메인으로</a>

</body>
</html>
```
### select List 일때 (DAO)
* select()의 ID값으로 설정한다. 
* 값을 돌려준는 것이 2개 이상일 경우 List사용

```java
public List<Member> list() { //회원정보 불러오기
		List<Member> list = new ArrayList<Member>(); //List 객체 생성
		SqlSession session=null;
		try { session = getSession(); //DB연동
			list = session.selectList("list"); //SQL문 실행 id값 설정 = mapper
			System.out.println(e.getMessage());
		}
		return list;
	}
```
### updateForm.jsp (회원정보 폼)

```jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@page import="model.Member"%>
<%@page import="dao.MemberDao"%>

<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

<%
	String id = request.getParameter("id"); // id값을 받는다.
	MemberDao md = new MemberDao(); //DAO 객체 생성
	Member mem = md.select(id); // 상세 정보 구하기 select(id)
%>

<h2>정보 수정</h2>
<form action="updatePro.jsp">
	<input type="hidden" name="id" value="<%=mem.getId() %>"> 
	<table><tr><td>아이디</td>
			   <td><input type="text" name="id" value="<%=mem.getId() %>" disabled="disabled"></td></tr>
			   
			<tr><td>암호</td>
				<td><input type="text" name="password" value="<%=mem.getPassword() %>"></td></tr>
			<tr><td colspan="2" align="right">
					<input type="submit" value="변경"> 
					<input type="button" onclick="history.go(-1)" value="취소"></td></tr>
	</table>
</form>

</body>
</html>
```
* 값이 넘어온 값을 받는다. 
* id 값은 disabled은 비활성하여 값이 넘어가지 않는다. 그래서 hidden으로 Id값을 전달 한다.   
만약 id 값을 readOnly은  설정하며,  비활성화 이지만, 자동으로 값이 전달이 된다.    
### Select (DAO) 상세 정보 
* selectone은 1개의 정보 구한다. 
>object 자료형으로 다운 캐스팅한다.   
mem = (Member) session.selectOne("select", id);

```java
public Member select(String id) throws SQLException { // 상세정보 구함
		Member mem = null;
		SqlSession session=null;
		try { session = getSession(); 
		mem = (Member) session.selectOne("select", id);
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return mem;
	}
```
### updatePro.jsp(회원정보 수정)


```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@page import="dao.MemberDao"%>
<%@page import="model.Member"%>

<jsp:useBean id="mem" class="model.Member"/>
<jsp:setProperty property="*" name="mem"/>

<%
	MemberDao md = new MemberDao();
	int result = md.update(mem);

if (result == 1) {
%>
	<script type="text/javascript">
		alert("수정 성공");
		location.href="list.jsp";
	</script>
<%
} else {
%>
	<script type="text/javascript">
		alert("수정 실패");
		history.go(-1);
	</script>
<%
}
%>
```
### update(DAO)

```java
public int update(Member mem) {
		int result = 0;
		SqlSession session=null;
		try { session = getSession(); 
			result = session.update("update", mem);
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return result;
	}
}
```
### delete.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@page import="dao.MemberDao"%>

<!DOCTYPE html>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
String id = request.getParameter("id"); // 넘어온 값을 받는다.
MemberDao md = new MemberDao();
int result = md.delete(id);

if (result == 1) {
	%>
	<script type="text/javascript">
	alert("삭제성공");
	location.href="list.jsp";
	</script>
	<%
} else {
	%>
	<script type="text/javascript">
	alert("실패");
	location.href="list.jsp";
	</script>
	<%
}
%>
</body>
</html>
```
* 값이 넘어온 것을 받는다. (id)값

### delete (DAO)
```java
public int delete(String id) { 
		int result = 0;
		SqlSession session=null;
		try { session = getSession(); 
			result = session.delete("delete", id);
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return result;
	}
```





