---
title:  "HTML - 문법"
excerpt: "HTML 기초"

categories:
  - HTML
tags:
  - HTML
  - 문법
  - 학생
last_modified_at: 2021-03-16T13:54:00
---



# **HTML**

[TOC]

## - HTTP



**HTTP**(**H**yper **T**ext **T**ransfer **P**rotocol) : HTML 문서를 주고받는 데에 쓰이는 문서 전달 규약(protocol)

**HTML(H**yper **T**ext **M**ark-up **L**anguege) : 

웹 페이지의 모습을 기술하기 위한 규약으로 프로그래밍 언어가 아닌 마크업 언어이다. 

HTML은 `<tag>`를 사용하여 온라인 상의 문서(page)를 만들기 위한 구조화(mark-up) 된 언어이다.



- **기본구조**

Client가 Server에 요청(request) 하면,

HTTP(통신 규약)에 의해

HTML 문서를, 정해진 모양으로 응답(response) 한다.

	  *   Client가 보는 화면(view)은 서버가 보내준 모양대로 나타나게 된다.



```html
<tagname>Contents</tagname>
```

- 위 모든 걸 요소(Element)라고 합니다.

- `<tagname>`을 시작 태그, `</tagname>`을 종료 태그, 둘을 합쳐 태그(tag)라고 합니다.

- Contests는 내용입니다.

  

```html
<!DOCTYPE html>							문서의 타입을 정의
<html>									이 문서는 html 이다.
  <head>								문서를 정의
    <title>Hello, World</title>
  </head>
  <body>								문서의 내용
    <p>Hello, Wolrd! Hello, Web!</p>
  </body>
</html>
```



**문서를 정의 하는(`<head>`) 기본 태그**

- `<meta>` : 문서의 기본 정보 등을 정의
- `<title>` : 문서의 제목을 정의
- `<script>` : javascript 등을 정의
- <`style`> : css 등을 정의
- <`link`> : 외부 문서를 연결할 때 정의



- **DTD**(**D**ocument **T**ype **D**efinition) 해석!

  <img src="https://user-images.githubusercontent.com/79016263/111899722-a939b100-8a71-11eb-815b-0a16eabc0929.png" alt="DTO해석" align="left" />

  

- **HTML** **작성 주의사항** 
  
  * 요소를 정확하게 매칭( `<tag>` 요소를 사용할 때 시작 부분과 끝 부분을 정확하게 매칭시켜야 한다.)
  
  * 요소, 속성 이름은 소문자
  
  * 요소는 항상 닫아야 한다.(일부 단일 tag 존재)
  
  * 특수문자를 사용할 때는 Entity Name(code) 으로 사용
  
    

## - 문법

HTML의 문법을 알아보기 위해 eclipse를 통해 코드를 작성하였다. 문법은 총 11가지로 구성되어 있으며, WebContent 폴더 안에 index.html을 먼저 작성하였다.

<img src="C:\Users\1\AppData\Roaming\Typora\typora-user-images\image-20210303143720690.png" alt="image-20210303143720690" style="zoom: 80%;" align="left" />

여기서 주의할 점은 html 파일은 꼭 **WebContent** 폴더 아래에 있어야 하는 점이다. META-INF와 WEB-INF는 사용자가 접근할 수 없으니 해당 폴더에 html을 생성하면 안되는 점을 기억하자. resources 폴더는 image를 넣기위해 임의로 만든 폴더이다.(resources/images/teri.png)

아직 index.html 파일에서 쓰여진 `<tag>`가 무엇인지 모르겠지만 `<a href="html01_block_inline.html">` 를 통해 html01_block_inline으로 link했다는 것만 알아두면 된다.  

<img src="https://user-images.githubusercontent.com/79016263/111899723-aa6ade00-8a71-11eb-8174-4a968236612d.png" alt="href 이동" align="left" />    













(해당 프로젝트를 Run on Server 하면 Chrome이 구동되며 위와 같은 화면이 실행된다. 원하는 곳을 누르면 해당 html 파일과 link된다. )

Chrome으로 실행하는 방법 : Window - Web Browser - Chrome

---

### 1. 블록요소, 인라인요소

##### 블록요소

- 기본적으로 줄 바꿈이 일어나는 형태로 영역의 너비가 상위 영역의 전체 너비만큼 되는 형태(즉, 줄바꿈)
- 블록 요소 안에 텍스트와 인라인 요소가 포함 가능
- 블록 요소 안에 블록 요소도 포함 가능(일부 불가)

|   artice   |    aside    |  audio   |  address   | bolckquote | canvas  |     dd      |    div    |    dl     |  fieldset  | figcaption |
| :--------: | :---------: | :------: | :--------: | :--------: | :-----: | :---------: | :-------: | :-------: | :--------: | :--------: |
| **figuer** | **footer**  | **form** |   **h1**   |   **h2**   | **h3**  |   **h4**    |  **h5**   |  **h6**   | **header** | **hgroup** |
|   **hr**   | **noscipt** |  **ol**  | **output** |   **p**    | **pre** | **section** | **table** | **ufoot** |   **ul**   | **video**  |



##### 인라인요소

- 새로운 줄을 만들지 않으며 필요한 너비만 차지(즉, 줄바꿈 x)
- 인라인 요소 안에 텍스트와 인라인 요소 포함 가능
- 인라인 요소 안에 블록 요소 포함 불가

|     a     |    abbr    |  acronym   |     b      |   bdo   |    br     |  button  |     cite     |  code  |   dfn    |
| :-------: | :--------: | :--------: | :--------: | :-----: | :-------: | :------: | :----------: | :----: | :------: |
|  **em**   |   **i**    |  **img**   | **input**  | **kbd** | **label** | **map**  |  **object**  | **q**  | **samp** |
| **small** | **script** | **select** | **strong** | **sub** |  **sup**  | **sapn** | **textarea** | **tt** | **var**  |



```html

<h1>블록 요소</h1>
<p style = "background-color: yellow;">줄바꿈</p> 
<!-- 블록 요소는 해당 라인을 잡기 때문에 줄바꿈이 이루어지는 겁니다. -->
<div>
    블록 요소 안에 텍스트, <strong>인라인 요소</strong>포함 가능
    <p>블록 요소 안에 블록 요소 포함 </p>
</div>

<!-- 

p : 블록 요소
q, strong, span : 인라인 요소

-->

<hr/>

<h1>인라인 요소</h1>
<a>줄바꿈 X</a>
<q style = "background-color: skyblue;">인라인 요소 안에 텍스트와 <a href = "http://www.naver.com">인라인 요소</a> 포함 가능</q> 
<!-- 블록 요소는 해당 라인을 잡기 때문에 줄바꿈이 이루어지는 겁니다. -->
<br/>
<span>인라인 요소 안에 <p>블록 요소</p> 포함 불가</span> 
<!-- 사용은 가능하나, 지양하자. -->
```

해당 코드를 입력하고, 실행시키면 아래와 같은 화면이 출력된다.

<img src="https://user-images.githubusercontent.com/79016263/111899732-ab9c0b00-8a71-11eb-801e-04be159327f2.png" alt="블록과 인라인요소" align="left" />

---

### 2. 제목, 단락, 주소

```html
<h1>제목</h1> <!-- h1, h2, ... 글자 크기 -->
<h2>글자</h2>
<h3>크기</h3>
<h4>지정</h4>
<h5>하는</h5>
<h6>태그</h6>

<p>p 요소는 단락을 정의</p> <!-- p는 단락(서론, 본론, 결론) div는 *영역(게임 영역, 요리 영역, 운동 영역), div 안에 div를 많이 쓴다. -->
<div>
    div 요소는 영역을 정의
</div>

<p>
    인라인 요소와 텍스트 요소를 포함할 수 있는 <strong>블록 요소</strong>이지만, <br/>
    또 다른 <em>블록 요소</em>를 포함할 수는 없다.
</p>

<adress> 연락처 : 010-0000-0000</adress>
```



---

### 3. 구분선, 인용문

```html
<h1>&lt;blockquote&gt;</h1>

<strong>드래곤라자</strong> 라는 소설에서, 내가 좋아하는 말이 있습니다.
<blockquote cite = "#" title = "드래곤라자 명언">
    <cite>드래곤라자</cite>
    나는 단수가 아니다.
</blockquote>

<hr/> <!-- 자기 영역, 한 줄 -->

<h1>&lt;q&gt;</h1>

<b>드래곤라자</b> 라는 소설에서, 내가 좋아하는 말이 있습니다.
<q cite = "#" title = "드래곤라자 명언"> 나는 단수가 아니다. </q>
```

<img src="https://user-images.githubusercontent.com/79016263/111899809-13525600-8a72-11eb-9be0-ca802700dc74.png" alt="blockquote" align="left" />

<!-- blockquote 요소를 쓴 곳은 들여쓰기가 된다. --!>

---

### 4. 텍스트, 이미지, 링크

```html
<body>

	<h1>text</h1>
	
	<p>
		<b>진하게(&lt;b&gt;)</b><br/>
		<strong>진하게(&lt;strong&gt;)</strong><br/>
		<i>기울임(&lt;i&gt;)</i><br/>
		<em>기울임(&lt;em&gt;)</em><br/>
		작은 <small> 텍스트 표시, 코멘트</small>(&lt;small&gt;)<br/>
		윗<sup>첨자</sup>(&lt;sup&gt;)<br/>
		아래<sub>첨자</sub>(&lt;sub&gt;)<br/>
		<ins>내용 추가 (&lt;ins&gt;)</ins><br/>
		<del>내용 삭제 (&lt;del&gt;)</del>
	</p>
	
	<h1>img</h1>
	
	<img alt = "설명" src = "resources/image/teri.png" width = "200px" height = "200px" title = "이미지" /> 
	<!-- 
	px : 픽셀(해상도 별 상대크기)
	pt : 포인트(1pt = 0.72인치)
	%, em : 지정/상속 등에 대한 백분율 (상대크기)
	 -->
	
	<h3>이미지 링크 걸기</h3>
	
	<a href = "index.html" title = "go index">
		<img alt = "링크" src = "resources/image/teri.png" width = "100px" height = "100px"/>
	</a>
	
	<h3>이미지 맵</h3>
	
	<img alt = "설명" src = "resources/image/teri.png" width = "100px" height = "100px" usemap = "#my"/>
	<map name = "my">
		<area alt = "" shape = "rect" coords = "40, 40, 100, 100" href = "index.html" title = "index go"/>
	</map>
	

</body>
```

<img src="https://user-images.githubusercontent.com/79016263/111899724-aa6ade00-8a71-11eb-9d63-92b1ec51fd21.png" alt="image-link" align="left" />

```html
<a href = "index.html" title = "go index">
		<img alt = "링크" src = "resources/image/teri.png" width = "100px" height = "100px"/>
</a>
```

링크의 기본 문법은  `<a href="xxx">Label</a>` 이와 같은 형태로 이루어진다. 이는 "xxx"로 지정한 웹페이지, 이미지, 파일 등의 대상을 처리하는 코드인데 onclick 이벤트와 비슷하게 해당 영역을 클릭해야 event를 실행한다. 기본 문법에서는 text 형태로 'Label'  문자열을 입력했기 때문에 Label의 text length만큼 영역이 정해지지만, 위의 소스 코드에서는 `<img alt="링크" src="resources/image/teri.png" width="100px" height="100px"/>` img 태그를 사용하여 width=100px이고, height=100px인 이미지 영역을 잡아놨기 때문에 해당 영역을 누르게 되면 index.html 파일로 이동하게 된다.

```html
<img alt = "설명" src = "resources/image/teri.png" width = "100px" height = "100px" usemap = "#my"/>
<map name = "my">
    <area alt = "" shape = "rect" coords = "40, 40, 100, 100" href = "index.html" title = "index go"/>
</map>
```

`<map>` 태그는 이미지 맵을 생성하기 위해서 사용된다. 이미지 맵은 하나의 이미지에 여러개의 링크를 거는 방법으로, 이미지 상의 클릭 위치에 따라 다른 링크가 열리게 할 수 있다. `<area>` 태그는 `<map>` ~ `</map>` 의 요소 안에서 작성되어야 하며, 이미지 맵의 영역을 지정하기 위해서 사용된다. 해당 `<area>`의 속성을 살펴보면, 사각형 형태로 왼쪽 모서리는 (40, 40) 오른쪽 모서리는 (100, 100)의 영역을 잡아주었고 해당 영역을 클릭하면 index.html 파일로 링크해라 라는 것을 알 수 있다.

---

### 5. 링크

```html
<body>

	<h1>a 태그 사용</h1>
	
	<a href = "http://www.naver.com">naver</a><br/>
	
	<a href = "#a">1번으로</a><br/>
	<a href = "#b">2번으로</a><br/>
	<a href = "#c">3번으로</a><br/>
    
    <br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
    
    <p id = "a">1번</p>
    
    <br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
    
    <p id = "b">2번</p>
    
    <br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
    
    <p id = "c">3번</p>
</body>
```

링크에 대한 부분은 이미 앞서 사용한 예가 많아 쉽게 넘어갈 것이라고 생각한다. 

`<a href = "http://www.naver.com">naver</a>`  : naver 클릭시 https://www.naver.com으로 이동

`<a href = "#a">1번으로</a>` : 1번으로 클릭시 id가 a인 태크로 이동 `<p id = "a">1번`

`<a href = "#b">2번으로</a>` : 2번으로 클릭시 id가 b인 태크로 이동 `<p id = "b">1번`

`<a href = "#c">3번으로</a>` : 3번으로 클릭시 id가 c인 태크로 이동 `<p id = "c">1번`

---

### 6. 목록

##### 1. 순차적 목록

```html
<h1>목록</h1>

<h2>순차적 목록</h2>

<b>학원 오는 순서</b>
<ol>
    <li>집에서 출발</li>
    <li>15번 버스를 타고</li>
    <li>사당역에서 내려서</li>
    <li>
        <ol>
            <li>2호선 타고</li>
            <li>역삼역으로</li>
        </ol>
    </li>
    <li>역삼역 출구에서 학원으로 걸어간다.</li>
</ol> 
<hr/>
```

<img src="https://user-images.githubusercontent.com/79016263/111899726-ab037480-8a71-11eb-9476-4b4c09fd0e9b.png" alt="순차적목록" align = "left"/>  















`<ol>` 태그는 순서가 있는 목록을 만들 때 사용되며, `<li>` 로 목록 list를 나열할 수 있다. (자동 들여쓰기)

##### 2. 비 순차적 목록

```html
<h2>비 순차적 목록</h2>

<b>집 가는 순서</b>
<ul>
    <li>학원 끝</li>
    <li>146번 버스를 탄다.</li>
    <li>공릉역에서 내린다.</li>
    <li>
        <ul>
            <li>버거킹으 들린다.</li>
            <li>스타벅스를 들린다.</li>
        </ul>
    </li>
    <li>집 도착</li>
</ul>
<hr/>
```

<img src="https://user-images.githubusercontent.com/79016263/111899728-ab037480-8a71-11eb-850f-5b7ae4312da4.png" alt="비순차적목록" align ="left" />  















비 순차적 목록 안에 순차적 목록도 사용 가능하다.

##### 3. 정의형 목록

```html
<h2>정의형 목록</h2>
	<dl>
		<dt>제목</dt>
		<dd>내용1</dd>
		<dd>내용2</dd>
		<dd>내용3</dd>
		<dd>
			<dl>
				<dt>강의목록</dt>
				<dd>JAVA</dd>
				<dd>DATABASE</dd>
				<dd>HTML</dd>
				<dd>CSS</dd>
			</dl>
		</dd>
	</dl>
```

<img src="https://user-images.githubusercontent.com/79016263/111899730-ab9c0b00-8a71-11eb-9547-caa19c632bde.png" alt="정의형목록" align = "left" />  















`<dl>` : datalist 혹은 definition list, description list로 불린다.

`<dt>` : 제목을 정의할 때 사용하며, 들여쓰기 x , <dd> : 내용이나 목록을 정의할 때 사용되며 들여쓰기 o



### 7. 테이블

"boder" : 테두리

"<tr>" : table row의 약자로, 가로줄을 만드는 역할(기본값은 보통 글씨체, 왼쪽정렬)

"<th>" :  table head의 약자로, 표의 제목을 쓰는 역할(기본값은 굵은 글씨체, 중앙정렬)

"<td>" : table datad의 약자로, 셀을 만드는 역할(기본값은 보통 글씨체, 왼쪽 정렬)

```html
<h1>기본 테이블 만들기</h1>
	
	<table border = "1">
		<tr>
			<th>컬럼1</th>
            <th>컬럼2</th>
		</tr>
		<tr>
			<td>1</td>
			<td>2</td>
		</tr>
		<tr>
			<td>3</td>
			<td>4</td>
		</tr>
	</table>
```

<img src="https://user-images.githubusercontent.com/79016263/111899733-accd3800-8a71-11eb-8620-7c4d117fe0ca.png" alt="컬럼테이블" align="left" />  





"<caption>" : 해당 테이블의 제목

"<colgroup>" : column의 속성을 정해줄 때 사용

"<col width = "100px" />" : column의 길이를 100px로 설정하겠다.

```html
<table border = "1">
    <caption>테이블 제목</caption>
    <colgroup>
        <col width = "100px" />
        <col width = "200px" />
        <col width = "300px" />
    </colgroup>

    <thead>
        <tr>
            <th>colum1</th>
            <th>colum2</th>
            <th>colum3</th>
        </tr>
    </thead>

    <tbody>
        <tr>
            <td>data1</td>
            <td>data2</td>
            <td>data3</td>
        </tr>
        <tr>
            <td>data4</td>
            <td>data5</td>
            <td>data6</td>
        </tr>
        <tr>
            <td>data7</td>
            <td>data8</td>
            <td>data9</td>
        </tr>
    </tbody>

    <tfoot>
        <tr>
            <td>footer1</td>
            <td>footer2</td>
            <td>footer3</td>
        </tr>
    </tfoot>
</table>
```

<img src="https://user-images.githubusercontent.com/79016263/111899731-ab9c0b00-8a71-11eb-9936-e7fcd801432b.png" alt="컬럼테이블" align="left" />  







post : ?key=value형태가 보이지 않는다.

get : 보인다.

post와 get 방식은 패킷으로 전달하므로  wireshark(패킷분석기)를 이용해 queryString으로 전달되는 id 또는 password의 값을 외부에서 접근하게 되면 개인정보를 유출할 위험이 생긴다. 이렇게 보완이 취약한 점을 해결하기 위해 나온 것이 'https'이다. 

http://localhost:8787/UI01_HTML/

html08_form01_res.html (form의 action 속성의 경로) <- key = value 값이 form의 action 지정 경로로 전달

?

id=myid

&

pw=password

&

rdo=y

&

cb1=html

&

cb2=css

&

cb3=javascript

&

myhidden=my+hidden+value 

-> queryString : 값을 전달해주겠다.

? key = value & key = value & k = value 형태로 값을 전달해준다.



WebContent - new folder(resources) - new folder(css) - new file(style.css)

href : css 파일 경로(위치)

rel : 연결 대상의 속성(stylesheet로 만들어져 있다.) -> 문서의 스타일과 관련된 파일이다.

type : 해당 파일의 정보값.

text/css -> text로 이루어져 있는데,  css역할을 할거야

b{
	color: green;
}

-> b 테그의 컬러를 초록색으로 바꿔라

<b style="color: pink;">1. 인라인 스타일시트 : 우선순위가 높다.</b>

ㄴ <!-- <b style="color: pink;">1. 인라인 스타일시트 : 우선순위가 높다.</b> -->

먼저 초록색으로 바뀌고, 우선순위가 높은 해당 라인을 통해 화면에 그려질 때 핑크로 바뀌어 출력된다.



자식 요소와 하위 요소

?="!" -> 속성 = "값" html

?:! -> { 중가로 안에 있을때} css



### 8. form01

[html08_form01.html]

```html
<body>
	
	<form action="html08_form01_res.html" method="post">
		<fieldset>
			<legend>회원가입</legend>
			<p>
				아이디 : <input type="text" name="id"/>
			</p>
			<p>
				비밀번호 : <input type="password" name="pw"/>
			</p>
			<p>
				이메일 수신여부<br/>
				<input type="radio" name="rdo" value="y"/>Y<br/>
				<input type="radio" name="rdo" value="n"/>N
				<!-- radio button은 같은 이름일 때 중복 선택 불가! -->
			</p>
			<p>
				관심분야<br/>
				<input type="checkbox" name="cb1" value="html" />HTML<br/>
				<input type="checkbox" name="cb2" value="css" />CSS<br/>
				<input type="checkbox" name="cb3" value="javascript" />JavaScript
			</p>
			<p>
				<input type="button" value="그냥 버튼" />
				<input type="reset" value="취소" />
				<input type="submit" value="전송" />
			</p>
			<p>
				<input type="file" />
				<input type="hidden" name="myhidden" value="my hidden value" />
			</p>
		</fieldset>
	</form>
</body>
```

[html08_form01_res.html]

```html
<body>
<script type="text/javascript">
	window.onload=function(){
		var val = location.search;
		alert(val);
	}
</script>

</body>
```

### 9. form02

```html
<body>

	<h1>select</h1>
	
	<form action="#">
		<fieldset>
			<legend>여러줄 글상자 &amp; 목록상자</legend>
			
			<div>
				<label for="reply">답글</label><br/>
				<textarea rows="3" cols="10" id="reply" name="re"></textarea>
				<!-- 3줄 10칸? -->
			</div>
			<div>
				과목선택
				<br/>
				<select name="sel01">
				<!-- 우측 하단 화살표를 통해 원하는 값 선택 가능 -->
					<option value="html">HTML</option>
					<option value="css">CSS</option>
					<option value="javascript">JavaScript</option>
					<option value="jq">JQuert</option>
				</select>
			</div>
			<div>
				저녁메뉴
				<select name="sel02">
				<!-- optgroup은 사용자가 선택할 수는 없지만 학식, 중식별로 음식을 보여주고 선택할 수 있다. -->
					<optgroup label="한식">
						<option value="갈비찜">갈비찜</option>
						<option value="잡채">잡채</option>
					</optgroup>
					<optgroup label="중식">
						<option value="마라샹궈">마라샹궈</option>
						<option value="훠궈">훠궈</option>
					</optgroup>
					<optgroup label="일식">
						<option value="스시">스시</option>
						<option value="우동">우동</option>
					</optgroup>
					<optgroup label="양식">
						<option value="스테이크">스테이크</option>
						<option value="감자튀김">감자튀김</option>
					</optgroup>
				</select>
			</div>
		</fieldset>
	</form>

</body>
```

### 10. menu

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>

<style type="text/css">
	*{
		padding: 0px;
		margin: 0px;
	}
	div{
		border: 1px dashed blue;
		margin: 10px;
	}
	#body{
		height: 400px;
	}
	#left{
		width: 48%;
		height: 90%;
		float: left;
	}
	#right{
		width: 48%;
		height: 90%;
		float: right;
	}
</style>
</head>
<body>
	
	<div id="header">
		<h1>제목</h1>
		<div>
			<span><a href="#">메뉴1</a></span>
			<span><a href="#">메뉴2</a></span>
			<span><a href="#">메뉴3</a></span>
			<span><a href="#">메뉴4</a></span>
		</div>
	</div>
	
	<div id="body">
		<div id="left">
			<p>내용1</p>
		</div>
		<div id="right">
			<p>내용2</p>
		</div>
	</div>
	
	<div id="footer">
		<address>copyright &copy; all rights reserved...</address>
	</div>
	
</body>
</html>
```

### 11. menu(semetioc)

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<style type="text/css">
	*{
		padding: 0px;
		margin: 0px;
	}
	.html5{
		border: 1px dotted red;
		margin: 10px;
	}
	section{
		height: 400px;
	}
	#left{
		width: 48%;
		height: 90%;
		float: left;
	}
	#right{
		width: 48%;
		height: 90%;
		float: right;
	}
</style>

</head>
<body>
	<header class="html5">
		<h1>제목</h1>
		<nav class="html5">
			<span><a href="#">메뉴1</a></span>
			<span><a href="#">메뉴2</a></span>
			<span><a href="#">메뉴3</a></span>
			<span><a href="#">메뉴4</a></span>
		</nav>
	</header>
	
	<section class="html5">
		<article class="html5" id="left">
			<p>내용1</p>
		</article>
		<article class="html5" id="right">
			<p>내용2</p>
		</article>
	</section>
	
	<footer class="html5">
		<address>copyright &copy; all rights reserved...	</address>
	</footer>
</body>
</html>
```





































