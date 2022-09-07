---
title:  "[이동준] build.gradle[1/2]"
excerpt: "build.gradle에 대해 알아보자"

toc: true
toc_sticky: true

categories:
- 스프링 부트와 AWS로 혼자 구현하는 웹 서비스

tags:
  - Java
  - SpringBoot
  - IntelliJ
  - build.gradle

last_modified_at: 2022-09-02T22:14:00
---



IntelliJ에서 프로젝트를 생성할 때 Build system으로 gradle을 선택하면 build.gradle 파일이 자동 생성된다. 

# 📂build.gradle


`build.gradle`은 파일 자체가 Project Object이다. 

여기서 말하는 Project Object는 Project Interface를 구현하는 구현체인데, Project 단위에서 필요한 작업을 수행하기 위해 모든 메서드와 프로퍼티를 모아놓은 슈퍼 객체이다.

```
public interface Project extends Comparable<Project>, ExtensionAware, PluginAware{
	...
}
```
우리가 build.gradle에 작성하는 수 많은 코드들은 모두 Project Object의 프로퍼티와 메서드가 되며, Project Object는 Project 이름부터 변수, 메서드를 모두 포함하는 객체가 된다.

Project 오브젝트는 내부에 수 많은 메서드(Methods)와 속성(Properties)을 가지고 있다. 
메서드 중에 대표적인 것은 java application용 build.gradle이 가진 plugins, repositories, dependencies, application 메서드이다.

우리가 Gradle Task를 이용해 java application을 빌드하게 되면 build task는 이 메서드들을 수행시킨다.

```
plugins{ // method of project object
	id 'application'
}

repositories{ // method of project object
	mavenCentral()
}

dependencies{ // method of project object
	testImplementation 'junit:junit:4.13.2'
    implementation 'com.google.guava:guava:30.1.1-jre'
}

application{ // method of project object
	mainClass = 'JavaProject.App'
}
```

위에서 {}으로 감싸여진 부분은 메서드의 인자로 받아지는 Groovy의 클로저(Closure)인데, Groovy의 클로저는 Java나 Kotilin의 람다와 같다.

따라서 {} 블록 내부의 메서드들은 아래 사진과 같이 메서드의 인자로 넘겨질 수도 있다.

```
plugins({
	id 'application'
})

repositories({
	mavenCentral()
})

dependencies({
	testImplementation 'junit:junit:4.13.2'
    implementation 'com.google.guava:guava:30.1.1-jre'
})

application({ 
	mainClass = 'JavaProject.App'
})
```



즉 앞서 두 개의 코드는 같은 코드이다.

---

## 📝build.gradle의 프로퍼티

build.gradle에는 Project 객체를 위한 프로퍼티를 정의할 수 있다.

프로퍼티를 정의하는 것은 간단하다. 아무 곳에서나 다음 문법을 사용하면 된다.

```
project.[프로퍼티명] = [값]

혹은 project를 생략하고 쓸 수도 있다.

[프로퍼티명] = [값]
```

예를 들어 Project 객체의 group을 재정의 하고 싶다고 하면 다음과 같이 쓰면 된다. 
여기서 [group = project.group]은 동일한 의미를 갖는다.

```
group = 'com.example'
project.group = 'com.springweb'

repositories{
	println group // 출력 com.springweb
    mavenCentral()
}
```

하지만 이렇게 지정하는 것은 Project 객체에 미리 정의된 프로퍼티만 정의하는 것이 가능하다. 직접 프로퍼티를 만들려면 다른 방법을 사용해야 한다.

---

## 📝커스텀 프로퍼티 만들기

커스텀 프로퍼티를 만들기 위해서는 project 객체의 extension을 넣는 방식을 사용한다.
아래 코드는 porject.ext를 통해 extension에 접근한다.
```
project.ext.[커스텀 프로퍼티명] = [값]
```

project.ext에 넣어진 변수는 Groovy의 특수한 문법을 사용해 project 객체에서 직접 접근이 가능하다.
```
project.[커스텀 변수명]
```

예를 들어 blogName이란 커스텀 프로퍼티를 설정한 다음 출력하기 위해서는 다음과 같이 사용할 수 있다.
```
project.ext.blogName = 'springweb'

repositories{
	println project.blogName // 출력 springweb
    mavenCentral()
}
```
---
## 📝build.gradle의 메서드

build.gradle에는 Project 객체를 위한 메서드를 정의할 수 있다. 대표적인 메서드들은 바로 build.gradle의 repositories 메서드와 dependencies 메서드이다.

```
repositories{
	mavenCentral()
}

dependencies{
	testImplementation "junit:junit:4.13.2"
    implementation "com.google.guava:guava:30.1.1-jre"
}
```

에 메서드들은 build.gradle 속에 메서드로 존재한다. 이 내부에 들어가는 Closure은 프로젝트가 빌드될 때 해당 메서드를 수행하는 task에 의해 수행된다.


위의 메서드들은 미리 빌드된 메서드들이다. 커스텀 메서드를 만들기 위해서는 메서드를 별도로 정의해야 한다.

---
## 📝커스텀 메서드 만들기

Groovy의 Lambda식인 Closure와 Gradle의 ext를 활용해 커스텀 메서드를 손쉽게 만들 수 있다.

```
ext.[메서드명] = { param1, param2 -> [메서드 바디]}

project.ext.[메서드명] = { param1, param2 -> [메서드 바디]}
```

예를 들어 blogName을 출력하는 커스텀 메서드를 만들고 싶다면 아래와 같다.

```
project.ext.getBlogName{
	return project.blogName
}
```

---

❗[참고 : 스프링부트와 AWS로 혼자 구현하는 웹 서비스, 이동욱 지음]****
