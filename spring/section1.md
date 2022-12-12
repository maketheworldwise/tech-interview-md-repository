- [IOC](#ioc)
- [DI](#di)
- [AOP](#aop)
- [PSA](#psa)
- [Bean](#bean)
- [ApplicationContext](#applicationcontext)
- [BeanFactoryPostProcessor vs BeanPostProcessor](#beanfactorypostprocessor-vs-beanpostprocessor)
- [Component Scanning](#component-scanning)
- [@Primary, @Qualifier, 모든 빈 주입](#primary-qualifier-모든-빈-주입)
- [Servlet Container vs IoC Container](#servlet-container-vs-ioc-container)
- [DispatcherServlet](#dispatcherservlet)
- [Servlet Filter vs HandlerInterceptor](#servlet-filter-vs-handlerinterceptor)
- [전체적인 동작 과정](#전체적인-동작-과정)

---

### IOC

> 💁🏻 : IOC는 Inversion Of Control로 스프링 컨테이너가 모든 빈을 관리하는 것을 의미한다.

IOC는 개발자가 아닌 스프링이 코드의 흐름을 역으로 제어해준다고 하여 제어의 역전이라한다. 스프링이 제어해주기 때문에 개발자의 부담은 덜어지고 SOLID 원칙을 보다 쉽게 지키면서 변경에 유연한 코드 구조로 개발을 할 수 있다는 장점을 가지고 있다. 즉, 인스턴스의 생성 및 소멸을 개발자 대신 스프링 컨테이너가 해주는 것을 IOC라 한다.

### DI

> 💁🏻 : DI는 Dependency Injection으로 의존성을 주입해주는 용어다. 스프링에서는 IOC를 구체적으로 DI라는 방식을 통해 의존성 역전 제어를 하고 있다.

DI는 IOC를 실제로 구현하는 방법이다. 코드를 통해 객체간의 의존관계를 명시하는 것이 아닌 스프링 설정 파일에 등록된 정보를 바탕으로 컨테이너가 자동으로 처리해준다. 즉, 스프링 컨테이너가 직접 객체들간의 의존관계를 처리하는 것을 DI라 한다.

### AOP

> 💁🏻 : AOP는 관점 지향 프로그래밍으로 공통 로직을 분리하여 개발하는 방법을 의미한다.

AOP는 로깅, 트랜잭션, 보안 등과 같이 반복적으로 수행하는 횡단 관심사들과 핵심 관심사들을 분리하고 한 곳에 모아 관리하는 방법을 의미한다. AOP로 구현한 코드는 추가 개발과 유지보수 관점에서 유리해지고 단일 책임 원칙을 적용할 수 있게 된다.

AOP는 프록시를 사용하고 인터페이스와 런타임 기반으로 동작한다. AOP에는 `Pointcut, JoinPoint, Advice, Aspect, Advisor` 키워드가 등장한다. Aspect라는 용어를 나열해서 본다면 다음과 같다.

- Aspect = Advice(언제, 무엇을) + Pointcut(어디에)

### PSA

> 💁🏻 : PSA는 어댑터 패턴 기반으로 동작하는 기법이다.

PSA는 Portable Service Abstraction으로 일관성 있는 추상화라고 한다. 서비스 추상화는 JDBC처럼 어댑터 패턴을 활용하여 같은 일을 하는 다수의 기술을 공통의 인터페이스로 제어할 수 있게 하는 것을 의미한다. 스프링에서는 대표적으로 OXM, ORM, 캐시, 트랜잭션 등 다양한 기술에 대한 PSA를 제공한다.

### Bean

> 💁🏻 : Bean은 객체를 의미한다.

Bean은 스프링 IOC 컨테이너가 관리하고 생성하는 객체를 의미한다. `new`키워드로 생성한 객체는 Bean이라고 할 수 없다. Bean들을 이용해야만 의존성이 주입할 수 있다. 대표적으로 Bean을 등록하는 방법은 `@Component, @Service, @Controller, @Repository, @Configuration`을 이용하면 되고 Bean을 꺼낼때는 `@Autowired` 어노테이션을 붙여서 주입할 수 있다.

Bean은 기본적으로 싱글톤 스코프로 등록된다. 따라서 애플리케이션에서 싱글톤 빈을 받아와 사용한다면 메모리와 런타임 성능 최적화에 유리해진다.

### ApplicationContext

> 💁🏻 : Bean을 관리해주는 인터페이스다.

IOC 컨테이너의 가장 최상위에 있는 인터페이스는 `BeanFactory`다. `BeanFactory`는 IOC 컨테이너의 가장 핵심적인 클래스이며, 다양한 라이프사이클 인터페이스들이 존재하는 것을 확인할 수 있다. 이러한 라이프사이클 인터페이스를 이용하면 컨테이너 내부의 빈들을 조작하는 등 다양한 작업을 수행할 수 있다.

그리고 `ApplicationContext`는 `BeanFactory`에 비해 더 많은 기능을 가지고 있는 인터페이스다. 참고로 `ApplicationContext`는 스프링 부트의 `main()`에서 `run()` 메서드를 통해 생성된다.

### BeanFactoryPostProcessor vs BeanPostProcessor

> 💁🏻 : `BeanFactoryPostProcessor`는 Bean이 생성되기 전에 실행되고 `BeanPostProcessor`는 Bean이 생성되는 과정에서 실행되는 것이다.

이 두 인터페이스의 차이는 함수형 인터페이스냐 아니냐의 차이도 있지만 더 정확한건 실행되는 시기다. `BeanFactoryPostProcessor`는 Bean에 대한 조작을 하기도 전에 처리하는 것을 의미하고 `BeanPostProcessor`는 Bean이 초기화되는 시점 전 후로 처리를 하는 것을 의미한다.

### Component Scanning

> 💁🏻 : 어노테이션을 기반으로 의존성 주입을 할 때 사용하는 방법이다.

Component Scanning에는 대표적으로 `@Component, @Repository, @Service, @Controller, @Configuration`이 있다. Component Scanning을 사용하려면 XML 파일에 정의를 해주거나 자바 설정 파일을 이용해 등록할 수 있는데, 후자의 방법이 많이 사용하는 편이다.

자바 설정 파일을 이용할 경우 `@ComponentScan` 어노테이션을 이용한다. 이때 살펴봐야하는 내용은 `@Filter`를 통해 어떤 어노테이션을 스캔할지 혹은 스캔하지 않을지에 대한 것과 스캔할 `basePackage` 설정 정도다. 참고로 `basePackage`에서 문자열로 처리하는 것보다 `basePackageClasses`를 이용하는 것을 권장한다.

스프링 부트에서는 `@ComponentScan` 어노테이션을 직접 사용하지 않았음에도 `@Component, @Repository, ...`등의 어노테이션으로 빈을 등록할 수 있다 생각할 수 있는데, 사실 `@SpringBootApplication` 어노테이션에 이미 지정되어있다.

이해하기 쉽지 않다면 Component Scanning에서 가장 중요한 점은 범위라는 것을 기억하자. 특정 클래스를 만들어 구성한다고 했을 때, Component Scanning의 범위 밖에 있을 경우 빈으로 등록이 안될 수 있기 때문이다.

### @Primary, @Qualifier, 모든 빈 주입

> 💁🏻 : 의존성을 주입할 때 발생할 수 있는 문제를 해결하기 위해 사용하는 어노테이션이다.

의존성을 주입할 때 발생할 수 있는 경우는 3가지다.

1. 의존성 주입할 때 해당 타입의 빈이 없는 경우
2. 의존성 주입할 때 해당 타입의 빈이 한 개인 경우
3. 의존성 주입할 때 해당 타입의 빈이 한 개 이상인 경우

1번은 당연히 에러가 발생하고, 2번은 문제없이 돌아간다. 다만 동일한 타입의 빈이 2개일 경우에는 해당 빈의 이름으로 주입을 시도한다. 3번에 대한 해결책은 `@Primary, @Qualifier`를 이용하여 해결하거나 모든 타입의 빈을 주입하는 방법이 있다.

`@Primary`를 이용해 우선적으로 주입할 빈을 지정할 수 있고, `@Qualifier`로 빈의 이름으로 주입받아 해결할 수 있다. 모든 타입의 빈을 주입하는 방법은 현업에서 많이 사용한다고는 하는데, List 자료구조에 모두 담아 해결하는 방법이다.

### Servlet Container vs IoC Container

> 💁🏻 : Servlet 컨테이너는 일련의 클라이언트의 요청들을 관리하는 것이다. 반대로 IoC 컨테이너는 Bean들의 생명주기를 관리하는 기능을 제공한다.

Servlet은 웹 요청과 응답의 흐름을 간단한 메서드 호출만으로 체계적으로 다루는 것을 의미한다. 따라서 Servlet 컨테이너는 개발자가 웹 서버와 통신하기 위해 소켓을 생성하고 특정 포트에 리스닝하고 스트림을 생성하는 등의 복잡한 일들을 할 필요가 없도록 해준다. Servlet 컨테이너는 요청이 들어올 때마다 새로은 자바 쓰레드를 생성한다. 자바 쓰레드는 무제한 만들 수 없기 때문에 쓰레드풀로 미리 쓰레드를 만들어두고 요청을 할당하여 스택에 할당한다. 쓰레드를 다 사용하면 스택을 제거하고 노는 상태로 만든다. 대표적인 Servlet 컨테이너는 톰캣이다. 톰캣같은 WAS가 Java 파일을 컴파일해서 Class로 만들고 메모리에 올려 Servlet 객체를 만든다.

IoC 컨테이너는 위에서 언급했듯, 등록한 Bean들을 관리해주는 역할을 수행한다. `BeanFactory`는 스프링 설정 파일에 등록된 객체를 생성하고 관리하는 가장 기본적인 컨테이너 기능을 제공한다. 그리고 컨테이너가 구동될 때 객체를 생성하는 것이 아닌 클라이언트로부터의 요청에 의해서만 객체를 생성한다. (Lazy Loading)

`ApplicationContext` 컨테이너는 `BeanFactory`를 확장하여 트랜잭션 관리나 메시지 기반 다국어 처리 등 다양한 기능을 제공한다. 또한 컨테이너가 구동되는 시점에 등록되어있는 클래스들을 객체화하는 즉시 로딩 방식으로 동작한다. `ApplicationContext`의 대표적인 구현체로는 `GenericXmlApplicationContext` 클래스가 있다.

### DispatcherServlet

> 💁🏻 : DispatcherServlet은 Front Controller 패턴으로, 공통적으로 처리하는 부분을 해결하기 위한 클래스다.

Front Controller 패턴은 하나의 Controller가 모든 요청을 처리하도록 구성된 패턴이다. 그리고 이 모든 요청을 처리할 Controller에서 각 요청을 처리할 Handler에게 분배하는데, 이를 Dispatch라 한다. 즉, 스프링에서는 이러한 Controller가 수행하는 Servlet을 구현해두었는데 이를 DispatchServlet이라 한다.

공식문서를 살펴보면 DispatcherServlet은 Servlet `Root WebApplicationContext`와 `Servlet WebApplicationContext` 개념으로 분리되는 것을 볼 수 있다. `Root WebApplicationContext`는 다른 DispatcherServlet에서 공용으로 사용이 가능하다면, `Servlet WebApplicationContext`는 해당 DispatcherServlet에서만 사용이 가능하다.

![](/docs/images/dispatcher-servlet.png)

본래라면 위의 이미지처럼 `Repositories, Services`과 `Controllers, ViewResolver, HandlerMapping`들은 분리해야한다. 즉, `Repositories, Services`는 `Root WebApplicationContext`에 등록되어야 하고 `Controllers, ViewResolver, HandlerMapping`은 `Servlet WebApplicationContext`에 등록되어야 한다는 의미다. 분리를 해줄 때는 `@ComponentScan`의 `excludeFilter` 속성을 이용하면 된다. 물론 DispatcherServlet과 다른 Servlet이 정의되어있는 경우가 아니라면 반드시 분리해야하는 것은 아니다.

DispatcherServlet 하나만 Servlet으로 등록해주고 `Servlet WebApplicationContext`가 모든 `Controllers, Repositories, Services, ...`등을 모두 가지도록 설정해도 무관하다. 단, 하나의 설정 파일 만이 사용되므로 해당 설정 파일이 모든 클래스에서 Component Scanning이 되도록 구성해야한다. 대다수 하나의 DispatcherServlet으로만 사용한다고하니 크게 신경쓸 필요는 없다.

DispatcherServlet이 적용된 과정도 알아두면 좋다.

**web.xml**

서버가 처음 로딩될 때는 `web.xml` 파일에 작성한 내용을 읽어들여 해당 환경 설정에 대해 톰캣에 적용하여 서버를 시작한다.

**ContextLoaderListener**

Servlet에서는 IOC 컨테이너를 연동해야한다. 이를 위해 `web.xml`에 스프링에 제공해주는 `ContextLoaderListener`를 리스너에 등록해준다. 이 리스너는 Servlet의 생명 주기에 맞춰 스프링이 제공하는 `ApplicationContext`를 연동시켜주는 가장 핵심적인 리스너다. 즉, Servlet이 IOC 컨테이너의 빈을 이용하기 위해 Servlet 컨테이너에 등록해주는 역할을 수행한다.

**ApplicationContext**

Servlet 컨테이너의 Servlet들이 스프링의 `ApplicationContext`를 이용하려면 `ServletContext`에 등록되어야한다. 위에서는 리스너만 등록했기 때문에 스프링 설정 파일을 통해 `ApplicationContext`를 생성해주어야 한다. 그리고 앞에서 설정한 리스너가 생성한 `ApplicationContext`를 바라보도록 `contextConfigLocation`을 통해 연결을 수행해준다.

### Servlet Filter vs HandlerInterceptor

> 💁🏻 : `HandlerInterceptor`는 Handler를 실행하기 전과 후 그리고 완료 시점에 추가적인 작업을 하고 싶을 때 사용한다.

`Servlet Filter` 보다 `HandlerInterceptor`가 더 구체적인 처리가 가능하다는 머에서 차이가 있다. `Servlet Filter`는 일반적인 용도의 기능을 구현하는데 사용하는게 좋다. 대표적인 예시로는 XSS 공격에 대해 Lucy-XSS 같은 오픈소스를 사용할 때 `Servlet Filter`를 사용한다.

### 전체적인 동작 과정

> 💁🏻 : 요청이 들어왔을 때의 흐름은 다음과 같다.
>
> 1. DispatcherServlet이 클라이언트의 요청을 받는다.
> 2. 요청을 처리할 Controller가 있는지 HandlerMapping을 통해 검사한다.
> 3. HandlerMapping을 통해 찾은 Controller를 직접 실행시킬 HandlerAdapter 목록을 찾는다.
> 4. 찾은 HandlerAdapter로 Controller를 실행시킨다.
> 5. ModelAndView를 반환받고 ViewResolver를 통해 결과를 출력할 View를 찾는다.
> 6. 찾은 View를 반환받아 클라이언트에게 전달한다.
>
> ![](/docs/images/dispatcher-servlet-process.png)

- 참고 이미지 1
  ![](/docs/images/spring-action-process1.png)
- 참고 이미지 2
  ![](/docs/images/spring-action-process2.png)
