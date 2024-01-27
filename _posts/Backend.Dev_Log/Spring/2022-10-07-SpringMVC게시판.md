---
layout: post
title: Spring MVC 게시판 실습
category: Backend_Dev.Log
tags: Spring
---
# Spring MVC 게시판 실습

### Data source Management 추가
<img width="236" alt="7" src="https://user-images.githubusercontent.com/107549149/194455558-45b113ba-9555-4790-9529-5b03deb71fe1.png">

<img width="703" alt="8" src="https://user-images.githubusercontent.com/107549149/194455653-1b68e398-ca38-4c29-b9dc-722823ea3336.png">

### 새로운 오라클 계정 연동


계정생성  
sqlplus system/oracle

create user spring identified by spring123;  
grant connect, resource to spring;  

### Data Source Explorer 새로운 계정 추가
<img width="355" alt="연동" src="https://user-images.githubusercontent.com/107549149/195006433-7efd9be3-6f58-410e-af6b-06b1aa7aa276.png">

<img width="340" alt="연동2" src="https://user-images.githubusercontent.com/107549149/195006465-4b10c0b8-c650-4c7f-b0a8-423c1a7ed965.png">

<img width="338" alt="확인" src="https://user-images.githubusercontent.com/107549149/195008060-8967cd5d-df3a-4941-8c88-7440dd1354e9.png">

<img width="386" alt="확인2" src="https://user-images.githubusercontent.com/107549149/195008115-211a0fef-5626-4377-ae0e-0a96566a8cc8.png">

<img width="626" alt="제목 없음" src="https://user-images.githubusercontent.com/107549149/195008206-913263b8-318a-457a-b97b-412e2f7e18d0.png">

* oracle 파일 위치 C:\Program Files\Java\jre1.8.0_231\lib\ext

### 게시판 실습 설계 
프로젝트 명: spring
```sql
-- 게시판
select *from tab;
select* from myboard;
select* from seq;

insert into myboard values(myboard_seq.nextval,'홍길동','1234','게시판연습',
	'게시판 내용',0,sysdate);
-- 테이블명 : myboard
create table myboard(
	  no number primary key,
	  writer varchar2(20),
       passwd varchar2(20),
	  subject varchar2(50),
	  content varchar2(100),
	  readcount number,
	  register date );

--시퀀스 :myboard_seq;
   create sequence myboard_seq;
```
### 프로그램 구조

```
java - myspring - controller - BoardController.java

	           service - BoardService.java

	           dao - BoardDao.java

	           model - Board.java


resources - util - configuration.xml
  
            sql - board.xml


board - boardform.jsp (글작성 폼)
	 
        boardlist.jsp (글목록)

        boardcontent.jsp (상세 페이지)

        boardupdateform.jsp (글수정 폼)

        boarddeleteform.jsp (글삭제 폼)


환경 설정 파일 작성 순서
1) pom.xml
2) web.xml
3) servlet-context.xml
4) configuration.xml
5) board.xml
6) root-context.xml
```
### pom.xml (maven 환경설정) 
* 잘 되어있는 pom.xml 파일을 복사 붙여넣기 하면 된다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.ch</groupId>
	<artifactId>myhome</artifactId> 
	<name>spring</name>
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
### web.xml 

* 한글 인코딩 코드 추가
* url-pattern 을 *.do 설정

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee https://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
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
*  base-package="myspring" 설정 한다. 
> java 폴더의 최상위 클래스 이다.
* java 폴더 하위에 myspring 생성한다. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

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
	
	<context:component-scan base-package="myspring" />
	
	
	
</beans:beans>
```
* myspring 하위에 controller , service , dao , model 폴더 생성한다. 

<img width="197" alt="설정" src="https://user-images.githubusercontent.com/107549149/194971726-075e1f9a-7277-479c-8938-e1a421c782ed.png">


### 처음에 해야하는 일
* controller 에 BoardController.java 클래스 생성 한다. 

> BoardController.java에 @Controller 생성 과 @ Autowired 생성한다.

* service 에 BoardService.java 클래스 생성

> BoardService.java  @Service 생성과 @Autowired 생성한다. 

* dao 에 BoardDao.java 클래스 생성한다.

>@Repository 와 @Autowired 설정 하는데 이떄 root-context를 먼저 세팅한다.

* model에 Board.java 클래스 생성한다. 
### configuration.xml(mybatis 환경설정)
* 테이블 갯수 만큼 늘어 나다. 

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

	<typeAliases>
		<typeAlias alias="board" type="myspring.model.Board" />
	</typeAliases>	
	
</configuration>
```
### board.xml(Sql문 실행 => insert , select, update , delete)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="boardns">
 <!--글 작성  -->
 <insert id="insert" parameterType="board">
 insert into myboard values(myboard_seq.nextval,#{writer}, 
  #{passwd},#{subject},#{content},0,sysdate)
 </insert>
 
<!-- 글 갯수-->
<select id="count" resultType="int">
select count(*) from myboard
</select>

<!-- 글목록 -->
<!--&gt; :  >  -->
<!--&lt; :  <  -->
<select id="list" parameterType="int" resultType="board" >
	<![CDATA[ 
	select * from (select  rownum rnum, board.* from (
	select * from myboard order by no desc)  board )
	where rnum  >=  ((#{page}-1 )* 10 +1)  and  rnum  <=  (#{page}*10)
	]]>
</select>
	
</mapper>
```
### root-context.xml (DB 연동)
1. Data Source 빈 생성
DB 연동을 위한 라이브러리 클래스 SimpleDriverDataSource 사용
> setDI 사용
2. sqlSessionFactory 빈 생성 
> mybaits 환경설정 파일 위치 와, mapper 파일 위치 설정
3. SqlSessionTemplate
> 생성자 매개 변수로 DI를 설정한다. 
DAO클래스에서 mapper 파일 ID값을 불러 올수 있다. 
> Autowired 가능하다. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd">
	
	<!-- <context:property-placeholder location="classpath:jdbc.properties" /> -->
	
	<!-- dataSource -->
	<!-- <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
		destroy-method="close">
		<property name="driverClass" value="${jdbc.driverClassName}" />
		<property name="jdbcUrl" value="${jdbc.url}" />
		<property name="user" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
		<property name="maxPoolSize" value="${jdbc.maxPoolSize}" />
	</bean> -->
	
	<!-- Data Source -->
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
		<property name="driverClass" value="oracle.jdbc.driver.OracleDriver" />
		<property name="url" value="jdbc:oracle:thin:@localhost:1521:xe" />
		<property name="username" value="spring" />
		<property name="password" value="spring123" />
	</bean>
	
	<!-- 스프링 jdbc 즉 스프링으로 oracle 디비 연결 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" /> // DB 연동 시켜준다.
		<property name="configLocation" value="classpath:configuration.xml" />// mabits 환경설정 파일
		<property name="mapperLocations" value="classpath:sql/*.xml" /> //mapper 파일위치
	</bean>
	
	<bean id="session" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory" />
	</bean>	
	
</beans>
```


**BoardController.java**

```java
package myspring.controller;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

import myspring.service.BoardService;

@Controller
public class BoardController {
@Autowired 
private BoardService bs ;
}
```
**BoardService.java**
```java
package myspring.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import myspring.dao.BoardDao;

@Service
public class BoardService {
@Autowired 
private BoardDao bd;
}
```
**BoardDao.java**
```java
package myspring.dao;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

@Repository
public class BoardDao {
//@ Autowired
private SqlSession session;
}
```
### Spring 환경 설정 

**servlet-context.xml**

* base-package="myspring" 설정 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

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
	
	<context:component-scan base-package="myspring" />
	
	
	
</beans:beans>
```
**root-context.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd">
	
	<!-- <context:property-placeholder location="classpath:jdbc.properties" /> -->
	
	<!-- dataSource -->
	<!-- <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
		destroy-method="close">
		<property name="driverClass" value="${jdbc.driverClassName}" />
		<property name="jdbcUrl" value="${jdbc.url}" />
		<property name="user" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
		<property name="maxPoolSize" value="${jdbc.maxPoolSize}" />
	</bean> -->
	
	<!-- Data Source -->
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
		<property name="driverClass" value="oracle.jdbc.driver.OracleDriver" />
		<property name="url" value="jdbc:oracle:thin:@localhost:1521:xe" />
		<property name="username" value="spring" />
		<property name="password" value="spring123" />
	</bean>
	
	<!-- 스프링 jdbc 즉 스프링으로 oracle 디비 연결 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" />
		<property name="configLocation" value="classpath:configuration.xml" />
		<property name="mapperLocations" value="classpath:sql/*.xml" />
	</bean>
	
	<bean id="session" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory" />
	</bean>	
	
</beans>
```

**boardform.jsp**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>글작성</title>
</head>
<body>

<form method=post action="boardwrite.do">
<table border=1 width=400 align=center>
	<caption>글작성</caption>
	<tr><th>작성자명</th>
		<td><input type=text name="writer" required="required"></td>
	</tr>
	<tr><th>비밀번호</th>
		<td><input type=password name="passwd" required="required"></td>
	</tr>
	<tr><th>제목</th>
		<td><input type=text name="subject" required="required"></td>
	</tr>
	<tr><th>내용</th>
		<td><textarea cols=40 rows=5 name="content" required="required"></textarea></td>
	</tr>
	<tr><td colspan=2 align=center>
			<input type=submit value="글작성">
			<input type=reset value="취소">
		</td>
	</tr>
</table>
</form>

</body>
</html>
```
### DTO

```java
package myspring.model;

import java.sql.Date;

public class Board {
	  private int  no ;
	  private String writer;
	  private String passwd ;
	  private String subject ;
	  private String content ;
	  private int readcount ;
	 private Date register ;
	
	 public int getNo() {
		return no;
	}
	public void setNo(int no) {
		this.no = no;
	}
	public String getWriter() {
		return writer;
	}
	public void setWriter(String writer) {
		this.writer = writer;
	}
	public String getPasswd() {
		return passwd;
	}
	public void setPasswd(String passwd) {
		this.passwd = passwd;
	}
	public String getSubject() {
		return subject;
	}
	public void setSubject(String subject) {
		this.subject = subject;
	}
	public String getContent() {
		return content;
	}
	public void setContent(String content) {
		this.content = content;
	}
	public int getReadcount() {
		return readcount;
	}
	public void setReadcount(int readcount) {
		this.readcount = readcount;
	}
	public Date getRegister() {
		return register;
	}
	public void setRegister(Date register) {
		this.register = register;
	}

}
```
### Cotroller 

```java
package myspring.controller;
import java.util.List;

import javax.servlet.http.HttpServletRequest;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.RequestMapping;

import myspring.model.Board;
import myspring.service.BoardService;

@Controller
public class BoardController {
@Autowired 
private BoardService bs ;

// 글작성 폼
@RequestMapping("boardform.do")
public String boardform() {

	return "board/boardform";
}

// 글 작성
@RequestMapping("boardwrite.do")
public String boardwrite(Board board , Model model) {
	int result =bs.insert(board);
	if(result==1) System.out.println("글작성 성공");
	model.addAttribute("result",result);
	
	return "board/insertresult";
   }
// 글목록 
@RequestMapping("boardlist.do")
public String boardlist(HttpServletRequest request, Model model)  {
	
	int page =1;   // 현재 페이지 번호
	int limit =10;  // 한 화면에 출력할 데이터 갯수
	
	int startRow = (page-1)* limit+1;
	int endRow = page* limit;
	
	int listcount = bs.getCount();  // 총데이터 갯수
	System.out.println("listcount:"+ listcount);

List<Board>boardlist = bs.getBoardList(page); // 게시판 목록
	System.out.println("boardlist:"+boardlist); 
	
	//총 페이지
	int pageCount = listcount/ limit + ((listcount%limit==0) ? 0: 1);
	
	int startPage = ((page-1)/10)*limit +1;  // 1, 11  ,21
	int endPage = startPage +10 -1 ;   // 10 ,20 ,30
	
	if(request.getParameter("page")!= null) {
		page =Integer.parseInt(request.getParameter("page"));
	}
	
	if(endPage > pageCount) endPage = pageCount ; // 실제 존재하는 페이지 까지만 설정
	
	model.addAttribute("page",page);
	model.addAttribute("listcount",listcount);
	model.addAttribute("boardlist",boardlist);
	model.addAttribute("pageCount",pageCount);
	model.addAttribute("startPage",startPage);
	model.addAttribute("endPage",endPage);
	
	return "board/boardlist";
    }
}
// 상세 페이지 : 조회수 1 증가 + 상세 정보 구하기 
@RequestMapping("boardcontent.do")
public String boardcontent(int no, int page, Model model)  {
	bs.updatecount(no);  // 조회수 1증가 
	Board board = bs.getBoard(no);  // 상세정보 구하기 
	
	String content = board.getContent().replace("\n", "<br>");  // 상세 페이지 내용, 줄 바꿈
	
model.addAttribute("board",board);
model.addAttribute("content",content);
model.addAttribute("page",page);

	return "board/boardcontent";
}
// 수정 폼
@RequestMapping("boardupdateform.do")
public String boardupdateform(int no , int page , Model model) { 
	
	Board board =bs.getBoard(no);  // 상세 정보 구하기 
	model.addAttribute("board", board);
	model.addAttribute("page", page);
	
	return "board/boardupdateform";
}
// 수정 등록
@RequestMapping("boardupdate.do")
public String boardupdate(Board board , int page,  Model model) {
	
int result =0;
Board old = bs.getBoard(board.getNo());

// 비번 비교 
if(old.getPasswd().equals(board.getPasswd())) {  //비번 일치
 	result = bs.update(board);   //글수정
}else {  // 비번 불일치 
	result=-1;
}
model.addAttribute("result", result);
model.addAttribute("board", board);
model.addAttribute("page", page);
	
	return "board/updateresult";
}
//삭제 폼
@RequestMapping("boarddeletform.do")
public String boarddeletform() { // model 객체로  no, page 값을 가져간다. or parmar으로 값을 가져간다.

	return "board/boarddeletform";
}
// 삭제 
@RequestMapping("boarddelet.do")
public String boarddelet(Board board , int page , Model model) {
	int result=0;
	Board old = bs.getBoard(board.getNo());  // 상세 정보 구하기
	
	// 비번 비교
	if(old.getPasswd().equals(board.getPasswd())) {  // 비번 일치
		result=bs.delete(board.getNo());
	}else {                                                             // 비번 불일치
		result=-1;
	}
	model.addAttribute("result",result);
	model.addAttribute("page",page);
	return "board/deleteresult";
}

```

### Service 
```java
package myspring.service;



import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import myspring.dao.BoardDao;
import myspring.model.Board;

@Service
public class BoardService {
@Autowired 
private BoardDao dao;

public int insert(Board board) {
	return dao.insert(board);
}
public int getCount() {
	return dao.getCount();
}
public List<Board>getBoardList(int page){
	return dao.getBoardList(page);
}
public updatecount(int no){
  return dao.updatecount(no);
}
public update(Board board){
  return dao.update(board);
}
public delete (int no){
  return dao.delete(no);
}
}
```
### DAO 
```java
package myspring.dao;



import java.util.List;

import org.apache.ibatis.session.SqlSession;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import myspring.model.Board;

@Repository
public class BoardDao {
@ Autowired
private SqlSession session;

public int insert(Board board) {
	return session.insert("insert",board);
     }

public int getCount() {
	return session.selectOne("count");
}
public List<Board>getBoardList(int page){
	return session.selectList("list",page);
}
public Board getBorad(int no){
  return session.selectone("count",no);
}
public int update(Board board){
  return session.update("update",update);
}
punlic int delete (int no){
  return session.delete("delete",no);
}
}
```
### board.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="boardns">
<insert id="insert" parameterType="board">
insert into myboard values(myboard_seq.nextval,#{writer},#{passwd},#{subject},
#{content},0,sysdate)
</insert>
<!-- 글 갯수-->
<select id="count" resultType="int">
select count(*) from myboard
</select>
<!-- 글목록 -->
<!--&gt; :  >  -->
<!--&lt; :  <  -->
<select id="list" parameterType="int" resultType="board" >
	<![CDATA[ 
	select * from (select  rownum rnum, board.* from (
	select * from myboard order by no desc)  board )
	where rnum  >=  ((#{page}-1 )* 10 +1)  and  rnum  <=  (#{page}*10)
	]]>
</select>
<!-- 조회수 1증가 -->
<select id="list" parameterType="int" resultType="board">
<![CDATA[ 
	select * from (select  rownum rnum, board.* from (
	select * from myboard order by no desc)  board )
	where rnum  >=  ((#{page}-1 )* 10 +1)  and  rnum  <=  (#{page}*10)
	]]>
</select>
<!-- 조회수 1증가 -->
	<update id="hit" parameterType="int">
		update myboard set readcount=readcount+1 where no=#{no}
	</update>
<!-- 상세정보 구하기 -->
	<select id="content" parameterType="int" resultType="board">
	   select * from myboard where no = #{no}
	</select>
<!--글 수정 -->
<update id="update" parameterType="board">
        update myboard set writer=#{writer}, subject=#{subject},
		content=#{content}, register=sysdate where no=#{no}
</update>	
<!-- 글 삭제 -->
<delete id="delete" parameterType="int">
delete from myboard where no=#{no}
</delete>
</mapper>
```
### view 페이지
**boardform.jsp(게시판 폼)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>글작성</title>
</head>
<body>

<form method=post action="boardwrite.do">
<table border=1 width=400 align=center>
	<caption>글작성</caption>
	<tr><th>작성자명</th>
		<td><input type=text name="writer" required="required"></td>
	</tr>
	<tr><th>비밀번호</th>
		<td><input type=password name="passwd" required="required"></td>
	</tr>
	<tr><th>제목</th>
		<td><input type=text name="subject" required="required"></td>
	</tr>
	<tr><th>내용</th>
		<td><textarea cols=40 rows=5 name="content" required="required"></textarea></td>
	</tr>
	<tr><td colspan=2 align=center>
			<input type=submit value="글작성">
			<input type=reset value="취소">
		</td>
	</tr>
</table>
</form>

</body>
</html>
```
**boardlist.jsp(게시판 목록)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
 <%@ taglib prefix="c"  uri="http://java.sun.com/jsp/jstl/core"%>
 <%@taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<a href="boardform.do">글작성</a> <br>
글갯수 : ${ listcount } 
<table border="1" align="center" width=700>
<caption>게시판 목록</caption>
<tr>

<th>번호</th>
<th>제목</th>
<th>작성자</th>
<th>날짜</th>
<th>조회수</th>
</tr>
<!--화면 출력 번호  -->
<c:set var="num" value="${listcount-(page-1)*10 }"/>
<c:forEach var="b" items="${boardlist }">
<tr>

					<td>${num }
					<c:set var="num" value="${num-1 }" />
					</td>
					<td>
					<a href="boardcontent.do?no=${b.no }&page=${page}">
					${b.subject }
					</a>
					</td>
					<td>${b.writer }</td>
					<td>
					<fmt:formatDate value="${b.register}"
					 pattern="yyyy-MM-dd HH:mm:ss"/>
					</td>
					 <td>${b.readcount }</td>

				</tr>
</c:forEach>


</table>

	<!--페이지 처리  -->
<center>
<c:if test="${listcount > 0 }">

<!-- 1페이지로 이동 -->
<a href="boardlist.do?page=1" style="text-decoration:none"> << </a>

<!-- 이전 블럭으로 이동 -->
<c:if test="${startPage > 10 }">
	<a href="boardlist.do?page=${startPage-10}">[이전]</a>
</c:if>

<!-- 각 블럭에 10개의 페이지 출력 -->
<c:forEach var="i" begin="${startPage}" end="${endPage}">
	<c:if test="${i == page }">      <!-- 현재 페이지 -->
		[${i}]
	</c:if>
	<c:if test="${i != page }">      <!-- 현재 페이지가 아닌 경우 -->
		<a href="boardlist.do?page=${i}">[${i}]</a>
	</c:if>
</c:forEach>

<!-- 다음 블럭으로 이동 -->
<c:if test="${endPage < pageCount}">
	<a href="boardlist.do?page=${startPage+10}">[다음]</a>
</c:if>

<!-- 마지막 페이지로 이동 -->
<a href="boardlist.do?page=${pageCount}" style="text-decoration:none"> >> </a>
</c:if>
</center>	
	
</body>
</html>
```
**boardcontent.jsp**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>    
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>상세 페이지</title>
</head>
<body>

<table border=1 width=400 align=center>
	<caption>상세 페이지</caption>
	<tr>
		<td>작성자</td>
		<td>${board.writer }</td>
	</tr>
	<tr>
		<td>날짜</td>
		<td>
			<fmt:formatDate value="${board.register }" 
					pattern="yyyy-MM-dd HH:mm:ss"/>
		</td>
	</tr>
	<tr>
		<td>조회수</td>
		<td>${board.readcount }</td>
	</tr>
	<tr>
		<td>제목</td>
		<td>${board.subject }</td>
	</tr>
	<tr>
		<td>내용</td>
		<td>
			<pre>${board.content }</pre>
			${content}
		</td>
	</tr>
	<tr>
		<td colspan=2 align=center>
			<input type="button" value="목록"  
			onClick="location.href='boardlist.do?page=${page}'">
			<input type="button" value="수정" 
			onClick="location.href='boardupdateform.do?no=${board.no}&page=${page}' " >
			<input type="button" value="삭제"
			onClick="location.href='boarddeletform.do?no=${board.no}&page=${page}' ">
		</td>
	</tr>
</table>

</body>
</html>
```
**boardcontent.jsp(글 상세페이지)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>    
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>상세 페이지</title>
</head>
<body>

<table border=1 width=400 align=center>
	<caption>상세 페이지</caption>
	<tr>
		<td>작성자</td>
		<td>${board.writer }</td>
	</tr>
	<tr>
		<td>날짜</td>
		<td>
			<fmt:formatDate value="${board.register }" 
					pattern="yyyy-MM-dd HH:mm:ss"/>
		</td>
	</tr>
	<tr>
		<td>조회수</td>
		<td>${board.readcount }</td>
	</tr>
	<tr>
		<td>제목</td>
		<td>${board.subject }</td>
	</tr>
	<tr>
		<td>내용</td>
		<td>
			<pre>${board.content }</pre>
			${content}
		</td>
	</tr>
	<tr>
		<td colspan=2 align=center>
			<input type="button" value="목록"  
			onClick="location.href='boardlist.do?page=${page}'">
			<input type="button" value="수정" 
			onClick="location.href='boardupdateform.do?no=${board.no}&page=${page}' " >
			<input type="button" value="삭제"
			onClick="location.href='boarddeletform.do?no=${board.no}&page=${page}' ">
		</td>
	</tr>
</table>

</body>
</html>
```
**boardupdateform.jsp(글 수정 폼)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>글수정</title>
</head>
<body>

<form method=post action="boardupdate.do">
<input type="hidden"  name="no" value="${board.no }">
<input type="hidden"  name="page" value="${page}">

<table border=1 width=400 align=center>
	<caption>글수정</caption>
	<tr><th>작성자명</th>
		<td><input type=text name="writer" required="required" value="${board.writer}"  autofocus="autofocus"></td>
	</tr>
	<tr><th>비밀번호</th>
		<td><input type=password name="passwd" required="required"></td>
	</tr>
	<tr><th>제목</th>
		<td><input type=text name="subject" required="required" value="${board.subject }"></td>
	</tr>
	<tr><th>내용</th>
		<td><textarea cols=40 rows=5 name="content" required="required">${board.content}</textarea></td>
	</tr>
	<tr><td colspan=2 align=center>
			<input type=submit value="글수정">
			<input type=reset value="취소">
		</td>
	</tr>
</table>
</form>

</body>
</html>
```
**updateresult.jsp(글 수정)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>    
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<c:if test="${result == 1 }">
	<script>
		alert("글수정 성공");
		location.href="boardlist.do?page=${page}";		            // 목록 페이지
//		location.href="boardcontent.do?no=${board.no}&page=${page}";// 상세 페이지
	</script>
</c:if>

<c:if test="${result != 1 }">
	<script>
		alert("수정 실패");
		history.go(-1);
	</script>
</c:if>

</body>
</html>
```
**boarddeletform.jsp(글삭제 폼)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>글삭제</title>
</head>
<body>

no:${param.no }<br>
page:${param.page }<br>
<!-- no,page 값을 param 으로 값을 가져간다.  -->

<form method=post action="boarddelet.do">
<input type="hidden"  name="no" value="${param.no }">
<input type="hidden"  name="page" value="${param.page}">

<table border=1 width=400 align=center>
	<caption>글삭제</caption>
	<tr><th>비밀번호</th>
		<td><input type=password name="passwd" required="required"></td>
    </tr>
	<tr><td colspan=2 align=center>
			<input type=submit value="글삭제">
			<input type=reset value="취소">
		</td>
	</tr>
</table>
</form>

</body>
</html>
```
**deletresult.jsp(글삭제)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>    
    
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<c:if test="${result == 1 }">
	<script>
		alert("글삭제");
		location.href="boardlist.do?page=${page}";		            // 목록 페이지
	</script>
</c:if>

<c:if test="${result != 1 }">
	<script>
		alert("글삭제 실패");
		history.go(-1);
	</script>
</c:if>

</body>
</html>
```







