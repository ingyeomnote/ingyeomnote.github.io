---
title:  "JavaScript - 기본 문법"
excerpt: "JavaScript 기초"

categories:
  - JavaScript
tags:
  - JavaScript
  - 프로그래밍
last_modified_at: 2021-03-14T23:34:00
---



# JavaSript

07

해당 단계에서는 자바스크립트에서 기본적으로 제공하는 객체 중 하나 NUMBER에 대해서 알아보았고,

NaN과 Infinity 의 예외도 알아보았다.



NUMBER 객체를 확인하는 방법

WorkSpace -> Java Resources 폴더 - Libraries -> Apache Tomcat v9.0 (JRE System은 자바할때) ->

08 형변환

	<h1>형변환</h1>
	
	<h2>1. 숫자형 형 변환 : number()</h2>
	<input type="text" id="num"/> <- 이곳에 넣는 값은 기본적으로 문자열
	<button onclick="numTest(num.value);">클릭!</button>
var val = Number(val) + 100; 숫자변환

문자열로 받아오는 숫자형 문자를 숫자로 변환

var val = parseInt(val) + 100; 정수변환

문자열로 받은 실수형을 실수형이 아닌 숫자형으로 변환

	<h2>3. 실수형 형 변환 : parseFloat()</h2>
	<input type="text" id="float"/>
	<button onclick="floatTest(float.value);">클릭!</button>
실수형 형 변환 -> number로 나오지만 소수점까지 잘 나옴



검증함수(EVAL함수는 보안 취약)

	<h2>검증함수 : eval()</h2>
	<p>문자열로 된 코드(수식)을 자바스크립트로 읽어들여서 실행</p>
	<input type="text" name="evalue" placeholder="예) 5 + 10"/>
	<button onclick="evalTest();">클릭!</button>
	<span></span>
	function evalTest(){
		var val = document.getElementsByName("evalue")[0];
		if(confirm("작성하신 코드가 맞나요? : " + val.value)){
			document.getElementsByName("span")[0].innerHTML = "<b>계산결과:"+eval(val.value)+"</b>";
		}
	}
---------------------------------------

	<input type="number" class="mycalc" />
	<input type="text" list="cal" class="mycalc"/>
	<datalist id="cal">
		<option value="+"/>
		<option value="-"/>
		<option value="*"/>
		<option value="/"/>
	</datalist>
	<input type="number" class="mycalc"/>
	<button onclick="calc();">계산</button>
	<span></span>

<script type="text/javascript">

	function calc(){
		var calc = document.querySelectorAll(".mycalc");
		var res = "";
		for(var i = 0; i < calc.length; i++){
			res += calc[i].value;
		}
		
		document.getElementsByTagName("span")[1].innerHTML = eval(res);
	}
class로 했을 때

--------

	<input type="number" id="calc1" />
	<input type="text" list="cal" id="calc2"/>
	<datalist id="cal">
		<option value="+"/>
		<option value="-"/>
		<option value="*"/>
		<option value="/"/>
	</datalist>
	<input type="number" id="calc3"/>
	<button onclick="calc();">계산</button>
	<span></span>

<script type="text/javascript">

	function calc(){
		var num01 = document.getElementById("calc1");
		var opr = document.getElementById("calc2");
		var num02 = document.getElementById("calc3");
		
		var res = num01.value + opr.value + num02.value;
		
		document.getElementsByTagName("span")[1].innerHTML = eval(res);
	}
id로 했을 때

-----

9. 날짜

date 객체

___

```html
<html>
<head> 
   (1)
</head>
    (2)
    <body>
        (3)
    </body>
</html>
```

javascript를 생성할 수 있는 곳은 (1), (2), (3) 모두이다.

나이 많은 사람들은 javascript를 body의 제일 마지막에 작성하라고 한다.(3)

w3schols -> javascript date object

getElementById("?") -> Node

querySelecotor("?")  -Node

ㄴcss선택자를 넣어야한다.

querySelectorAll("?") -> NodeList

___

11

	function strTest04(){
		// indexOf() : 왼쪽 -> 오른쪽으로 검색(첫번째 인덱스 반환)
		// lastIndexOf() : 오른쪽 -> 왼쪽으로 검색(첫번째 인덱스 반환)
		// 만약, 검색하는 값이 없으면 -1 반환
		
		var str = "홍길동 이순신 유재석 강호동 홍길동 신동엽";
		var prop = prompt("검색할 이름을 입력해주세요");
		alert("indexOf : " + str.indexOf(prop));
		// index는 기본적으로 앞에서부터 찾지만,
		// indexOf는 0번지부터 해당 이름을 찾고 다시 앞에서부터 번지수를 세지만
		// lastIndexOf는 마지막 번지부터 해당 이름을 찾고 다시 앞에서부터 번지수를 센다.
		alert("lastIndexOf : " + str.lastIndexOf(prop));

---

