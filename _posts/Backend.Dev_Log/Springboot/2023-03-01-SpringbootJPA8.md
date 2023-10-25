---
layout: post
title:  Spring boot JPA 연습
category: Backend_Dev.Log
tags: Springboot
---
# Spring boot JPA 1

### 프로젝트 생성  
<img width="484" alt="환경설정" src="https://user-images.githubusercontent.com/107549149/222143551-2478e008-801b-486b-962b-edf60f3d8d51.png">


<img width="338" alt="환경설정2" src="https://user-images.githubusercontent.com/107549149/222143612-7fe81e94-e20f-41ac-a4a8-48ea819bd8fe.png">

<img width="341" alt="환경설정3" src="https://user-images.githubusercontent.com/107549149/222143666-a5321db7-6fcc-4cb2-8b07-94f498771003.png">

### 의존성 설정 
* Spring boot devtools
* JAP
* Spring Security
* JSP
* JSTL 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.0.3</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.animals.test</groupId>
	<artifactId>blog</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>
	<name>blog</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
	
	 <!-- JPA -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-security</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>3.0.0</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-devtools</artifactId>
			<scope>runtime</scope>
			<optional>true</optional>
		</dependency>
		
		<!-- MySQL -->
		<dependency>
			<groupId>com.mysql</groupId>
			<artifactId>mysql-connector-j</artifactId>
			<scope>runtime</scope>
		</dependency>
		
		<!-- Oracle -->
		<dependency>
			<groupId>com.oracle.database.jdbc</groupId>
			<artifactId>ojdbc8</artifactId>
			<scope>runtime</scope>
		</dependency>
		
		<!-- Lombok -->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
			<scope>provided</scope>
		</dependency>
		
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
		
		<!-- security -->
		<dependency>
			<groupId>org.springframework.security</groupId>
			<artifactId>spring-security-test</artifactId>
			<scope>test</scope>
		</dependency>
		
		<!-- JSP 템플릿 엔진 -->
		<dependency>
			<groupId>org.apache.tomcat.embed</groupId>
			<artifactId>tomcat-embed-jasper</artifactId>
		</dependency>

		<!-- JSTL -->
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
		</dependency>
	</dependencies>
	

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<excludes>
						<exclude>
							<groupId>org.projectlombok</groupId>
							<artifactId>lombok</artifactId>
						</exclude>
					</excludes>
				</configuration>
			</plugin>
		</plugins>
	</build>

</project>

```
### postman 을 설치 한다. 웹브라우저에서 바로 볼수있다. 

<img width="910" alt="1" src="https://user-images.githubusercontent.com/107549149/222298411-ae65385b-9cf4-400f-a24c-bb092f1a0b81.png">

<img width="819" alt="2" src="https://user-images.githubusercontent.com/107549149/222298465-dad0b171-5ef6-41b5-9e4d-be737ba98d7b.png"> 


### MySQL 연동 
#### applicaiotn.yml
* Hibernate 사용 
* show-sql=true  콘솔창에 sql 출력
* hibernate.format_sql=true  콘솔창 sql 보기좋게 출력 된다. 



```yml

server:
  port: 8000
  servlet:
    context-path: /blog
    encoding:
      charset: UTF-8
      enabled: true
      force: true
    
spring:
  mvc:
    view:
      prefix: /WEB-INF/views/
      suffix: .jsp
      
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3307/blog?serverTimezone=Asia/Seoul
    username: cos
    password: cos1234
    
  jpa:  
    open-in-view: true
    hibernate:
      ddl-auto: create
      naming:
        physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
      use-new-id-generator-mappings: false
    show-sql: true
    properties:
      hibernate.format_sql: true

  jackson:
    serialization:
      fail-on-empty-beans: false
      
      
    
```
### 콘솔창에 테이블 생성 확인 

```c
create table Board (
       id integer not null auto_increment,
        content longtext,
        count integer default 0 not null,
        createDate datetime(6),
        title varchar(100) not null,
        userid integer,
        primary key (id)
    ) engine=InnoDB
Hibernate: 
    
    create table Reply (
       id integer not null auto_increment,
        content varchar(200) not null,
        cresteDate datetime(6),
        boardid integer,
        userid integer,
        primary key (id)
    ) engine=InnoDB
Hibernate: 
    
    create table User (
       id integer not null auto_increment,
        createDate datetime(6),
        email varchar(30) not null,
        password varchar(100) not null,
        role varchar(255) default 'user',
        username varchar(30) not null,
        primary key (id)
    ) engine=InnoDB
Hibernate: 
    
    alter table Board 
       add constraint FK7vj0ibv08qy57pbi6xjcmqlbg 
       foreign key (userid) 
       references User (id)
Hibernate: 
    
    alter table Reply 
       add constraint FKij3x9i33xc0ywol0qjwirktjt 
       foreign key (boardid) 
       references Board (id)
Hibernate: 
    
    alter table Reply 
       add constraint FKib3ncel5kkj8if1h2up5he43t 
       foreign key (userid) 
       references User (id)
```

### Model 팩토리

#### User 

```java
package com.example.demo.Model;

import java.sql.Time;
import java.sql.Timestamp;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.EnumType;
import javax.persistence.Enumerated;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;  

import org.hibernate.annotations.ColumnDefault;
import org.hibernate.annotations.CreationTimestamp;
import org.hibernate.annotations.DynamicInsert;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.CustomLog;
import lombok.Data;
import lombok.NoArgsConstructor;
//ORM->JAVA(다른언어)Object ->테이블 매핑해주는 기능

@Data
@NoArgsConstructor 파라미터가 없는 기본 생성자 생성
@AllArgsConstructor  모든필드 값을 파라미터로 받는 생성자를 만들어준다. 
@Builder //빌더 패턴 
@Entity // User 클래스가 통해서 읽어서, 자동으로 MySQL에 생성된다.
//@DynamicInsert insert 할때 null 필드를 제외시켜준다.
public class User {

	@Id // primary key
	@GeneratedValue(strategy=GenerationType.IDENTITY) // 프로젝트에서 연결된 DB의 넘버링 전략을 따라간다. 
	private int id; // 시퀀스, auto_increment
	
	@Column(nullable=false, length=30)
	private String username; // 아이디  
	
	@Column(nullable=false, length=60) //123456 => 해쉬(비밀번호 암호화)
	private String password;
	
	@Column(nullable=false, length=30)
	private String email;
	
  //@ColumnDefault("user")
  //DB 는 RoleType이 없다.
	@Enumerated(EnumType.STRING)
	private RoleType role; // Enum을 쓰는게 좋다. // admin  , user, manager  // 도메인이 정해 졌다. 
	
	@CreationTimestamp // 시간이 자동 입력
	private Timestamp createDate;  
	 
	
}
```

##### UserRepository

```java
package com.cos.blog.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.cos.blog.model.User;

//DAO
// 자동으로 bean 등록이 된다. 
//@Repository 생략 가능
public interface UserRepository extends JpaRepository<User, Integer>{

}
```
#### JpaRepository 

* UserRepository 인터페이스 생성 하여 JpaRepository를 상속받아 생성한다. 
* Spring JPA에서 Repository의 내부 구현체를 자동으로 생성시켜 주기 때문에 별도의 구현체를 따로 생성하지 않아도 된다.


|method|기능|
|------|----|
|save()|레코드 저장 (insert, update)|
|findOne()| primary key로 레코드 한건 찾기|
|findAll()|전체 레코드 불러오기. 정렬(sort), 페이징(pageable) 가능|
|count()|레코드 갯수|
| delete()|레코드 삭제|

* keyword

|메서드이름키워드|샘플|설명|
|---------------|----|----|
| And           |findByEmailAndUserId(String email, String userId)|여러필드를 and 로 검색|
| Or            |findByEmailOrUserId(String email, String userId)| 여러필드를 or 로 검색|
|Between        |findByCreatedAtBetween(Date fromDate, Date toDate)|필드의 두 값 사이에 있는 항목 검색|
| LessThan      |findByAgeGraterThanEqual(int age)|작은 항목 검색|
|GreaterThanEqual	|findByAgeGraterThanEqual(int age)| 크거나 같은 항목 검색|
|Like            | findByNameLike(String name)|like 검색|
|IsNull|findByJobIsNull()	|null 인 항목 검색|
| In| findByJob(String … jobs)|여러 값중에 하나인 항목 검색|
| OrderBy| findByEmailOrderByNameAsc(String email)| 검색 결과를 정렬하여 전달|

#### JPA 참조문서
<https://docs.spring.io/spring-data/jpa/docs/1.10.1.RELEASE/reference/html/>

#### Board
```java
package com.example.demo.Model;

import java.sql.Timestamp;
import java.util.List;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.Lob;
import javax.persistence.ManyToOne;
import javax.persistence.OneToMany;
import javax.persistence.Table;

import org.hibernate.annotations.ColumnDefault;
import org.hibernate.annotations.CreationTimestamp;

import lombok.AllArgsConstructor;
import lombok.Builder;
import lombok.Data;
import lombok.NoArgsConstructor;
@Data
@Table(name="board") 
@NoArgsConstructor
@AllArgsConstructor
@Builder
@Entity
public class Board {

	@Id
	@GeneratedValue(strategy=GenerationType.IDENTITY) // 프로젝트에서 연결된 DB의 넘버링 전략을 따라간다.
	private int id; // 시퀀스, auto_increment
	
	@Column(nullable=false,length=100)
	private String title;
	
	@Lob  // 대용량 데이터 
	private String content; //썸머노트 라이브러리<html>태그가 섞여서 디지인 됨
    
	@ColumnDefault("0")
	private int count; //조회수
	
	@ManyToOne (fetch=FetchType.EAGER)// Many=Board User=One
	@JoinColumn(name="userid")
	private User user;  //DB는 오브젝트를 저장할 수없다. FK 자바는 오브젝트를 저장할 수 없다.
	   
	@OneToMany(mappedBy="board",fetch=FetchType.EAGER)//mappedBy 연관관계의 주인이 아니다(난 FK가 아니에요) DB에 컬럼을 만들지 않는다.
	private List<Reply>reply; // 댓글이 여러개 이므로 List로 가져온다. 
	
	@CreationTimestamp
	private Timestamp createDate;
	
}
```
### Reply

```java
package com.example.demo.Model;

import java.sql.Timestamp;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;

import org.hibernate.annotations.CreationTimestamp;

import lombok.Data;

@Entity
public class Reply {

	@Id // primary key
	@GeneratedValue(strategy = GenerationType.IDENTITY) //
	private int id; // 시퀀스 ,auto_omcrement

	@Column(nullable = false, length = 200)
	private String content;
	
	@ManyToOne // 여러개의 답변은 하나의 게시물에 있다.
	@JoinColumn(name = "boardid") // 어떤 게시물인지 알기위해서 조인컬럼, FK키
	private Board board;
    
	@ManyToOne// 여러개의 답변에 한명이 적을수 있다. 
	@JoinColumn(name="userid") 
	private User user; // 답변을 적은 사람
	
	@CreationTimestamp
	private Timestamp createDate; 
}
```

### DummyController  

#### Insert 회원등록
* Insert 는 Post방식
* key=vule
* 어노테이션 의존성 주임 
  
```java
@Autowired //의존성 주임
	 private UserRepository userRepository;

//http://localhost:8000/blog/dummy/join(요청) 페이지
	//http의 body에 username password email 데이터를 가지고 요청
	@PostMapping("/dummy/join") 
	public String join(User user) {
	   System.out.println("id:"+user.getId());
	   System.out.println("username:"+user.getUsername());
	   System.out.println("passwrod:"+user.getPassword());
	   System.out.println("email:"+user.getEmail());
	   System.out.println("role:"+user.getRole());
	   System.out.println("create:"+user.getCreateDate());
	   
	   user.setRole(RoleType.USER);  // DTO 에 Role 타입 설정
	   userRepository.save(user);  
		return "회원가입이 완료되었습니다.";  
	}
```
<img width="735" alt="회1" src="https://user-images.githubusercontent.com/107549149/222860809-9f3c5fd1-053a-4649-82e5-66ba6ad12f59.png">

* Role 이 null 값이 들어간다. 
1. 방식 :User 테이블에 @DynamicInsert insert 할때 null 필드를 제외시켜준다.
2. 방식: DTO에 RoleType 클래스 생성 => enum 설정 한다. 

#### RoleType

* Insert 할떄 Role null 들어가는 것을 막기 위해 범위를 정한다. 

```java
package com.example.demo.Model;

public enum RoleType {
 USER,ADMIN
}
```
   
```java
id:0
username:test
passwrod:1234
email:test@naver.com
role:null
create:null
Hibernate: 
    insert 
    into
        User
        (createDate, email, password, username) 
    values
        (?, ?, ?, ?)
```

```java
id:0
username:sarr
passwrod:1234
email:sarr@naver.com
role:null
create:null
Hibernate: 
    insert 
    into
        User
        (createDate, email, password, role, username) 
    values
        (?, ?, ?, ?, ?)
```


<img width="700" alt="회원" src="https://user-images.githubusercontent.com/107549149/222860478-920569b3-649b-45dd-a703-b3e2c51cd3c7.png">


#### select 회원 검색

* 등록되지 않은 id 값을 데이터 베이스에서 못찾아오면, null값이 리턴 된다.=> 프로그램 문제 생긴다.
* optional로 너의 user객체를 감싸서 가져올테니 null이 아닌지 판단해서 return
* orElseThrow 를 사용하여 코드 가독성 향상 
* 해당 item이 없다면 예외, 있다면 return -> 가독성 향상
* 해당하지 하지 않는 id 값이 들어오면,.orElseGet()으로 들어오면서, User() 빈객체를 user에 넣어준다. 
* 이때 빈객체를 넣어주면 적어도 null이 아니게 된다. IllegalArgumentException 던져준다. 

```java
//{id}주소로 파라레터를 전달
	 //http://loclhost:8000/blog/dummy/user/1
	@GetMapping("/dummy/user/{id}")
	public User detail(@PathVariable int id) {
	    
		
		// 람다식
//		 User user=UserRepository.findById(id).orElseThrow(()->{
//			return new IllegalArgumentException("해당유저는 없습니다.id:"+id); IllegalArgumentException
//			});	
	
	 User user = userRepository.findById(id).orElseThrow(new Supplier<IllegalArgumentException>() {
		// UserRepository.findById(id).get();으로 하면, null 없으니 return 해라<문제가 없으니>
		 // DB에 해당하는 id 값을 select 할때 user값이 들어오는데 , 	
		 // 해당하지 하지 않는 id 값이 들어오면,.orElseGet()으로 들어오면서, User() 빈객체를 user에 넣어준다. 
		 // 이때 빈객체를 넣어주면 적어도 null이 아니게 된다.
		 // IllegalArgumentException 던져준다.
		 
		@Override
		public IllegalArgumentException get() {
			// TODO Auto-generated method stub
			return new IllegalArgumentException("해당 유저는 없습니다. ");
		}
	});
	 return user;
	}

```
* html 파일이 아니라 data를 리턴해주는 controller => @RestController
* 요청 : 웹브라우저 
* user 객체=> 자바 오브젝트 
*  변환(웹브라우저가 이해할수 있는 데이터 )-> json 사용한다. 
* 스부링부트 =MessageConverter라는 애가 응답시에 자동 작동
*  만약  자바 오브젝트를 리턴하게 되면 MessageConverter가 Jackson 라이브러리 호출
* user 오브젝트를 json 으로 변환해서 브라우저에 던져준다. 

<img width="767" alt="Json응답" src="https://user-images.githubusercontent.com/107549149/222299958-c24485cd-dbe5-4f68-a008-2c07cc957304.png">
### 회원 id  DB에 등록 확인 

<img width="741" alt="usr id3" src="https://user-images.githubusercontent.com/107549149/222299023-f4b84173-6e56-40c2-b8e7-b6fd2760fdd9.png">

#### 회원 id 가 DB에 없는 경우

<img width="508" alt="에러" src="https://user-images.githubusercontent.com/107549149/222862669-63f14a60-9672-4475-82c5-f5fb9be57b2f.png">


### 회원 리스트

```java
//http://loclhost:8000/blog/dummy/user
	@GetMapping("dummy/user")
	public List<User>list(){
		return userRepository.findAll(); // 전체를 리턴 
	}

```

<img width="371" alt="list" src="https://user-images.githubusercontent.com/107549149/222864269-8b36bcbd-5b60-41d6-924e-ea42d5b9a072.png">

### 회원 페이지

```java
@Transactional // 함수 종료시에  자동으로 commit 됨. 
	    @PutMapping("/dummy/user/{id}")   // json 경우는 requestBody 사용
	    public User updateUser(@PathVariable int id,@RequestBody User requestUser) { //json데이터를 요청=>JavaO()ject(MessageConverter의 Jackson라이브러리가 변환해서 받아준다.
	   	 System.out.println("id:"+id);
	   	 System.out.println("password:"+requestUser.getPassword()); //5678
	   	 System.out.println("email:"+requestUser.getEmail());             // love@gamil.com
	      
	   	  User user = userRepository.findById(id).orElseThrow(()->{ // 람다식 
	   		 return new  IllegalArgumentException("수정에 실패했습니다.");
	   	  });
	   	  user.setPassword(requestUser.getPassword());
	   	  user.setEmail(requestUser.getEmail());
	   	    
	   	 //requestUser.setId(id);
	   	// requestUser.setUsername("test");  
	   	  
	   	 //UserRepository.save(user);  
	   	  // 더티 체킹
	   	 return user;
	    }
```

<img width="383" alt="회원리스트" src="https://user-images.githubusercontent.com/107549149/223034373-57c07794-b76e-40d4-bf51-04649f78eb85.png">

<img width="401" alt="리스트2" src="https://user-images.githubusercontent.com/107549149/223034499-bd18cd87-8174-4348-9c81-055ff92a6496.png">

### Exception 페이지 만들기 (에러페이지) 

* id 값이 없는 경우 예외 처리

* handler 패키지 - GlobalExceptionHandeler 클래스 생성 

* @ControllerAdvice는 모든 @Controller 즉, 전역에서 발생할 수 있는 예외를 잡아 처리해주는 annotation이다.  

* 예외 컨트롤러 설정 => @ExceptionHandler(value=IllegalArgumentException.class)
Bean내에서 발생하는 예외를 잡아서 하나의 메서드에서 처리해주는 기능을 한다.

```java
package com.cos.blog.handler;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestController;
@ControllerAdvice 
@RestController
public class GlobalExceptionHandler {

	@ExceptionHandler(value=IllegalArgumentException.class)
	public String handleArgumentException(IllegalArgumentException e) {
		return"<h1>"+e.getMessage()+"</h1>";
	}
}
```

<img width="404" alt="id" src="https://user-images.githubusercontent.com/107549149/223035662-ac39ba44-5470-4fca-9848-a96d7a0fbfc7.png">

### 회원 수정 
* @Transactional // 함수 종료시에  자동으로 commit 됨. 
* json 경우는 requestBody 사용

```java

@Transactional // 함수 종료시에  자동으로 commit 됨. 
	    @PutMapping("/dummy/user/{id}")   // json 경우는 requestBody 사용
	    public User updateUser(@PathVariable int id,@RequestBody User requestUser) { //json데이터를 요청=>JavaO()ject(MessageConverter의 Jackson라이브러리가 변환해서 받아준다.
	   	 System.out.println("id:"+id);
	   	 System.out.println("password:"+requestUser.getPassword()); //5678
	   	 System.out.println("email:"+requestUser.getEmail());             // love@gamil.com
	      
	   	  User user = userRepository.findById(id).orElseThrow(()->{ // 람다식 
	   		 return new  IllegalArgumentException("수정에 실패했습니다.");
	   	  });
	   	  user.setPassword(requestUser.getPassword());
	   	  user.setEmail(requestUser.getEmail());
	   	    
	   	 //requestUser.setId(id);
	   	// requestUser.setUsername("test");  
	   	  
	   	 //UserRepository.save(user);  
	   	  // 더티 체킹
	   	 return user;
	    }
```

* 요청을 json으로 => 응답을 json 한다. 

<img width="750" alt="수정1" src="https://user-images.githubusercontent.com/107549149/222299200-ca5fe05d-4ee1-43f4-9527-e5d5a244fdfb.png">



### 회원삭제 

```java

 // 삭제 
    @DeleteMapping("/dummy/user/{id}")
    public String delete(@PathVariable int id) {
    	try {
    	userRepository.deleteById(id);
    	}catch (EmptyResultDataAccessException e) {  //  Exception 가능하다. 
			return"삭제에 실패하였습니다. 해당 id는 데이터 베이스에 없습니다.";
		}  	
    	return"삭제되었습니다.id:"+id;
    }
```


* 회원이 없을 경우  에러 표시 
<img width="744" alt="오류캐치" src="https://user-images.githubusercontent.com/107549149/222299657-a6166f91-bb1a-45ef-a224-8f6e001f7551.png">

* 회원 삭제 

<img width="460" alt="삭제" src="https://user-images.githubusercontent.com/107549149/222299882-841a7a9b-170b-4033-87e5-7886a8dec4df.png">





