## 22.1 this 키워드

- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수를 this로 한다.
- 자바스크립트 엔진에 의해 암묵적으로 생성된다.
- 코드 어디서든 참조가 가능하다.
- 함수 호출 시 arguments 객체와 this가 암묵적으로 함수 내부에 전달되므로 지역변수처럼 사용 가능하다.
- this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.

this 바인딩? 바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 예를 들어, 변수 선언은 변수이름과 확보된 메모리 공관의 주소를 바인딩하는 것이다. this 바인딩은 this(키워드로 분류되지만 식별자 역할을 한다)와 this가 가리킬 객체를 바인딩하는 것이다.

## 22.2 함수 호출 방식과 this 바인딩

- this 바인딩(this에 바인딩될 값)은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정 된다.
- 렉시컬 스코프와 this 바인딩은 결정 시기가 다르다. > 함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 this 바인딩은 함수 호출 시점에 결정된다.
- 함수 호출 방식
  - 일반 함수 호출: 일반 함수로 호출 시 전역 객체로 바인딩, strict mode에서는 undefined가 바인딩된다
  - 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩이 메서드의 this 바인딩과 일치하지 않으면 이들 함수의 본래 기능 활용이 어렵다.
- 메서드 호출
  - 메서드 내부의 this에서는 메서드를 호출한 객체가 바인딩된다.(메서드를 소유한 객체가 아니다.)
- 생성자 함수 호출
  - 생성자 함수 내부의 this에는 생성자 함수가 미래에 생성할 인스언스가 바인딩된다.
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출
  - 모든 함수가 상속받아 사용 가능
  - apply와 call 메서드는 함수 호출 시, 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩된다.
  - bind 메서드는 함수를 호출하지 않으므로, 첫 번째 인수를 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성하여 반환한다.(메서드의 this와 중첩함수 또는 콜백 함수의 this가 불일치하는 문제는 해결이 가능하다.)

## 22.2.1 일반 함수 호출

- 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.
- 콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다.
- 어떤 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.
- setTimeout 함수란? setTimeout 함수는 두 번째 인수로 전달한 시간만큼 대기한 다음, 첫 번째 인수로 전달한 콜백 함수를 호출하는 타이머 함수다.

## 22.2.2 메서드 호출

- 메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩된다.

```
const person = {
  name: 'Lee',
  getName() {
    // 메서드 나부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};
// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```

## 22.2.3 생성자 함수 호출

- 생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

```
function Circle(radius){
  this.radius= radius;
  this.getDiameter = function() {
    return 2 * this.radius;
  };
}
// 반지름 5 객체 생성
const circle1 = new Circle(5);
console.log(circle1.getDiameter()); // 10

// 일반 함수로 호출
const circle2 = Circle(10);
console.log(circle2); // undefined
```

- new 연산자로 생성하면 생성자 함수로, new 연산자로 생성하지 않으면 일반 함수로 동작한다.

## 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출

- apply, call, bind 메서드는 Function.prototype의 메서드이다.
- 이 메서드는 모든 함수가 상속받아 사용할 수 있다.
- apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이다.

```
function getThisBinding(){
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1};

console.log(getThisBinding()); // window

console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a:1}
```

- bind 메서드는 함수를 호출하지 않고 this로 사용할 객체만 전달한다.
- bind 메서드는 메서드의 this와 메서드 내부의 중첩 함수 또는 콜백 함수의 this가 불일치하는 문제를 해결하기 위해 사용된다.

```
function getThisBinding(){
  return this;
}
// this로 사용할 객체
const thisArg = { a: 1};

console.log(getThisBinding.bind(thisArg)); // getThisBinding

console.log(getThisBinding.bind(thisArg)()); // {a:1}
```

<img width="593" alt="스크린샷 2022-10-04 오후 4 49 26" src="https://user-images.githubusercontent.com/78072931/193764311-b186f076-668d-4318-ac34-cf42d6c0842b.png">
