---
title: Interview
author: Rosie Yang
date: 2023-05-25T00:00:00.000Z
category: archive
---

# Interview

> 인터뷰를 위한 포스팅 페이지입니다 :)

\


**동시에 같은 DB Table row를 업데이트 하는 상황을 방어하기 위해 어떻게 개발하실건지 설명해주세요.**\
먼저, 서비스에 사용되고 있는 데이터베이스 동시성 제어 수준을 확인합니다. row lock이 동시 업데이트 상황에서 어떻게 작동하는지를 테스트하고 상황에 따라 제어 수준을 조정할 것 같습니다. 그 다음에는 lock이 걸린 데이터에 동시 접속률 요청이 많은 상황이라고 한다면, 일정 시간을 기다리고 몇 번의 재요청을 넣을 것인지 속성을 정합니다. 하지만 이런 방법들은 여전히 서버에 큰 부하가 발생할 수 있기 때문에 사전 개발단계에서 트랜잭션을 교착상태 발생을 줄일 수 있도록 더 작은 단위로 나누고 트랜잭션의 방향을 일치시키도록 해야 합니다.\
\


**TCP 와 UDP 의 차이를 작성해주세요.**\
TCP (Transmission Control Protocol)와 UDP (User Datagram Protocol)는 전송 계층 프로토콜로 TCP는 연결 지향 프로토콜로 통신 전 클라이언트와 서버 사이의 연결을 설정하고 UDP는 데이터그램 통신에 중점을 두고 패킷의 순서와 손실을 보장하지 않습니다. TCP는 응답확인, 재전송 메커니즘으로 추가적인 오버헤드가 발생할 수 있지만 신뢰성이 높습니다. UDP는 신뢰성을 보장하지 않고 오버헤드가 작아 전송지연도 적다는 특징이 있습니다. 따라서 신속한 데이터 전송이 중요한 경우 UDP를, 신뢰성이 중요한 파일 전송이나 이메일, 원격 접속(SSH) 등의 경우에 TCP를 사용합니다.

\


**웹 브라우저에 네이버 를 검색하고 화면에 네이버 화면이 출력이 될 때 까지 내부적으로 어떤 동작들이 수행이 되는지 설명해주세요.**\
클라이언트가 네이버를 검색하면, 네이버 도메인 주소로 DNS 조회를 통해 IP 주소를 얻습니다. 그리고 그 IP 주소로 네이버 서버와 TCP/IP 연결을 TCP handshake가 수행합니다. 연결이 되면 HTTP 요청으로 url 주소를 get 요청해서 서버 응답을 받습니다. 이렇게 받은 응답에는 HTML, CSS, JS 및 기타 리소스를 포함합니다. 이 응답을 기반으로 DOM 트리를 생성해서 화면을 랜더링하고 클라이언트가 볼 수 있도록 화면을 출력합니다.\
DOM 트리란 무엇인가?\
DOM(Document Object Model)은 웹 페이지의 구조와 내용을 표현하는 객체의 계층 구조입니다. 웹 브라우저는 HTML 문서를 받아들여 이를 파싱하여 DOM 트리를 생성합니다. 각 HTML 요소, 텍스트, 속성 등은 DOM의 노드로 표현되며, 이러한 노드들은 부모-자식 관계를 가지고 계층적으로 구성됩니다.

\


**본인이 주력으로 사용하는 언어에서 설계적 결함 한 가지를 작성해주세요.**\
제가 주력으로 사용하는 언어는 Java입니다.\
개발자 입장에서 가장 크게 생각되는 부분은 Null 처리에 대한 부분입니다. Java에서는 null 처리로 변수에 아무런 값이 없다는 것을 표현하지만 이와 관련된 처리를 하지 않을 경우, NullPointException이 발생해 예외 상황을 일으킬 수 있습니다. 대신 Optional을 이용해 해결하는 방법이 있습니다. 하지만 상황에 따라 바르게 Optional을 사용할 필요가 있습니다.\
그리고 메모리 오버헤드가 발생할 수 있습니다. GC(가비지 컬렉션)으로 메모리 할당과 해제를 자동적으로 관리하지만 일정 시간 텀을 두고 관리하기 때문에 오버헤드가 발생할 수 있습니다.\
Kotlin과 Python과 같은 언어들과 비교했을 때, 코드 자체가 복잡하기 때문에 유지보수와 테스트에서 java가 제공하는 다양한 기능들을 오용할 가능성이 있습니다.

\


**본인이 주력으로 사용하는 언어에서 자료구조와 관련 된 클래스가 내부적으로 어떻게 동작하는지 한 가지 사례를 정하여 작성해주세요. ex) ArrayList, HashMap 등등**\
Java의 ArrayList를 선언하면, initialCapacity에 따라 초기용량이 설정된 객체가 생성됩니다. 만약 별도 설정을 하지 않았다면, 10이라는 기본값으로 생성됩니다. 그리고 elementData라는 Object 타입 Array가 생성됩니다. 따라서 인덱스에 따라 임의의 데이터에 접근가능합니다.

```java
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

ArrayList에서 요소를 추가할 때, 기존 array를 복사하고 새롭게 계산된 크기의 배열을 생성한 뒤(grow 메서드 참조), 기존 배열을 삭제합니다.

```java
private void add(E e, Object[] elementData, int s) {
  if (s == elementData.length)
      elementData = grow();
  elementData[s] = e;
  size = s + 1;
}

private Object[] grow() {
  return grow(size + 1);
}
```

* [Java : ArrayList, LinkedList, HashMap](https://velog.io/@seonmikimm/Java-ArrayList-LinkedList-HashMap)
