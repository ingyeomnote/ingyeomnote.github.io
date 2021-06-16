---
title:  "Spring Framework의 기본 개념"
excerpt: "SpringFramework와 IoC, DI, DL에 대해서 정리했다."

toc: true
toc_sticky: true

categories:
  - Spring
tags:
  - Spring
  - IoC
  - DI
  - DL
  - 의존성
last_modified_at: 2021-06-16T23:32:00
---



## **Spring Framework란?**

자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크로서 엔터프라이즈급 애플리케이션을 개발하기 위한 모든 기능을 종합적으로 제공하는 **경량화**된 솔루션이다.

솔루션이란?  : 특정한 상황에 대한 해결 방안. 사용자의 요구에 대응되는 H/W , S/W, Skill 등

(ex. ERP, DBMS, POS등..)

엔터프라이즈급 개발이란 뜻대로만 풀이하면 기업을 대상으로 하는 개발이라는 말이다. 즉, 대규모 데이터 처리와 트랜잭션이 동시에 여러 사용자로 부터 행해지는 매우 큰 규모의 환경을 엔터프라이즈 환경을 뜻한다.

Spring Framework는 **경량 컨테이너**로 자바 객체를 담고 직접 관리한다. 객체의 생성 및 소멸 그리고 라이프 사이클을 관리하며 언제든 **[Spring 컨테이너](https://www.notion.so/Spring-Container-b3aa5e2a724e46998e7f1bd471daf748)로부터 필요한 객체**를 가져와 사용할 수 있다. 

이는 Spring이 IoC 기반의 Framework임을 의미한다.



## Spring Framework는 IoC기반이다. IoC란?

**IoC**는 Inversion of Control의 약자로 말 그대로 **제어의 역전**이다. 제어의 역전은 'Framework와 Library'글에서 설명했지만 여기서 더 자세히 공부해보자

일반적으로 지금까지의 프로그램은 **객체 결정 및 생성 → 의존성 객체 생성 → 객체 내의 메소드 호출**하는 작업을 반복했다. 이는 각 객체들이 프로그램의 흐름을 결정하고 각 객체를 구성하는 작업에 직접적으로 참여한 것으로 **모든 작업을 사용자가 제어하는 구조이다.**

하지만, IoC에서는 이 흐름의 구조를 바꾼다. IoC에서의 객체는 자기가 사용할 객체를 선택하거나 생성하지 않는다. 또한 자신이 어디서 만들어지고 어떻게 사용되는지도 모른다. 자신의 모든 권한을 다른 대상에 위임함으로써 제어권한을 위임받은 특별한 객체에 의해 결정되고 만들어진다.

**즉, IoC란 기존 사용자가 모든 작업을 제어하던 것을 특별한 객체에 모든 것을 위임하여 객체의 생성부터 생명주기 등 객체에 대한 제어권이 넘어간 것을 IoC, 제어의 역전이라고 한다.**

![https://user-images.githubusercontent.com/79016263/121176682-785e6080-c897-11eb-993b-30ddc70ee54e.png](https://user-images.githubusercontent.com/79016263/121176682-785e6080-c897-11eb-993b-30ddc70ee54e.png)



## 의존성이란?

DI와 DL에 대해 알아보기 전에 **의존성**을 먼저 알아보자. 이해하기 쉽도록 예를 들어보자면 RPG게임을 하는 도중에 게임 플레이어가 여러 무기를 활용하여 게임을 즐기게 되는데 이것을 코드 상으로 나타내면 플레이어를 나타내는 Player 클래스와 무기를 나타내는 Weapon 클래스로 작성하게 될 것이다.

Player는 무기를 꼭 필요로 하기 때문에 Weapon클래스 객체를 꼭 가지고 있어야 하는데, 이것을 나타내는 용어가 바로 의존성(Dependency)이다. 어떤 특정 객체가 존재하기 위해 꼭 필요로하는 것!그리고 그 관계를 말한다. 

코드 상에서는 new 키워드로서 객체간의 의존성이 생성된다.

```java
class Player {
    private Weapon weapon;

    Plyaer(){
    }

    void setWeapon(){
        this.weapon = new Weapon();
    }
    // 나머지 코드
}
```

하지만 이러한 방식은 객체 간의 **강결합(Tightly Coupled)**으로 묶여지면서 코드의 유연성을 떨어트리고 코드의 중복 및 가독성을 떨어뜨리는 원인이 된다. 만약 Weapon클래스가 Weapon만 아닌 Knife, Gun 등 여러 종류의 무기를 사용하게 될 경우 코드 상에 무수한 if else문이 작성되게 되고 그 부담이 프로그래머에게 돌아오게 된다.

이러한 강결합, 높아진 결합도를 없애고 코드의 유연성을 확보하는 방법 중 하나가 제어 역전이다.

```java
class Knife implements Weapon{

    public void useWeapon() {
        System.out.println("Use Knife");
    }
}

class Gun implements Weapon{

    public void useWeapon() {
        System.out.println("Use Gun");
    }
}

class Spike implements Weapon{

    public void useWeapon() {
        System.out.println("Use Spike");
    }
}

interface Weapon {

    void useWeapon();
}

public class Player {

    private Weapon weapon;

    Player(){
    }

    public void setWeapon(Weapon weapon) {
        this.weapon = weapon;
    }

    public void usePlayerWeapon() {
        weapon.useWeapon();
    }
}

public class Main {

    public static void main(String[] args) {
        // TODO Auto-generated method stub
        Player player = new Player();

        player.setWeapon(new Gun());
        player.usePlayerWeapon();

        player.setWeapon(new Spike());
        player.usePlayerWeapon();

        player.setWeapon(new Knife());
        player.usePlayerWeapon();
    }
}

    // Result : 
    Use Gun
    Use Spike
    Use Knife
```



![DI](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile27.uf.tistory.com%2Fimage%2F998B76365BDE51F126F04D)



위와 같이 코드가 설계되면 Player가 특정 상황에서 어떤 무기가 필요하다고 하면 코드 상에서 if else문이 아니라 interface에 맞춰서 구현된 무기들을 인스턴스에 갈아 끼우면 되므로 코드가 유연해지고 가독성이 높아질 뿐만 아니라 코드의 중복이 필연적으로 제거된다.



## IoC 컨테이너(Container)

스프링(Spring)에서는 위와 같은 Knife, Gun, Spike 등을 컨테이너(Container)라는 곳에 Bean이라는 인스턴스 형태로 관리한다. xml이나 java config 파일로부터 설정값들을 입력한 후 이 정보를 토대로 스프링이 컨테이너를 생성하여 설정 파일들에 등록되어 있는 Bean 객체들을 관리한다. 

또한, 각 Bean이 가지고 있는 의존성의 정보가 입력된 설정파일을 토대로 컨테이너가 생성될 시 제어의 역전(IoC) 원칙을 지키면서 자동으로 해당 의존성을 주입해준다.

이것을 스프링에서는 해당 컨테이너를 IoC컨테이너라고 부르고 이런 의존성을 주입시키는 행위를 DI(Dependency Injection)라고 부른다. 

이제 의존성에 대해서는 어느정도 파악이 완료되었으니 **DI와 DL**에 대해 알아보자.



## IoC의 구성요소 DI와 DL

**DL**(Dependency Lookup) - 의존성 검색 :

컨테이너에서는 객체들을 관리하기 위해 별도의 저장소에 Bean을 저장하는데 이렇게 저장되어 있는 Bean에 접근하기 위해 특정 컨테이너가 제공하는 API를 이용하여 Bean을 Lookup하는 것이다.

DL 사용시 컨테이너의 종속성 증가

**DI**(Dependency Injection) - 의존성 주입 :

의존성 주입이란 객체가 서로 의존하는 관계가 되게 의존성을 주입하는 것이다. 객체지향프로그래밍에서 의존성이란 하나의 객체가 어떠한 다른 객체를 사용하고 있음을 의미한다. 따라서 IoC에서 DI는 각 클래스 사이에서 필요로 하는 의존관계를 빈 설정 정보를 바탕으로 컨테이너가 자동으로 연결해 주는 것이다. **즉, 객체가 필요로하는 어떤 객체를 생성자(Constructor) 혹은 새터(Setter)를 통해서 주입하는 것을 말한다.**

의존관계는 간단히 말해 **new 라는 키워드를 통해 생성된다**. 코드를 짤 때 이러한 강결합(Tightly Coupled)를 일으키는 요소를 무분별하게 짜게 된다면 나중에 어마어마한 유비보수 비용을 지불할 때가 온다.  쉽게 생각해서 일체형 배터리와 분리형 배터리에서 어떤 것이 나중에 배터리를 갈아끼울 때 편리할까? 여기서 갈아끼운다는 것이 소프트웨어서 유지보수에 해당한다. 그리고 일체형 배터리에서 기계와 배터리의 관계는 강결합 관계이다. 나중을 생각하면 이 둘을 교체할 것을 대비했을 때 둘의 관계를 약하게 하는 것이 더 효율적이다.

즉, **의존성 주입은 제어 역전의 원칙 하에 객체 간의 결합을 약하게 해주고 좋은 코드를 만들어준다.**

앞서 의존성에 대해 설명할 때는 일반적인 java의 DI를 보여주었고 이제 알아볼 것은 스프링 컨테이너를 통한 의존성 주입이다.



## 스프링을 통한 컨테이너 생성 및 의존성 주입

스프링에서는 xml 혹은 java config, annotation을 통해서 컨테이너 설정 정보 및 bean 객체 그리고 의존성 관계 정보를 입력할 수 있다. 여기서 bean은 스프링 컨테이너 상에서 생성된 객체라고 보면된다. 또한 컨테이너는 스프링에서 IoC원칙을 통한 객체와 그 의존성들을 관리하기 위해 만든 요소다. bean객체들이 들어가있는 통이라고 쉽게 생각해도 된다.

이 bean은 말했듯이 IoC 컨테이너 상에서 관리된다. 스프링 설정파일에 등록된 bean은 프로그램 실행 도중에 변경될 수 없다.

위 코드를 스프링에서 xml 파일로 설정해서 똑같이 구현해볼 것이다.

```java
<-- appContext.xml -->
<-- 스프링 xml 설정파일 -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="gun" class="com.tutorial.spring.Gun"/>
    <bean id="knife" class="com.tutorial.spring.Knife"/>
    <bean id="spike" class="com.tutorial.spring.Spike"/>

    <bean id="gunPlayer" class="com.tutorial.spring.Player">
        <constructor-arg ref="gun"/>
    </bean>
    <bean id="knifePlayer" class="com.tutorial.spring.Player">
        <property name="weapon" ref="knife"/>
    </bean>
    <bean id="spikePlayer" class="com.tutorial.spring.Player">
        <property name="weapon" ref="spike"/>
    </bean>

</beans>

 * Container : 담는 그릇(bean을 저장하고 관리하는 객체)
 * Content   : 기능
 * Context   : 기능을 구현하기 위한 정보(설정)
```

위 appContext.xml 파일은 **스프링 xml 설정 파일**이다. 이 xml 파일을 통해 컨테이너 정보를 입력하고 이 컨테이너가 관리할 bean 객체들을 등록한다. 

bean 객체를 생성할 때 **id**와 **class** 정보를 입력해야한다. id는 bean 객체의 식별자가 되며, class는 스프링 객체인 bean의 클래스를 지정하는 속성이다. 속성을 주입하는 것도 두 가지 방법이 있다. **생성자를 통한 방법과** **새터(Setter)**를 통한 방법이다. 생성자를 통한 방법은 <constructor-arg>, Setter를 통한 방법은 <property> 태그를 통해 정보를 입력하면 된다. 주입할 bean 객체의 정보는 ref="[ID명]"을 속성에 입력하면 된다.

아래 코드는 자바 코드 상에서 스프링 컨테이너를 생성한 후 컨테이너에 생성된 bean객체를 가져와서 사용하는 예시다.

```java
public interface Weapon {
    public void useWeapon();
}

class Knife implements Weapon{

    public void useWeapon() {
        System.out.println("Use Knife");
    }
}

class Spike implements Weapon{

    public void useWeapon() {
        System.out.println("Use Spike");
    }
}

public interface Weapon {
    public void useWeapon();
}

public class Player {

    private Weapon weapon;

    Player(){
    }

    public Player(Weapon weapon) {
        super();
        this.weapon = weapon;
    }

    public void setWeapon(Weapon weapon) {
        this.weapon = weapon;
    }

    public Weapon getWeapon() {
        return weapon;
    }

    public void usePlayerWeapon() {
        weapon.useWeapon();
    }
}

import org.springframework.context.support.GenericXmlApplicationContext;

public class Main {

    public static void main(String[] args) {
        GenericXmlApplicationContext ctx = new GenericXmlApplicationContext("classpath:appContext.xml");

        Player gunPlayer = ctx.getBean("gunPlayer", Player.class);
        gunPlayer.usePlayerWeapon();

        Player spikePlayer = ctx.getBean("spikePlayer", Player.class);
        spikePlayer.usePlayerWeapon();

        Player knifePlayer = ctx.getBean("knifePlayer", Player.class);
        knifePlayer.usePlayerWeapon();
    }
}

// 결과
11월 28, 2018 12:12:04 오전 org.springframework.beans.factory.xml.XmlBeanDefinitionReader loadBeanDefinitions
정보: Loading XML bean definitions from class path resource [appContext.xml]
11월 28, 2018 12:12:04 오전 org.springframework.context.support.AbstractApplicationContext prepareRefresh
정보: Refreshing org.springframework.context.support.GenericXmlApplicationContext@439f5b3d: startup date [Wed Nov 28 00:12:04 KST 2018]; root of context hierarchy
11월 28, 2018 12:12:04 오전 org.springframework.beans.factory.support.DefaultListableBeanFactory preInstantiateSingletons
정보: Pre-instantiating singletons in org.springframework.beans.factory.support.DefaultListableBeanFactory@59a6e353: defining beans [gun,knife,spike,gunPlayer,knifePlayer,spikePlayer]; root of factory hierarchy
Use Gun
Use Spike
Use Knife
```

**GenericXmlApplicationContext**는 xml 설정파일을 통한 스프링 컨테이너를 생성해주는 객체다. 이 객체를 통해 스프링 컨테이너에 접근할 수 있다.

스프링 컨테이너의 bean객체에 접근할 때는**getBean**을 쓴다. 이 때, getBean에 클래스 정보를 입력하면 번거롭고 불필요한 Type-casting을 하지 않아도 된다.

실행시키면 결과에서 알 수 있듯이 스프링 프레임워크가 구동되면서 컨테이너를 생성한다. 그리고 이 생성된 컨테이너를 기반으로 위 코드가 실행된다.

마지막으로 DI의 장점에 대해 간단하게 요약하자면 다음과 같다.

- Unit Test가 용이해진다.
- 코드의 재활용성이 높아진다.
- 객체 간의 의존성을 줄이거나 없앨 수 있다.
- 객체 간의 결합도가 낮아지면서 유연한 코드를 만들 수 있다.