---
layout: post
title: Spring Mybatis 연동 예제2
category: Backend
tags: Spring
---
# Spring Mybatis 연동 예제2
### Spring MVC 흐름
<img width="583" alt="흐름" src="https://user-images.githubusercontent.com/107549149/193986855-c24e0c65-0eda-4914-97a0-aa51eb38bbfd.png">

### 환경설정 파일 

**pox.xml**

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
**web.xml** 
1. DispatcherServlet 등록
2. Spring 환경설정(root-context.xml ,servlet-contexet.xml ) 등록
3. 한글 인코딩

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
**servlet-context.xml(Spring 환경설정)** 

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
	
	<context:component-scan base-package="myBatis2" />	
	
</beans:beans>
```
1. base-package 설정 한다.=> 어노테이션 기반으로 DI를사용 한다.

>  base-package="myBatis2" 는 myBatis2 하위 클래스를 읽어온다 라는 의미이다.

* <span style="background-color:#fffd91">base-package="myBatis2"(= 최상위 디렉토리: java 파일) 하위 클래스에   
 @Component, @Controller, @Service, @Repository 어노테이션이 붙어있어야 한다.</span>

> @Autowired 사용할 수 있다. => set메서드에 주입을 해서 DTO 객체 생성이 된다. 

* Dao 클래스 에 @Repository 가 붙어있고 , (SqlSessionTemplate) 구현클래스에 @Autowired 주입한다.
* SqlSessionTemplate(DAO 구현클래스) or SqlSession(DAO 인터페이스 클래스) @Autowired 에 주입가능.
> SQL 문을 실행 할 수 있다. 

 ![image](https://user-images.githubusercontent.com/107549149/193976110-243211e4-e7ae-43c4-a2c6-d96accdc8504.png){:width="500" height="500"}
 
 2. prefix :controller 클래스에서 return 할떄 설정되어 있는 최상위 폴더를 생략 한다.     
    suffix :controller 클래스에서 return 할떄 설정되어 있는 확장자 생략 

**jdbc.properties (oracle 연동)**

```js
jdbc.driverClassName=oracle.jdbc.driver.OracleDriver
jdbc.url=jdbc:oracle:thin:@127.0.0.1:1521:xe
jdbc.username=scott
jdbc.password=tiger
jdbc.maxPoolSize=20
```

**root-context.xml(Spring 환경설정 = DB 연동)**

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
		<property name="username" value="scott" />
		<property name="password" value="tiger" />
	</bean>
	
	<!-- 스프링 jdbc 즉 스프링으로 oracle 디비 연결 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource" /> // DB연동 시켜주는 코드
		<property name="configLocation" value="classpath:configuration.xml" /> mybatis 환경설정 파일 불러온다.(경로)
		<property name="mapperLocations" value="classpath:sql/*.xml" /> mapper 파일 불러온다.
	</bean>
	
	<bean id="session" class="org.mybatis.spring.SqlSessionTemplate">
		<constructor-arg index="0" ref="sqlSessionFactory" /> 
	</bean>	
	
</beans>
```
1. SimpleDriverDataSource 클래스 생성 하고  bean id = dataSource 객체 생성한다. 
* set DI를 사용한다. => set 메서드로 주입한다. 
2. SqlSessionFactoryBean 클래스 생성하고, beab id= sqlSessionFactory    

> <span style="background-color:#fffd91"> property name="dataSource" ref="dataSource"  DB연동</span>

* mabtis 파일 과 mapper 파일을 불러온다. 
resources 폴더에 있을떄 classpath: 붙여셔 불러온다. 
3. 생성자 매개변수로 주입하는 한다.
* <span style="background-color:#fffd91">SqlSessionTemplate (구현)클래스 생성 하고, 생성자 매개변수 주입 bean 객체 생성.</span>

> constructor-arg index="0" <span style="background-color:#fffd91" >ref="sqlSessionFactory"</span>
 SQL문의 id 값을 불러올수 있다. 




**configuration.xml (mybatis 환경설정)**
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

	<typeAliases>
		<typeAlias alias="dept" type="myBatis2.model.Dept" />
		<typeAlias alias="emp"  type="myBatis2.model.Emp" />
	</typeAliases>	
	
	<!-- 
	<mappers>
		<mapper resource="sql/Dept.xml" />
		<mapper resource="sql/Emp.xml" />
	</mappers>  -->	
	
</configuration>
```
1. typeAlias alias = DTO 치환된 명칭  type= DTO 클래스 경로

### index.jsp
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
	location.href="deptList.do";
</script>
</body>
</html>
```
### model(DTO)

**Dept.java** 

```java
package myBatis2.model;

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
**Emp.java**

```java
package myBatis2.model;

import java.sql.Date;

public class Emp {
	private int empno;
	private String ename;
	private String job;
	private int mgr;
	private Date hiredate;
	private int sal;
	private int comm;
	private int deptno;
	private String dname;
	private String loc;

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

	public int getEmpno() {
		return empno;
	}

	public void setEmpno(int empno) {
		this.empno = empno;
	}

	public String getEname() {
		return ename;
	}

	public void setEname(String ename) {
		this.ename = ename;
	}

	public String getJob() {
		return job;
	}

	public void setJob(String job) {
		this.job = job;
	}

	public int getMgr() {
		return mgr;
	}

	public void setMgr(int mgr) {
		this.mgr = mgr;
	}

	public Date getHiredate() {
		return hiredate;
	}

	public void setHiredate(Date hiredate) {
		this.hiredate = hiredate;
	}

	public int getSal() {
		return sal;
	}

	public void setSal(int sal) {
		this.sal = sal;
	}

	public int getComm() {
		return comm;
	}

	public void setComm(int comm) {
		this.comm = comm;
	}

	public int getDeptno() {
		return deptno;
	}

	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}
}
```
### Controller 클래스(dept)
**DeptController.java**

```java
package myBatis2.controller;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.RequestMapping;

import myBatis2.model.Dept;
import myBatis2.service.DeptService;

@Controller
public class DeptController {
	@Autowired
	private DeptService ds;
// 부서목록
	@RequestMapping("deptList.do") // 첫번째 요청 구간
	public String deptList(Model model) {  Model 객체 생성
		List<Dept> list = ds.list();
		model.addAttribute("list", list); view 페이지로 값을 가져간다.
		return "deptList";
	}
// 부서 등록폼
	@RequestMapping("deptInsertForm.do")
	public String deptInsertForm() {
		return "deptInsertForm";
	}
// 부서등록
	@RequestMapping("deptInsert.do")
	public String deptInsert(@ModelAttribute Dept dept, 
			                 Model model) {  // Dept(DTO) 객체 생성 => 한번에 많은 데이터 저장
		Dept dt = ds.select(dept.getDeptno()); // Dept(DTO)부서번호 중복 검사
		if (dt == null) {                 // 부서번호 중복이 아닌경우
			int result = ds.insert(dept);
			model.addAttribute("result", result);
		} else {                         // 부서번호 중복
			model.addAttribute("msg", "이미 있는 데이터입니다");
			model.addAttribute("result", -1);
		}
		return "deptInsert"; // 부서 중복 검사 메세지 및 등록 페이지 처리
	}
// 부서정보 수정 폼
	@RequestMapping("deptUpdateForm.do")
	public String deptUpdateForm(int deptno, Model model) {
		Dept dept = ds.select(deptno);
		model.addAttribute("dept", dept);
		return "deptUpdateForm"; // 부서정보 수정 폼 페이지
	}
// 부서 정보수정
	@RequestMapping("deptUpdate.do")
	public String deptUpdate(Dept dept, Model model) {
		int result = ds.update(dept);
		model.addAttribute("result", result);
		return "deptUpdate"; // 정보수정 성공 메세지 출력 페이지
	}
// 부서 삭제 
	@RequestMapping("deptDelete.do")
	public String deptDelete(int deptno, Model model) {
		int result = ds.delete(deptno);
		model.addAttribute("result", result);
		return "deptDelete"; // 삭제 성공 메세지 출력 페이지 
	}
}
```
* Service로 넘어가기 위해서 @Autowired 설정한다.
>private DeptService ds;
* Model: view 페이지로 값을 가져간다.
* 2개 이상의 데이터 를 가져오면  list 사용한다. 
> 값을 view 페이지에 출력할때 forEach태그 item 속성 값=list 출력. 
* 1개 인 경우에는 DTO 클래스를 사용한다. 
> 값을 view 페이지에 출력할때 ${DTO.필드명}으로 출력

### Service (dept)

**DeptService.java(service 인터페이스)**
```java
package myBatis2.service;

import java.util.List;
import myBatis2.model.Dept;

public interface DeptService {
	List<Dept> list();

	int insert(Dept dept);

	Dept select(int deptno);

	int update(Dept dept);

	int delete(int deptno);
}
```
**DeptServiceImple.java(service 구현클래스)**

```java
package myBatis2.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import myBatis2.dao.DeptDao;
import myBatis2.model.Dept;

@Service
public class DeptServiceImpl implements DeptService {
	@Autowired
	private DeptDao dd;

	public List<Dept> list() {
		return dd.list();
	}

	public int insert(Dept dept) {
		return dd.insert(dept);
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
* DAO 로 넘어가기 위해서 @Autowired 설정
>private DeptDao dd;

### DAO
**DeptDao.java (DAO 인터페이스)**
```java
package myBatis2.dao;

import java.util.List;
import myBatis2.model.Dept;

public interface DeptDao {
	List<Dept> list();

	int insert(Dept dept);

	Dept select(int deptno);

	int update(Dept dept);

	int delete(int deptno);
}
```
**DeptDaoImpl.java (DAO 구현클래스)**
```java
package myBatis2.dao;

import java.util.List;
import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;
import myBatis2.model.Dept;

@Repository
public class DeptDaoImpl implements DeptDao {
	@Autowired
	private SqlSessionTemplate st;

	public List<Dept> list() {
		return st.selectList("deptns.list");
	}

	public int insert(Dept dept) {
		return st.insert("deptns.insert", dept);
	}

	public Dept select(int deptno) {
		return st.selectOne("deptns.select", deptno);
	}

	public int update(Dept dept) {
		return st.update("deptns.update", dept);
	}

	public int delete(int deptno) {
		return st.delete("deptns.delete", deptno);
	}
}
```
* selectList는 검색하는 데이터가 2개 이상 일때 사용된다.(list)
* selectOne은 검색하는 데이터가 1개 일때 사용된다. (DTO)


**Dept.xml(SQL문)**

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
		select * from dept order by deptno // 오름 차순으로 정렬
	</select>
	
	<select id="select" parameterType="int" resultType="dept"> 
		select * from dept where deptno=#{deptno} 부서번호 변수 
	</select>
	
	<update id="update" parameterType="dept">
		update dept set dname=#{dname},loc=#{loc} where deptno=#{deptno}
	</update>
	
	<delete id="delete" parameterType="int">
		delete from dept where deptno=#{deptno}
	</delete>
	
	<insert id="insert" parameterType="dept">
		insert into dept values(#{deptno},#{dname},#{loc})
	</insert>
	
</mapper>
```
* namespance ="deptns" => namespance를 설정해야 ID 값이 중복 오류를 해결할 수 있다. 

### view
**deptList.jsp(부서 목록)**
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
		<h2 class="text-primary">부서 목록</h2>
		<table class="table table-hover">
			<tr>
				<th>부서코드</th>
				<th>부서명</th>
				<th>근무지</th>
				<th></th>
				<th></th>
			</tr>
			<c:if test="${empty list}">
				<tr>
					<td colspan="5">데이터가 없습니다</td>
				</tr>
			</c:if>
			<c:if test="${not empty list }">
				<c:forEach var="dept" items="${list}">
					<tr>
						<td>${dept.deptno}</td>
						<td><a href="empList.do?deptno=${dept.deptno}" // 부서직원와 부서번호
							   class="btn btn-info">${dept.dname}</a></td> 부서명
						<td>${dept.loc }</td>
						<td><a href="deptUpdateForm.do?deptno=${dept.deptno }"
							   class="btn btn-success">수정</a></td>
						<td><a href="deptDelete.do?deptno=${dept.deptno }"
							   class="btn btn-danger">삭제</a></td>
				</c:forEach>
			</c:if>
		</table>
		<a href="deptInsertForm.do" class="btn btn-info">부서입력</a> 
		<a href="empAllList.do" class="btn btn-default">전체 직원목록</a>
	</div>
</body>
</html>
```
### Controller(emp)
**EmpController.java**

```java
package myBatis2.controller;

import java.sql.Date;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import myBatis2.model.Dept;
import myBatis2.model.Emp;
import myBatis2.service.DeptService;
import myBatis2.service.EmpService;

@Controller
public class EmpController {
	@Autowired
	private EmpService es;
	@Autowired
	private DeptService ds;

	@RequestMapping("empList.do")
	public String empList(int deptno, Model model) { Model  객체 생성
		Dept dept = ds.select(deptno); // 부서정보 list
		List<Emp> list = es.list(deptno); // 직원 정보list 
		model.addAttribute("dept", dept); 
		model.addAttribute("list", list);
		return "emp/empList";  // 하위 디렉토리 부터 작성한다. 
	}
// 부서 직원 등록 폼
	@RequestMapping("empInsertForm.do")
	public String empInsertForm(Model model) {
		List<Dept> deptList = ds.list(); // 부서 정보 list  저장
		List<Emp> empList = es.empList(); // 직원 정보 list 저장
		model.addAttribute("deptList", deptList);  //Model 값을 전달 한다. list
		model.addAttribute("empList", empList);    //Model 값을 전달 한다. list
		return "emp/empInsertForm";
	}
// 사원번호 중복검사
	@RequestMapping("dupCheck.do")
	public String dupCheck(int empno, Model model) { 
		System.out.println("empno:"+empno); // 콘솔창 출력
		Emp emp = es.select(empno); // empno 검색 
		String msg = "";
		if (emp != null)
			msg = "이미 있는 데이터입니다";
		else
			msg = "사용 가능한 사번 입니다";
		model.addAttribute("msg", msg);

    // 웹 브라우저 출력되는 결과가callback()함수로 리턴된다.
		return "emp/dupCheck"; // callback() 함수 리턴 결과(브라우저) 출력 페이지
	}
// 사원 등록
	@RequestMapping("empInsert.do")
//	public String empInsert(Emp emp, String hiredate1, Model model) {
	public String empInsert(Emp emp, Model model) { //emp객체생성(DTO)
//		emp.setHiredate(Date.valueOf(hiredate1)); // String -> Date 형변환
		int result = es.insert(emp); // insert 수행되면 결과를 돌려준다.
		model.addAttribute("result", result);
		model.addAttribute("deptno", emp.getDeptno());  // 부서번호를 가져간다.
		return "emp/empInsert";
	}
// 사원 상세 페이지 
	@RequestMapping("empView.do")
	public String empView(int empno, Model model) {전달 변수명 = 값을 받는 변수(@RequsetParam생략)
  	Emp emp = es.select(empno); // 사원 상세 정보 구하기 결과를 받는 자료형은 emp(DTO) 매개변수는empno(사원 번호)
		model.addAttribute("emp", emp); Model에 emp(DTO)을 저장한다. view 페이지 출력${emp.필드}
		return "emp/empView";
	}
// 사원 삭제 
	@RequestMapping("empDelete.do")
	public String empDelete(int empno, Model model) { empno와 model 객체 생성
		Emp emp = es.select(empno);  // 상세정보 구하기 => 삭제 후 사원 부서로 돌아가기 위해서
		int result = es.delete(empno); // 삭제 할때 weher 조건절에 empno 
		model.addAttribute("result", result);
		model.addAttribute("deptno", emp.getDeptno()); // 사원부서로 가기 위새서,
		return "emp/empDelete"; 
	}
//사원 정보 수정폼
	@RequestMapping("empUpdateForm.do")
	public String empUpdateForm(int empno, Model model) {
		Emp emp = es.select(empno);      // 사원 상세 정보
		List<Dept> deptList = ds.list(); // 부서 정보 list 
		model.addAttribute("emp", emp);
		model.addAttribute("deptList", deptList); 
		return "emp/empUpdateForm";
	}
// 사원 정보 수정
	@RequestMapping("empUpdate.do")
	public String empUpdate(Emp emp, Model model) {  //DTO , Model 객체 생성
		int result = es.update(emp);  // 사원 정보 수정
		model.addAttribute("deptno", emp.getDeptno());  // 부서번호도 가져간다. => 부서상세 페이지
		model.addAttribute("result", result);
		return "emp/empUpdate"; 
	}
// 사원 목록
	@RequestMapping("empAllList.do")
	public String empAllList(Model model) {
		List<Emp> list = es.empAllList();
		model.addAttribute("list", list);
		return "emp/empAllList"; 
	}
}
```
* Service 로 가기 위해서 @Autowired 등록 . 
<span sytle="background-color:#fffd91">Service 가 2개이면, 개별적으로 @Autowired를 사용해야 한다.</span>
* EmpService 와 DeptService를 등록한다. 

```java
@Controller
public class EmpController {
	@Autowired
	private EmpService es;
	@Autowired
	private DeptService ds;
``` 

### Service (emp)
EmpService (인터페이스)
```java
package myBatis2.service;

import java.util.List;
import myBatis2.model.Emp;

public interface EmpService {
	List<Emp> list(int deptno);

	Emp select(int empno);

	int insert(Emp emp);

	int delete(int empno);

	int update(Emp emp);

	List<Emp> empList();

	List<Emp> empAllList();
}
```
**EmpServiceImpl.java(구현 클래스)**
```java
package myBatis2.service;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import myBatis2.dao.EmpDao;
import myBatis2.model.Emp;

@Service
public class EmpServiceImpl implements EmpService {
	@Autowired
	private EmpDao ed;

	public List<Emp> list(int deptno) {
		return ed.list(deptno);
	}

	public List<Emp> empList() {
		return ed.empList();
	}

	public Emp select(int empno) {
		return ed.select(empno);
	}

	public int insert(Emp emp) {
		return ed.insert(emp);
	}

	public int delete(int empno) {
		return ed.delete(empno);
	}

	public int update(Emp emp) {
		return ed.update(emp);
	}

	public List<Emp> empAllList() {
		return ed.empAllList();
	}
}
```
### DAO (emp)
**EmpDao(DAO 인터페이스)**

```java
package myBatis2.dao;

import java.util.List;
import myBatis2.model.Emp;

public interface EmpDao {
	List<Emp> list(int deptno);

	List<Emp> empList();

	Emp select(int empno);

	int insert(Emp emp);

	int delete(int empno);

	int update(Emp emp);

	List<Emp> empAllList();
}
```
**EmpDaoImpl.java (DAO 구현클래스)**

```java
package myBatis2.dao;

import java.util.List;

import org.mybatis.spring.SqlSessionTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Repository;

import myBatis2.model.Emp;

@Repository
public class EmpDaoImpl implements EmpDao {
	@Autowired
	private SqlSessionTemplate sst; //SQL문 실행 역할

	public List<Emp> list(int deptno) { 
		return sst.selectList("empns.list", deptno);
	}

	public List<Emp> empList() {  // 사원목록
		return sst.selectList("empns.empList");
	}

	public Emp select(int empno) {  // 사원상세 정보 구하기 결과를 DTO 받는다. 
		return sst.selectOne("empns.select", empno); // select =id , empno=전달된 값
	}

	public int insert(Emp emp) {
		return sst.insert("empns.insert", emp);
	}

	public int delete(int empno) {
		return sst.delete("empns.delete", empno);
	}

	public int update(Emp emp) { // 사원정보 수정
		return sst.update("empns.update", emp);
	}

	public List<Emp> empAllList() {
		return sst.selectList("empAllList");
	}
}
```
### Emp.xml ( EMP SQL문)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="empns">

	<!-- Use type aliases to avoid typing the full classname every time. -->
	<resultMap id="empResult"    	type="emp">
		<result property="empno" 	column="empno" />
		<result property="ename"  	column="ename" />
		<result property="job"		column="job" />
		<result property="mgr" 		column="mgr" />
		<result property="hiredate" column="hiredate" />
		<result property="sal"	  	column="sal" />
		<result property="comm"	   	column="comm" />
		<result property="deptno"   column="deptno" />
		<result property="dname"	column="dname" />
		<result property="loc"   	column="loc" />
	</resultMap>
	
	<select id="empList" resultMap="empResult">
		select * from emp order by empno
	</select>
	
	<select id="list" parameterType="int" resultMap="empResult">
		select * from emp where deptno=#{deptno} order by empno
	</select>
		
	<select id="select" parameterType="int" resultType="emp">
		select * from emp where empno=#{empno}
	</select>
	
	<insert id="insert" parameterType="emp">
		insert into emp values(#{empno},#{ename},#{job},#{mgr},
			#{hiredate},#{sal},#{comm},#{deptno})
	</insert>
	
	<delete id="delete" parameterType="int"> 
		delete from emp where empno=#{empno}
	</delete>
	
	<update id="update" parameterType="emp"> 
		update emp set ename=#{ename},job=#{job},sal=${sal},
			comm=#{comm},deptno=#{deptno} where empno=#{empno} // 사원 번호를 where 조건절
	</update>
	
	<select id="empAllList" resultMap="empResult">
		select e.*,dname,loc from emp e, dept d  // 등가조인: 두개의 테이블을 조인한다. 
			where e.deptno=d.deptno order by empno  // 별칭을 사용한다. 
	</select>                                   
</mapper>
```
* parameterType는 전달되는 자료형 .
* resultType= 결과를 돌려주는 변수 DTO 
* resultMap은  테이블 컬럼 명 과 DTO의 필드 명이 달라서 Mapping을 해야 한다.
  resultMap의 id 값을 설정 
### view 페이지(emp)
* view 파일 생성 할때 그룹으로 폴더를 생성하여 나누어서 개발한다.
ex) member 폴더 , board폴더, 등 으로 생성한다. 

**empAllList.jsp (전체 직원 목록)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="../header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div class="container">
		<table class="table table-striped">
			<tr>
				<td>사번</td>
				<td>이름</td>
				<td>업무</td>
				<td>급여</td>
				<td>부서코드</td>
				<td>부서명</td>
				<td>근무지</td>
			</tr>
			<c:forEach var="emp" items="${list }">
				<tr>
					<td>${emp.empno }</td>
					<td>${emp.ename }</td>
					<td>${emp.job }</td>
					<td>${emp.sal }</td>
					<td>${emp.deptno }</td>
					<td>${emp.dname }</td>
					<td>${emp.loc }</td>
				</tr>
			</c:forEach>
		</table>
		<a href="deptList.do" class="btn btn-default">부서목록</a>
	</div>
</body>
</html>
```
 **deptInsertForm.jsp(부서등록 폼)**

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
		<h2 class="text-primary">부서정보 입력</h2>
		<form action="deptInsert.do" method="post">
			<table class="table table-hover">
				<tr>
					<td>부서코드</td>
					<td><input type="number" maxlength="2" name="deptno"
						required="required" autofocus="autofocus"></td>
				</tr>
				<tr>
					<td>부서명</td>
					<td><input type="text" name="dname" required="required"></td>
				</tr>
				<tr>
					<td>근무지</td>
					<td><input type="text" name="loc" required="required"></td>
				</tr>
				<tr>
					<td colspan="2"><input type="submit" value="확인"></td>
				</tr>
			</table>
		</form>
	</div>
</body>
</html>
```
**deptInsert.jsp(부서등록)**
* primarykey 제약 조건으로 부서 번호를 중복이 되면 안되게 설계 
* 부서 등록을 했을때 alert()함수로 메세지 출력 하는 페이지 


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
			alert("입력 성공");
			location.href = "deptList.do";
		</script>
	</c:if>
	<c:if test="${result == 0 }">
		<script type="text/javascript">
			alert("입력 실패");
			history.go(-1);
		</script>
	</c:if>
	<c:if test="${result == -1 }">
		<script type="text/javascript">
			alert("${msg}");
			history.go(-1);
		</script>
	</c:if>
</body>
</html>
```
**deptUpdateForm.jsp(부서정보 수정)**
* input type="hidden"으로 deptno(번호값)을 가져간다. 
> 업데이트 SQL문을 실행할때 where 조건절로 deptno을 사용된다.

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
			<input type="hidden" name="deptno" value="${dept.deptno }">
			<table class="table table-hover">
				<tr>
					<td>부서코드</td>
					<td>${dept.deptno}</td>
				</tr>
				<tr>
					<td>부서명</td>
					<td><input type="text" name="dname" required="required"
							   value="${dept.dname}"></td>
				</tr>
				<tr>
					<td>근무지</td>
					<td><input type="text" name="loc" required="required"
							   value="${dept.loc}"></td>
				</tr>
				<tr>
					<td colspan="2"><input type="submit" value="확인"></td>
				</tr>
			</table>
		</form>
	</div>
</body>
</html>
```
**deptUpdate.jsp(수정 성공 메세지 출력 페이지)**

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
			location.href = "deptList.do"; // 부서목록 페이지
		</script>
	</c:if>
	<c:if test="${result <= 0 }">
		<script type="text/javascript">
			alert("수정 실패");
			history.go(-1); // 수정 페이지
		</script>
	</c:if>
</body>
</html>
```
**deptDelete.jsp(부서삭제)**
* 부서 삭제 메세지 출력 페이지 

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
			location.href = "deptList.do";
		</script>
	</c:if>
</body>
</html>
```
### view (emp)
**empList.jsp (부서 직원 목록)**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="../header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	$(function() {
    // deptList.do 요청 결과를 div 태그 사이에 출력한다. 
		$('#list').load('deptList.do'); // 부서 목록 불러온다. 
	});
</script>
</head>
<body>
	<div class="container" align="center">
		<h2 class="text-primary">${dept.dname}직원목록</h2>
		<table class="table table-striped">
			<tr>
				<td>사번</td>
				<td>이름</td>
				<td>업무</td>
				<td>급여</td>
				<td>부서코드</td>
			</tr>
			<tr>
				<c:if test="${empty list }">
					<tr>
						<td colspan="5">직원이 없는 부서입니다</td>
					</tr>
				</c:if>
				<c:if test="${not empty list }">
					<c:forEach var="emp" items="${list }">
						<tr>
							<td>${emp.empno }</td> // 사원번호
							<td><a href="empView.do?empno=${emp.empno}" // 직원상세 정보
								   class="btn btn-info">${emp.ename}</a></td> // 직원명
							<td>${emp.job}</td> 
							<td>${emp.sal}</td>
							<td>${emp.deptno}</td>
						</tr>
					</c:forEach>
				</c:if>
		</table>
		<a href="deptList.do" class="btn btn-success">부서목록</a> 
		<a href="empInsertForm.do" class="btn btn-success">직원 입력</a>
		<div id="list"></div>
	</div>
</body>
</html>
```
**empInsertForm.jsp (직원 등록 폼)**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="../header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	$(function() { 
		$('#dupCheck').click(function() {
			var empno = $('#empno').val();
			if (!empno) {
				alert('사번 입력후에 체크');
				$('#empno').focus();
				return false;
			}// $.post("요청이름","전달될 값","콜백함수"); // 비동기적 처리
			$.post('dupCheck.do', 'empno(변수명) =' + empno(전달되는 변수명), function(msg) {
				alert(msg);
			});
		});
	});
</script>
</head>
<body>
	<div class="container">
		<h2 class="text-primary">직원 등록</h2>
		<form action="empInsert.do" method="post"> 
			<table class="table table-bordered">
				<tr>
					<td>사번</td>
					<td><input type="number" name="empno" required="required"
							   id="empno" autofocus="autofocus">  
						<input type="button" value="중복" id="dupCheck"></td> // 중복 검사 
				</tr>
				<tr>
					<td>이름</td>
					<td><input type="text" name="ename" required="required"></td>
				</tr>
				<tr>
					<td>업무</td>
					<td><input type="text" name="job" required="required"></td>
				</tr>
				<tr>
					<td>관리자</td>  // 직속상사
					<td><select name="mgr"> 
							<c:forEach var="emp" items="${empList }"> // 사원목록
								<option value="${emp.empno}">${emp.ename}</option>
							</c:forEach> //mgr에 전달되는 값이 사원번호(empno) // 사원명이 출력 ${emp.ename}
					    </select>
					</td>
				</tr>
				<tr>
					<td>입사일</td>
					<td><input type="date" name="hiredate" required="required"></td>
				</tr>  // intput type="date"은 달력 
				<tr>
					<td>급여</td>
					<td><input type="number" name="sal" required="required"></td>
				</tr>
				<tr>
					<td>보너스</td>
					<td><input type="number" name="comm" required="required"></td>
				</tr>
				<tr>
					<td>부서코드</td>
					<td><select name="deptno">
							<c:forEach var="dept" items="${deptList }">// 부서정보
								<option value="${dept.deptno}">${dept.dname}</option>
							</c:forEach> // 부서번호를 가져간다.// 부서 명이 출력
						</select>
					</td>
				</tr>
				<tr>
					<td colspan="2"><input type="submit" value="확인"></td>
				</tr>
			</table>
		</form>
	</div>
</body>
</html>
```
**dupCheck.jsp**
* 사원 번호 중복 검사  에서 callback () 로 리턴되는 처리  페이지.
* msg  메세지 출력 .  

```js
${msg}

```
**empInsert.jsp(직원 등록)**

```jsp

<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="../header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<c:if test="${result > 0 }">
		<script type="text/javascript">
			alert("입력 성공");
			location.href = "empList.do?deptno=${deptno}"; 부서 번호를 등록 
		</script>
	</c:if>
	<c:if test="${result <= 0 }">
		<script type="text/javascript">
			alert("입력 실패");
			history.go(-1);
		</script>
	</c:if>
</body>
</html>
```
**emp View.jsp (사원 상세 정보)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="../header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	$(function() {
		$('#list').load('empList.do?deptno=${emp.deptno}');
	});
</script>
</head>
<body>
	<div class="container">
		<h2 class="text-primary">직원 상세정보</h2>
		<table class="table table-bordered">
			<tr>
				<td>사번</td>
				<td>${emp.empno }</td>
			</tr>
			<tr>
				<td>이름</td>
				<td>${emp.ename}</td>
			</tr>
			<tr>
				<td>업무</td>
				<td>${emp.job }</td>
			</tr>
			<tr>
				<td>관리자</td>
				<td>${emp.mgr }</td>
			</tr>
			<tr>
				<td>입사일</td>
				<td>${emp.hiredate }</td>
			</tr>
			<tr>
				<td>급여</td>
				<td>${emp.sal }</td>
			</tr>
			<tr>
				<td>보너스</td>
				<td>${emp.comm }</td>
			</tr>
			<tr>
				<td>부서코드</td>
				<td>${emp.deptno }</td>
			</tr>
		</table>
		<a href="empUpdateForm.do?empno=${emp.empno}" class="btn btn-info">수정</a>
		<a class="btn btn-danger" href="empDelete.do?empno=${emp.empno}">삭제</a>
		<a href="empList.do?deptno=${emp.deptno}" class="btn btn-default">목록</a>
		<div id="list"></div>
	</div>
</body>
</html>
```
**empUpdateForm.jsp(사원수정 폼)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="../header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<div class="container" align="center">
		<h2 class="text-primary">직원정보 수정</h2>
		<form action="empUpdate.do" method="post">
			<table class="table table-bordered">
				<tr>
					<td>사번</td>
					<td><input type="text" name="empno" readonly="readonly" 
						value="${emp.empno}"></td> // readonly: 비활성 => 폼을 통해서 값 전달 가능
				</tr>
				<tr>
					<td>이름</td>
					<td><input type="text" name="ename" required="required"
						value="${emp.ename }"></td>
				</tr>
				<tr>
					<td>업무</td>
					<td><input type="text" name="job" required="required"
						value="${emp.job }"></td>
				</tr>
				<tr>
					<td>급여</td>
					<td><input type="text" name="sal" required="required"
						value="${emp.sal}"></td>
				</tr>
				<tr>
					<td>보너스</td>
					<td><input type="text" name="comm" required="required"
						value="${emp.comm }"></td>
				</tr>
				<tr>
					<td>부서코드</td>
					<td><select name="deptno"> 부서 번호 
							<c:forEach var="dept" items="${deptList}"> 
								<c:if test="${emp.deptno==dept.deptno}"> emp.deptno 가입 했던, dept.deptno 부서 번호가 같으면
									<option value="${dept.deptno}" selected="selected"> // 가입 했을때 부서명을 선택되게 만든다.
										${dept.dname}(${dept.deptno})</option>
								</c:if> // 부서명   부서 번호 
								<c:if test="${emp.deptno!=dept.deptno}"> 
									<option value="${dept.deptno}">${dept.dname}(${dept.deptno})</option>
								</c:if>            
							</c:forEach>
					</select></td>
				</tr>
				<tr>
					<td colspan="2"><input type="submit" value="수정"></td>
				</tr>
			</table>
		</form>
	</div>
</body>
</html>
```
**empUpdate.jap(사원수정)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="../header.jsp"%>

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
			location.href = "empList.do?deptno=${deptno}";  //부서상세 페이지로 돌아갈떄 부서번호값 필요
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
**empDelete.jsp(사원삭제)**
```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="../header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<c:if test="${result > 0 }">
		<script type="text/javascript">
			alert("삭제 성공 ");
			location.href = "empList.do?deptno=${deptno}";
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

