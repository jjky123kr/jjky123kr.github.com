---
layout: post
title: javascript 비교, 조건 ,논리 연산자
category: Frontend
tags: javaScript
---

# Javascript 연산자3

### 비교 연산자 :  >, >=, < , <=, ==, !=

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>비교 연산자</title>
</head>
<body>
<script>
   	 
   var a =10;
   var b =20;
   var c =10;
   var f ="20";
   var result;
   
   result = a > b;
   document.write("a > b="+result+"<br>");   //falses
   
   result = a < b;   
   document.write("a < b="+result+"<br>");   //true
   console.log("a < b="+result+"<br>");
   
   result= b == f;
   document.write("b == f" +result+"<br>");
   
   result = a!=b;
   document.write("a ! = b"+result+"<br>");
   
   result = b!=f;
   document.write(" b ! = f"+result+"<br>");
</script>
</body>
</html>
``````
### 문제 

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>

// 키보드로 정수2개 입력 받아서 최대값과 최소값을 구하는 프로그램을 작성하세요

var n1 = prompt("정수1을 입력하세요?","");
var n2 = prompt("정수2을 입력하세요?","");

document.write(typeof(n1)+"<br>");  // string 
document.write(typeof(n2)+"<br>");  // string

var max, min;

// Number() : 문자를 숫자로 변환 해주는 함수
var n1= Number(n1);       //Number("30")-->30
var n2= Number(n2);       //Number("3.14") -->3.14


if(n1>n2){           //문자형 변수끼리 비교연산을 할 수 있다. 
	max = n1;
	min = n2;
}else{
	max = n2;
	min = n1;
}
 document.write("max="+ max +"<br>");
 document.write("min="+ min +"<br>");
</script>
</body>
</html>
`````
### 조건 연산자
* 변수 =(조건식) ?  값1: 값2;
  조건식이 참 이면 값1 리턴 , 조건식이 거짓 이면 값2 리턴
````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title> 조건 연산자 </title>
</head>
<body>
<script>

   
//키보드로 정수2개 입력 받아서 최대 값과 최솟값 구하기
// 조건 연산자를 이용하여 처리 하는 프로그램 작성?

var n1 = prompt("정수1 입력하세요","");
var n2 = prompt("정수2 입력하세요","");


var max= (n1>n2) ? n1 :n2;
var min= (n1<n2) ? n1 :n2;

document.write("max="+max+"<br>");
document.write("min="+min+"<br>");

</script>
</body>
</html>
`````
### 논리 연산자 : 논리 연산자 : && , || , ! 

* 문제 
 5과목 점수를 입력 받아서 합격 불합격을 판별하는 
 프로그램 작성?  
 단 과목당 과락은 40 점 이고 평균 60 점 이상   받아야 합격 합니다.   

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>논리 연산자</title>
</head>
<body>

<script>


var n1=prompt("과목1 점수 입력","");    //string
var n2=prompt("과목2 점수 입력","");
var n3=prompt("과목3 점수 입력","");
var n4=prompt("과목4 점수 입력","");
var n5=prompt("과목5 점수 입력","");

//var total= n1 + n2 + n3+ n4+ n5; 합이 구해지지 않는다. 
//Number(): 문자를 숫자로 변환햊는 함수
// var total= Number(n1)+Number(n2)+ Number(n3) +Number(n4) + Number(n5);
  
  
  var total= Number(n1)+Number(n2)+ Number(n3) +Number(n4) + Number(n5);
  console.log("total:"+total);
  console.log("avg:" +avg);
  var avg= total/5;
   
if( n1>=40 && n2>=40 && n3>=40 && n4>=40 && n5>=40 && avg>=60){
	alert("합격");
}else{
	alert("불합격");
}
</script>

</body>
</html>
``````