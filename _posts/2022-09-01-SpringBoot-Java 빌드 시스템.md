---
title:  "[SpringBoot] Java 빌드 시스템 - IntelliJ"
excerpt: "자바의 빌드 시스템에 대해 알아보자"

toc: true
toc_sticky: true

categories:
  - SpringBoot
tags:
  - Java
  - SpringBoot
  - IntelliJ
last_modified_at: 2022-09-01T21:34:00
---





# 🦅1.Maven



Project Object Model을 관리하는 pom.xml 파일을 사용하며 개발자가 해당 파일에 사용할 라이브러리를 정의해 두면 정의된 라이브러리뿐 아니라 그 라이브러리를 사용하는데 필요한 종속된 다른 라이브러리까지 자동으로 다운로드하여 사용할 수 있게 해준다.

빌드 동작 방식이 미리 정해져 있으며 라이프사이클에 의해 순차적으로 동작한다.

# 🐘2.Gradle

Gradle은 기존 ANT와 Groovy 기반으로 구축되어 기존의 ANT 역할과 배포 기능을 모드 지원한다.

Gradle은 Maven보다 늦게 출시된 빌드 툴인데, 그런만큼 Maven보다 더 나은 사용성과 성능을 제공하고 있다. 안드로이드 앱의 공식 빌드 시스템이기도 하며 Java, C/C++, Python등 다양한 언어에 대한 빌드를 지원하고 잇다.

Maven과 달리 build.gradle 파일을 사용하며 Maven과 동일하게 개발자가 사용할 라이브러리를 정의해 둘 수 있다. 

# 🐱‍🐉Maven vs Gradle

사실 두 빌드 시스템은 자주 비교되지만 Gradle이 Maven보다 성능과 편의성면에서 당연히 우세하다.

Maven의 경우 xml로 관리하고 Gradle의 경우 Groovy를 사용하니, 라이브러리가 많아질수록 더 쉽게 관리할 수 있는 Gradle과 Maven은 차이가 날 수 밖에 없는 것이다. 심지어 Gradle은 Groovy를 사용해 개발자가 직접 스크립트를 작성해 빌드를 커스텀할 수도 있고 플러그인을 호출할 수도 있다. 

![cleanCode](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbCxK5A%2FbtrFzNHXf6u%2FjUAffGgEzAkYflPHoNct60%2Fimg.png)

성능 측면에서 바라봤을 때, 동일한 클린 상태에서 빌드를 실행하면 서로 두 배가량 차이가 발생한다. 심지어 캐싱된 상태에서는 최대 100까지 난다고 하니 실로 어마어마하다고 볼 수 있다.

하지만, 빌드 시스템의 점유율은 아직까지 Maven이 더 높은 쪽을 차지하고 있다. 아마 이미 Maven으로 작성된 곳이 많기 때문이지 않을까 싶다.

# 🦞IntelliJ

IntelliJ는 IntelliJ에서 제공해주는 독자적인 빌드 방식이다.
IntelliJ 빌드 시스템은 IntelliJ의 자체 빌드 매커니즘으로 단순히 프로젝트의 모든 수정 내용과 종속 파일을 컴파일하는 기능이다.
