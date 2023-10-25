---
layout: post
title: javascript 조건식
category: Frontend
tags: javaScript
---

# Javascript 조건식

### if 문

예문

```html

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>if문</title>
</head>
<body>
<script >
var min = prompt("당신의 하루 통화량은 몇 번 입니까","0");

if(min>=60){
	alert("많이 사용하는 편이네요")
}

</script>
</body>
</html>
````
2. if ~ else 문
예문

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>if~else문</title>
</head>
<body>

<script>

//  if ~ else 문

var  num= Number(prompt("당신이 좋아하는 숫자는?","0"));

if(num%2==0){
	alert("당신이 좋아하는 숫자 짝수")	;
}else{            
	alert("당신이 좋아하는 숫자 홀수");
}


</script>
</body>
</html>
````
3. if ~lese if ~ else 
예문 문제

`````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<script>

// if~ else if ~ else 문

// 키보드로 입력한 0~100 사이에 점수를 입력받아서 학점을 출력하는 프로그램 작성?
// A학점:90~100
// B학점:80~89
// C점:70~79
// D학점:60~69
// F학점:60점 미만

var s = prompt("0~100 사이의 점수 입력","");
if(s>=90){
	alert("A학점")
}else if(s>=80){
	alert("B학점")	
}else if(s>70){
	alert("C학점")
}else if(s>60){
	alert("D학점")
}else{
	alert("F학점")
}
</script>
</body>
</html>
``````
### switch ~ cese 문
1.예문
* location 객체는 window객체 하위 객체이다.  
  주로 페이지를 이동시켜준다. 
* location.href="http://www.oracle.com;" 

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>
var site = prompt("네이버,다음,네이트,구글중에서 즐겨 사용하는 사이트?","");

 switch(site){
 case "네이버":  url ="www.naver.com";
               break;
 case "다음" :  url ="www.daum.net";
               break;
 case "네이트":  url ="www.nate.com";
               break;
 case "구글" :  url ="www.google.com";
               break;
 default : alert("보기 중에 없는 사이트");
 
 
 }
 
 if(url){
	 location.href="http://" +url;

 }
                  
</script>
</body>
</html>
`````

2. 예문 문제  
키보드로 입력한 0~100 사이에 점수를 입력받아서   학점을 출력하는 프로그램 작성?  
 swith~ case 문 으로 작성 하기 ?  
 * parseInt("9.5")---> 9 
 A학점:90~100  
 B학점:80~89  
 C점:70~79  
 D학점:60~69   
 F학점:60점 미만  

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>

var s= Number(prompt("0~100점 사이의 입력하세요",""));
switch(parseInt(s/10)){
    case 10:
	case 9 : alert("A학점");
	          break;
	case 8 : alert("B학점");
	          break;
	case 7 : alert ("C학점");
	          break;
	case 60: alert ("D학점");
	         break;
	default  :alert("F학점") ;
             break

}

</script>
</body>
</html>
````
