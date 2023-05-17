---
title: Architecture
author: Rosie Yang
date: 2023-04-14
category: cs
layout: post
---

## DDD(Domain-Driven Design)

![ddd.png](/assets/gitbook/post_images/architecture/ddd.png)

도메인 주도 설계는 소프트웨어의 존재 가치는 사용자의 사용에 있다는 생각에서 비롯되어 <span style="background-color:#fff5b1">비즈니스 도메인을 중심</span>으로 고려한 설계 방식입니다. 즉, 사용자가 원하는 목적에 맞게 사용할 수 있는 소프트웨어가 기술보다 우선순위에 두고 고민할 필요성이 있다는 점에서 시작됩니다. 사용자의 관점에서 정해지는 부분이기 때문에 도메인은 관점에 따라 그 수가 달라질 수 있습니다. 하지만 DDD를 사용하므로써 개발자는 단순히 기술영역에만 국한되지 않고 도메인 영역까지 사고하는 생각의 범주를 더 넓힐 수 있습니다.  
<span style="background-color:#fff5b1">바운디드 컨텍스트</span>란 모델이 구현되는 곳이자 각각의 분리된 소프트웨어 산출물이 나오게 되는 곳입니다. 유비쿼터스 언어로 표현해 공동 작업을 하는 팀원과 유관 부서 간의 혼동을 피하는 것을 기본으로 합니다. 

**반 버논의 도메인 분류**
+ 메인(핵심) 도메인
+ 서브 도메인
  + <span style="background-color:#fff5b1">핵심 서브 도메인</span>
    + 다른 경쟁자와 차별화를 만들 수 있는 비즈니스 영역
    + 높은 우선순위를 갖는 전략적 투자 영역
    + 가장 큰 투자가 필요한 곳
  + <span style="background-color:#fff5b1">지원 서브 도메인</span>
    + 맞춤 개발이 필요한 영역
    + 핵심 서브 도메인의 성공을 위한 중요한 영역
  + <span style="background-color:#fff5b1">일반 서브 도메인</span>
    + 기존 제품 구매를 통해 바로 충족시킬 수 있는 영역
    + 핵심/지원 서브도메인이 할당된 팀에서 직접 구현 가능

<br>

**DDD 파일시스템**  





****
[Domain-Driven Design Simplified.](https://medium.com/@jaysonmulwa/domain-driven-design-simplified-a03c732401c9)  
[도메인 주도 설계에서의 전략적 설계](https://engineering-skcc.github.io/msa/DDD-StrategicDesign/)