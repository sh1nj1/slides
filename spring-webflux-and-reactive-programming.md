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
* Reactive Programming, Stream
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

### Sync vs Async, Blocking vs Non-blocking

Concurrency vs Parallelism

???

* Concurrency - 일정시간 동안의 동시처리
* Parallelism - 말그대로 완전히 분리되어 동시에 동작
* 효율 vs 비효율, parallelism 이 좋지만, 상대적으로 어렵고, 모든 것을 완전히 parallelise 하게 할 수 없다. 따라서 타협과 최적화 필요.

---
### Back Pressure

* 처리 가능한 양을 알려주는 매커니즘
* Back pressure 가 처리된다는 것은 그 피드백을 통해 data 생성자가 적절한 처리를 하는 것

![:img Back Pressure, 60%](https://docs.google.com/drawings/d/e/2PACX-1vRTluTraubSsXydTXM9TjZkiYy3s15Dkk7xIXG5E_n4CzaV3zWRlVVNvz4zn_6b6c_Oj2y-hX14EdWN/pub?w=857&h=537)

???

* [Reactive Manifesto Glossary](https://www.reactivemanifesto.org/glossary)
* [멈추지 않고 기다리기(Non-blocking)와 비동기(Asynchronous) 그리고 동시성(Concurrency)](https://tech.peoplefund.co.kr/2017/08/02/non-blocking-asynchronous-concurrency.html)
---

## Reactive-Stream Specification


SPI (Service Provider Interface)

서로 다른 라이브러리가 서로 통합하여 사용될 수 있도록 Service Provider 를 위한 API를 제공하고 구현에 대한 정확한 스펙을 정의한다. 실제 구현은 개별 서비스 provider
에게 맞겨 개발자가 어떤 구현체를 사용하더라도 상호호환되게 한다.

Spec + TCK(Technology Compatibility Kit)

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

### Component Flow

![Reactive-Stream Flow](https://i0.wp.com/javasampleapproach.com/wp-content/uploads/2017/04/reactive-stream-flow-interface-behavior.png)

---

### Reactive-Stream Implementations

.no-bullet-center[
* Reactor
* RxJava2
* Akka streams
]

---
### 비교


* 선언적 스타일 프로그래밍
* OOP
* FP
* Reactive Programming

???
* OOP, CBP - 내부 상태는 숨기고 개별 Component 구조라는 추상화를 제공하여 컴포넌트의 상호작용으로 프로그램의 목표를 이루도록 프로그래밍 하는 스타일
* Reactive Programming - 프로그램에서 데이터의 흐름을 체인으로 연결하여 In-Out 데이터를 처리하고 그 체인의 상의 오류나 문제에 대한 피드백을 줄수 있도록 하여 목표를ㄹ 이루도록 프로그래밍 하는 스타일

---
### 코드의 예

```java
userService.getFavorites(userId)
           .timeout(Duration.ofMillis(800)) 
           .onErrorResume(cacheService.cachedFavoritesFor(userId)) 
           .flatMap(favoriteService::getDetails) 
           .switchIfEmpty(suggestionService.getSuggestions())
           .take(5)
           .publishOn(UiUtils.uiThreadScheduler())
           .subscribe(uiList::show, UiUtils::errorPopup);
```

???

* 쉽게 데이터 흐름과 로직을 풍부한 operator 단어로 설명이 가능.
* 이상과 현실은 다름 :(

---

### 공장 조립 라인 비유

![:img OOP Analogy, 70%](https://docs.google.com/drawings/d/e/2PACX-1vShZSSMYfIvq-dq1fF9oYismF24HyWZhAK79RQnfbJQ0xh_6U1I-9tE8qJaezrMiN-wYDt0ZOYFwqY-/pub?w=908&amp;h=627)

???
* OOP 에서는 모든 것을 특별한 기능공으로 구분하여 설계하고 서로 어떻게 통신할지 설정한다.
* 개별 컴포넌트는 점점 무겁고, 멀티 쓰레드에서 상태를 확인하기 힘들어진다.

---

![:img Reactive-Stream Analogy, 100%](https://docs.google.com/drawings/d/e/2PACX-1vTvyKURkX5xk6oXd2eqdvbIUof7TEdTjbXaSSUnGMkjy0R6zmK1esMo_sOk1JxdpqHh30GeAUBoVRny/pub?w=911&amp;h=422)

???
* 데이터들을 컨베이어 벨트에 흐르는 파트들로 개발 작업장은 최소한의 가공만 효율적으로 거치고 다음 작업장으로 연결된다.
* 만약 한 작업장에 문제가 생기면 컨베이어 벨트를 느리게 하거나 일시 중지한다. 그것은 이전 작업장으로 전파되고, 안정적으로 처리된다.
* Component 의 상태를 버리고, 각자의 조립라인에서 IN-OUT 만 가지는 가벼운 funtional 한 operators 들을 조립함으로써, 재활용성은 더 높아지고,
  개발자의 생산력을 높인다.

---

# Reactor

---
* Reactive-Stream Spec 의 구현체로, Spring WebFlux 에서 채택한 구현체
* 4세대 reactive library
* RX operator 를 따르는 operator 들을 제공.

???

Reactor is the reactive library of choice for Spring WebFlux

---
## History

![:img Reactor History, 70%](https://i.ytimg.com/vi/ctZGFTI3XI8/maxresdefault.jpg)

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
## Flux, Mono

* Publisher<T> 의 구현체
* Flux<T> - 0 ~ N
* Mono<T> - 0 ~ 1

---
### Flux

![:img Flux Marble, 60%](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/doc-files/marbles/flux.svg)

---
### Mono

![:img Mono Marble, 60%](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/doc-files/marbles/mono.svg)
---

## Operators

Reactive-Stream 에서는 Operator 를 정의하지 않는다. 구현 벤더에 의해 달라짐.

---

전형적인 Reactor 프로그램

* Publisher 생성
* Operator 체인 조립
* Subscriber 로 subscribe


---
### distinct

![:img distinct marble, 70%](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/doc-files/marbles/distinct.svg)

---

### flatMap

![:img flatMap marble, 70%](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/doc-files/marbles/flatMapForFlux.svg)


???

* 비동기적으로 처리 element 를 받아서 여러 element 로 변환하는 inner publisher 를 모아서 하나의 Flux 로 반환.
* 순서 없음.

---

## Processor

* DirectProcessor - 단순 onNext 로 데이터르 보내고, 특별한 backpressure 를 제공하지 않음
* UnicastProcessor - 하나의 subscriber 만 허용
* EmitterProcessor - pub - sub 처리를 sync 하게 처리
* TopicProcessor - pub - sub 처리를 async event loop 를 통해 처리
* WorkQueueProcessor - 여러 subscriber 에 task 분산

---
## Schedulers

* single - single thread 
* elastic - 유동적인 thread pool 요청에 따라 늘어나거나 줄어들고, 60초간 유지. Blocking Task 에 적합
* parallel - 효율적인 처리 Thread pool.
* immediate - 현재 실행 thread

---
### Scheduler 바꾸기

* subscribeOn - 해당 stream 전체에 적용 - 위치 상관없음, 여러개의 경우 마지막 것이 적용.
* publishOn - 이후에 적용

---
## Hot VS Cold Publisher

* Cold - 정적
* Hot - 동적

---
# Spring WebFlux

---

## MVC or WebFlux

![:img MVC or WebFlux, 70%](https://docs.spring.io/spring/docs/current/spring-framework-reference/images/spring-mvc-and-webflux-venn.png)

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

* webmvc 모듈 dependency 추가했다면, `@EnableWebFlux` annotation 를 추가한다.
* `@RequestBody` 는 `Mono<T>` 또는 `Flux<T>` 등을 사용하여 체인 연결하여 사용.
* `Mono<Void>` 리턴 타입은 Mono 가 종료될때 빈응답을 보냄
* Non-reactive 리턴 타입 void, T 등은 사용 가능하나, 싱크하게 호출되고 따라서 메쏘드 안에서 blocking 하면 안됨,
  T 는 Mono 등으로 랩핑되서 async 하게 전송됨.

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

## Blocking call -> Non-blocking call

```java
public Mono<String> blockingMethod() {

    return Mono.fromSupplier(() -> this.blockingWebService())
               .subscribeOn(Schedulers.elastic());
}
```
???

blocking 메쏘드를 Mono 등으로 감싸고, elastic 쓰레드 풀에서 작동하도록 한다.
---


* Subscribe 하기 전에는 아무것도 동작하지 않음.
* Operator chain 이 끊어지면 안됨, 특히 flatMap 등을 쓸때.
* operator, subscribe 안에서 다시 subscribe 하지 않기.
* Blocking Operation 은 Scheduler 를 통해 비동기적으로 처리해야 함.
* Flux, Mono 등 Null 이 발생하면 안됨, 필요에 따라서는 Optional.of 등을 사용.
* exception 이 발생하면 기본적으로 스트림이 종료됨, Flux, Mono 를 리턴하는 경우 Mono.error(e) 등으로 랩핑해서 리턴.
* Mono.empty() 등 의 값이 리턴되면 그 이후 실행이 안됨.
* Operator 의 정확한 구현을 알아야 한다. 그냥 유추해서 쓰면 문제
* Akka streams, RxJava 등과 통합 가능
* 개별 operator 블럭 안은 간결하게 유지한다.
* `@Scheduled` `@Async` 다른 스프링 모듈과의 통합은 아직 안되고 있다.

---

# 생각

---

기존 방식이랑 양립하기 어렵다

???

* reactive-stream 방식으로 하기로 하면 모든 부분에서 바꿔야 하고 기존 스타일로 코드와 같이 존제하기 힘들다.
* 따라서 충분한 사전 조기와 개념 정리, 검증, 학습 등이 필요

---

.impact[우연에 맡기는 프로그래밍을 하지 말자]

---
# Q & A
---

* 왜 쓰는가?
* 언제 쓰는가?
* 성능에 차이가 있는가?
* 단점?

???

* 왜? - 작은 단위로 이미 조합할 수 있는 operator 들로 학습만 되어 있다면 가독성밑 비지니스 로직 구현이 쉬움. 최신 하드웨어를 보다 더 효율적으로 사용, MSA 에 적합
  클라우드 환경으로의 변화에 보다 많은 Network IO 필요.
* Spring 의 다른 여버부분은 통합되어 있지 않음, @Scheduled 도 통합이 되어 있지 않음.

---
# References

.center[
* [Advanced Reactive Java](http://akarnokd.blogspot.com/2016/03/operator-fusion-part-1.html)
* [Reactive Streams](https://www.reactive-streams.org/)
* [Reactor 3 Reference Guide](https://projectreactor.io/docs/core/release/reference/)
* [Spring WebFlux Documentation](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html)
* [JAVA9: Learning the new features -Part3](https://alexandreesl.com/tag/reactive-streams/)
* [Reactor 3.0, a JVM foundation for Java 8 and Reactive Streams](https://www.youtube.com/watch?v=ctZGFTI3XI8)
* [Reactor by Example](https://www.infoq.com/articles/reactor-by-example/)
* [사용하면서 알게 된 Reactor, 예제 코드로 살펴보기](https://tech.kakao.com/2018/05/29/reactor-programming/)
* [Reactor Java - series 1](https://medium.com/@cheron.antoine/reactor-java-1-how-to-create-mono-and-flux-471c505fa158)
* [REACTOR: Java Meets Reactive Programming](https://medium.com/wolox-driving-innovation/reactor-java-meets-reactive-programming-16105c026fc3)
* [Notes on Reactive Programming Part I: The Reactive Landscape](https://spring.io/blog/2016/06/07/notes-on-reactive-programming-part-i-the-reactive-landscape)
]

---
감사합니다
