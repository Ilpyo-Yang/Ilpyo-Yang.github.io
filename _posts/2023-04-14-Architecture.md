---
title: Architecture
author: Rosie Yang
date: 2023-04-14
category: cs
layout: post
---

> "아키텍처는 세부사항에 대한 결정을 내리는 것이 아니라, 결정을 내리는 방법에 대한 것이다." - Ralph Johnson
{: .block-tip }

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
계층형 아키텍처는 일반적으로 3계층으로 나뉜 아키텍처를 말하는데 N계층도 있고 그 계층의 수는 다양하게 나눌 수 있습니다. 3계층으로 살펴보자면 크게 Presentation 계층, Domain(Business or Service) 계층, Data Access(Persistence) 계층이 있습니다.  

**Presentation 계층**  
사용자와의 상호작용을 처리하는 계층으로 Model, View, Controller 그리고 HTTP 요청 처리 및 HTML 랜더링에 대해 알고 있는 웹 계층에 해당합니다.

**Domain 계층**  
시스템의 핵심로직을 담고 있는 계층으로 유효성 검사 및 계산들과 도메인 관련 작업들을 담당합니다. 토비의 스프링에서는 서비스 계층과 기반 서비스 계층으로 나누는데, [이동욱님 블로그](https://jojoldu.tistory.com/603)를 보면, 이를 도메인 계층과 서비스 계층으로 분류해서 명확히 한 것을 볼 수 있었습니다. 도메인 계층을 Rich Domain 모델을 기반으로 문제 도메인 해결에 순수하게 집중하는 계층으로 표현했고, 서비스 계층은 트랜잭션, 메일&SMS 발송 등 다른 인프라와의 통신을 담당하는 역할을 하는 계층으로 분리해서 설명했습니다.  
<span style="background-color:#DCFFE4">Rich Domain 모델은 무엇인가</span>  


**Data Access 계층**  
데이터베이스, Message Queue, 외부 API 통신 등을 처리하는 계층입니다. 



****
[계층형 아키텍처](https://jojoldu.tistory.com/603)  
[Anemic Domain Model vs. Rich Domain Model](https://medium.com/@inzuael/anemic-domain-model-vs-rich-domain-model-78752b46098f)
