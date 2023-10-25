---
layout: post
title:  Spring boot JPA , ORM 개념
category: Backend_Dev.Log
tags: Springboot
---

# JPA 와 ORM

# ORM(Object-Relational Mapping)
* 애플리케이션 Class 와 RDB(Relational DataBase)의 테이블을 매핑(연결)한다.
* 어플리케이션의 객체를 RDB테이블에 자동으로 영속화 해주는것 이다. 

## 장점 
* SQL문이 아닌 Method를 통해 DB를 조작할 수 있어,       
개발자는 객체 모델을 이용하여 비즈니스 로직을 구성하는데만 집중할 수 있음.    
(내부적으로는 쿼리를 생성하여 DB를 조작함. 
하지만 개발자가 이를 신경 쓰지 않아도됨)

* Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어들어,   
각종 객체에 대한 코드를 별도로 작성하여 코드의 가독성을 높임

* 객체지향적인 코드 작성이 가능하다.  
 오직 객체지향적 접근만 고려하면 되기때문에 생산성 증가  

* 매핑하는 정보가 Class로 명시 되었기 때문에 ERD를 보는 의존도를 낮출 수 있고  
 유지보수 및 리팩토링에 유리하다.

* 기존 방식에서 MySQL 데이터베이스를 사용하다가 PostgreSQL로 변환한다고 가정해보면,  
 새로 쿼리를 짜야하는 경우가 생김.   
 이런 경우에 ORM을 사용한다면 쿼리를 수정할 필요가 없다.

## 단점
 * 프로젝트의 규모가 크고 복잡하여 설계가 잘못된 경우,   
 속도 저하 및 일관성을 무너뜨리는 문제점이 생길 수 있음  

 * 복잡하고 무거운 Query는 속도를 위해   
   별도의 튜닝이 필요하기 때문에 결국 SQL문을 써야할 수도 있음  

 * 학습비용이 비쌈  

# JPA(Java Persistence API)
* 객체와 관계형 데이터베이스를 매핑
* JAVA 에서 ORM(Object-Relational Mapping) 기술 표준으로 사용되는 <span style="background-color:#fffd91; color:#000">인터페이스 모음</span>  
* 실제적으로 구현된것이 아니라 구현된 클래스 와 매핑을 해주기 위해 사용되는 프레임워크이다. 
*  <span style="background-color:#fffd91; color:#000">Hibernate: JAP 구현한 대표적인 오픈소스</span>

### 왜 JPA 사용할까? 
* JAP는 반복적인  CRUD SQL을 처리해준다.
* JPA는 매핑된 관계를 이용해서 SQL을 생성하고 실행하는데 있어서, 
개발자는 어떤 SQL이 실행될지 생각하면 되고, 예측도 쉽게 할수 있다. 
* JPA는 네이티브 SQL이란 기능을 제공해주는데   
관계 매핑이 어렵거나 성능에 대한 이슈가 우려되는 경우 SQL을 직접 작성하여 사용할 수 있다.

### JAP 장점

* JPA를 사용하여 얻을 수 있는 가장 큰 것은 SQL아닌 객체 중심으로 개발할 수 있다는 것이다. 
* 생산성이 좋아지고 유지보수도 수월하다
* 부모클래스와 자식클래스의 관계 즉, 상속관계가 존재하는데 데이터베이스에서는 이러한 객체의 상속관계를 지원하지 않는다.


###  JDBC경우 

* 자바에서는 Board 테이블을 select 할떄 작성자가 User 테이블에 있는 ID 이며, 
  조인을 하여서 DB에서 정보를 가져와야 한다. 
> 부모클래스와 자식클래스의 관계 즉, 상속관계가 존재하는데 
  데이터베이스에서는 이러한 객체의 상속관계를 지원하지 않는다

### JAP 의 연관 관계 

<img width="637" alt="연관 관계" src="https://user-images.githubusercontent.com/107549149/221579917-b2e882ed-ed5b-45d2-b668-55245f19e9fa.png">

* Board select(검색)하면 User 정보를 가져온다.    
  그이유:  Board 테이블에  User Object 가 있어서 이다.


![image](https://user-images.githubusercontent.com/107549149/221576676-098d3d23-6491-40e1-8b75-2eb449df2082.png)
출처:<https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/>





