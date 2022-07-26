# 2장 기본 문법 배우기

# 2.1 코드 작성 규칙
## 2.1.1 세미콜론
없어도되지만, 통일성을 위해 붙이기 권고

## 2.1.2 주석
```js
// 한줄주석
/*여러줄
주석*/
```

## 2.1.3 들여쓰기
상관없지만 2칸으로 통일

# 2.2 자료형
## 2.2.1 문자열
```js
'hello, world'
"hello world"
typeof "hello" // "string"
'' == ' ' // false
"문자열 (\)" //  문자열 () 
"여러\n줄"
`여러
줄`
'합'+'치기'
```
\ : 이스케이핑, 문자로 해석

## 2.2.2 숫자
```js
typeof 5 //"number"
parseInt('5') // 5
parseFloat('3.14') // 3.14
parseInt(111, 2) // 2진법 111, 7
parseInt("") // NaN Not a Number
typeof NaN // "number"

2 ** 4 // 2^4, 16
2/0 // Infinity
-2/0 // -Infinity
typeof Infinity // "number"
Infinity - Infinity // NaN

"문자열"+0 // "문자열0"
'9'-5 // 4 "9"가 number로 typecasting
```
typecasting : 자료형 변환

### 연산자 우선순위
|연산자 우선순위 표 (내림차순)|
---
|,|
---
|yield|
---
|=,+=,-=,**=,*=,/=,%=,<<=,>>=,>>>=,&=,^=,\|=|
---
|? :|
---
||||
---
|&&|
---
|\||
---
|^|
---
|&|
---
|==,!=,===,!==|
---
|<,<=,>,>=,in,instanceof|
---
|<<,>>,>>>|
---
|+,-|
---
|*,/,%(다항)|
---
|**|
---
|!,~,*(단항),-(단항),++(전위),--(전위),typeof,void,delete,await|
---
|++(후위),--(후위)|
---
|new(인수 없이)|
---
|.,[],new,()(함수호출)|
---
|()(그룹화)|
---

### 실수 계산시 주의점
```js
0.1+0.2 // 0.30000000000000004
```
부동소수점으로 인한 오차
해결 실수를 정수로 바꿔 계산하고 마지막에 다시 실수로 바꾸기

### 불값
```js
true // true
typeof true // "boolean"
5 > 3 // true

NaN == NaN // false 유일하게 false만
'3'<5 // true
'abc' < 5 // false
```

```js
'a'.charCodeAt() // 97
```

### ==와 === 차이
=== 자료형 까지 같을때 true
```js
'1' == 1 // true 형변환
1 == true // true
'1' === 1 // false
```

### 논리 연산자
```js
10 > 5 && 6 < 8 // true
```
falsy value : 형변환후 false가 되는값(undefined, null, NaN, ''(빈문자열))
truthy value : 형변환후 true가 되는값

## 2.2.4 빈 값  사용
```js
typeof undefined // undefined
null == undefined // true
undefined === null // false
typeof null // "object", 버그
```

# 2.3 변수
```js
let total = 5000; // declaration, initialization
let string = 'Hello';
console.log(totla);


```

## 2.3.1 변수명 짓기
알기 쉽게 정확하게, 예약어X
## 2.3.2 변수 값 수정
```js
let change = '';
change = 'a';
```
## 2.3.3 변수 활용
## 2.3.4 상수
```js
const string1 = 'a'; // 수정되지 않음을 보장 안전
const string1 = 'b'; // Uncaught SyntaxError: Identifier 'value' has already been declared
string1 = 'c'; // Uncaught TypeError: Assignment to constant variable.
const a; // Uncaught SyntaxError: Missing initializer in const declaration
```

## 2.3.5 var
```js
var variable = 'a'; // init 없으면 undefined
variable; // 재 선언 가능
```
*예약어도 변수명으로 사용가능* 주의

# 2.4 조건문
조건에 따라 실행
```js
if(로그인한 사용자){ 정보 보여준다; }
```
## 2.4.1 if-else
```js
if(~){}
else{}
```
## 2.4.2 else if
## 2.4.3 중첩 if
## switch
```js
switch(~){
    case ~: 실행문; break;
    default: 실행문; // break 불필요
}
```
## 2.4.5 조건부 연산자 사용
삼항연산자

# 2.5 반복문
## 2.5.1 while
```js
while(조건문){
    실행문;
    if(조건문) continue;
    실행문;
}
```

## 2.5.6 중첩 반복문
# 2.6 객체
## 2.6.1 배열
```js
const fruits = ['사과','배']
const arrayOfArray = [[1,2,3],[4,5]];
arrayOfArray[0]; // [1,2,3]
arrayOfArray[0][1]; // 2
arrayOfArray.unshift('가'); // 맨앞에 '가' 추가
arrayOfArray.push('바'); // 맨뒤에 '바' 추가
arrayOfArray[1] = '나'; // 2번째 element 수정
arrayOfArray.pop(); // 맨뒤에 element 삭제
arrayOfArray.shift(); // 맨앞에 element 삭제
arrayOfArray.splice(1,1); // 인덱스 1부터 1개 제거
arrayOfArray.splice(1); // 인덱스 1부터 모두 제거
arrayOfArray.includes('가'); // 배열 '가'  포함여부 확인 true/false
arrayOfArray.indexOf('가'); // 배열 '가'의 인덱스 없으면 -1(앞에서 부터 탐색)
arrayOfArray.lastIndexOf('가'); // 배열 '가'의 마지막 인덱스 없으면 -1 (뒤에서 부터 탐색)
```

## 2.6.2 함수
```js
function() {}
() => {} // arrow function

const b = function() {} // 무기명 함수에 이름 부여
const c = () => {} // 무기명 함수에 이름 부여
```
## 2.6.3 객체 리터럴
```js
const person = {
    name : "Jordan", // Property 속성이름: 속성값
    age : 18, // git check때문에 마지막에도 , 를 넣음
};
person['name']; // "Jordan"
delete person.name; // name property 삭제 -> undefined으로 치환됨
person['log'] = function(value){ // 메서드
    console.log(value);
}
```
***배열과 함수가 객체인 이유***
 : 배열과 함수는 속성들을 추가하거나 수정 및 제거할 수 있음. 객체는 함수와 배열을 포함하는 개념 {}를 사용해 만든 객체를 객체 리터럴이라고 따로 부름

### 객체 비교
```js
"str" === "str" // true 값, 타입 모두 같아서
{} === {} // false -> 객체는 모양이 같아도 생성할때 마다 새로운 객체가 생성됨으로 같은 객체인지 비교하려면 기존 객체를 변수에 저장해두고 비교해야함

const a = {name: 'jordan'};
const arr = [1,2,a];
a===arr[2]; // true
```

### 참조& 복사
```js
const person1 = {name : "jordan"};
const refPerson = person1; // 참조
person1.name = 'ref jordan';
refPerson.name // 'ref jordan'

let a = 'hello'
let b = a // 복사
a = 'world'
b // 'hello' / 객체가 아닌 값(문자열, 숫자, 불, null, undefined)는 참조가 아닌 복사
```