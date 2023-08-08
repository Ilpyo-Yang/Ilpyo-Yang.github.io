---
title: 📖 Effective Java (진행중)
author: Rosie Yang
date: 2023-04-30T00:00:00.000Z
category: backend
---

# Effective Java (진행중)

> 작년에 [스터디](https://github.com/BacknPacker/effective\_java)를 하면서 공부했던 내용이지만 복습을 위해 핵심 내용 위주로 정리하면서 학습했습니다. 이 부분의 대부분의 예시 코드는 이펙티브 자바 제공 [오픈소스](https://github.com/jbloch/effective-java-3e-source-code)에서 발췌했습니다.

![](https://image.yes24.com/goods/65551284/XL)\


**Contents**

* [2장. 객체 생성과 파괴](../backend/2023/04/30/Java.html#2%EC%9E%A5-%EA%B0%9D%EC%B2%B4-%EC%83%9D%EC%84%B1%EA%B3%BC-%ED%8C%8C%EA%B4%B4)
* [3장. 모든 객체의 공통 메서드](../backend/2023/04/30/Java.html#3%EC%9E%A5-%EB%AA%A8%EB%93%A0-%EA%B0%9D%EC%B2%B4%EC%9D%98-%EA%B3%B5%ED%86%B5-%EB%A9%94%EC%84%9C%EB%93%9C)

\


#### 2장. 객체 생성과 파괴

>

**아이템 1. 생성자 대신 정적 팩터리 메서드를 고려하라.**\
객체를 생성할 때 생성자 대신 정적 팩터리 메서드를 이용하면 객체의 생성과 파괴를 관리할 수 있습니다.\
여기서 `getInstance()`를 정적 팩터리 메서드 사례로 볼 수 있습니다. 이렇게 객체 생성에 대한 메서드이기 때문에 메서드명을 사용해서 반환 객체에 대한 설명 ex)`probablePrime` 을 쓸 수 있게 됩니다. 그리고 아래 싱글톤 예제처럼 인스턴스를 호출시마다 생성하지 않는 등 인스턴스 통제가 가능해집니다.

```java
class Singleton {
    private static Singleton singleton = null;
    private Singleton() {}
    static Singleton getInstance() {
        if (singleton == null) {
            singleton = new Singleton();
        }
        return singleton;
    }
}
```

반환 타입의 하위 타입 객체를 반환할 수 있습니다.

**아이템 2. 생성자에 매개변수가 많다면 필터를 고려하라.**\
점층적 생성자 패턴을 사용하면 받는 매개변수가 어떤 것인지 확인하기 한 눈에 파악이 어렵고 순서를 바꿔쓸 경우 문제가 발생할 수 있습니다. 물론 IntelliJ는 그 어려운 걸 해결해줍니다. 그래도 점층적 생성자 패턴이 여러 개 있을 때는 개발시 파악이 너무 어렵습니다.

```java
NutritionFacts cocaCola = new NutritionFacts(240, 8, 100, 0, 35, 27);
```

그렇다면 자바 빈즈 패턴은 어떨까요?

\


#### 3장. 모든 객체의 공통 메서드

***

[effective-java-3e-source-code](https://github.com/jbloch/effective-java-3e-source-code)\
[이펙티브 자바 Effective Java 3/E](https://www.yes24.com/Product/Goods/65551284)\
[한달한권｜이펙티브 자바](https://zero-base.co.kr/me/courses/207614)
