## 39장 Dom

- HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조이다.

## 39.1장 노드

## 39.1.1장 HTML 요소와 노드 객체

- HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미한다.
  <img width="722" alt="스크린샷 2022-12-08 오후 11 52 55" src="https://user-images.githubusercontent.com/78072931/206478151-b523f80b-4fed-48d8-8508-80c61cc57ece.png">

- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환된다.
  <img width="606" alt="스크린샷 2022-12-08 오후 11 54 01" src="https://user-images.githubusercontent.com/78072931/206478401-73e10ff3-cf54-4c60-89ca-1001d1f0e306.png">

- 노드 객체들로 구성된 트리 자료구조를 DOM이라 한다. DOM 트리라고 부르기도 한다.

## 39.1.2 노드 객체의 타입

```
<!DOCTYPE html>
<html>
  <head>
    <mata charset="UTF-8">
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <ul>
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script src="app.js"></script>
  </body>
</html>
```

- 렌더링 엔진은 위 HTML 문서를 파싱하여 다음과 같이 DOM을 생성한다.
  <img width="649" alt="스크린샷 2022-12-09 오전 12 20 24" src="https://user-images.githubusercontent.com/78072931/206484368-b9cd62fb-1c7a-4de9-9d1a-0f12849ae78d.png">

- 중요한 노드 타입은 4가지이다.

  - 문서 노드
    DOM 트리의 최상단에 존재하는 루트노드, document 객체를 가리킨다.
    document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가르킨다
    전역 객체의 document 프로퍼티에 바인딩 되어있다. --> documnet로 참조 가능하다.
    최상단에 있기 때문에 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 한다.

  - 요소 노드
    HTML 요소를 가르키는 객체
    HTML 요소들의 중첩관계를 통해 문서의 구조를 표현한다.

  - 어트리뷰트 노드
    HTML 요소의 어트리뷰트를 가리키는 객체
    어트리뷰트 노드는 해당 요소 노드에만 연결되어 있다.

  - 텍스트 노드
    HTML 요소의 텍스트를 가리키는 객체
    해당 요소 노드의 자식 노드이며 말단 노드이다.

## 39.1.3 노드 객체의 상속 구조

- DOM을 구성하는 노드 객체는 프로토타입에 의한 상속 구조를 갖는다.
  <img width="766" alt="스크린샷 2022-12-09 오전 12 32 06" src="https://user-images.githubusercontent.com/78072931/206487392-2367d69f-6a1f-4887-b0d8-b78e2f924f5f.png">

- DOM 을 구성하는 노드 객체는 표준 빌트인 객체가 아니라 브라우저 환경에서 추가적으로 제공하는 호스트 객체
- 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 가진다.
- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속
- HTML 요소의 종류에 따라 고유의 기능을 가지기도 한다.
- DOM은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류. 즉, 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API로 제공.
- 이 DOM API를 통해 HTML의 구조나 내용 또는 스타일 등을 동적으로 조작 가능.
