---
title:  "[SpringBoot] build.gradle[2/2]"
excerpt: "build.gradle에 대해 알아보자"

toc: true
toc_sticky: true

categories:
  - SpringBoot

tags:
  - Java
  - SpringBoot
  - IntelliJ
  - build.gradle

last_modified_at: 2022-09-03T22:14:00
---



# 📂build.gradle
build.gradle의 원리를 어느정도 파악했으니 이제 gradle 프로젝트를 스프링 부트 프로젝트로 변경해보자. 인텔리제이에서 build.gradle 파일을 열어보면, 다음과 같은 간단한 코드들이 있다.

```
plugins {
    id 'java'
}

group 'com.webspring.book'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1'
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}

```
앞의 코드들은 자바 개발에 가장 기초적인 설정만 되어있는 상태다. 여기에 스프링 부트에 필요한 설정들을 하나씩 추가하면 된다.

## 📝buildscript 추가
먼저, build.gradle 맨 위에 위치할 코드다.

```
buildscript{
    ext{
        springBootVersion = '2.7.1.RELEASE'
    }
    
    repositories{
        mavenCentral()
        jcenter()
    }
    
    dependencies{
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}
```
지금 작성한 코드는 이 프로젝트의 플러그인 의존성 관리를 위한 설정이다. **ext**라는 키워드는 build.gradle에서 사용하는 전역변수를 설정하겠다는 것인데, 여기서는 springBootVersion 전역변수를 생성하고 그 값을 '2.1.7.RELEASE'로 하겠다는 의미이다. 즉, spring-boot-gradle-plugin이라는 Spring boot gradle의 2.1.7.RELEASE 버전 플러그인을 의존성으로 받는다.

## 📝plugin 추가
아래의 코드는 앞서 선언한 플러그인 의존성들을 적용할 것인지 결정하는 코드이다.
```
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'
```
io.spring.dependency-management 플러그인은 스프링 부트의 의존성을 관리해 주는 플러그인으로 꼭 추가해야만 한다. 앞 4개의 플러그인은 자바와 스프링 부트를 사용하기 위해서는 필수 플러그인들이니 항상 추가하면 된다. 

## 📝dependencies 수정
나머지 코드는 다음과 같다.

```
repositories{
    mavenCentral()
    jcenter()
}
    
dependencies {
    compile('org.springframework.boot:spring-boot-starter-web')
    testCompile('org.springframework.boot:spring-boot-starter-test')
}
```

repositories는 각종 의존성(라이브러리)들을 어떤 원격 저장소에서 받을지를 정한다. 기본적으로 mavenCentral을 많이 사용하지만, 최근에는 라이브러리 업로드 난이도 때문에 jcenter도 많이 사용한다.
mavenCentral은 이전부터 많이 사용하는 저장소지만, 본인이 만든 라이브러리를 업로드 하기 위해서는 정말 많은 과정과 설정이 필요하다.
jcenter은 이런 문제점을 개선하여 라이브러리 업로드를 간단하게 하였다. 또한, jcenter에 라이브러리를 업로드하면 mavenCentral에 업로드 될 수 있도록 자동화를 할 수 있다.

dependencies는 프로젝트 개발에 필요한 의존성들을 선언하는 곳이다. 여기서는 org.springframework.boot:spring-boot-starter-web과 org.springframework.boot:spring-boot-test를 받도록 선언되어 있다. 인텔리제이는 메이븐 저장소의 데이터를 인덱싱해서 관리하기 때문에 커뮤니티 버전을 사용해도 의존성 자동완성이 가능하다.

이 코드를 적용한 전체 코드는 다음과 같다.
```
buildscript{
    ext{
        springBootVersion = '2.7.1.RELEASE'
    }

    repositories{
        mavenCentral()
        jcenter()
    }

    dependencies{
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'

group 'com.webspring.book'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    compile('org.springframework.boot:spring-boot-starter-web')
    testCompile('org.springframework.boot:spring-boot-starter-test')
}

```
---
# 🤦‍♂️실행 에러
이제 성공적으로 반영될 것이라 생각했는데, 위 코드를 동작하면 2가지 에러가 발생한다.

## 👀 Could not find...plugin
>Could not find org.springframework.boot:spring-boot-gradle-plugin:2.7.1.RELEASE

위의 에러는 spring-boot-gradle-plugin:2.7.1.RELEASE 플러그인을 찾을 수 없다는 뜻이다. 앞서 우리는 ext 키워드를 사용하여 springBootVersion 전역 변수를 생성했고, 2.7.1. RELEASE 값을 담았다. 그런데 왜 찾을 수 없을까..?하고 다시 한 번 코드를 살펴보니, 내가 원하는 것은 2.1.7.RELEASE 버전인데 2.7.1.RELEASE 값을 담았기 때문이었다.

## 👀 Could not find method compile 
> Could not find method compile() for arguments

위의 에러는 compile() 메소드를 찾을 수 없다는 뜻이다. 이유는 Gradle의 버전이 올라가면서 Gradle 4.10(2018년 8월 27일)부터 compile, testCompile, runtime, testRuntime 기능이 사용되지 않았고 Gradle 7.0(2021년 4월 9일)부터 제거되었기 때문이다.
이는 implementation, testImplementation, testRuntime, testRuntimeOnly 으로 대체되었다.
참고 사이트 : [The Java Plugin](https://docs.gradle.org/4.10/userguide/java_plugin.html#sec:java_plugin_and_dependency_management)

수정된 전체 코드는 아래와 같다.

```
buildscript{
    ext{
        springBootVersion = '2.1.7.RELEASE'
    }

    repositories{
        mavenCentral()
        jcenter()
    }

    dependencies{
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'

group 'com.webspring.book'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation('org.springframework.boot:spring-boot-starter-web')
    testImplementation('org.springframework.boot:spring-boot-starter-test')
}
```

이제 다시 build.gradle을 실행하면 성공적으로 실행이 완료된 것을 확인할 수 있다.

```
Deprecated Gradle features were used in this build, making it incompatible with Gradle 8.0.

You can use '--warning-mode all' to show the individual deprecation warnings and determine if they come from your own scripts or plugins.

See https://docs.gradle.org/7.4/userguide/command_line_interface.html#sec:command_line_warnings

BUILD SUCCESSFUL in 126ms
1 actionable task: 1 executed
오전 1:40:09: 실행이 완료되었습니다.
```

[warning에 대한 설명](https://parkho79.tistory.com/175)

# 📌Gradle 소스 세트 사용

Gradle 프로젝트를 생성할 때 IntelliJ IDEA는 두 개의 소스 세트(main 및 test)를 포함하는 기본 소스 세트 디렉토리를 자동으로 생성한다. IntelliJ IDEA는 또한 Gradle 도구 창의 종속성 노드에 컴파일 및 런타임 구성을 표시한다.
![Gradle source sets](https://resources.jetbrains.com/help/img/idea/2022.2/gradle_main_source_set.png)

