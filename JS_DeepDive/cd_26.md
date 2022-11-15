# 26장. ES6 함수의 추가 기능
## 26.1 함수의 구분
ES6 이전의 함수는 동일한 함수라도 다양한 형태로 호출가능
```js
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // 1

// 생성자 함수로서 호출
new foo(); // foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // 1
```
ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출 가능
ES6 이전의 모든 함수는 callable 이면서 constructor

```js
var foo = function () {};

// ES6 이전의 모든 함수는 callable이면서 constructor다.
foo(); // undefined
new foo(); // foo {}
```
> callable과 constructor/non-constructor
>> 17.2.4절 "내부메서드 \[\[Call]]과 \[\[Construct]]" 호출할 수 있는 함수 객체를 callable이라 하며, 인스턴스를 생성할 수 있는 함수 객체를 constructor, 인스턴스를 생성할 수 없는 함수 객체를 non-constructor
>> ES6 이전에 일반적으로 메서드라고 부르던 객체에 바인딩된 함수도 callable이며 constructor
>> 객체에 바인딩된 함수도 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로 호출가능

```js
// 프로퍼티 f에 바인딩된 함수는 callable이며 constructor다.
var obj = {
  x: 10,
  f: function () { return this.x; }
};

// 프로퍼티 f에 바인딩된 함수를 메서드로서 호출
console.log(obj.f()); // 10

// 프로퍼티 f에 바인딩된 함수를 일반 함수로서 호출
var bar = obj.f;
console.log(bar()); // undefined

// 프로퍼티 f에 바인딩된 함수를 생성자 함수로서 호출
console.log(new obj.f()); // f {}
```

위 예제와 같이 객체에 바인딩된 함수를 생성자 함수로 호출이 문법상 가능하다는 것은 문제
-> 성능 문제도

객체에 바인딩된 함수가 constructor라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티 가지며, 프로토타입 객체도 생성
콜백 함수도 마찬가지, 콜백 함수도 constructor이기 때문에 불필요한 프로토타입 객체를 생성

```js
// 콜백 함수를 사용하는 고차 함수 map. 콜백 함수도 constructor이며 프로토타입을 생성한다.
[1, 2, 3].map(function (item) {
  return item * 2;
}); // [ 2, 4, 6 ]
```
ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 프로토타입 객체를 생성
이는 혼란스러우며 실수를 유발할 가능성이 있고 성능에도 좋지 않다.

ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분했다.
|ES6 함수의 구분|constructor|prototype|super|arguments|
|--|--|--|--|--|
|일반 함수(Normal)|O|O|X|O|
|메서드(Method)|X|X|O|O|
|화살표 함수(Arrow)|X|X|X|X|

일반 함수는 함수 선언문이나 함수 표현식으로 정의한 함수(ES6 이전의 함수와 차이X)
ES6의 메서드와 화살표 함수는 ES6 이전의 함수와 명확한 차이

일반 함수는 constructor이지만 ES6의 메서드와 화살표 함수는 non—constructor

## 26.2 메서드
ES6 이전 사양에는 메서드에 대한 명확한 정의X
일반적으로 메서드는 객체에 바인딩된 함수를 일컫는 의미로 사용
ES6 사양에서는 메서드에 대한 정의가 명확하게 규정 -> 메서드 축약 표현으로 정의된 함수만 의미
```js
const obj = {
  x: 1,
  // foo는 메서드다.
  foo() { return this.x; },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수다.
  bar: function() { return this.x; }
};
console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```
ES6 사양에서 정의한 메서드(이하 ES6 메서드)는 인스턴스를 생성할 수 없는 non-constructor
따라서 ES6 메서드는 생성자 함수로서 호출X

```js
new obj.foo(); // TypeError: obj.foo is not a constructor
new obj.bar(); // bar {}
```

ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성X
```js
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // false
// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // true
```

참고로 표준 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor다.
```js
String.prototype.toUpperCase.prototype; // undefined
String.fromCharCode.prototype; // undefined

Number.prototype.toFixed.prototype; // undefined
Number.isFinite.prototype; // undefined

Array.prototype.map.prototype; // undefined
Array.from.prototype; // undefined
```
ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 \[\[HomeObject]]
super 참조는 내부 슬롯 \[\[HomeObject] 사용 수퍼클래스의 메서드를 참조
내부 슬롯 \[\[HomeObject]]를 갖는 ES6 메서드는 super 키워드를 사용가능

```js
const base = {
  name: 'Lee',
  sayHi() {
    return `Hi! ${this.name}`;
  }
};
const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
  // sayHi의 [[HomeObject]]는 derived.prototype을 가리키고
  // super는 sayHi의 [[HomeObject]의 프로토타입인 base.prototype을 가리킨다.
  sayHi() {
    return `${super.sayHi()}. how are you doing?`;
  }
};

console.log(derived.sayHi()); // Hi! Lee. how are you doing?
```
ES6 메서드가 아닌 함수는 super 키워드를 사용X
ES6 메서드가 아닌 함수는 내부 슬롯 \[\[HomeObject]] 없음
```js
const derived = {
  __proto_ : base,
  // sayHi는 ES6 메서드가 아니다.
  // 따라서 sayHi는 [[HomeObject]]를 갖지 않으므로 super 키워드를 사용할 수 없다.
  sayHi: function () {
    // SyntaxError: 'super' keyword unexpected here
    return `${super.sayHi()}. how are you doing?`;
  }
};
```

ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거
메서드를 정의할 때 <font size=16>***프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋다.***</font>

## 26.3 화살표 함수
화살표 함수는 function 키워드 대신 화살표(=>, fat arrow)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의
화살표 함수는 표현만 간략한 것이 아니라 내부 동작도 기존의 함수보다 간략
특히 화살표 함수는 <font size=16>콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용</font>

### 26.3.1 화살표 함수정의
화살표 함수 정의 문법

#### 함수 정의
화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의, 호출 방식은 기존 함수와 동일
```js
const multiply = (x, y) => x * y;
multiply(2, 3); // 6
```
매개변수 선언
매개변수가 여러 개인 경우 소괄호 () 안에 매개변수 선언
```js
const arrow = (x, y) => { ... };
```
매개변수가 한 개인 경우 소괄호 ()를 생략
```js
const arrow = x => { ... };
```
매개변수가 없는 경우 소괄호 ()를 생략X
```js
const arrow = () => { ... };
```

#### 함수 몸체 정의
함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략
이때 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환
```js
// concise body
const power = x => x ** 2;
power(2); // 4

// 위 표현은 다음과 동일하다.
// block body
const power = x => { return x ** 2; };
```
함수 몸체를 감싸는 중괄호 {}를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생
표현식이 아닌 문은 반환할 수 없기 때문
```js
const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'

// 위 표현은 다음과 같이 해석된다.
const arrow = () => { return const x = 1; };
```
따라서 함수 몸체가 하나의 문으로 구성된다 해도 함수 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략X
```js
const arrow = () => { const x = 1; };
```
객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호 ()로 감싸
```js
const create = (id, content) => ({ id, content });
create(1, 'JavaScript'); // {id:1, content:"JavaScript"}
// 위 표현은 다음과 동일하다.
const create = (id, content) => { return { id, content }; };
```
객체 리터럴을 소괄호()로 감싸지 않으면 객체 리터럴의 중괄호 {}를 함수 몸체를 감싸는 중괄호 {}로 잘못 해석
```js
// { id, content }를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
const create = (id, content) => { id, content };
create(1, 'JavaScript'); // undefined
```
함수 몸체가 여러 개의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {}를 생략X
이때 반환값이 있다면 명시적으로 반환해야함
```js
const sum = (a, b) => {
  const result = a + b;
  return result;
};
```
화살표 함수도 즉시 실행 함수로 사용가능
```js
const person = (name => ({
  sayHi() { return `Hi? My name is ${name}.`; }
}))('Lee');

console.log(person.sayHi()); // Hi? My name is Lee.
```
화살표 함수도 일급 객체이므로 Array.prototype.map, Array.prototype.filter, Array.prototype.reduce 같은 고차 함수에 인수로 전달가능
일반적인 함수 표현식보다 표현 간결, 가독성 좋음
```js
// ES5
[1, 2, 3].map(function (v) {
  return v * 2;
});

// ES6
[1, 2, 3].map(v => v * 2); // [ 2, 4, 6 ]
```
화살표 함수는 콜백 함수로서 정의할 때 유용
화살표 함수는 일반 함수의 기능을 간략화, this도 편리하게 설계

### 26.3.2 화살표 함수와 일반 함수의 차이
#### 01. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor
```js
const Foo = () => {};
// 화살표 함수는 생성자 함수로서 호출할 수 없다.
new Foo(); // TypeError:Foo is not a constructor
```
화살표 함수는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성X
```js
const Foo = () => {};
// 화살표 함수는 prototype 프로퍼티가 없다.
Foo.hasOwnProperty('prototype'); // false
```
#### 02. 중복된 매개변수 이름을 선언X
일반 함수는 중복된 매개변수 이름을 선언해도 에러가 발생X
```js
function normal(a, a) { return a + a; }
console.log(normal(1, 2)); // 4
```
strict mode에서 중복된 매개변수 이름을 선언하면 에러
```js
'use strict';

function normal(a, a) { return a + a; }
// SyntaxError: Duplicate parameter name not allowed in this context
```

화살표 함수에서도 중복된 매개변수 이름을 선언하면 에러 발생
```js
const arrow = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```
#### 03. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지X
화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조

만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도
this, arguments, super, new.target 바인딩이 없으므로
스코프 체인 상에서 가장 가까운 상위 함수 중에서
화살표 함수가 아닌 함수의 this, arguments, super, new.target을 참조

### 26.3.3 this
화살표 함수가 일반 함수와 구별되는 가장 큰 특징 this
화살표 함수는 다른 함수의 인수로 전달되어 콜백 함수로 사용되는 경우가 많음

화살표 함수의 this는 일반 함수의 this와 다르게 동작
이는 콜백 함수 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계된 것
"콜백 함수 내부의 this 문제"에 대해 다시보기(22장 "this")

this 바인딩은 함수의 호출 방식에 따라 동적 결정
주의할 것은 일반 함수로서 호출되는 콜백 함수의 경우
고차 함수의 인수로 전달되어 고차 함수 내부에서 호출되는 콜백 함수도 중첩 함수라고 할 수 있음
주어진 배열의 각 요소에 접두어를 추가하는 다음 예제
```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  
  add(arr) {
    // add 메서드는 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가
    // (1)
    return arr.map(function (item) {
      return this.prefix + item; // (2)
      // TypeError: Cannot read property 'prefix ' of undefined
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```
위 예제를 실행했을 때 기대하는 결과는
['-webkit-transition', '-webkit-user-select'], 하지만 TypeError가 발생

프로토타입 메서드 내부인 (1)에서 this는 메서드를 호출한 객체(위 예제의 경우 prefixer 객체)를 가리킴
그런데 Array.prototype.map의 인수로 전달한 콜백 함수의 내부인 (2)에서 this는 undefined를 가리킴
이는 Array.prototype.map 메서드가 콜백 함수를 일반 함수로서 호출하기 때문

> Array.prototype.map 메서드
>> Array.prototype.map 메서드는 배열을 순회하며 배열의 각 요소에 대하여 인수로 전달된 콜백 함수를 호출
>> 콜백 함수의 반환값들로 구성된 새로운 배열을 반환
>> 위 예제에서 map 메서드는 매개변수 arr에 전달된 ['transition', 'user-select']를 순회하며
>> 콜백 함수의 item 매개변수에게 arr의 요소값을 전달하면서 콜백 함수를 arr의 요소 개수만큼 호출
>> 그리고 콜백 함수의 반환값들로 구성된 새로운 배열을 반환
>> 27.9.3절 "Array.prototype.map"에서 자세히
>> 지금은 일반 함수로서 호출되는 "콜백 함수 내부의 this 문제"에 주목

22장 "this"에서 살펴보았듯이 일반 함수로서 호출되는 모든 함수 내부의 this는 전역 객체를 가리킴
클래스 내부의 모든 코드에는 strict mode가 암묵적으로 적용
따라서 Array.prototype.map 메서드의 콜백 함수에도 strict mode가 적용
strict mode에서 일반 함수로서 호출된 모든 함수 내부의 this에는 전역 객체가 아니라 undefined가 바인딩
일반 함수로서 호출되는 Array.prototype.map 메서드의 콜백 함수 내부의 this에는 undefined가 바인딩

이때 발생하는 문제가 바로 "콜백 함수 내부의 this 문제"
콜백 함수의 this (2)와 외부 함수의 this (1)가 서로 다른 값을 가리키고 있기 때문에 TypeError가 발생
이와 같은 "콜백 함수 내부의 this 문제"를 해결하기 위해 ES6 이전에는 다음과 같은 방법을 사용

#### 01. add 메서드를 호출한 prefixer 를 가리키는 this를 일단 회피시킨 후에 콜백 함수 내부에서 사용
```js
...
add(arr) {
  // this를 일단 회피시킨다.
  const that = this;
  return arr.map(function (item) {
    // this 대신 that을 참조한다.
    return that.prefix + ' ' + item;
  });
}
...
```
#### 02. Array.prototype.map의 두 번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this를 전달
ES5에서 도입된 Array.prototype.map은 "콜백 함수 내부의 this 문제"를 해결하기 위해 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달가능
```js
...
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }, this); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
}
...
```
#### 03. Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩
```js
...
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }.bind(this)); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
}
...
```
ES6에서는 화살표 함수를 사용하여 "콜백 함수 내부의 this 문제"를 해결
```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  } 

  add(arr) {
    return arr.map(item => this.prefix + item);
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
// ['-webkit-transition', '-webkit-user-select']
```
화살표 함수는 함수 자체의 this 바인딩을 갖지X
<font size=16>화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조 -> lexical this </font>

마치 렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위치에 의해 결정된다는 것을 의미

화살표 함수를 제외한 모든 함수에는 this 바인딩이 반드시 존재
ES6에서 화살표 함수가 도입되기 이전에는 일반적인 식별자처럼 스코프 체인을 통해 this를 탐색할 필요X
화살표 함수는 함수 자체의 this 바인딩이 존재X
화살표 함수 내부에서 this를 참조하면 일반적인 식별자처럼 스코프 체인을 통해 상위 스코프에서 this를 탐색
화살표 함수를 Function.prototype.bind를 사용하여 표현하면
```js
// 화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;

// 익명 함수에 상위 스코프의 this를 주입한다. 위 화살표 함수와 동일하게 동직한다.
(function () { return this.x; }).bind(this);
```
만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로
스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조
```js
// 중첩 함수 foo의 상위 스코프는 즉시 실행 함수다.
// 따라서 화살표 함수 foo의 this는 상위 스코프인 즉시 실행 함수의 this를 가리킨다.
(function () {
  const foo = () => console.log(this);
  foo();
}).call( { a: 1 }); // { a:1 }

// bar 함수는 화살표 함수를 반환한다.
// bar 함수가 반환한 화살표 함수의 상위 스코프는 화살표 함수 bar다.
// 하지만 화살표 함수는 함수 자체의 this 바인딩을 갖지 않으므로 bar 함수가 반환한
// 화살표 함수 내부에서 참조하는 this는 화살표 함수가 아닌 즉시 실행 함수의 this를 가리킨다.
(function () {
  const bar = () => () => console.log(this);
  bar()();
}).call( { a: 1 }); // { a:1 }
```
화살표 함수가 전역 함수라면 화살표 함수의 this는 전역 객체를 가리킴
전역 함수의 상위 스코프는 전역이고 전역에서 this는 전역 객체를 가리키기 때문
```js
// 전역 함수 foo의 상위 스코프는 전역이므로 화살표 함수 foo의 this는 전역 객체를 가리킨다.
const foo = () => console.log(this);
foo(); // window
```
프로퍼티에 할당한 화살표 함수도 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조
```js
// increase 프로퍼티에 할당한 화살표 함수의 상위 스코프는 전역이다.
// 따라서 increase 프로퍼티에 할당한 화살표 함수의 this는 전역 객체를 가리킨다.
const counter = {
  num: 1,
  increase: () => ++this.num
};

console.log(counter.increase()); // NaN
```
화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 Function.prototype.call, Function.prototype.
apply, Function.prototype.bind 메서드를 사용해도 화살표 함수 내부의 this를 교체X
```js
window.x = 1;

const normal = function () { return this.x; };
const arrow = () => this.x;

console.log(normal.call({ x: 10 })); // 10
console.log(arrow.call({ x: 10 })); // 1
```
화살표 함수가 Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를
호출할 수 없다는 의미X
<font size=16>화살표 함수는 함수 자체의 this 바인딩X, this 교체X, 상위 스코프의 this 바인딩 참조</font>

```js
const add = (a, b) => a + b;
console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
console.log(add.bind(null, 1, 2)()); // 3
```
메서드를 화살표 함수로 정의하는 것은 피해야
화살표 함수로 메서드를 정의 (ES6 이전 메서드)
```js
// Bad
const person = {
  name: 'Lee',
  sayHi: () => console.log(`Hi ${this.name}`)
};

// sayHi 프로퍼티에 할당된 화살표 함수 내부의 this는 상위 스코프인 전역의 this가 가리키는
// 전역 객체를 가리키므로 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는 window.name과 같다.
// 전역 객체 window에는 빌트인 프로퍼티 name이 존재한다.
person.sayHi(); // Hi
```
위 예제 sayHi 프로퍼티에 할당한 화살표 함수 내부의 this는 메서드를 호출한 객체인 person을 가리키지 않고
상위 스코프인 전역의 this 가 가리키는 전역 객체를 가리킴

따라서 화살표 함수로 메서드를 정의하는 것은 바람직하지X
메서드를 정의할 때는 ES6 메서드 사용
```js
// Good
const person = {
  name: 'Lee',
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
};

person.sayHi(); // Hi Lee
```
프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제가 발생
```js
// Bad
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = () => console.log(`Hi ${this.name}`);

const person = new Person('Lee');
// 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는 window.name과 같다.
person.sayHi(); // Hi
```
프로퍼티를 동적 추가할 때는 ES6 메서드 정의를 사용할 수 없으므로 일반 함수를 할당
```js
// Good
function Person(name) {
  this.name = name;
}

Person.prototype.sayHi = function () { console.log(`Hi ${this.name}`); };

const person = new Person('Lee');
person.sayHi(); // Hi Lee
```
일반 함수가 아닌 ES6 메서드를 동적 추가하고 싶다면 다음과 같이 객체 리터럴을 바인딩하고
프로토타입의 constructor 프로퍼티와 생성자 함수 간의 연결을 재설정
```js
function Person(name) {
  this.name = name;
}

Person.prototype = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 재설정
  constructor: Person,
  sayHi() { console.log(`Hi ${this.name}`); }
};

const person = new Person('Lee');
person.sayHi(); // Hi Lee
```
클래스 필드 정의 제안을 사용하여 클래스 필드에 화살표 함수를 할당가능
```js
// Bad
class Person {
  // 클래스 필드 정의 제안
  name = 'Lee';
  sayHi = () => console.log(`Hi ${this.name}`);
}

const person = new Person();
person.sayHi(); // Hi Lee
```
이때 sayHi 클래스 필드에 할당한 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this 바인딩을 참조
그렇다면 sayHi 클래스 필드에 할당한 화살표 함수의 상위 스코프는?
-> sayHi 클래스 필드는 인스턴스 프로퍼티이므로 아래와 같음
```js
class Person {
  constructor() {
  this.name = 'Lee';
    // 클래스가 생성한 인스턴스(this)의 sayHi 프로퍼티에 화살표 함수를 할당한다.
    // 따라서 sayHi 프로퍼티는 인스턴스 프로퍼티다.
    this.sayHi = () => console.log(`Hi ${this.name}`);
  }
}
```
sayHi 클래스 필드에 할당한 화살표 함수의 상위 스코프는 사실 클래스 외부
하지만 this는 클래스 외부의 this를 참조하지 않고 클래스가 생성할 인스턴스를 참조
따라서 sayHi 클래스 필드에 할당한 화살표 함수 내부에서 참조한 this는 constructor 내부의 this 바인딩과 같음
constructor 내부의 this 바인딩은 클래스가 생성한 인스턴스를 가리키므로
sayHi 클래스 필드에 할당한 화살표 함수 내부의 this 또한 클래스가 생성한 인스턴스를 가리킴
하지만 클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아니라 인스턴스 메서드가 됨
따라서 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋음
```js
// Good
class Person {
  // 클래스 필드 정의
  name = ' Lee';
  
  sayHi() { console.log(`Hi ${this.name}`); }
}
const person = new Person();
person.sayHi(); // Hi Lee
```

### 26.3.4 super
화살표 함수는 함수 자체의 super 바인딩X
화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조
```js
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  // 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
  sayHi = () => `${super.sayHi()} how are you doing?`;
} 

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```
super는 내부 슬롯 \[\[HomeObject]]를 갖는 ES6 메서드 내에서만 사용할 수 있는 키워드
sayHi 클래스 필드에 할당한 화살표 함수는 ES6 메서드는 아니지만 함수 자체의 super 바인딩을 갖지 않으므로
super를 참조해도 에러가 발생하지 않고
this와 마찬가지로 클래스 필드에 할당한 화살표 함수 내부에서 super를 참조하면 constructor 내부의 super 바인딩을 참조
위 경우 Derived 클래스의 constructor는 생략 되었지만 암묵적으로 constructor가 생성(25.8.4절 "서브클래스의 constructor" 참고)

### 26.3.5 arguments
화살표 함수는 함수 자체의 arguments 바인딩 X
화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조
```js
(function () {
  // 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
  const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
  foo(3, 4);
}(1, 2));

// 화살표 함수 foo의 arguments는 상위 스크프인 전역의 arguments를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError: arguments is not defined
```
(18.2.1절 "arguments 프로퍼티") arguments 객체는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용
하지만 화살표 함수에서는 arguments 객체 사용X
상위 스코프의 arguments 객체를 참조할 수는 있지만 화살표 함수 자신에게 전달된 인수 목록을 확인할 수 없고
상위 함수에게 전달된 인수 목록을 참조하므로 필요X
화살표 함수로 가변 인자 함수를 구현해야 할 때는 반드시 Rest 파라미터를 사용

## 26.4 Rest 파라미터
### 26.4.1 기본문법
Rest 파라미터(나머지 매개변수)는 매개변수 이름 앞에 세개의 점 ... 을 붙여서 정의한 매개변수를 의미
Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달
```js
function foo(...rest) {
  // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터다.
  console.log(rest); // [ 1, 2, 3, 4, 5 ]
}

foo(1, 2, 3, 4, 5);
```
일반 매개변수와 Rest 파라미터는 함께 사용가능
함수에 전달된 인수들은 매개변수와 Rest 파라미터에 순차적으로 할당
```js
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest); // [ 2, 3, 4, 5 ]
} 

foo(1, 2, 3, 4, 5);

function bar(param1, param2, ...rest) {
  console.log(param1); // 1
  console.Iog(param2); // 2
  console.log(rest); // [ 3, 4, 5 ]
}

bar(1, 2, 3, 4, 5);
```
Rest 파라미터는 이름 그대로 먼저 선언된 매개변수에 할당된 인수를 제외한 나머지 인수들로 구성된 배열이 할당
Rest 파라미터는 반드시 마지막 파라미터로
```js
function foo(...rest, param1, param2) { }
foo(1, 2, 3, 4, 5);
// SyntaxError: Rest parameter must be last formal parameter
```
Rest 피리미터는 하나만 선언가능
```js
function foo(...restl, ...rest2) { }
foo(1, 2, 3, 4, 5);
// SyntaxError: Rest parameter must be last formal parameter
```
Rest 파라미터는 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티에 영향X
```js
function foo(...rest) {}
console.log(foo.length); // 0

function bar(x, ...rest) {}
console.log(bar.length); // 1

function baz(x, y, ...rest) {}
console.log(baz.length); // 2
```
### 26.4.2 Rest 파라미터와 arguments 객체
ES5에서 가변 인자 함수, arguments 활용
arguments 객체 순회 가능한 유사 배열 객체
```js
// 매개변수의 개수를 사전에 알 수 없는 가변 인자 함수
function sum() {
  // 가변 인자 함수는 arguments 객체를 통해 인수를 전달받는다.
  console.log(arguments);
}

sum(1, 2); // {length: 2, '0': 1, '1': 2}
```
arguments 유사 배열 객체 배열 메서드를 사용하려면 
Function.prototype.call이나 Function.prototype.apply 메서드를 사용
arguments 객체를 배열로 변환
```js
function sum() {
  // 유사 배열 객체인 arguments 객체를 배열로 변환한다.
  var array = Array.prototype.slice.call(arguments);
  
  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
} 
console.log(sum(1, 2, 3, 4, 5)); // 15
```
ES6에서는 rest 파라미터를 사용하여 가변 인자 함수의 인수 목록을 배열로 직접 전달
```js
function sum(...args) {
  // Rest 파라미터 args에는 배열 [1, 2, 3, 4, 5]가 할당된다.
  return args.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // 15
```
함수와 ES6 메서드는 Rest 파라미터와 arguments 객체를 모두 사용가능
화살표 함수는 arguments X, Rest 파라미터를 사용

## 26.5 매개변수 기본값
매개변수가 비어도 에러X
인수가 전달되지 않은 매개변수의 값은 undefined -> 의도치 않은 결과
```js
function sum(x, y) {
  return x + y;
} 
console.log(sum(1)); // NaN
```
인수 전달 확인, 기본값 할당 -> 방어 코드
```js
function sum(x, y) {
  // 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당한다.
  x = x || 0;
  y = y || 0;

  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```
ES6에서 도입된 매개변수 기본값 인수 체크 및 초기화 간소화
```js
function sum(x = 0, y = 0) {
  return x + y;
} 

console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
```
인수를 전달하지 않은 경우와 undefined를 전달하는 경우 유효
```js
function logName(name = 'Lee') {
  console.log(name);
}

logName(); // Lee
logName(undefined); // Lee
logName(null); // null

```
Rest 기본값X
```js
function foo(...rest = []) {
  console.log(rest);
}
// SyntaxError: Rest parameter may not have a default in itializer
```
매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 함수 객체의 length 프로퍼티와 arguments 객체에 영향 X
```js
function sum(x, y = 0) {
  console.log(arguments);
}

console.log(sum.length); // 1

sum(1); // Arguments { '0': 1 }
sum(1, 2); // Arguments { '0': 1, '1': 2 }
```