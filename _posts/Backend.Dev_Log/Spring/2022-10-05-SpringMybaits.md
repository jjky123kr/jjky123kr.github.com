---
layout: post
title: Spring My Batis 연동 예제 1
folder: "spring"
category: Backend
tags: Spring
---
# Spring My Batis 연동 예제 1
### Spring MVC 흐름
<img width="583" alt="흐름" src="https://user-images.githubusercontent.com/107549149/193986855-c24e0c65-0eda-4914-97a0-aa51eb38bbfd.png">

### 프로젝트 구조

<img width="208" alt="mybatis1" src="https://user-images.githubusercontent.com/107549149/193985245-50ef19f0-9d6a-4821-84af-1c5fd3f83508.png">

### pox.xml (환경설정)

* artifactID는 프로젝트 명 

```xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.ch</groupId>
	<artifactId>myBatis2</artifactId> 
	<name>myBatis2</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>
		<java-version>1.8</java-version>
		<org.springframework-version>4.3.6.RELEASE</org.springframework-version>
		<org.aspectj-version>1.6.10</org.aspectj-version>
		<org.slf4j-version>1.6.6</org.slf4j-version>
	</properties>
	<repositories>
		<repository>
			<id>codelds</id>
			<url>https://code.lds.org/nexus/content/groups/main-repo</url>
		</repository>
	</repositories>
	<dependencies>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-validator</artifactId>
			<version>5.1.3.Final</version>
		</dependency>
		
		<!-- oracle -->
		<dependency>
			<groupId>com.oracle</groupId>
			<artifactId>ojdbc6</artifactId>
			<version>11.2.0.3</version>
			<scope>compile</scope>
		</dependency>
		
		<!-- dbcp jdbc connection pool -->
		<dependency>
			<groupId>commons-dbcp</groupId>
			<artifactId>commons-dbcp</artifactId>
			<version>1.2.2</version>
		</dependency>
		<dependency>
			<groupId>c3p0</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.1.2</version>
		</dependency>
		
		<!-- mybatis -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.2.3</version>
		</dependency>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>1.2.1</version>
		</dependency>
		
		<!-- ibatis -->
		<dependency>
			<groupId>org.apache.ibatis</groupId>
			<artifactId>ibatis-sqlmap</artifactId>
			<version>2.3.4.726</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-ibatis</artifactId>
			<version>2.0.8</version>
		</dependency>
		
		<!-- cglib ibatis-->
		<dependency>
			<groupId>cglib</groupId>
			<artifactId>cglib</artifactId>
			<version>2.2</version>
		</dependency>
		
		<!-- Spring -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${org.springframework-version}</version>
			<exclusions>
			
				<!-- Exclude Commons Logging in favor of SLF4j -->
				<exclusion>
					<groupId>commons-logging</groupId>
					<artifactId>commons-logging</artifactId>
				</exclusion>
			</exclusions>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- AspectJ -->
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>${org.aspectj-version}</version>
		</dependency>
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.6.8</version>
		</dependency>

		<!-- Logging -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>${org.slf4j-version}</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>jcl-over-slf4j</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>${org.slf4j-version}</version>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.15</version>
			<exclusions>
				<exclusion>
					<groupId>javax.mail</groupId>
					<artifactId>mail</artifactId>
				</exclusion>
				<exclusion>
					<groupId>javax.jms</groupId>
					<artifactId>jms</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jdmk</groupId>
					<artifactId>jmxtools</artifactId>
				</exclusion>
				<exclusion>
					<groupId>com.sun.jmx</groupId>
					<artifactId>jmxri</artifactId>
				</exclusion>
			</exclusions>
			<scope>runtime</scope>
		</dependency>

		<!-- @Inject -->
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>

		<!-- Servlet -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>servlet-api</artifactId>
			<version>2.5</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.1</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>
		
		<!-- Test -->
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.7</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>commons-fileupload</groupId>
			<artifactId>commons-fileupload</artifactId>
			<version>1.2</version>
		</dependency>

		<!-- Spring Security -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-core</artifactId>
			<version>3.2.4.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-web</artifactId>
			<version>3.2.4.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-config</artifactId>
			<version>3.2.4.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-taglibs</artifactId>
			<version>3.1.4.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>1.3.2</version>
		</dependency>

		<dependency>
			<groupId>org.scala-lang</groupId>
			<artifactId>scala-library</artifactId>
			<version>2.10.4</version>
		</dependency>
		 
		<!-- javax.mail-->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>4.1.3.RELEASE</version>
		</dependency>
		<dependency>
			<groupId>javax.mail</groupId>
			<artifactId>mail</artifactId>
			<version>1.4.7</version>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<source>${java-version}</source>
					<target>${java-version}</target>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-war-plugin</artifactId>
				<configuration>
					<warName>abc</warName>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-dependency-plugin</artifactId>
				<executions>
					<execution>
						<id>install</id>
						<phase>install</phase>
						<goals>
							<goal>sources</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-resources-plugin</artifactId>
				<version>2.5</version>
				<configuration>
					<encoding>UTF-8</encoding>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```

web.xml (프로젝트 환경설정)
* 한글 인코딩 등록 
* DispatcherServlet 등록 , servlet-mapping 설정
* Spring 환경설정 파일(root-context.xml,servlet-context.xml) 등록

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
	<!-- The definition of the Root Spring Container shared by all Servlets and Filters -->
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
		<url-pattern>*.do</url-pattern>
	</servlet-mapping>
</web-app>
```

servlet-context.xml
* base-package="myBatis1" java 폴더 하위에 패키지(폴더) 설정.  

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
	
	<!-- Resolves views selected for rendering by @Controllers to .jsp resources in the /WEB-INF/views directory -->
	<beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<beans:property name="prefix" value="/WEB-INF/views/" />
		<beans:property name="suffix" value=".jsp" />
	</beans:bean>
		
	<context:component-scan base-package="myBatis1" />	
	
</beans:beans>
```
* mybatis1 하위 클래스를 읽어온다. 
* base-package="myBatis1" 하위 클래스에 @Component, @Controller, @Service, @Repository 어노테이션이 붙어야 한다.
> @Autowired 사용할 수 있다. 


![image](https://user-images.githubusercontent.com/107549149/193976110-243211e4-e7ae-43c4-a2c6-d96accdc8504.png){:width="500" height="500"}

* resources 폴더 

* jdbc.properties (DB 연동)

```
jdbc.driverClassName=oracle.jdbc.driver.OracleDriver
jdbc.url=jdbc:oracle:thin:@127.0.0.1:1521:xe
jdbc.username=scott
jdbc.password=tiger
jdbc.maxPoolSize=20
```

configuration.xml (Mybatis 환경설정 파일)

1. typeAlias DTO 클래스를 별칭설정 dept
2. DB접속 설정 (jdbc.properties ) 불러와서 value설정
3. mapper 파일 설정 (SQL 문)

```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<properties resource="jdbc.properties" />
	<typeAliases>
		<typeAlias alias="dept" type="myBatis1.model.Dept" />
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
		<mapper resource="Dept.xml" />
	</mappers>
</configuration>
```
### DTO

```java
package myBatis1.model;

public class Dept {
	private int deptno;
	private String dname;
	private String loc;

	public int getDeptno() {
		return deptno;
	}

	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}

	public String getDname() {
		return dname;
	}

	public void setDname(String dname) {
		this.dname = dname;
	}

	public String getLoc() {
		return loc;
	}

	public void setLoc(String loc) {
		this.loc = loc;
	}

}
```

index.jsp

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<script type="text/javascript">
	location.href="deptList.do";
</script>
</body>
</html>
```
### Controller 

DeptController.java  
```java

package myBatis1.controller; 

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import myBatis1.model.Dept;
import myBatis1.service.DeptService;

@Controller
public class DeptController {
	@Autowired
	private DeptService ds; // DeptService(인터페이스) 
// 부서 목록
	@RequestMapping("deptList.do")
	public String list(Model model) {   //Model 객체 생성: view 페이지에 값을 저장하고, 값을 전달한다.
		List<Dept> list = ds.list();      // 데이터가 2개 이상 검색할떄는 list 로 결과를 받는다.  
		model.addAttribute("list", list); // view 페이지에서 list 값 전달 할떄  forEach 태그 item 값으로 list 사용된다.
		return "deptList"; // prefix 와 확장자를 생략 
	}
// 부서 상세페이지
	@RequestMapping("deptView.do")
	public String deptView(int deptno, Model model) {  // 전달되는 변수 명과, 값을 받는 변수 명이 같으면, @RequestParm("deptno") 생략
		Dept dept = ds.select(deptno);
		model.addAttribute("dept", dept);  // Model이 값을 저장하고, 값을 전달한다. 이때 자료형이 DTO 이면 view페이지 에서 ${dept.필드명}
		return "deptView";
	}
// 수정 폼
	@RequestMapping("deptUpdateForm.do")
	public String deptUpdateForm(int deptno, Model model) { 
		Dept dept = ds.select(deptno);
		model.addAttribute("dept", dept); // Model에 값을 저장하고, 값을 전달한다. 이때 자료형이 DTO 이면 view페이지 에서 ${dept.필드명}
		return "deptUpdateForm";
	}
// 
	@RequestMapping("deptUpdate.do")
	public String deptUpdate(Dept dept, Model model) {
		int result = ds.update(dept);
		model.addAttribute("result", result); 
		return "deptUpdate";
	}

	@RequestMapping("deptDelete.do")
	public String deptDelete(int deptno, Model model) {
		int result = ds.delete(deptno);
		model.addAttribute("result", result);
		return "deptDelete";
	}
}
```
* @Autowired 은 Service 에 set 메소드로 주입한다
> DTO 객체를 생성 하지 않아도 된다.
* 데이터가 2개 이상 검색할때 list로 결과를 전달한다.
> view 페이지에서 결과를 출력 할때 forEach 태그 item 값 list
* 데이터가 1개 일때 DTO 객체로 결과를 전달 
>view 페이지 에서 결과를 출력 할때 ${dept(DTO).필드명}
* 전달되는 변수 명과, 값을 받는 변수 명이 같으면, @RequestParm("deptno") 생략




### Service 

DeptService.java(인터페이스)

```java
package myBatis1.service;

import java.util.List;
import myBatis1.model.Dept;

public interface DeptService {
	List<Dept> list();

	Dept select(int deptno);

	int update(Dept dept);

	int delete(int deptno);

}
```
DeptServiceImpl.java  (구현 클래스 )

* @Autowired 으로 set메소드로 주입하여 DTO 객체를 자동으로 생성된다.

```java

package myBatis1.service;

import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import myBatis1.dao.DeptDao;
import myBatis1.model.Dept;

@Service
public class DeptServiceImpl implements DeptService { // 인터페이스 상속
	@Autowired
	private DeptDao dd;

	public List<Dept> list() {
		return dd.list();
	}

	public Dept select(int deptno) {
		return dd.select(deptno);
	}

	public int update(Dept dept) {
		return dd.update(dept);
	}

	public int delete(int deptno) {
		return dd.delete(deptno);
	}
}
```
### DAO 

DeptDao(인터페이스)

```java
package myBatis1.dao;

import java.util.List;
import myBatis1.model.Dept;

public interface DeptDao {
	List<Dept> list();

	Dept select(int deptno);

	int update(Dept dept);

	int delete(int deptno);

}
```
DeptDaoImpl.java

* DepDao 인터페이스 상속 

```java
package myBatis1.dao;

import java.io.IOException;
import java.io.Reader;
import java.util.List;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.springframework.stereotype.Repository;

import myBatis1.model.Dept;

@Repository
public class DeptDaoImpl implements DeptDao {
	private static SqlSession session;
	static {
	    try { 
	    	Reader reader = Resources.getResourceAsReader("configuration.xml");
		      SqlSessionFactory ssf = new SqlSessionFactoryBuilder().build(reader);
		      session = ssf.openSession(true);
		      reader.close(); 
		} catch (IOException e) {
		  System.out.println("read file error : "+e.getMessage());
		}
	}
	public List<Dept> list() {                  // list를  불러서, Dept에 저장을 한다. 
		return session.selectList("deptns.list"); // deptns : mapper 파일의 namespace 이다. list : mapper 파일의 ID값
	}                                           //list SQL문을 불러온다.
	public Dept select(int deptno) {            // 한개의 정보를  불러올때 DTO 객체 명(Dept) 
		return session.selectOne("deptns.select",deptno);  // ID =select (SQL문) 자료형= deptno 
	}
	public int update(Dept dept) {
		return session.update("deptns.update",dept);
	}
	public int delete(int deptno) {
		return session.delete("deptns.delete",deptno);
	}
}
```
### Dept.xml (SQL문)
* 컬럼명과 DTO 필드 명이 같으면 자동으로 mpping 실행된다. 
> 컬럼명 과 DTO 필드 명 과 입력양식의 name 값을 일치 해야 Mapping 자동으로 실행.
* resultMap의 역할은 DTO 클래스(필드)와 테이블 컬럼명 하고 다를때 mapping 설정
* resultMap은  조인을 할 경우에 사용된다.
* resultMap ID는 deptResult 로 설정과, type은 alias명(DTO)객체 이다.
* resultType 은 결과를 돌려주는 곳 (DTO) select 만 사용한다. 
* 제약 조건 부모를 참조하기 때문에 부모도 같이 삭제되는 경우가 발생하여 삭제 불가하다.  
  이때 자식만 삭제하는 코드를 추가한다.(oracle을 참조하자)

```xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="deptns">
	
	<!-- Use type aliases to avoid typing the full classname every time. -->
	<resultMap id="deptResult"    type="dept">
		<result property="deptno" column="deptno" />
		<result property="dname"  column="dname" />
		<result property="loc"	  column="loc" />
	</resultMap>
	
	<select id="list" resultMap="deptResult">
		select * from dept order by deptno
	</select>
	
	<select id="select" parameterType="int" resultType="dept"> 
		select * from dept where deptno=#{deptno}
	</select>
	
	<update id="update" parameterType="dept">
		update dept set dname=#{dname},loc=#{loc} where deptno=#{deptno}
	</update>
	
	<delete id="delete" parameterType="int">
		delete from dept where deptno=#{deptno}
	</delete>
	
</mapper>
```
### 공통 내용 파일
header.jsp 

```js
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%--@ taglib prefix="spring" uri="http://www.springframework.org/tags"--%>
<%--@ taglib prefix="form" uri="http://www.springframework.org/tags/form"--%>
<c:set var="path" value="${pageContext.request.contextPath }" />
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link href="${path}/css/bootstrap.min.css" rel="stylesheet">
<script src="${path}/js/jquery.js"></script>
<script src="${path}/js/bootstrap.min..js"></script>
<style>
.err {
	color: red;
	font-size: 20px;
}
</style>
```
### view 페이지
deptList.java (목록페이지)

* 부서명을 선택 하면 상세 페이지 링크 연동 할떄 get 방식으로 부서번호를 전달

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
	<div class="container">
		<h2 class="text-primary">부서 목록</h2>
		<table class="table table-hover">
			<tr>
				<th>부서코드</th>
				<th>부서명</th>
				<th>근무지</th>
			</tr>
			<c:if test="${not empty list }"> // list가 비어있지 않을때 
				<c:forEach var="dept" items="${list}">
					<tr>
						<td>${dept.deptno }</td> // 부서번호
						<td><a href="deptView.do?deptno=${dept.deptno}"    //부서명 링크 연결: 생세페이지
							   class="btn btn-info"> ${dept.dname}</a></td>  // get 방식 부서 번호 가져간다. 
						<td>${dept.loc }</td> // 근무지
					</tr>
				</c:forEach>
			</c:if>
			<c:if test="${empty list }"> // list가 비어있을때
				<tr>
					<th colspan="3">데이터가 없습니다</th>
				</tr>
			</c:if>
		</table>
	</div>
</body>
</html>
```
deptView.jsp(상세페이지)

* load () 로 div 특정 영역에 출력한다.

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

<!-- load() deptList.do 요청한 결과 페이지(deptList.jsp)의 table태그만 불러와서 id=list값에 출력 -->
<script type="text/javascript">
	$(function() {
		$('#list').load("deptList.do table"); 
	});
</script>
</head>
<body>
	<div class="container">
		<h2>부서 상세정보</h2>
		<table class="table table-bordered">
			<tr>
				<th>부서코드</th>
				<td>${dept.deptno}</td>
			</tr>
			<tr>
				<th>부서명</th>
				<td>${dept.dname }</td>
			</tr>
			<tr>
				<th>근무지</th>
				<td>${dept.loc }</td>
			</tr>
			<tr>
				<th colspan="2"><a href="deptList.do" class="btn btn-info">목록</a> 
					<a href="deptUpdateForm.do?deptno=${dept.deptno}" class="btn btn-info">수정</a>  // get 방식으로 부서번호를 가져간다.
					<a href="deptDelete.do?deptno=${dept.deptno}" class="btn btn-info">삭제</a></th> 
			</tr>
		</table>
	</div>
	<div id="list"></div> // 로드함수로 불러와서 div 사이에 출력한다.
</body>
</html>
```
deptUpdateForm.jsp(수정폼)
* 부서번호 값을 input type="hidden"으로 값을 전달 한다.

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
		<h2 class="text-primary">부서정보 수정</h2>
		<form action="deptUpdate.do" method="post">
			<input type="hidden" name="deptno" value="${dept.deptno}">
			<table class="table table-striped">
				<tr>
					<th>부서코드</th>
					<td>${dept.deptno}</td> 
				</tr>
				<tr>
					<th>부서명</th>
					<td><input type="text" name="dname" required="required"	value="${dept.dname }"></td>
				</tr>
				<tr>
					<th>근무지</th>
					<td><input type="text" name="loc" required="required" value="${dept.loc }"></td>
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
deptUpdate.jsp (수정완료)
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
	<c:if test="${result > 0 }">
		<script type="text/javascript">
			alert("수정 성공");
			location.href = "deptList.do";
		</script>
	</c:if>
	<c:if test="${result <= 0 }">
		<script type="text/javascript">
			alert("수정 실패");
			history.go(-1);
		</script>
	</c:if>
</body>
</html>
```
deptDelete.jsp(삭제)
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
	<c:if test="${result > 0 }">
		<script type="text/javascript">
			alert("삭제 성공");
			location.href = "deptList.do";
		</script>
	</c:if>
	<c:if test="${result <= 0 }">
		<script type="text/javascript">
			alert("삭제 실패");
			history.go(-1);
		</script>
	</c:if>
</body>
</html>
```
