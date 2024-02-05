---
layout: post
related_posts:
  - /frontdev-log/javascript/javascript-9/
title:  Javascript  배열
categories: 
  - frontdev-log
  - javascript
---

##  Javascript 배열 생성 

### 1. 방법 

> length: 배열의 크기를 구해주는 속성

```js

var a1 = new Array();
	a1[0] = 80;
	a1[1] = 90;
	a1[2] = 70;
	
	for(var i=0; i<a1.length; i++){
		document.write(a1[i]+"\t")
	}
	document.write("<br>");
```

### 2. 방법

```js
var a2 = new Array(80, 90, 70);
	
	for(var i=0; i<a2.length; i++){
		document.write(a2[i]+"\t")
	}
	document.write("<br>");
```

### 3. 방법

```js

var a3 = [80, 90, 70];

for(var i=0; i<a3.length; i++){
		document.write(a3[i]+"\t");
	}

```
## 배열의 메소드

### 1. join(): 문자('-')를 기준으로 배열 값들을 하나의 문자 데이터로 결합

```js

 var num =['사당','교대','방배','강남'];
  document.write(num+"<br>");                        // 사당,교대,방배,강남
  document.write(typeof(num)+"<br>");  

  document.write(num.join('-')+"<br>");             //사당-교대-방배-강남
  document.write(typeof(num.join('-'))+"<br>");     // string
```

### 2. reverse(): 배열 원소 값들의 순서를 반대로 바꾸어 주는 역할

```js

var num =['사당','교대','방배','강남'];
document.write(num.reverse()+"<br>");             //강남,방배,교대,사당
  document.write(typeof(num.reverse())+"<br>");     // object 배열 객체 
  document.write(num+"<br>");                       //사당,교대,방배,강남 (바뀌지 않음 )
```

### 3. sort(): 배열 원소 값들을 오림 차순으로 정렬(문자: 사전순)
       > sort() 함수로 오름 차순 정렬 시키면, 원본 배열이 바뀜

```js
var num =['사당','교대','방배','강남'];

document.write(num.sort()+"<br>");               //강남,교대,방배,사당
document.write(typeof(num.sort)+"<br>");         //function
document.write(num+"<br>");                      //강남,교대,방배,사당  (원본값이 바뀜: 사전순)
```

### 4.  배열 원소 값들을 내림차순 정렬시켜보자.(문자: 사전 역순 정렬)

```js

 document.write(num.sort().reverse()+"<br>");    // 사당,교대,방배,강남

```

## 배열 메소드2 

### 1. slice(start index , end index): start ~ ena-1번 원소를 추출

```js

var greenLine=['사당','교대','방배','강남'];
var yellowLine=['미금','정자','모란','수서'];

document.write(greenLine.slice(1,3)+"<br>"); // 교대,방배
```

### 2.concat(): 2개의 배열 객체를 하나로 결합 

```js

document.write(greenLine.concat(yellowLine)+"<br>"); // 사당,교대,방배,강남,미금,정자,모란,수서
```

### 3. pop(): 배열 데이터 중에서 마지막 index에 저장된 데이터를 삭제 해준다.

```js

greenLine.pop();                   // '강남'삭제
document.write(greenLine+"<br>");  // 사당,교대,방배

```

### 4. push(): 배열 객체에 마지막 index에 새로운 데이터를 삽입

```js

greenLine.push('삼성');             // '삼성' 삽입
document.write(greenLine+"<br>");  // 사당,교대,방배,삼성

```

### 5.shift(): 배열 데이터 중에서 첫번째 데이터를 삭제 

```js

greenLine.shift();                 // '사당'삭제
document.write(greenLine+"<br>");  //교대,방배,삼성

```

### 6. unshift(): 배열의 첫번째 index에 새로운 데이터 삽입

```js

greenLine.unshift('신도림');         //'신도림'삽입
document.write(greenLine+"<br>");  //신도림,교대,방배,삼성
```



