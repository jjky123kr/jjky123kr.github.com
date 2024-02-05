---
layout: post
related_posts:
  - /frontdev-log/javascript/javascript-5/
title:  Javascript 반복문
categories: 
  - frontdev-log
  - javascript
---

# Javascript 반복문

## for문 형식

````html
for(초기값; 조건식; 증감식){
  반복 실행 
}
`````
###  1 예문 문제

> 사랑해요 메세지 출력10번

`````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>
 for(var i=1; i<=10; i++){
	 document.write(i+" 사랑해요.<br>")
 }
</script>
</body>
</html>
``````
### 2 예문 문제
> 1~10까지의 합을 구하는 프로그램 작성
````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>
  var  sum=0;
  for(var i=1; i<=10; i++){
 sum+=i;      
   }
document.write("1~10의 합"+sum);
</script>
</body>
</html>
```````
### 3. 예문문제
> 1~100까지 홀수짝수의 합을 구하는 프로그램 작성

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>

// 1~100 까지 홀수 짝수의 합을 구하는 프로그램 작성하세요?

		var odd =0 ,even=0;
		for(var i=1; i<=100; i++){
			if(i%2==1){    //홀수
				odd+=i
			}else{        // 짝수
				even+=i;
			}
		}
      document.write("1~100의 홀수의 합:"+odd+"<br>");
      document.write("1~100의 짝수의 합:"+even+"<br>");
      

</script>
</body>
</html>
````
## while 문

###  1. 예문 문제
> 사랑해요 메세지 10번 출력

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>

var i=1;   // 초기값 설정

while(i<=10){
	document.write(i+"사랑해요.<br>");
	i++;          //증감식 i++, ++i, i+=1, i=i+1
}

</script>
</body>
</html>
``````
###  2.예문 

> 1~100 까지의 홀수, 짝수의 합을 구하는 프로그램 작성?

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>

//1~100까지 홀수 ,짝수 의 합을 구하는 프로그램 작성

var i=1, odd=0; even=0;

while(i<=100){
	if(i%2==1){
	 odd+=i;	
	}else{
	even+=i;
	} 
    i++;
}
document.write("1~100까지 홀수의 합"+odd+"<br>");
document.write("1~100 까지 짝수의 합"+even+"<br>");

</script>

</body>
</html>
``````
### 3.예문 문제
> 키보드로 원하는 단을 입력 받아서 구구단1개를 출력하는 프로그램 작성?

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>	
var i=1;  //초기값
var dan = prompt("원하는 단을 입력하세요","");
while(i<=9){        // 조건식
	document.write(dan+"*"+i+"="+dan*i+"<br>")
    i++;
}

</script>
</body>
</html>
``````
###  4.예문 문제
> while 문  이용해서 구구단2~9단 출력하는 프로그램 작성

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>	
    var dan=2;
    while(dan<=9){
    	document.write("["+dan+"단]")
    	var i=1;
    	while(i<=9){
    		document.write(dan+"*"+i+"="+dan*i+"<br>");
    		i++
    	}
       dan++;
       document.write("<br>");
    }
    
</script>
</body>
</html>
``````
### do~ while 문 
예문 문제
사랑새요 메세지 10 번 출력
````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>
//do~while 문  
// '사랑해요' 메세지 10번 출력

var i=1;   //초기값

do{
	document.write(i+"사랑해요.<br>")
	i++           //증감식
}while(i<=10);   //조건식
</script>
</body>
</html>
`````
### 2.예문 문제
> 키보드로 수식(10+20)을 출력하는 프로그램 작성하세요?

1. 함수()우선순위 로 출력이 된다.  
 document.write 보다 prompt가 우선순위 이라서 루트가 계속 돌아간다.  
 2. eval(): 문자형식의 수식을 연산해주는 함수   
 eval("10+20")-->30

`````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<script>
do{
	//s="10+20"
	
	s = prompt("수식을 입력하세요? (종료하려면 0을 입력하세요)","");
	if(s==0) break;
		
	// eval():문자형식의 수식을 연산 해주는 함수
	// eval("10+20") -->30
	document.write(s+"의 결과는"+ eval(s)+"<br>")
	
	
}while(true); 무한루프   // 조건식 

</script>
</body>
</html>

`````

