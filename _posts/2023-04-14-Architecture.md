---
title: Architecture
author: Rosie Yang
date: 2023-04-14
category: cs
layout: post
---

> "아키텍처는 세부사항에 대한 결정을 내리는 것이 아니라, 결정을 내리는 방법에 대한 것이다." - Ralph Johnson
{: .block-tip }  
> 아키텍처 뿐만 아니라 웹, 모바일에 대한 내용도 함께 담은 포스팅 페이지입니다.

<br>

+ [API 버저닝 방법 정하기]({{site.baseurl}}/cs/2023/04/14/Architecture.html#API-버저닝-방법-정하기)
+ [백엔드 아키텍처 설계시 고려사항]({{site.baseurl}}/cs/2023/04/14/Architecture.html#백엔드-아키텍처-설계시-고려사항)
+ [아키텍처 기본지식]({{site.baseurl}}/cs/2023/04/14/Architecture.html#아키텍처-기본지식)
+ [폰 노이만 아키텍처]({{site.baseurl}}/cs/2023/04/14/Architecture.html#폰-노이만-아키텍처)
+ [Layered Architecture, 계층형 아키텍처]({{site.baseurl}}/cs/2023/04/14/Architecture.html#layered-architecture-계층형-아키텍처)

<br>

## API 버저닝 방법 정하기
프로젝트를 하면서 백엔드 API 개발시 필요한 버저닝에 대한 고민을 하게 되었습니다. 사실 버저닝은 개인 프로젝트라서 편한대로 하면 된다고 생각했지만 좀 더 일반적인 방법을 적용하고 싶다는 생각이 들어서 어떤 방법들을 사용하는지 찾아봤습니다.

#### HTTP Rest API 성숙도 모델
Leonard Richardson의 [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)에서 보면 HTTP Rest API 성숙도 모델을 레벨 4개로 나누어 사용했습니다.
- Level 0 : HTTP 사용
   - RPC(Remote Precedure Call) 형태로 리소스 구분 없이 설계된 HTTP API입니다.
   - 하나의 endpoint에서 여러 매개변수에 따라 다른 동작을 하게 합니다.
- Level 1 : 개별 리소스 개념 도입
   - 리소스별로 고유한 URI를 사용해서 식별하고 HTTP method는 GET과 POST만 사용합니다.
   - HTTP headers에 Content-Type이나 Cache 관련 정보를 제공하지 않습니다.
- Level 2 : HTTP 메소드 원칙 준수
   - GET, POST, PUT, DELETE HTTP method를 사용해 CRUD를 나타내고 메시지에 Status Code도 반환합니다.
   - URI에는 행위(Action)가 포함되지 않고 HTTP Method로 표현한다. GET은 매번 같은 결과를 반환하고, 헤더에 Content-Type을 제공하며 멱등성을 보장하는 GET의 경우 캐시가 적용됩니다.
- Level 3 : HATEOAS 원칙 준수
   - Hypermedia(링크)를 통해서 다음 가능한 행동(action)에 대한 정보를 응답 본문에 넣어주어야 합니다.

사내에서 업무를 하면서 백엔드 서버와 프론트 서버가 확실하게 분리되어 있어서 HATEOAS 원칙에 따라 응답에 유형에 따라 다음 행동을 위한 링크를 리턴할 일은 많지 않았습니다. 기본적으로 `Level 2`에 해당하는 방식으로 적용하되 다음 액션이 필요한 경우는 링크를 제공하기로 했습니다.

#### API 컨벤션의 종류
API의 형식은 `api.example.com/v1/orders` 형태의 버저닝을 따르기로 했습니다. 백엔드 서버만 개발하고 추후 프론트 개발을 할지에 대해 고민 중이였기 때문입니다. 이전에는 도메인에 `/api/v1/orders`와 같은 방식을 사용했지만 도메인 분리를 한다면 처음 언급한 방식을 사용하는 것이 적합하다고 생각했습니다.  
버저닝을 API에 표기하는 방식에 대해서 별로 찬성하는 편은 아니지만 이번에는 간단한 개인 프로젝트이므로 버저닝을 API에 담기로 했습니다. 실운영할 상용 프로젝트에서는 body에 적용하거나 서버에서 처리하는 방법을 사용할 것 같습니다. 개인적으로 파라미터에 담는 방식은 불필요하고 다른 매개변수 적용도 해야하는 호출부에선 적절하지 않을 것 같다는 생각이 들었습니다.  
`{orderId}`는 아이디를 API에 담는 방식이지만 이번 프로젝트에서는 사용하지 않을 예정입니다. API가 깔끔하지 않습니다. 그리고 주요 정보를 유추해 낼 수 있고 보안관리에 좋지 않습니다. 그리고 2개 이상의 변수를 Path에 담지 않는 것으로 합니다.

#### 결론
이번 개인 프로젝트에서는 다음의 API 컨벤션을 따르기로 했습니다.
- HTTP Rest API 성숙도 모델 `Level 2`에 해당하는 방식으로 적용하되 다음 액션이 필요한 링크는 제공
- 백엔드 API 서버를 위한 `api.example.com/v1/orders` 방식 적용
- Path에 `ID` 정보는 되도록 담지 않기

<br><br>

## 백엔드 아키텍처 설계시 고려사항
우리가 백엔드 아키텍처를 설계한다고 했을 때 고려해야 할 사항들과 각 상황별로 어떻게 대처하는 것이 좋을까에 대해 대해서 정리했습니다.  
하나의 애플리케이션을 운용하기 위해서는 단일 서버라고 한다면, 애플리케이션과 연동되는 DNS, 웹서버, 데이터베이스로 구성할 수 있습니다.

**그럼 이 때 어떤 데이터베이스를 써야 할까요?**  
데이터베이스의 선택은 서비스에 따라 달라질 것이고, <span style="background-color:#fff5b1">CAP 이론에 따라 가용성, 일관성, 부분 결함을 고려해야 합니다.</span> 가장 중요시하는 성질이 무엇인가에 따라 우리는 데이터베이스를 선택할 수 있습니다.
일관성보다 가용성과 신속함이 우선시 된다면 NoSQL을 사용할 수도 있을 것이고, 일반적으로 우리가 사용하는 RDBMS는 일관성 측면을 좀 더 중시하는 프로그램에서 적합합니다. 경험상 모두가 같은 정보를 확인해야만 하는 경우에서 일관성이 중요하기 때문에 RDBMS를 사용했고, 모든 노드의 데이터가 동일하지 않지만 대용량, 데이터베이스 부하를 줄여야만 하는 경우에는 변동 사항이 별로 없는 경우의 환경의 데이터베이스는 NoSQL을 사용했습니다.
+ 가용성: 모든 노드가 정상적인 응답을 해야 하는 경우
+ 일관성: 모든 노드가 동일한 데이터를 가지고 있는 경우
+ 부분결함 또는 분할 내구성: 네트워크 특성으로 노드 간 통신 장애가 있더라도 동작해야 함

<span style="background-color:#DCFFE4">분할 내구성과 가용성은 어떻게 다른가?</span>  
분할내구성은 분산 시스템에서 어떤 노드가 실패하더라도 시스템이 계속해서 작동하도록 하는 것을 의미합니다. 이는 시스템의 일부가 실패하더라도 다른 노드가 해당 기능을 계속 수행할 수 있어야 함을 의미합니다. 반면에, 가용성은 시스템이 항상 사용 가능한 상태여야 함을 의미합니다. 즉, 사용자가 시스템에 접속하고 서비스를 이용할 수 있어야 합니다. 따라서, 가용성은 시스템 전체적으로 작동하는 데 필요한 모든 서비스와 기능에 대한 가용성을 의미합니다.  
<span style="background-color:#fff5b1">분할내구성은 시스템의 일부가 실패하더라도 시스템이 계속해서 작동할 수 있도록 하는 것을 의미하고, 가용성은 시스템 전체적으로 항상 사용 가능한 상태여야 함을 의미합니다.</span>  
하지만, [CAP Theorem, 오해와 진실](http://eincs.com/2013/07/misleading-and-truth-of-cap-theorem/) 이라는 글에서는 분할 내구성에 대한 정의가 글마다 다르게 사용되고 있으며, 네트워크 장애가 있을 수 있는지에 대한 사항은 완벽한 프로그램이 없으므로 선택될 수 밖에 없는 사항이라는 점에서 PACELC를 설명하고 있습니다. 장애 상황에서는 A,C가 상충하며 둘 중 하나를 선택해야 하고, 정상 상황에서는 가용성이 아닌 지연(latency)과 일관성 중에 선택해야 하는 것으로 나누고 있습니다. CAP 자체가 분산시스템만의 상황이 아닌 네트워크 상태도 고려해야 한다는 점에서 RDMBS를 CA로 적용하기에는 명확하지 않다는 점을 고려해야 합니다.

**서비스 규모의 확장성도 고려해보자**  
서비스 규모가 커지면 서버를 확장해야 하는 부분도 고려한 설계를 해야 합니다. <span style="background-color:#fff5b1">수평적 확장은 서버를 추가하는 것, 수직적 확장은 서버의 CPU, Memory를 증설하는 것을 의미합니다.</span>  
수평적 확장을 하는 경우에는 서버를 추가하기 때문에 특정 페이지에 접근 고객을 분리하거나 고객별 분리 등 다양한 방법으로 관리가 가능하고, Slave server처럼 서버의 문제가 생겼을 때, Master 서버로서 대체할 수 있는 방안이 되기도 합니다. 수직적 확장은 수평적 확장보다 오히려 비용이 많이 들 수 있으며, 하나의 서버가 더 많은 양을 담당하기 때문에 서버에 문제가 생기면 큰 서비스 장애로 이어질 수 있습니다. 그리고 수직적 확장이 2배가 된 것이 성능 2배 향상으로 이어진다고 볼 수 없다는 단점이 있습니다.
+ 대용량 서비스에서는 데이터베이스는 주로 읽기 비중이 높기 때문에 주 데이터베이스 서버와 부 데이터베이스 서버들도 분산해 읽기 연산만을 다루는 데이터베이스 서버를 두어 관리합니다.
+ 캐시를 이용합니다. 직접적인 데이터베이스 접근이 없이 자주 사용되는 데이터를 미리 캐싱해놓으면 빠르고 부하를 줄일 수 있다는 장점이 있습니다. 하지만, 캐시와 DB 간의 일관성 문제가 발생할 수 있다는 단점이 있습니다.

**CDN의 활용**  
CDN은 Contents Delivery Network의 약자로, 지리적으로 가까운 프록시 서버로부터 웹 페이지, 이미지, 비디오 등의 콘텐츠를 캐싱하고 사용자 요청에 따라 신속하게 처리될 수 있도록 합니다. CDN 밴더 서비스를 제공하는 업체는 AWS CloudFront, Akamai Technologies, Cloudflare 등이 있습니다.
+ AWS CloudFront를 사용하는 방법을 예로 들면, 먼저 AWS CloudFront 콘솔에서 전달 콘텐츠 유형과 콘텐츠 원본을 설정합니다. 기본 캐시 동작을 구성하여 CloudFront가 콘텐츠 요청을 처리하는 방법을 지정하고, SSL 인증서, 가격 등급 및 오류 페이지와 같은 배포 설정을 사용자 정의합니다. 배포가 생성되면 CloudFront에서 제공하는 도메인 이름을 사용하여 콘텐츠에 액세스할 수 있습니다.

**트랜잭션과 DB 락 설정**  
백엔드 설계시 트랜잭션 단위를 어떻게 가지고 갈 것인지는 매우 중요합니다. 불필요하게 트랜잭션 단위가 커지면, 의도치 않은 데드락 현상이 생길수도 있고, 트랜잭션 소요 시간이 길어져 같은 트랜잭션 요청이 과도하게 몰리는 경우 성능이 저하될 수 있습니다. 또 트랜잭션 단위가 작아서 하나의 비지니스 흐름에 여러 트랜잭션이 있어 중간 장애가 발생하면 비즈니스 일부분은 수행된 채로 rollback 처리되는 문제가 발생할 수 있습니다.  
데이테베이스 락 설정도 중요합니다. 비관적 락을 사용하면 경쟁상황이 없는 경우에도 불필요하게 락이 생성되기 때문에 성능이 저하될 수 있습니다. 반대로 낙관적 락은 트랜잭션 커밋시 격리 위반을 체크하기 때문에 rollback 처리로 경쟁상황에서 성능이 떨어질 수 있습니다. 따라서 경쟁여부, 락이 걸리는 범위(row, page), 속도 등을 판단해서 적절한 락 설정이 필요합니다.  
<span style="background-color:#fff5b1">분산락을 이용하는 방법도 있습니다.</span> 분산락은 여러 서버에서 공유 데이터의 접근을 하나의 컴퓨터만 할 수 있도록 제어하는 방법입니다. 단순히 조회가 많은 애플리케이션이라면, 분산락을 이용해서 접근처리를 하고 RDBMS는 낙관적 락을 이용하는 방법으로 성능 부하를 줄일 수 있습니다.

****
+ 원티드 백엔드 온보딩 챌린지 7월
+ [CAP Theorem, 오해와 진실](http://eincs.com/2013/07/misleading-and-truth-of-cap-theorem/)
+ [레디스를 활용한 분산 락과 안전하고 빠른 락의 구현](https://hyperconnect.github.io/2019/11/15/redis-distributed-lock-1.html)

<br><br>

## 아키텍처 기본지식
<span style="background-color:#DCFFE4">애플리케이션 아키텍처는 왜 필요할까?</span>  
<span style="background-color:#fff5b1">아키텍처는 애플리케이션의 구조와 구성요소의 조직화 방식을 정한 것</span>으로 개발자가 애플리케이션을 만들 때, 어떤 기준으로 애플리케이션을 만들지를 알 수 있는 청사진을 제공합니다. 아키텍처는 일반적으로 UI, 비즈니스 로직, 데이터 액세스 계층, 인프라구조로 구성됩니다.  
좋은 아키텍처를 구축한다는 것은 구조를 잡고, 도메인을 통해 애플리케이션 핵심 기능을 파악하기 용이한 아키텍처를 말합니다. 그리고 팀원들과 공통의 룰을 가지고 유지보수 및 코드의 통일성을 만들어줍니다.

### 아키텍처의 종류
크게 모놀리식 아키텍처와 분산형 아키텍처로 나눌 수 있습니다.

**Monolithic Architecture, 모놀리식 아키텍처**  
<span style="background-color:#fff5b1">하나의 코드 베이스를 갖춘 대규모의 단일 컴퓨팅 네트워크입니다.</span> 주로 작은 애플리케이션 개발시에 코드 관리, 배포에 유용합니다. 하지만 전체 스택을 업데이트해야 한다는 단점이 있습니다. 종류로는 Layered Architecture, Clean Architecture, Hexagonal Architecture가 있습니다.

**Distributed Architecture, 분산형 아키텍처**
> The microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API.
> These services are built around business capabilities and independently deployable by fully automated deployment machinery. [Martin Fowler]

<span style="background-color:#fff5b1">소프트웨어 시스템을 여러 독립적인 구성 요소로 분할하고, 이러한 구성 요소들이 분산된 환경에서 동작하며 서로 통신하도록 설계된 아키텍처입니다.</span> Service Oriented Architecture, Event-based Architecture, MicroService Architecture(MSA)가 분산형 아키텍처에 해당됩니다.

****

[Microservice Principles: Decentralized Governance](https://nathanpeck.com/microservice-principles-decentralized-governance/)  
[Monolithic Architecture Is Still Worth at 2021 ?](https://medium.com/design-microservices-architecture-with-patterns/monolithic-architecture-is-still-worth-at-2021-98bfc112dc24)  
[마이크로서비스와 모놀리식 아키텍처 비교](https://www.atlassian.com/ko/microservices/microservices-architecture/microservices-vs-monolith)

<br><br>

## 폰 노이만 아키텍처
컴퓨터는 폰 노이만 구조를 근간으로 발전했으며, 이를 통해 CPU, 메모리, 프로그램 구조를 갖는 범용 컴퓨터 구조를 확립했습니다. 컴퓨터 시스템의 전통적인 아키텍처 형태로, 기본적으로는 모놀리식 아키텍처에 해당합니다. 분산시스템에서도 적용될 수 있는 아키텍처로 확장성과 가용성을 향상시킬 수 있습니다.

**작동원리**  
사용자가 컴퓨터에 값을 입력하거나 프로그램을 실행할 경우 그 정보를 먼저 메모리에 저장시키고 CPU가 순차적으로 그 정보를 해석하고 계산해 사용자에게 결과값을 전달합니다.

<br><br>

## Layered Architecture, 계층형 아키텍처
> Presentation 계층, Domain(Business or Service) 계층, Data Access(Persistence or Infrastructure) 계층

계층형 아키텍처는 단일 소프르퉤어 단위로 함께 작동하는 여러 개의 개별 수평 계층으로 구성된 아키텍처 패턴을 말합니다. 그 계층의 수는 소프트웨어에 따라 다양하게 나눌 수 있습니다.

### 계층구조
**Presentation 계층**  
사용자와의 상호작용을 처리하는 계층으로 Model, View, Controller 그리고 HTTP 요청 처리 및 HTML 랜더링에 대해 알고 있는 웹 계층에 해당합니다.

**Domain 계층**  
시스템의 핵심로직을 담고 있는 계층으로 유효성 검사 및 계산들과 도메인 관련 작업들을 담당합니다. 토비의 스프링에서는 서비스 계층과 기반 서비스 계층으로 나누는데, [이동욱님 블로그](https://jojoldu.tistory.com/603)를 보면, 이를 도메인 계층과 서비스 계층으로 분류해서 명확히 한 것을 볼 수 있었습니다. 도메인 계층을 Rich Domain 모델을 기반으로 문제 도메인 해결에 순수하게 집중하는 계층으로 표현했고, 서비스 계층은 트랜잭션, 메일&SMS 발송 등 다른 인프라와의 통신을 담당하는 역할을 하는 계층으로 분리해서 설명했습니다.  
<span style="background-color:#DCFFE4">Rich Domain Model은 무엇인가</span>  
Anemic Domain Model은 객체가 데이터만을 갖고 있고 동작을 수행하는 데 필요한 비즈니스 로직이 다른 객체에 의해 처리되는 모델을 의미합니다. 이는 비즈니스 로직이 외부 서비스나 관리 객체에 의해 조작되는 형태를 말합니다. <span style="background-color:#fff5b1">반면, Rich Domain Model은 객체 자체가 비즈니스 로직을 포함하고 있으며, 데이터와 로직이 함께 동작합니다. 그러므로 Anemic Domain Model에 비해 비즈니스 중복 로직을 줄이고 객체 간 응집도를 높일 수 있는 장점이 있습니다.</span>

1. Anemic Domain Model의 예
```java
public class Order {
    private int orderId;
    private Date orderDate;
    private List<OrderItem> orderItems;

    // Getter and Setter methods for data fields

    public double calculateTotalPrice() {
        double totalPrice = 0;
        for (OrderItem item : orderItems) {
            totalPrice += item.getPrice();
        }
        return totalPrice;
    }
}
```

2. Rich Domain Model 예  
   Anemic Domain Model의 예와 달리 ```addOrderItem```, ```removeOrderItem```와 같이 동작을 수행하는 메서드도 포함되어 있고, ```OrderItem``` 객체에 ```calculatePrice()``` 메서드를 사용하여 각 주문 항목의 가격을 계산합니다. Rich Domain Model은 객체가 스스로 상태를 관리할 수 있도록 데이터와 데이터를 조작하는 로직이 함께 존재합니다.
```java
public class Order {
    private int orderId;
    private Date orderDate;
    private List<OrderItem> orderItems;

    // Getter and Setter methods for data fields

    public void addOrderItem(OrderItem item) {
        orderItems.add(item);
    }

    public void removeOrderItem(OrderItem item) {
        orderItems.remove(item);
    }

    public double calculateTotalPrice() {
        double totalPrice = 0;
        for (OrderItem item : orderItems) {
            totalPrice += item.calculatePrice();
        }
        return totalPrice;
    }
}
```

**Data Access 계층**  
데이터베이스, Message Queue, 외부 API 통신 등을 처리하는 계층입니다.

<br>

### 계층형 아키텍처의 문제점
<span style="background-color:#fff5b1">데이터베이스 주도 설계를 유도합니다.</span>  
도메인 중심이 아닌 영속성 계층에 의존하는 데이터베이스 중심으로 도메인이 만들어집니다. 이는 ORM 프레임워크에 따라 의존성의 방향이 보통 엔티티들이 속한 영속성 계층에 있기 때문입니다. 결과적으로 도메인 모델에 대한 상태 변경이 아닌 행동 중심으로 모델링이 된다는 단점이 있습니다. 뿐만 아니라 도메인 계층에서 즉시로딩/지연로딩, 트랜잭션 등을 담당하기 때문에 영속성 계층과의 강한 결합이 생기도록 한다는 문제가 있습니다.  
<span style="background-color:#fff5b1">지름길을 택하기 쉬워집니다.</span>  
계층형 아키텍처는 하위 계층에서 상위 계층으로의 접근이 어렵다는 문제가 있습니다. 그래서 ```지름길```로써 상위 컴포넌트를 하위 계층으로 내려서 접근이 편하도록 하는 잘못된 방법을 택해 영속성 계층을 비대하게 만드는 방법을 채택할 수 있습니다. 이를 ```깨진 창문 이론```이라는 심리적 효과로 표현하기도 합니다. 결국 하나의 계층에서 다양한 역할을 수행하게 되면서 관심사의 분리가 제대로 이뤄지지 않는다는 단점이 있습니다.  
<span style="background-color:#fff5b1">테스트하기 어려워집니다.</span>  
이렇게 프로젝트를 만든 적은 없지만, 웹 계층에서 바로 영속성 계층으로 접근하는 경우가 발생할 수 있습니다. 그 경우 도메인이 해야할 역할이 웹 계층에서 수행되는 등 계층 분리가 모호해지고 그에 따른 유닛테스트 수행이 어려워집니다.   
<span style="background-color:#fff5b1">유스케이스를 숨깁니다.</span>  
위 문제와 연장선 상에서 볼 수 있습니다. 계층의 모호한 분리는 유스케이스를 추가하려고 할 때, 정확히 어디에 기능을 추가할지 어려워집니다.  
<span style="background-color:#fff5b1">동시 작업이 어려워집니다.</span>  
영속성 계층을 중심으로 하기 때문에 협업이 어렵습니다. 도메인 또는 영속성 계층이 비대해지면 그 과정에서 동시에 편집해야 하는 경우가 발생하게 되고, merge하면서 코드 충돌이 발생할 수 있기 때문입니다.

<br>

**문제를 해결하기 위한 방법**  
위와 같은 계층형 아키텍처의 문제를 해결하기 위해서는 <span style="background-color:#fff5b1">객체지향 개발 원칙인 SOLID 윈칙을 따르고, 단위 테스트를 적용</span>해서 부적절하게 넓은 계층이 만들어지거나 불필요한 영속성 계층으로의 의존도를 줄일 필요가 있습니다.

****
[Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)  
[Layered Architecture](https://www.baeldung.com/cs/layered-architecture)  
[계층형 아키텍처](https://jojoldu.tistory.com/603)  
[Anemic Domain Model vs. Rich Domain Model](https://medium.com/@inzuael/anemic-domain-model-vs-rich-domain-model-78752b46098f)  
[Rich vs Anemic Domain Model](https://stackoverflow.com/questions/23314330/rich-vs-anemic-domain-model)

<div style="padding:3px; margin:200px 0;"></div>   