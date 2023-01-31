## 43 Ajax

- 자바스크립트를 이용하여 비동기 식으로 서버와 통신을 합니다.
- 비동기통신이기 때문에 서버에 요청이 가더라도 화면의 깜빡 거림이나 화면이 이동 된다는 느낌을 주지 않고 상당히 자연스럽고 빠르게 클라이언트의 화면을 변화 시켜줍니다.
- XML형식보다 JSON 형식이 더 많습니다.

### Ajax 사용법

- jquery ajax 사용하기 위해서는 jquery를 import 해야한다.
- method는 GET방식과 POST방식을 사용

### ajax 호출 속성

- type : http 전송 method를 지정한다.
- url : 호출 URL 지정
- dataType : Ajax를 통해 호출한 페이지의 return 형식
- error : 에러났을때 처리하는 함수
- success : 성공했을때 처리하는 함수

- Ajax의 장점

  - 서버에서 처리가 완료될때까지 기다리지 않고 다른 프로세스를 진행할 수 있다.
  - 비동기 방식이기 때문에 동기 방식과 다르게 UI를 변경할 수 있어서 장점이 된다.
  - 웹페이지 속도향상.

- Ajax의 단점
  - 연속으로 데이터를 요청하면 서버 부하가 증가할 수 있다.
  - 히스토리 관리가 안되므로 보안에 좀 취약하다.
  - HTTP 클라이언트의 기능이 한정되어 있다.

### AJAX의 진행과정

1. XMLHttpRequest Object를 만든다.
   - request를 보낼 준비를 브라우저에게 시키는 과정
   - 이것을 위해서 필요한 method를 갖춘 object가 필요함
2. callback 함수를 만든다.
   - 서버에서 response가 왔을 때 실행시키는 함수
   - HTML 페이지를 업데이트 함
3. Open a request
   - 서버에서 response가 왔을 때 실행시키는 함수
   - HTML 페이지를 업데이트 함
4. send the request
