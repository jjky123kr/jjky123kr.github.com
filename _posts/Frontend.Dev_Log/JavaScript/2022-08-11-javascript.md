---
layout: post
related_posts:
  - /frontdev-log/javascript/javascript-1/
title: Javascript 개념 과 특징
categories: 
  - frontdev-log
  - javascript
---

# Javascript 개념 과 특징

## Javascript  개념 
1. 자바언어의 특성을 지닌 스크립트언어 이다.  
2. 브라우저 에서 해석되어지는 프로그램언어이다.  
* 특징
자바객체지향의 특성이 있는 스크립트 언어로서 프로그램 코드가 
HTML 문서 사이에 직접 기입하게 된다.

## 자바스크립트와 자바의 차이점

### =>  자바스크립트는 브라우저에서 프로그램 코드가 해석된다.

1. 자바는 프로그램을 만든 후 반드시 컴파일러로 컴파일한 결과를 브라우저에  
삽입하기 때문에 미리 컴파일러로 코드를 해석하게 된다.
2. 자바스크립트는 자바처럼 타입 체크를 철저하게 하지 않는다.  
  자바에서 에러가 발생하면, 에러 메세지가 발생 하지만,    
  자바스크립트에서는 에러가 발생하면 메세지를 발생 하지 않는다.  
  > **_자바스크립트에서 에러를 찾을 때는 웹페이지 f12 개발자 도구에서   콘솔창에서 확인_** 

###  javascript 출력 유형

`````html
1. <script></script>
2. <script type="text/javascript"></script>
3. <script langage="javascript"></script>
``````
### jacascript 주석
1. 단일 주석 : //  
2. 다중행 주석: /*  ~  */   
3. 주석 단축키: ctrl+shift+/  
4. 주석해제는 잘 적용이 되지않는다.   

###  함수를 정의를 할때는 head 태그 안에서 script 적용  
> 그 예외는 body 태그 안에서 사용이 가능하다.  

*  자바스크립트 내장객체이다  . 
* 자바스크립트는 소문자 대문자 구분을 한다.  
* document  브라우저 내장 객체를 사용한다. 
-> write 함수가 문서를 출력한다. 
## Java Script 내장 객체 







```html
* 줄 바꿈 : " <br> " , ' <br> ' 안에 사용해야 한다. 
````  
`````html
 > 밖에 <br> 쓰면, 에러가 난다.   
``````
````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
 <script>

 document.write("처음작성하는 자바스크립트1 <br>");
 document.write('처음작성하는 자바스크립트2'+"<br>")
 document.write("처음작성하는 자바스크립트3"+"<br>")
</script>

<script type="text/javascript">
document.write("처음작성하는 자바스크립트4 <br>");

</script>

<script langage="javascript">

document.write("처음작성하는 자바스크립트5 <br>");
console.log('콘솔에 출력됨');
</script>

</body>
</html>
```````
### propmt ("메세지?","초기값");
초기값을 쓰지 않을때 사용자가 키보드로 입력한 결과

* 예문 

````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>prompt</title>
</head>
<body>
<script>
// prompt ("메세지?","초기값");
// 초기값을 쓰지 않을때  사용자가 키보드로 입력한 결과가 name 
var name=prompt("이름을 입력하세요?","      ");
document.write("당신의 이름은"+ name+"<br>");
document.write(typeof(name)+"<br>"); // name이 무슨 변수인지 알려주는 함수 typeof()
</script>
<script >

var age=prompt("나이를 입력하세요","");     //age="25"
alert(typeof(age));                   // string
if(age>=20){
	alert("당신은 성인 입니다.");   
}else{
	alert("당신은 미성년자 입니다.")
}
</script>

</body>
</html>
```````








