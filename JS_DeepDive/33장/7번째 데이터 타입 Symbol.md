## 33 7번째 데이터 타입 Symbol

## 33.1 심벌이란?

- ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값이다.
- 심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다.
- 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.
- 프로퍼티 키로 사용할 수 있는 값은 빈 문자열을 포함하는 모든 문자열 또는 심벌 값이다.

## 33.2 심벌 값의 생성

## 33.2.1 Symbol 함수

```
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()
```

- Symbol 함수는 String, Number, Boolean 생성자 함수와 달리 new 연산자와 함께 호출하지 않는다.
- new 연산자와 함께 생성자 함수 또는 클래스를 호출하면 객체가 생성되지만 심벌 값은 변경 불가능한 원시 값이다.

```
new Symbol(); // TypeError
```

- Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있다.
- 문자열은 생성된 심벌 값에 대한 설명으로 디버깅 용도로만 사용되고, 심벌 값 생성에 어떠한 영향도 주지 않는다.

```
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = symbol('mySymbol');
const mySymbol2 = symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); // false
```

```
// 심벌 값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.
const mySymbol = Symbol('mySymbol');

// 심벌도 래퍼 객체를 생성한다.
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
- 불리언 타입으로는 암묵적으로 타입이 변환 가능한데 확인 하려면 if 문에서 존재 확인이 가능하다.

## 32.2.2 Symbol.for / Symbol.keyFor 메서드

- Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.
  - 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환한다.
  - 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환한다.
const s2 = Symbol.for('mySymbol');

console.log(s1 === s2); // true
```

```
// Symbol.keyFor 메서드를 사용하여 전역 심벌 레지스트리에 저장된 심벌 값의 키 추출

// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // undefined
```

## 33.3 심벌과 상수

```
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성.
const Direction = {
    UP: Symvol('up'),
    DOWN: Symbol('down'),
    LEFT: Symbol('left'),
    RIGHT: Symbol('right')
};

const myDirection = Direction.UP;

if(myDirection === Direction.UP) {
    console.log('You are going UP.');
}
```

- enum이란? enum은 명명된 숫자 상수의 집합으로 열거형이라고 부른다. 자바스크립트는 enum을 지원하지 않지만, c, 파이썬, 자바 등 여러 프로그래밍 언어와 자바스크립트의 상위 확장인 타입스크립트에서는 enum을 지원한다. 자바스크립트에서 enum을 흉내 내어 사용하려면 객체의 변경을 방지하기 위해 객체를 동결하는 Object.freeze 메서드와 심벌 값을 사용한다.

## 33.4 심벌과 프로퍼티 키

- 객체의 프로퍼티 키는 빈 문자열을 포함하는 모든 문자열 또는 심벌 값으로 만들 수 있으며, 동적으로 생성 할 수도 있다.

```
const onj = {
    // 심벌 값으로 프로퍼티 키를 생성
    // 심벌 값을 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용
    [Symbol.for('mySymbol')]: 1
};
// 프로퍼티에 접근할 때도 대괄호 사용
obj[Symbol.for('mySymbol')]; // 1
```

- 심벌 값을 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.

## 33.5 심벌과 프로퍼티 은닉

- 심벌 값을 프로퍼티 키로 사용하여 생상한 프로퍼티 for ...in 문이나 object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다. 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```
const obj = {
    // 심벌 값으로 프로퍼티 키 생성
    [Symbol('mySymbol')]: 1
};

for (const key in obj) {
    console.log(key); // 아무것도 출력이 안된다.
}
console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

- 프로퍼티 키를 찾을수 있는 메서드는 ES6에서 도입된 Object.getOwnPropertySymbols 메서드를 사용하면 된다.

## 33.6 심벌과 표준 빌트인 객체 확장

- 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.
- 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.

## 33.7 Well-Known Symbol

- JS가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known Symbol 이라고 부른다.

Well-known Symbol은 JS 엔진의 내부 알고리즘에 사용된다. 예를 들어, Array, String … 과 같이 for…of로 순회 가능한 빌트인 이터러블은 Well-knwon Symbol인 Symbol.iterator를 키로 갖는 메서드를 가진다.

심벌은 중복되지 않는 상수 값을 생성하는 것은 물론 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해, 즉 하위 호환성을 보장하기 위해 도입되었다.
