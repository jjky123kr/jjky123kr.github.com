---
layout: post
title: javascript 산술연산자
category: Frontend
tags: javaScript
---

# Javascript 연산자1

### 산술 연산자 :  +, -, /, %(나머지)

* var 변수 선언
* 1번 예제
````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>산술 연산자</title>
</head>
<body>
<script>
var num1=15;
var num2=2;
var result;

result= num1 + num2;     // 더하기
document.write("num1+num2=" +result+"<br>");
                       
result= num1 - num2;     // 빼기
document.write("num1-num2=" +result+"<br>");

result= num1 * num2;    // 곱하기
document.write("num1*num2=" +result+"<br>");

result= num1 / num2;    // 나누기    
document.write("num1/num2=" +result+"<br>"); //7.5
 
result= num1 % num2;    // 나머지
document.write("num1% num2=" +result+"<br>");
</script>
</body>
</html>
``````
* 2.예제  : 산술 연산자

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>

var t1 ="학교종이"
var t2 ="땡땡땡"
var t3 ="8282"
var t4 ="어서 모이지"
var reslult;

result = t1+t2 +t3 +t4;
document.write("result="+result);

</script>
</body>
</html>
``````







