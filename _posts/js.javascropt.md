# JS : Java Script

JavaScript는 기본적으로, 웹 페이지를 만들 때 사용한다.

예를 들어, html과 css로 화면을 디자인하고, 디자인된 화면에서 실질적으로 기능을 부여할 때 사용하는 게 js이다.

**react.js, vue.js, ECMAScript, ES6~9 문법**

**W3C : 웹 표준** 

JavaScript 선언 방식에는 대표적으로 3가지 방법이 있다.

**첫 번째,** inline 방식으로 html `<body>` 부분에서 함수를 호출하지 않고, 바로 명령을 수행하게 하는 방법이다. 

예로 아래와 같다.

```html
<p onclick="alert('inline!');">inline</p>
```

alert은 javascript에서 window 객체가 기본적으로 제공해주는 함수이다.

여기서 window 객체란, 클라이언트 측 자바 스크립트 프로그램의 전역 변수 객체이다.

```javascript
var global = {data: 0};
alert(global === window.global); // true를 반환한다.
```

또한, 우리가 선언한 전역변수도 모두 window 객체 안에 등록되어 window.global과 그냥 global 변수의 값이 동일하다는 것을 알 수 있다.

**두 번째,** 내부 작성 방식이다. 외부에서 파일을 추가하거나 사용하지 않고, 내부에서만 작성하고 처리한다.

```html
<p onclick="embeddedTest();">내부작성방식</p>
```

```html
<script type="text/javascript">
    // 내부 작성 방식
    function embeddedTest(){
        var doc = document.getElementsByTagName("li")[1];
        doc.style.color = redl;
        doc.innertHTML = "<strong>글자가 바뀝니다.</strong>";
    }
</script>
```

**세 번째,** 외부작성방식이다.  외부에서 작성한 .js파일을 원하는 위치에 추가하고 script 영역에서는 외부에서 추가한 파일의 경로와 파일의 이름을 입력하면 따로 내부적으로 함수를 정의하지 않아도 함수가 호출된다.

```html
<body>
    <!-- 외부 링크방식 -->
    <script type="text/javascript" src="resources/js/test.js"></script>
    
    <p onclick="test();">외부 링크 방식></p>
</body>

```

```javascript
// test.js
function test(){
	window.alert("외부 작성 방식");
}

window.onload=function(){
	alert("윈도우 로딩 후!");
}
```



  

























