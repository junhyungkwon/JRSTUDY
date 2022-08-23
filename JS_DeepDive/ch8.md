# 8장 제어문
조건에 따라 코드 블록을 실행 하거나 반복 실행 할때 사용

나중에 살펴볼 forEach, map, filter, reduce 같은 고차 함수를 사용한 함수형 프로그래밍 기법에서는 제어문의 사용을 억제하여 복잡성을 해결하려고 노력한다.

## 8.1 블록문
0개 이상의 문을 중괄호로 묶은것으로, 코드 블록 또는 블록이라함
자바스크립트는 *블록문을 하나의 실행 단위로 취급*
일반적으로는 제어문이나 함수를 정의할때 사용하는것이 일반적이며, 단독으로도 사용가능

블록문은 자체 종결성을 갖기 때문에 블록문의 끝에는 세미콜론을 붙이지 않는다.

```js
// 블록문
{
    var foo = 10;
}

// 제어문
var x = 1;
if (x < 10){
    x++;
}

// 함수 선언문
function sum(a, b){
    return a + b;
}
```

## 8.2 조건문
조건식의 평가 결과에 따라 코드 블록의 실행을 결정한다.
> 조건식 불리언 값으로 평가될 수 있는 표현식

if...else, switch문으로 두가지 조건문 제공

### 8.2.1 if...else
주어진 조건식(불리언 값으로 평가될 수 있는 표현식)의 평가 결과, 논리적 참 또는 거짓에 따라 실행할 코드 블록을 결정한다.

```js
if (조건식) {
    // 조건식이 참이면 이 코드 블록이 실행된다.
}else {
    // 조건식이 거짓이면 이 코드 블록이 실행된다.
}
```

만약 조건식이 불리언 값이 아닌 값으로 평가되면 자바스크립트 엔진에 의해 암묵적으로 불리언 값으로 강제 변환되어 실행할 코드 블록을 결정 *9.2 암묵적 타입 변환*에서 자세히

조건식을 추가하여 조건에 따라 실행될 코드 블록을 늘리려면 else if문을 사용
```js
if (조건식) {
    // 조건식이 참이면 이 코드 블록이 실행된다.
}else if (조건식2) {
    // 조건식2가 참이면 이 코드 블록이 실행된다.
}else {
    // 조건식이 거짓이면 이 코드 블록이 실행된다.
}
```

else if 문과 else 문은 옵션이다. else if문은 여러번 사용할 수 있다.
만약 코드 블록 내의 문이 하나뿐이라면 중괄호를 생략할 수 있다.
```js
var num = 2;
var kind;
if (num > 0)        kind = '양수';
else if (num < 0)   kind = '음수';
else                kind = '영' ;

console.log(kind); // 양수
```
대부분의 if else는 삼항 연산자로 바꿔 쓸 수 있다.
```js
// x가 짝수이면 result 변수에 문자열 '짝수'을 할당하고, 홀수이면 문자열 '홀수'를 할당한다.
var x = 2;
var result;
if (x % 2) { // 2%2는 0이다. 이때 0은 false로 암묵적 강제 변환된다.
    result = '홀수' ;
} else {
    result = '짝수' ;
}

// 삼항 연산자로 바꾸면
// 0은 false로 취급된다.
var result = x % 2 ? '홑수' : '짝수';
console.log(result); // 짝수

// 삼항 연산자로 짝수, 홀수, 영은
var kind = num ? (num > 0 ? '양수' : '음수') : '영';
console.log(result); // 짝수
```
삼항 조건 연산자는 값으로 평가되는 표현식이므로 변수에 할당 할 수 있지만,
if...else문은 표현식이 아닌 문이기 때문에 변수에 할당할 수 없다.

### 8.2.2 switch 문
switch 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 흐름을 옮긴다.
case문은 표현식을 지정하고 콜론으로 마친다. 그 뒤에 실행할 문들을 위치시킨다.
일치하는 case문이 없다면 default 문으로 이동한다. (default는 선택사항)

```js
switch(표현식){
    case 표현식1:
        // switch 문의 표현식과 일치하면 실행
        break;
    default:
        // 일치하는 case문이 없을때 실행
}
```

switch 문의 표현식은 불리언 값보다는 문자열이나 숫자값인 경우가 많다.
switch는 논리적 참,거짓보다는 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용
```js
// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;
switch (month) {
    case 1：monthName  = 'January';  break;
    case 2: monthName  = 'February'; break;
    case 3：monthName  = 'March';    break;
    case 4：monthName  = 'April';    break;
    case 5：monthName  = 'May';  break;
    case 6: monthName  = 'June'; break;
    case 7: monthName  = 'July'; break;
    case 8: monthName  = 'August';   break;
    case 9：monthName  = 'September';    break;
    case 10：monthName = 'October';  break;
    case 11：monthName = 'November'; break;
    case 12: monthName = 'December'; break;
    default: monthName = 'Invalid month';
}
console.log(monthName); // Invalid month
```
break가 없으면 일치하는 case부터 아래 위치한 모든 case와 default가 실행됨
(fall through)
이를 활용하는 경우도 있음
```js
var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
var month = 2;
var days = 0;
switch (month) {
case 1 ：case 3 ：case 5: case 7: case 8 ：case 10: case 12:
    days = 31;
    break;
case 4 ：case 6 ：case 9 ：case 11:
    days = 30;
    break;
case 2:
    // 윤년 계산 알고리즘
    // 1. 연도가 4로 나누어떨어지는 해(2000, 2004, 2008, 2012, 2016, 2020... )는 윤년이다.
    // 2. 연도가 4로 나누어떨어지더라도 연도가 100으로 나누어떨어지는 해 (2000, 2100, 2200... )는 평년이다.
    // 3. 연도가 4000로 나누어떧어지는 채(2000, 2400, 2800... )는 윤년이다.
    days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
    break;
default:
    console.log ('Invalid month');
} 
console.log (days); // 29
```

switch문은 fall through가 발생하는 등 문법도 복잡하지만 c언어를 기반으로 하는 프로그래밍 언어에서는 대부분 지원합니다. (python 처럼 지원하지 않는 언어도 있음)

if...else로 해결되면 switch보다 if...else로
조건이 많을땐 가독성을 위해 switch

## 8.3 반복문
조건식의 평가 결과가 참인 경우 코드 블록을 실행.
조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행 (조건식이 거짓일때 까지 반복)
for, while, do...while

> 반복문을 대체할 수 있는 다양한 기능
>> 자바스크립트는 배열을 순회할 때 사용하는 foreEach 메서드. 객체의 프로퍼티를 열거할 때 사용하는 for... in 문. ES6에서 도입된 이터러블을 순회할 수 있는 for...of문과 같이 반복문을 대체할 수 있는 다양한 기능을 제공한다. 이에 대해서는 해당 장에서 자세히

> 27.9.2 foreach
> 19.14.1 for...in
> 34.3 for...of

### 8.3.1 for 문
조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행
```js
for (변수 선언문 또는 할당문; 조건식; 증감식) {
    조건식이 참인 경우 반복 실행될 문;
}
```

```js
for (var i = 0; i < 2; i++) {
    console.log(i) ;
} // 0 1
```
실행순서
① for 문을 실행하면 맨 먼저 반수 선언문 var i = 0이 실행된다. 반수 선언문은 단 한 번만 실행된다.
② 변수 선언문의 실행이 종료되면 조건식이 실행된다. 현재 i 변수의 값은 0이므로 조건식의 평가 결과는 true다.
③ 조건식의 평가 결과가 true이므로 코드 블록이 실행된다. 증감문으로 실행 흐름이 이동하는 것이 아니라 코드 블록으로 실행 흐름이 이동하는 것에 주의하자.
④ 코드 블록의 실행이 종료되면 증감식 i++가 실행되어 i 변수의 값은 1이 된다.
⑤ 증감식 실행이 종료되면 다시 조건식이 실행된다. 변수 선언문이 실행되는 것이 아니라 조건식이 실행된다는 점에 주의하자(앞에서 설명했지만 변수 선언문은 단 한 번만 실행된다). 현재 i 변수의 값은 1이므로 조건식의 평가 결과는 true다
⑥ 조건식의 평가 결과가 true이므로 코드 블록이 다시 실행된다.
⑦ 코드 블록의 실행이 종료되면 증감식 i++가 실행되어 i 변수의 값은 2가 된다.
⑧ 증감식 실행이 종료되면 다시 조건식이 실행된다. 현재 i 변수의 값은 2이므로 조건식의 평가 결과는 false다. 조건식의 평가 결과가 false이므로 for 문의 실행이 종료된다

```js
for (var i = 1; i >= 0; i -) {
    console.log ( i ) ; // 1 0
}

// 무한루프
for ( ;; ) { ... }

// 중첩
for (var i = 1; i <= 6; i++) {
    for (var j = 1; j <= 6; j++) {
        if (i + j === 6) console.log(`[${i}, ${j}]`) ;
    }
}
```

### 8.3.2 while 문
조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행for문은 반복 횟수가 명확할때 주로 사용, while은 반복 횟수가 불명확할 때 주로 사용

```js
var count = 0;
// counts 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
while (count < 3) {
    console.log (count) ; / / 0 1 2
    count++;
}

// 무한루프
while (true ) { ... }

var count = 0;
// 무한루프
while ( true) {
    console.log (count) ;
    count++;
    // count가 3이면 코드 블록을 탈출한다.
    if (count === 3) break;
} // 0 1 2

```

### 8.3.3 do...while 문
코드 블록을 먼저 실행하고 조건식을 평가한다. 따라서 코드 블록은 무조건 한 번 이상 실행

```js
var count = 0;
// counts 3보다 작을 때까지코드 블록을 계속 반복 실행한다.
do {
    console.log (count); // 0 1 2
    count++;
} while (count < 3);
```

## 8.4 break 문
switch 문과 while 문에서 살펴보았듯이 break 문은 코드 블록을 탈출한다. 좀 더 정확히 표현하자면 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문(for. fo r... in, fo r... of. while, do... while) 또는 switch 문의 코드 블록을 탈출한다. 레이블 문, 반복문. switch 문의 코드 블록 외에 break 문을 사용하면 SyntaxError(문법 에러)가 발생

```js
if (true ) {
    break; // Uncaught SyntaxError: Illegal break statement
}
```

> 레이블문 : 식별자가 붙은 문
```js
// foo라는 레이블 식별자가붙은 레이블문
foo: console.log (' foo ');

// foo라는 식별자가붙은 레이블블록문
foo: {
    console.log (1) ;
    break foo; // foo 레이블 블록문을 탈출한다.
    console.log (2);
} 
console.log (' Done!' ) ;

// outer라는 식별자가 붙은 레이블 for 문
outer: for (var i = 0; i < 3; i++) {
    for (var j = 0；j < 3; j++) {
        // i + j === 3이면 outers 식별자가 붙은 레아블 for 문을 탈출한다.
        if ( i + j === 3) break outer;
        console.log (`inner [${i} , ${j}]`);
    }
} 
console.log (`Done!`) ;
```

> 레이블 문은 중첩된 for 문 외부로 탈출할 때 유용하지만 그 밖의 경우에는 일반적으로 권장하지 않는다. 레이블 문을 사용하면 프로그램의 흐름이 복잡해져서 가독성이 나빠지고 오류를 발생시킬 가능성이 높아지기 때문이다.

```js
var string = 'Hello World.' ;
var search = 'l' ;
var index;
// 문자열은 유사 배열이므로 for 문으로 순회할수 있다.
for (var i = 0; i < string.length; i++) {
    // 문자열의 개별 문자가 'l' 이면
    if ( string[i] === search) {
        index = i;
        break; // 반복문을 탈출한다.
    }
} 
console.log (index); // 2

// 참고로 String.prototype.indexOf 메서드를 사용해도 같은 동작을 한다.
console.log (string.indexOf(search)); // 2
```

## 8.5 continue 문
반복문의 코드 블록 실행을 힌 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다. break 문처럼 반독문을 탈출하지는 않는다.

```js
var string = 'Hello World.' ;
var search = 'l' ;
var count = 0;
// 문자열은 유사 배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
    // 'l' 이 아니면 현 지점에서 실행을 중단하고 반복문의 증감삭으로 이동한다.
    if ( string[i] !== search) continue;
    count++; // continue 문이 실행되면 이 문은 실행되지 않는다.
}
console.log (count); // 3

// 참고로 String.prototype.match 메서드를 사용해도 같은 동작을 한다.
const regexp = new RegExp(search, 'g') ;
console.log(string.match(regexp).length); // 3

// 동일결과
for (var i = 0; i < string.length; i++) {
    // 'l' 이먼 카운트를 증가시킨다.
    if ( string[i] !== search) count++;
}
```

이럴때 continue 쓰면 depth가 줄어들어서 좋습니다.
```js
// continue 운을 사용하지 않으면 if 문 내에 코드를 작성해야 한다.
for (var i = 0; i < string.length; i++) {
    // 'l' 이면 카운트를 증가시킨다.
    if ( string[i] === search) {
        count++;
        // code
        // code
        // code
    }
}
// continue 문을 사용하면 if 문 밖에 코드를 작성할 수 있다.
for (var i = 0; i < string.length; i++) {
    // 'l' 이 아니면 카운트를 증가시키지 않는다.
    if ( string[i] !== search) continue;
    count++;
    // code
    // code
    // code
}
```