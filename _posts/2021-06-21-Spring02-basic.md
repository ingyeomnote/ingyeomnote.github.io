---
title:  "Spring Framework의 기본 개념2"
excerpt: "SpringFramework의 특징"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
  - Spring
last_modified_at: 2021-06-26T23:12:00
---



전 포스팅에선 Spring Framework란 무엇인지, 또 IoC와 IoC의 구성요소 DI와 DL에 대해 알아보았다. 이번 시간에는 Spring Framework의 특징과 구조에 대해 알아보자.

## Spring Framework

짧게나마 다시 Spring Framework란 무엇인가?부터 살펴보면 다음과 같다.

- 자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크

- EJB(Enterprise Java Beans)의 단점을 보완한 프레임워크

- 경량 컨테이너로 자바 객체를 직접 관리(각각의 객체 생성, 소멸과 같은 라이프 사이클을 관리하며 스프링으로부터 필요한 객체를 얻어올 수 있다.)

- DI(Dependency Injection, 의존성주입)과 AOP(Aspect-Oriented Programming, 관점 지향프로그래밍)이 지원되는 경량 컨테이너 & 프레임워크

    
    
    ![](https://t1.daumcdn.net/cfile/tistory/2549104458AFAB4023)



## Spring Framework의 특징

### 경량화

- 스프링은 경량 애플리케이션 프레임워크라는 게 큰 특징 중 하나인데 사실 스프링 자체가 가볍거나 작은 규모의 코드로 이루어진 것은 아니다.
- 오히려 스프링은 20여개의 모듈로 세분화 되어있고 복잡하고 방대한 코드를 가진 프레임워크다.
- 경량화가 특징인 이유는 기존 자바 엔터프라이즈 기술의 불필요한 복잡함에 반대되는 개념에서 시작되었다.
- 주류 기술이었던 EJB는 고가의 무거운 자바 서버(WAS)가 필요했고, 다루기 힘든 설정파일 구조, 패키징, 불편한 배포 등이 단점이었다.
- 반면, 스프링은 톰캣과 같은 단순한 서버환경에도 동작하며 단순한 개발환경으로도 엔터프라이즈 애플리케이션을 개발하였다.
- 또 EJB 등의 기존 프레임워크에서 만들어진 코드에 비해 코드량이 적고 단순하다.
- 즉, 기존에 비해 빠르고 간편하게 애플리케이션을 개발할 수 있어 생상성이 뛰어난 프레임워크이고, 이러한 특징들 덕분에 경량 프레임워크라고 불리게 되었다.

### POJO(Plain Old Java Object):

POJO란 말 그대로 평범한 자바 오브젝트를 뜻한다. 이전 EJB(Enterprise Java Beans)는 확장과 재사용이 가능한 로직을 개발하기 위해 사용 되었지만 한 가지 기능을 위해 불필요한 복잡한 로직이 과도하게 들어가는 단점이 있었다. 그래서 EJB 다음으로 조명을 받은 게 POJO이다. 

POJO는 getter/setter를 가진 단순 자바 오브젝트로 정의를 하고 있다. 이러한 단순 오브젝트는 의존성이 없고 추후 테스트 및 유지보수가 편리한 유연성의 장점을 갖고 있다. 이러한 장점들로 인해 객체지향적인 다양한 설계와 구현이 가능해지고  POJO 기반의 Framework 또한 자주 쓰이게 되었다.

Spring Framework에서는 POJO를 지원하고 있으며 실제 홈페이지에 다음과 같은 글이 존재한다.

"POJO를 사용함으로써 당신의 코드는 더욱 심플해졌고, 그로인해 테스트하기에 좋으며 유연하고 요구사항에 따라 기술적 선택을 바꿀수 있게 되었다."

### AOP(Aspect Oriented Programming):

대부분 소프트웨어 개발 프로세스에서 사용하는 방법은 OOP(Object Oriented Programming)이다. OOP는 객체지향 원칙에 따라 관심사가 같은 데이터를 한 곳에 모아 분리하고 낮은 결합도를 갖게하여 독립적이고 유연한 모듈로 캡슐화를 하는 것을 일컫는다. 하지만 이러한 과정 중 중복된 코드들이 많아지고 가독성, 확장성, 유지보수성을 떨어트리게 된다. 이러한 문제를 보완하기 위해 나온 것이 AOP이다.

AOP에서는 핵심기능과 공통기능을 분리시켜 핵심 로직에 영향을 끼치지 않게 공통기능을 끼워 넣는 개발 형태이며 이렇게 개발함에 따라 무분별하게 중복되는 코드를 한 곳에 모아 중복되는 코드를 제거할 수 있어지고 공통기능을 한 곳에 보관함으로써 공통 기능 하나의 수정으로 모든 핵심 기술들의 공통기능을 수정할 수 있는 효율적인 유지보수가 가능하며 재활용성이 극대화된다.

즉, AOP는 흩어진 Aspect들을 모아서 모듈화 하는 기법이다.

여기서 Aspect는 쉽게 생각하서 객체지향 언어의 클래스와 비슷한 개념이라고 생각하면 된다. 그 자체로 애플리케이션의 도메인 로직을 담은 핵심기능은 아니지만 많은 오브젝트에 걸쳐서 필요한 부가기능을 추상화해놓은 것이다.

물론 AOP로 만들 수 있는 기능은 OOP로 구현할 수 있는 기능이지만 Spring에서는 AOP를 편리하게 사용할 수 있도록 이를 지원하고 있다.

### AOP 용어

**CC(Core Concern)** : 핵심 기능(공통적이지 않은 관심사)

**CCC(Cross Cutting Concerns)** : 부가 기능, 공통 기능

**Target** : 어떤 대상에 부가 기능을 부여할 것인가

**Advice** : PointCut에서 지정한 Join Point에서 실행(삽입)되어야 하는 코드. Aspect의 실제 구현체

**Join point** : 인스턴스의 생성 시점, 메소드를 호출하는 시점, Exception이 발생하는 시점과 같이 애플리케이션이 실행될 때 특정 작업이 실행되는 시점을 의미한다. → 어디에 적용할 것인가? 

**PointCut** : 실제 advice가 Join Point에 적용되어야 하는지 정의한다. 명시적인 클래스나 메소드의 이름과 패턴이 일치하는 Join Point 영역을 지정할 수 있도록 해준다. Spring AOP에서는 advice가 적용될 메서드를 선정

**Aspect** : Aspect는 AOP의 중심 단위로 Advice와 PointCut을 합친 것이다. 구현하고자 하는 횡단 관심사의 기능, 애플리케이션의 모듈화 하고자 하는 부분을 의미한다.

**Weaving** : Aspect를 대상 객체에 적용하여 새로운 프록시 객체를 생성하는 과정

![https://user-images.githubusercontent.com/79016263/122897860-57146e80-d385-11eb-979f-d15612ae3fdc.png](https://user-images.githubusercontent.com/79016263/122897860-57146e80-d385-11eb-979f-d15612ae3fdc.png)

## SpringMVC

## 스프링의 구성



![](https://t1.daumcdn.net/cfile/tistory/24251A4D58AFABBD33)

**Spring Core**

- Core 컨테이너 기능
- Spring 프레임워크의 근간이 되는 IoC (또는 DI) 기능을 지원하는 영역
- BeanFactory를 기반으로 Bean 클래스들을 제어할 수 있는 기능을 지원한다.

- **Spring Context**
    - Spring Core에서 지원하는 기능외에 추가적인 기능들과 좀 더 쉬운 개발이 가능하도록 지원한다.
    - 또, JNDI, EJB 등을 위한 Adaptor들을 포함하고 있다.
- **AOP**
    - Spring 프레임워크에 AOP를 지원하는 기능이다.
- **DAO**
    - JDBC 기반하의 DAO 개발을 좀 더 쉽고, 일관된 방법으로 개발하는 것이 가능하도록 지원한다.
- **ORM**
    - Object-Relational Mapping 프레임워크인 Hibernate, MyBatis, JDO와의 결합을 지원하기 위한 기능이다.
- **Spring Web**
    - Web Application 개발에 필요한 Web Application Context와 MultipartRequest 등의 기능을 지원한다.
- **Portlet**
    - 포탈 서버에서 돌아가는 독립된 웹 애플리케이션 원격 지원 기능