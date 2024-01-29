---
layout: post
title: Spring  web sock (채팅 기능)
category: Backend
tags: Spring
---
# Spring web sock (채팅 기능)
### 프로젝트 구조 

### pom.xml 
* WebSocket 라이브러리 추가 설정

```

<!-- WebSocket -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-websocket</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.ch</groupId>
	<artifactId>webSock</artifactId>
	<name>webSock</name>
	<packaging>war</packaging>
	<version>1.0.0-BUILD-SNAPSHOT</version>
	<properties>
		<java-version>1.11</java-version>
		<org.springframework-version>4.1.4.RELEASE</org.springframework-version>
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

		<!-- JDK11 추가 -->
		<dependency>
			<groupId>javax.xml.bind</groupId>
			<artifactId>jaxb-api</artifactId>
			<version>2.3.1</version>
		</dependency>
		<dependency>
			<groupId>com.sun.xml.bind</groupId>
			<artifactId>jaxb-core</artifactId>
			<version>2.3.0.1</version>
		</dependency>
		<dependency>
			<groupId>com.sun.xml.bind</groupId>
			<artifactId>jaxb-impl</artifactId>
			<version>2.3.1</version>
		</dependency>

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
		<!-- cglib ibatis -->
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

		<!-- WebSocket -->
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-websocket</artifactId>
			<version>${org.springframework-version}</version>
		</dependency>

		<!-- jackson -->
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.9.5</version>
		</dependency>

		<!-- jackson -->
		<!-- <dependency>
			<groupId>org.codehaus.jackson</groupId>
			<artifactId>jackson-mapper-asl</artifactId>
			<version>1.4.2</version>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.4.1.3</version>
			<version>2.5.2</version>
			<version>2.8.0</version>
			<version>2.9.9</version>
		</dependency> -->

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
		<!-- javax.mail -->
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

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee" 
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" version="3.0">
	
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
		<async-supported>true</async-supported>
	</servlet>		
	<servlet-mapping>
		<servlet-name>appServlet</servlet-name>
		<url-pattern>*.do</url-pattern>
	</servlet-mapping>
</web-app>
```
### servlet-context.xml (Spring 환경설정)
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:beans="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:websocket="http://www.springframework.org/schema/websocket"
	xsi:schemaLocation="http://www.springframework.org/schema/websocket http://www.springframework.org/schema/websocket/spring-websocket-4.1.xsd
		http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
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
	
	<context:component-scan base-package="com.ch.webSock" />
	
	<default-servlet-handler/>
	<websocket:handlers>
		<websocket:mapping handler="chatHandler" path="chat-ws.do"/>
	</websocket:handlers>
	<beans:bean id="chatHandler" class="com.ch.webSock.WebChatHandler"/>
		
</beans:beans>
```

* 추가 기능  역할
* chat-ws.do 요청이 들어오면, WebChatHandler 클래스가 동작한다. 

```xml
<default-servlet-handler/>
	<websocket:handlers>
		<websocket:mapping handler="chatHandler" path="chat-ws.do"/>
	</websocket:handlers>
	<beans:bean id="chatHandler" class="com.ch.webSock.WebChatHandler"/>
```
### HomeController 

```java
package com.ch.webSock;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HomeController {
	@RequestMapping("chat.do")
	public String home() {
		return "chat";
	}	
}
```
### WebChatHandler.java
```java
package com.ch.webSock;
import java.util.Collection;
import java.util.HashMap;
import java.util.Map;

import org.springframework.web.socket.CloseStatus;
import org.springframework.web.socket.TextMessage;
import org.springframework.web.socket.WebSocketSession;
import org.springframework.web.socket.handler.TextWebSocketHandler;

public class WebChatHandler extends TextWebSocketHandler { 
	Map<String, WebSocketSession> users = new HashMap<String, WebSocketSession>();
	public void afterConnectionEstablished(WebSocketSession session) throws Exception {
		users.put(session.getId(), session); 
	}
	public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {
		users.remove(session.getId());
	}
	protected void handleTextMessage(WebSocketSession session, 
			TextMessage message) throws Exception {
		String msg = message.getPayload(); // 실제 메세지 전송
		TextMessage tMsg = new TextMessage(msg.substring(4));
		Collection<WebSocketSession> list = users.values();
		for (WebSocketSession wss : list) {  // 목록을 구한다. 
			wss.sendMessage(tMsg);
		}
	}
}
```
*  TextWebSocketHandler 상속을 받는다. 
> 메소드 오버라이딩을 한다. 
1. Map를 이용해서 users을 생성한다.

> users는 사용자ID값 + session값을 구한다.

2. afterConnectionEstablished 은 websock 연결 후 작동한다. 
3. handleTextMessage  



### heahder.jsp 

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags"%>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form"%>

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

### chat.jsp 

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ include file="header.jsp"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
	var websock;
	$(function() {
		$('#message').keypress(function(event) { // keypress =키를 눌렀을떄 
			var keycode = event.keyCode ? event.keyCode : event.which;
			if (keycode == 13) // 아스키 코드 13 = Enter  전송 기능 
				send();          
			event.stopPropagation();
		});
		$('#enterBtn').click(function() { 연결
			connect();      // 입장
		});
		$('#exitBtn').click(function() {   // 전송을 클릭 했을떄 
			disconnect();   // 전송
		});
		$('#sendBtn').click(function() {
			send();         // 퇴장
		});
	});
	function send() {      // 전송 메세지 
		var nickName = $('#nickName').val(); // 대화명 호출= 입력한 값(nickName) 구하기 
		var msg = $('#message').val();       // 메세지 호출 = 입력한 값(message) 구하기
		websocket.send('msg:' + nickName + ' => ' + msg); // send ()함수 호출 해서 대화명+ 메세지전달
		$('#message').val(''); // 
	}
	function connect() {    // 호출   포트 번호    /webSock/ hat-ws.do(요청이름)
		websock = new WebSocket("ws://localhost:8088/webSock/chat-ws.do");
		websock.onopen = onOpen; 
//		websock.onclose = onClose;
		websock.onmessage = onMessage;
	}
	function disconnect() {  // 퇴장
		websock.close();
	}
	function send() {    
		var nickname = $('#nickName').val(); 
		var message = $('#message').val();
		websock.send('msg:' + nickname + ' : ' + message);
		$('#message').val('');
	}
	function onOpen(event) {  // appendMassge는 연결 메세지 
		appendMessage("연결되었습니다.");
	}
	function onClose(event) {
		appendMessage("연결이 종료되었니다.");
	}
	function onMessage(event) {
		var data = event.data;
		appendMessage(data);
	}
	function appendMessage(msg) {  // appendMassge 연결 메세지 내용
		$('#chatMessageArea').append(msg + "<br>");
		var chatAreaheight = $('#chatArea').height();
		var maxscroll = $('#chatMessageArea').height() - chatAreaheight;
		$('#chatArea').scrollTop(maxscroll);
	}
</script>
</head>
<body>
	<div class="container">
		별명 : <input type="text" id="nickName"> 
		     <input type="button" value="입장" id="enterBtn" class="btn btn-success"> 
		     <input	type="button" value="퇴장" id="exitBtn" class="btn btn-danger">
		     
			 <h2 class="text-primary">대화영역</h2>
			 <input type="text" id="message" required="required"> 
			 <input	type="button" value="전송" id="sendBtn" class="btn btn-info">
			 <div id="chatArea">
				<div id="chatMessageArea"></div>
			 </div>
	</div>
</body>
</html>
```








