---
layout: single
title: Javascript 확장 대입 연산자 , 증감 연산자
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
### 확장대입 연산자 : +=, -=, *=  /=, %=  

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>확장 대입 연산자</title>
</head>
<body>
<script >  
    	var num1=10;
    	var num2=3;
   num1+= num2;  // num1= num1 + num2;
   document.write("num1="+num1+"<br>");      //13
   num1-= num2;  // num1= num1 - num2;
   document.write("num1="+num1+"<br>");      //10
   num1*= num2;  // num1= num1 * num2;
   document.write("num1="+num1+"<br>");      //30
   num1/= num2;  // num1= num1 / num2;
   document.write("num1="+num1+"<br>");      //10
   num1%= num2;   // num1= num1 % num2;
   document.write("num1="+num1+"<br>");      // 1


</script>
</body>
</html>
`````
### 증감 연산자

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>중감 연산자</title>
</head>
<body>
<script>
   //증감 연산자  : ++ , -- 
   var num1=10;
   var num2=20;
   num1--;              //후행연산
   document.write("num1="+num1+"<br>");    // 9
   num1++;              //후행연산
   document.write("num1="+num1+"<br>");    //10
   result= num2++;     //후행연산
   document.write("num2="+num1+"<br>");    //21
   document.write("result="+num1+"<br>");  // 20
   
   result= ++num2;     //선행 연산
   document.write("num2="+num2+"<br>")    //22
   document.write("result="+num2+"<br>")
   

</script>
</body>
</html>
```````