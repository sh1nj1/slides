layout: true
class: center, middle
---
# Spring WebFlux And Reactive Programming

.footnote[
  by [Soonoh](mailto:soonoh.jung@gmail.com)
]

???

Anypoint Media Dev Day 1 을 위해 만듬

---
# 내용
---

.no-bullet-center[
* Reactive Streams
* Reactor
* Spring WebFlux
* Tips
* Q & A
]

---
# 중요 개념
---
.no-bullet-center[
* Reactive System, Programming, Stream
* Asynchronous
* Stream Processing
* Non-blocking
* Back pressure
]
---
## Spring Webflux

Reactive-Stream + Web stack in Spring Framework

???

The original web framework included in the Spring Framework, Spring Web MVC, was purpose-built for the Servlet API and Servlet containers. The reactive-stack web framework, Spring WebFlux, was added later in version 5.0. It is fully non-blocking, supports Reactive Streams back pressure, and runs on such servers as Netty, Undertow, and Servlet 3.1+ containers.
* Optional with spring-webmvc
* Could use both webmvc and webflux
---
## Reactive System

Reactive System - [Reactive Manifesto](https://www.reactivemanifesto.org/)

![Reactive System](https://docs.google.com/drawings/d/e/2PACX-1vR5vszqNbA0J7FosRHflhHOVqb2Fccb_WfWU1sLRYbfOuNYgPykLOudj7a8EoKGdh0lgq7s_N0eZsk1/pub?w=689&amp;h=434)

???

Reactive System > Reactive Programming > Reactive Stream
---

## Reactive Programming

---
### History

.no-bullet[
* 2007 MS Volta => Reactive Framework 
* 2009 MS .Net RX (Reactive Extension)
* 2012 ReativeX Open Source
* 2013 Lightbend - Reactive Streams
]

---
### Concept

.no-bullet[
* Push Model - 외부에서 이벤트 주입 그에 대한 처리
* Pull Model - 외부에 상태 요청 그에 대한 처리
]

???
* push == reactive, pull == proactive?
---
## Reactive-Stream

Asynchronous Stream Processing with Non-blocking Back Pressure

---
### Asynchronous

요청이 동시에 처리되지 않고 서비스가 언제 처리할지 모른다.
반대로, 동기적(Synchronous)처리는 요청이 처리된 후에 다음 동작을 할 수 있다.

---

#### Asynchronous - 비동기식

.no-bullet[
* Client: 처리해 주세요.
* Server: 접수했습니다.
]

---

#### Asynchronous - 비동기식

.no-bullet[
* Client: 처리해 주세요. 처리되면 알려주세요.
* Server: 접수했습니다.
* Client: (스마트폰 게임 하기 :)
* Server: 처리되었습니다.
]

---
#### Asynchronous - 동기식

.no-bullet[
* Client: 처리해 주세요.
* Client: (기다리는 중…)
* Server: 처리되었습니다.
]
---
### Stream

연속되어 발생되는 데이터
---
### Non-blocking

.no-bullet-center[
* Non-blocking Algorithm - e.g. lock free
* Non-blocking IO - e.g. File IO, Network IO
]

???
CPU 가 막힘없이 일을 사용되느냐 마느냐의 문제
---
### Back Pressure

처리 가능한 양을 알려주는 매커니즘

![Back Pressure](https://docs.google.com/drawings/d/e/2PACX-1vRTluTraubSsXydTXM9TjZkiYy3s15Dkk7xIXG5E_n4CzaV3zWRlVVNvz4zn_6b6c_Oj2y-hX14EdWN/pub?w=572&h=358)

???

* [Reactive Manifesto Glossary](https://www.reactivemanifesto.org/glossary)
* [멈추지 않고 기다리기(Non-blocking)와 비동기(Asynchronous) 그리고 동시성(Concurrency)](https://tech.peoplefund.co.kr/2017/08/02/non-blocking-asynchronous-concurrency.html)
---

## Reactive-Stream Specification

---

SPI (Service Provider Interface)

서로 다른 라이브러리가 서로 통합하여 사용될 수 있도록 Service Provider 를 위한 API를 제공하고 구현에 대한 정확한 스펙을 정의한다. 실제 구현은 개별 서비스 provider
에게 맞겨 개발자가 어떤 구현체를 사용하더라도 상호호환되게 한다.

---

### 제공 되는 Component

* Publisher
* Subscriber
* Subscription
* Processor

???

이들 컴포넌트가 개발자(Business Logic 개발자)에 의해 직접 사용되지는 않는다.

---
### Publisher

```java
public interface Publisher<T> {
    public void subscribe(Subscriber<? super T> s);
}
```

---

### Subscriber

```java
public interface Subscriber<T> {
    public void onSubscribe(Subscription s);
    public void onNext(T t);
    public void onError(Throwable t);
    public void onComplete();
}
```

---

### Subscription

```java
public interface Subscription {
    public void request(long n);
    public void cancel();
}
```
---

### Processor

```java
public interface Processor<T, R> extends Subscriber<T>, Publisher<R> {
}
```

???
* publisher 와 subscriber 역활을 동시에 함.
* publisher의 operator 를 사용하고 되도록 사용을 피해야 함.

---

### Reactive-Stream Implementations

.no-bullet-center[
* Reactor
* RxJava2
* Akka streams
]

---
### Style 의 문제

* 선언적 스타일 프로그래밍
* OOP
* FP
* Reactive Programming

???
* OOP, CBP - 내부 상태는 숨기고 개별 Component 구조라는 추상화를 제공하여 컴포넌트의 상호작용으로 프로그램의 목표를 이루도록 프로그래밍 하는 스타일
* Reactive Programming - 프로그램에서 데이터의 흐름을 체인으로 연결하여 In-Out 데이터를 처리하고 그 체인의 상의 오류나 문제에 대한 피드백을 줄수 있도록 하여 목표를ㄹ 이루도록 프로그래밍 하는 스타일

---

### 배관공

배관공이 되어 배관 공사를 하는 것

---

# Reactor

---
Reactive-Stream Spec 의 구현체로, Spring WebFlux 에서 채택한 구현체

???

Reactor is the reactive library of choice for Spring WebFlux

---
## Flux, Mono

* Publisher<T> 의 구현체
* Flux<T> - 0 ~ N
* Mono<T> - 0 ~ 1

---

## Operators

Reactive-Stream 에서는 Operator 를 정의하지 않는다. 구현 벤더에 의해 달라짐.

---

전형적인 Reactor 프로그램

* Publisher 생성
* Operator 조립
* Subscriber 로 subscribe

---
### 마블 다이어그램

클래스/컴포넌트 다이어그램 같이 새로운 프로그래밍 스타일에 맞게 reactive programming 스타일의 operator 들을 효과적으로 시각화 하기 위한 다이어그램

[Understanding Marble Diagrams for Reactive Streams](https://medium.com/@jshvarts/read-marble-diagrams-like-a-pro-3d72934d3ef5)

---

![-------->](https://miro.medium.com/max/535/1*brbCs4smjZfqitE0kHSHTQ.png)

빈스트림 - 왼쪽에서 오른쪽으로 임의의 시간 흐름

---

![-@-^-|-->](https://miro.medium.com/max/537/1*b-7_jU--CKfTkZ3hL66U6Q.png)

데이터 생성 - 원, 오각형, 삼각형 순서로 데이터가 생성되며, 마지막에 해당 스트림은 종료

---

![-@-^-X-->](https://miro.medium.com/max/540/1*DxXNdInXrcKT0Jg3WdGafQ.png)

스트림 에러 - X 표시에서 에러가 발생

---
![-O-@-^-->](https://miro.medium.com/max/537/1*WrctZLIzj2Ptr5qrfHlW4g.png)

경계 없는 스트림 - 스트림이 종료되지 않음

---

#### map

---

#### filter

---

## Processor

---
## Schedulers

* parallel
* single

---
# Spring WebFlux


---

## MVC or WebFlux

![MVC or WebFlux](https://docs.spring.io/spring/docs/current/spring-framework-reference/images/spring-mvc-and-webflux-venn.png)

---

### MVC 

* Legacy MVC - Annotation base 로 mix 가능, 점진적 이동 가능
* Blocking IO 사용 (e.g. JDBC)
* 사이즈가 큰 여러 Service 제공하는 앱

---

### WebFlux

* Kotlin, Java8+, New Project
* Micro services
* Small web with no blocking IO 사용
* 학습곡선은 매우 큼

---

### Annotated Controllers

기존과 동일 단지 webflux dependency 사용

---

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String handle() {
        return "Hello WebFlux";
    }
}
```

---

### Functional Endpoints

```java
@Component
public class GreetingHandler {

	public Mono<ServerResponse> hello(ServerRequest request) {
		return ServerResponse.ok().contentType(MediaType.TEXT_PLAIN)
			.body(BodyInserters.fromObject("Hello, Spring!"));
	}
}
```

---

```java
@Configuration
public class GreetingRouter {

	@Bean
	public RouterFunction<ServerResponse> route(GreetingHandler greetingHandler) {

		return RouterFunctions
			.route(RequestPredicates.GET("/hello")
            .and(RequestPredicates.accept(MediaType.TEXT_PLAIN)), greetingHandler::hello);
	}
}
```

---
# Tips
---

* Wrapping blocking operations
* Debugging
* Operator 의 정확한 구현을 알아야 한다. 그냥 유추해서 쓰면 문제
* Akka stream 통합
* Null 이 발생하면 안된다

---
# Q & A
---

* 왜 쓰는가?
* 언제 쓰는가?
* 성능에 차이가 있는가?
* Spring 의 Scheduler annotation 과의 통합 방법이 있는가?

---
# References

.center[
* [Reactive Streams](https://www.reactive-streams.org/)
* [Reactor 3 Reference Guide](https://projectreactor.io/docs/core/release/reference/)
* [Spring WebFlux Documentation](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html)
]

---
감사합니다
