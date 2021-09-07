---
title : "vs code에 java 개발 환경 만들기"
excerpt: "vs code에 java 개발 환경 만들기"

toc: true
toc_sticky: true

categories:
  - vscode	
  - Java
tags:
  - Java
last_modified_at: 2021-08-06T21:12:00
---



## Java 설치

기본적으로 java 11버전 이상 및 vs code가 설치되어 있어야한다.

위 2가지 프로그램이 설치되어 있으면 vs code를 실행하여 마켓에서 java를 검색해본다.

![vscode-1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F7hYX8%2FbtqL38xSRce%2F1p17V3NkzRsUGNkPihje6k%2Fimg.png)



가장 처음 검색하면 Java Extension Pack이라는 항목이 나오는데 클릭해준다.

![vscode-2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwUraw%2FbtqL2w0yhhn%2FnNE9Kh1cKZuMIbUAcoMgw0%2Fimg.png)

Java Extension Pack은 Visual Studio Code에서 Java 응용 프로그램을 쓰기, 테스트 및 디버그하는 데 도움이 될 수 있는 널리 사용되는 확장 기능의 모음이다.

Java Extension Pack을 설치한 후 vscode를 재실행해준다.

그리고 파일 - 기본 설정 탭으로 이동하여 "설정"을 눌러준다.

![vscode-3](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Flpndd%2FbtqL39DzYJb%2Fz0mU5VJTb2uQBTKiA3ND5k%2Fimg.png)



그러면 조금한 검색창이 실행되는데 java.home 이라고 입력해준다.

java.home은 vs코드에게 자바가 설치된 위치를 알려주는 기능이다.



![vscode-4](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FkS118%2FbtqLXSKjnRU%2FYQm7dXfWI8INLp51Jwei2k%2Fimg.png)



위 사진처럼 "settings.json에서 편집" 버튼을 눌러서 자바가 설치된 디렉토리까지 입력해준다.

디렉토리는 bin 이전까지의 디렉토리를 입력해야한다.

그 후 디렉토리를 생성한 후 java파일을 만들고 간단한 명령어를 입력한 후 실행하면

Run|Debug 라는 텍스트가 생긴다. 여기서 Run을 누르면 정상적으로 실행된다.

---

## Spring 설치

이렇게 정상적으로 작동이 되는 것을 확인했으면 **Spring Boot  Extension Pack**을 검색한 후 설치하자

<p align="center">
	<img src="https://i.imgur.com/79AQwzq.png">
</p>
다운이 완료되었으면 [다시로드] 버튼을 클릭한다.

<p align="center">
	<img src="https://i.imgur.com/9EGBonJ.png">
</p>


**Lombok Annotations Support for VS Code** 확장팩도 설치한다.

**Spring Boot Tools 확장팩**은 다음 파일 패턴을 가지는 파일에 대해서 파일 수정할 때 활성화된다.

- .java : 스프링 부트 사양을 따르는 경우(@SpringBoot 에노테이션과 main() 메서드가 함께있음) 활성화
- application*.properties
- application*.yml

다음 기능을 제공한다.

- @RequestMapping에 선언된 경로(path)를 탐색할 수 있는 기능
- 실행 중인 앱이 제공하는 경로에 접근할 수 있는 기능
- 스프링 부트에 정의된 "Spring Boot Properties Metadata"를 이용해서 .properties와 .yml에서 자동완성 및 검증기능을 제공

**Spring Initializr Java Support**확장팩은 VScode 내에서 Spring Initialzr(https://start.spring.io/) API를 이용하여 스프링 부트 프로젝트를 구성할 수 있다.

**Spring Boot Dashboard**확장팩은 피보탈이 이클립스를 통해 제공하는 대시보드와 유사하게 작동한다. 등록된 스프링 부트 애플리케이션 조회, 실행, 중단, 디버그 및 실행중인 스프링 부트 앱을 브라우저에서 열어볼 수 있다.

### 프로젝트 생성

https://start.spring.io/으로 이동하여 하단의 [Switch to the full version]을 클릭하자

<p align="center">
	<img src="https://i.imgur.com/SW8Iic5.png">
</p>

<p align="center">
	<img src="https://i.imgur.com/kNkBPwb.png">
</p>

<p align="center">
	<img src="https://i.imgur.com/o8Oqtdb.png">
</p>
마지막으로 [Generate...] 버튼을 클릭하면 zip 파일을 다운받을 수 있다.

해당 zip파일을 압축해제한 후 VS code를 사용하여 실행한다.



### 프로젝트 생성 2

**ctrl + shift + p**를 입력한 후 입력창에 **Spring Initializr : Crate a Maven Project**를 선택

version은 **2.4.10** 선택, project language는 **Java** Group Id 및 Artifact Id 설정 Packaging type은 **Jar**, Java version은 설치되어 있는 **Jdk 버전** 선택 후 생성하려는 폴더를 클릭해주면 생성이 완료된다.



**porm.xml**

porm.xml에서 <dependencies>태그 내에 아래의 코드를 추가한다.

```xml
<dependency> 
    <groupId>org.apache.tomcat.embed</groupId> 
    <artifactId>tomcat-embed-jasper</artifactId> 
</dependency>
```

**application.properties**

application.properties 내에 아래의 코드를 추가한다.

```xml
server.port = 9090
```

**view(jsp)추가**

\src\main내에 \WEB-INF\jsp를 추가한다.
