# 가위바위보(객체)

1. 시작 -> 0.05초마다 가위바위보 값 바꾸기 -> 대기
2. 버튼 클릭 -> 그림 멈춤 -> 점수 계산 -> 표시 -> 1초 후에 그림 돌리기 -> 대기

# setInterval
setTimeout과 비슷 주기적으로 지정한 함수를 실행
```js
setInterval(() => {
  // 내용
}, 밀리초);
 ```

# clearInterval, clearTimeout
setInterval과 setTimeout 함수는 각각 clearInterval과 clearTimeout 함수로 취소
clearTimeout은 setTimeout에 지정한 함수가 아직 실행되지 않았을 때만 취소가능
```js
let 아이디 = setInterval(함수, 밀리초);
clearInterval(아이디);
let 아이디 = setTimeout(함수, 밀리초);
clearTimeout(아이디);
 ```

# 배열.includes
||을 사용한 코드는 배열의 includes 메서드로 반복을 줄일 수

```js
diff === '바나나' || diff === '사과' || diff === '오렌지'
['바나나', '사과', '오렌지'].includes(diff)
 ```

# removeEventListener
addEventListener로 연결한 함수를 removeEventListener로 제거(함수가 같아야)
```js
function 함수() {}
태그.addEventListener('이벤트', 함수);
태그.removeEventListener('이벤트', 함수);
```

[rsp.html](./7장/rsp.html)