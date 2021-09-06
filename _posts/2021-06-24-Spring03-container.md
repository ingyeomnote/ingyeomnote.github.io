---
title: "Spring 컨테이너"
excerpt: "Spring 컨테이너의 이해"
toc: true
toc_sticky: true
categories:
  - Spring
tags:
  - Spring
last_modified_at: 2021-07-02T21:15:00

---

## 스프링 컨테이너란?

스프링 프레임워크에서 Container개발자를 대신하여 스프링의 **Bean**을 생성하고 관리한다. Container가 Bean를 관리해주기 때문에 개발자는 모듈 간 의존 및 결합으로 발생하는 문제로부터 자유로워졌으며 이를 통해서 스프링의 주개념인 IOC나 AOP에 대해서 관리하곤 한다.                                                                                                                                                                                          

그 주체가 바로 오늘 적을 Application Context다.

### 스프링 컨테이너의 종류

먼저 스프링 컨테이너의 종류는 Bean Factory와 이를 상속한 ApplicationContext 2가지 유형이 존재한다.

#### Bean Factory

Bean Factory는 스프링 설정 파일에 등록된 Bean 객체를 생성하고 관리하는 기본적인 기능만 제공한다. 컨테이너가 구동될 때 Bean 객체를 생성하는 것이 아니라 **클라이언트의 요청에 의해서 Bean 객체가 사용되는 시점(Lazy Loading)**에 객체를 생성하는 방식을 사용하고 있다.

일반적으로 스프링 프로젝트에서는 사용될 일이 없지만, ApplicationContext는 BeanFactory를 상속받고 있다는 것을 알아두자

#### Application Context

Bean Factory와 마찬가지로, Bean 객체를 생성하고 관리하는 기능을 가지고 있다. 뿐만 아니라 **트랜잭션 관리, 메시지 기반의 다국어 처리, AOP처리 **등 DI(Dependency Injection)과 Ioc(Inversion of Control)외에도 많은 부분을 지원하고 있다. 

Bean Factory와의 큰 차이점은 Application Context는 **컨테이너가 구동되는 시점에 객체들을 생성하는 Pre-Loading 방식과 트랜잭션 관리**가 가능하다는 점이다.

