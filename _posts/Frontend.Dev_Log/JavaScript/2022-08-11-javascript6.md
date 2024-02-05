---
layout: post
related_posts:
  - /frontdev-log/javascript/javascript-7/
title:  Javascript 내장 함수
categories: 
  - frontdev-log
  - javascript
---
#
  Javascript 내장 함수 종류

  |  함수   |형식   |   
  |---------|-------|
  |alert()  |       |
  |confirm()|       |
  |prompt() |       |
  |parseInt()|parseInt("3.14") -->  3|
  |parseFloat()| parseFloat("10.5") -->  10.5 |
  |Number()| Number("42.195") -->  42.195  |
  |String()|String(1000) --> "1000"|
  | eval()|eval("10+20") -->  30 |
  | isNaN()| Not a Number 문자가 포함되면 true를 반환함.|


### 내장 함수 예문

`````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>내장 함수</title>
</head>
<body>
<script>

1. parseInt ("3.14")-->3
document.write("문자형 데이터를 정수형 데이터로 변환:"+parseInt("3.14")+"<br>");

2. parseFloat("3.14")-->3.14
document.write("문자형 데이터를 실수형 데이터로 변환:"
		+(parseFloat("3.14")+10)+"<br>");
 3. Number("3.14")-->3.14 (정수,실수 상관없다.)
document.write("문자형 데이터를 숫자형 데이터로 변환:"
		       +Number("3.14")+"<br>");
document.write(typeof("3.14")+"<br>");		   // String      
document.write(typeof(Number("3.14"))+"<br>"); // number	 	       

 4.String(1000)---->"1000"
document.write("숫자형 데이터를 문자형 데이터로 변환:"
		       +String(1000)+"<br>");
document.write(typeof((1000))+"<br>");		 //number       
document.write(typeof(String(1000))+"<br>"); // String

 5. eval("10+20") -->30
document.write("문자형 산술식을 연산 수행:"+eval("10+20")+"<br>"); //30

 6. isNaN(Not a  Number); 숫자가 아니면(문자가 포함되면)true값을 리턴한다.
 // isNaN("10+20") -->true
 //  isNaN("10") -->false
document.write("문자가 포함되면 true 값을 리턴:"+isNaN("10+20")+"<br>")	; // +가 문자로 인식	       
document.write("문자가 포함되면 true 값을 리턴:"+isNaN("10")+"<br>");	 // " "	안에 있어도 숫자로 인식       
document.write("문자가 포함되면 true 값을 리턴:"+isNaN(10)+"<br>");		       
		
</script>
</body>
</html>
``````
