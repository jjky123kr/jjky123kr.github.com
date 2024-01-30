---
layout: single
title: 재귀함수
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
### 개념
함수 안에서 자기 자신의 함수를 호출하는 함수

예문 

``````html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script>
  function f(n){
	  if(n>1){
		  var result =n* f(n-1); // n!= n*f(n-1)*f(n-2)*f(n-3).....1
		                         // 3!=3*(3-1)*(3-2)*(3-3)..1
		  return result;
     }else{
	     return n;
    }
}

</script>
</head>
<body>
<script >

var n=prompt("팩토리얼을 구할 정수를 입력하세요?","");
document.write(n+"!="+f(n));
</script>

</body>
</html>
``````