---
layout: single
title: javascript 함수
folder: "Javascript"
categories: Javascript 
tags: [blog,Javascript ]

author_profile: true
sidebar:
  nav: "main"

toc: ture
toc_alble: 목록
toc_icon: "bars"
toc_sticky: true
---
### 함수의 정의

```html

function 함수 이름(매개 변수1. 매개변수2,.....){
        수행할 문장
}
````
### 함수 호출 방법 1.
* head 태그 안에 사용자 정의 함수 
* 함수명은 대.소문자 구분이 없다.
*  함수 호출은 body태그 안에서 호출한다.

### 함수 호출 방법2.

이벤트를 이용해서 함수 호출  (주로 많이 사용)  
벤트명에다 on을 붙여서 이벤트 핸들러를 생성  
이벤트 핸드러(클릭) : onClick  
전송기능이 없는 것으로 할때 click 이벤트 발생   
on: 소문자,이벤트명:첫글자 대문자 Click  


### 매개변수가 없는 함수의 정의

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>사용자 정의 함수</title>
<script > 
  function check(){   
	alert("함수 호출 성공");
}
</script>
</head>
<body>
<!--방법1. 함수 호출 -->

<script>
check();   //body 태그 안에서 함수 호출  
</script>
// 방법2. 함수 호출 
// 이벤트를 이용해서 호출
<input type="button" value="함수 호출 " onClick="check()">
</body>
</html>
```
### 매개변수가 있는 함수의 정의
// 매개 변수 가 있는 함수 정의  
// 값을 돌려줄때 return
예문 
````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>매개 변수 가 있는 함수 정의</title>
<script>
function ask(question){
	var result=confirm(question); 
	if(result){       //'확인' 버튼 클릭 
		return "찬성";
	}else{             /'취소' 버튼 클릭  
		return "반대";          
	}
}
</script>
</head>
<body>
<script>

var result=ask("찬성하면 확인 버튼, 반대하면 취소 버튼"); //함수 호출
 alert(result);
</script>
</body>
</html>
````
### 매개변수가 2개 일때

* 예문 문제: 최대값 과 최솟값 구하는 함수

````html

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
// 매개변수가 2개 일때 
// 최대값과 최솟값 구하는 함수
function max(a,b){  //최대값
	if(a>b)
		return a;
	else 
		return b;
}
function min(a,b){  //최솟값
	
	if(a<b)
		return a;
	else
		return b;
}
</script>
</head>
<body>
<script>

a= prompt("정수1을 입력하세요?","");
b= prompt("정수2을 입력하세요?","");

var max= max(a,b);   // max()
var min= min(a,b);   // min()
document.write("max:"+max+"<br>");
document.write("min:"+min+"<br>");
</script>

</body>
</html>
``````
### 내장 함수  사용자의 정의 

예문 1.

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	// Number("50")  --> 50
	var n1 = Number(prompt("정수1을 입력하세요?",""));
	var n2 = Number(prompt("정수2를 입력하세요?",""));
	
	function plus(){
		alert(n1+n2);
	}
	function minus(){
		alert(n1-n2);
	}
	function mul(){
		alert(n1*n2);
	}
	function div(){
		alert(n1/n2);
	}
</script>
</head>
<body>

<form>
	<input type="button" value="더하기" onClick="plus()">
	<input type="button" value="빼기" onClick="minus()">
	<input type="button" value="곱하기" onClick="mul()">
	<input type="button" value="나누기" onClick="div()">
</form>

</body>
</html>
``````
예문2. 

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
	// Number("50")  --> 50
	var n1 = Number(prompt("정수1을 입력하세요?",""));
	var n2 = Number(prompt("정수2를 입력하세요?",""));
	
	function plus(){
		alert(n1+n2);
	}
	function minus(){
		alert(n1-n2);
	}
	function mul(){
		alert(n1*n2);
	}
	function div(){
		alert(n1/n2);
	}
</script>
</head>
<body>

<form>
	<input type="button" value="더하기" onClick="plus()">
	<input type="button" value="빼기" onClick="minus()">
	<input type="button" value="곱하기" onClick="mul()">
	<input type="button" value="나누기" onClick="div()">
</form>

</body>
</html>
``````