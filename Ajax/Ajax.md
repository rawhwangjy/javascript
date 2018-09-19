# AJAX

* AJAX란 비동기 자바스크립트와 XML (**A**synchronous **J**avaScript **A**nd **X**ML)을 말한다.
* XMLHttpRequest 객체를 사용해 서버와 통신한다는 뜻이다.
* JSON, XML, HTML 그리고 일반 텍스트 형식 등의 다양한 포맷을 주고 받을 수 있다.
* 전체 페이지를 새로고침하지 않고 일부분만 업데이트할 수 있게 해준다.

## Step1. HTTP request 만들기

HTML 페이지에서 해당 웹 서비스에 접근해 데이터를 가져오려면, <br>자바스크립트를 이용하여 요청을 서버로 보내는 HTTP request를 만들어야한다.<br>HTTP request를 만들기 위해선 그 기능을 제공하는 Object 인스턴스가 필요하다. <br>XMLHttpRequest가 그러한 Object이다. <br>

### new 생성자를 이용하여 인스턴스를 만든다.

````javascript
var request = new XMLHttpRequest();
````

XMLHttpRequest 객체의 readyState 프로퍼티 값이 변할 때마다 자동으로 호출되는 함수를 설정한다.

### onreadystatechange

```javascript
request.onreadystatechange = nameOfTheFunction;
```

request의 onreadystatechange 속성에 특정 함수(nameOfTheFunction)를 할당하면, 요청에 대한 상태가 변화할 때마다 해당 함수를 호출한다.<br>

해당 함수를 실행하는 것이 아니라 단순하게 불러온다.

```javascript
// 위처럼 특정 함수를 지정하거나 익명 함수를 직접적으로 작성해도 된다.
request.onreadystatechange = function(){
    // 서버의 응답에 따른 로직을 여기에 작성한다.
};
```

위와 같이 서버로부터 응답은 받은 후의 동작을 호출한 뒤에, open() 메서드와 send() 메서드를 이용해 서버에 요청한다.



## Step2. 서버에 요청하기

데이터 요청 위치와 데이터 전송 방식을 지정하여 요청할 수 있다.

### open() 메서드

````javascript
request.open('GET', 'http://www.example.org/some.file', true);
````

* 첫번째 파라미터는 HTTP 요청 방식(request method)이다.<br>

  GET, POST, HEAD 중 하나를 사용하거나 서버에서 지원하는 방식 중 하나를 사용한다.<br>

  HTTP 표준에 따라 대문자로 표기한다.

* 두번째 파라미터는 전송 위치이다.<br>

  보안상의 이유로 서드 파티 도메인 상의 URL은 호출할 수 없다.<br>

  따라서 요청하는 도메인의 정확한 네임을 지정해야한다.<br>

  안그러면 'permission denied' 에러가 발생한다.<br>

  만약 다른 도메인으로 요청을 보낸다면 동일 출처 정책(Same-Origin Policy)에 의해 에러가 뜰 것이다.<br>

  - --disable-web-security 옵션을 추가하여 크롬을 실행하거나
  - 서버에서 받은 응답 header에 Access-Control-Allow-Origin: *을 추가하거나
  - jsonp방식으로 리소스들을 가져와서 해결한다.

* 세번째 파라미터(생략 가능)는 요청을 동기로 실행할 지 , 비동기로 실행할 지 정한다.<br>

  기본값은 true 이고, true는 비동기로 실행하라는 뜻이다.<br>

  false로 할 경우 동기로 실행하고, send() 함수에서 서버로부터 응답이 올 때까지 기다린다.

### send() 메서드

````javascript
request.send(<데이터>);
````

* open() 메서드의 파라미터를 POST 방식으로 실행한 경우, send() 메서드의 파라미터는 어떤 데이터라도 가능하다.
* 데이터는 쉽게 parse할 수 있는 형식(format)이어야 한다.
* multipart/form-data, JSON, XML, SOAP 등과 같은 다른 형식도 가능하다.
* POST 형식으로 데이터를 보내려면, 인스턴스에 MIME type을 먼저 설정 해야 한다.

````javascript
// request.setRequestHeader(HTTP 요청 헤더의 이름, HTTP 요청 헤더의 값, true/false);
request.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');
````



## Step3. 서버로부터의 응답에 대한 처리하기

위에서 지정한 함수에서 요청의 상태값을 검사할 필요가 있다.<br>

onreadystatechange 함수는 request의 상태가 변경될 때 마다 호출되기 때문에 현재 요청의 상태값과 HTTP 응답 상태를 알 수 있다.

### XMLHttpRequest 객체의 현재 상태 검사

readyState 속성을 사용하여 XMLHttpRequest의 현재 상태를 검사한다.

````javascript
// request.onreadystatechange = nameOfTheFunction;

request.onreadystatechange = function(){
    // 요청의 상태값
    if (request.readyState === XMLHttpRequest.DONE) {
        // 이상 없음, 응답 받았음
    } else {
        // 아직 준비되지 않음
    }
};
````

XMLHttpRequest.DONE은 상수 `4`로 정의되어 있는 값으로, request에 대한 처리가 끝났으며(모든 데이터를 받았음) 응답할 준비가 완료되었다는 뜻이다. <br>

아래처럼 상수 4와 비교하기도 한다.

````javascript
request.onreadystatechange = function(){
    // 요청의 상태값
    if (request.readyState == 4) {
        // 이상 없음, 응답 받았음
    } else {
        // 아직 준비되지 않음
    }
};
````

| readyState 속성   | 설명                                                         |
| ----------------- | ------------------------------------------------------------ |
| 0 (uninitialized) | request 객체를 만들었지만, open() 메서드로 초기화하지 않았음 |
| 1 (loading)       | request 객체를 만들고 초기화했지만(서버와 연경 됨), send() 메서드 사용되지 않음 |
| 2 (loaded)        | 서버가 request를 받음. send() 메서드를 사용했지만, 아직 데이터를 받지 못함 |
| 3 (interactive)   | 서버가 request 처리 중. 데이터의 일부를 받음                 |
| 4 (complete)      | request에 대한 처리 끝, 응답할 준비가 완료됨. 모든 데이터를 받음. |

### HTTP 응답 상태 코드 검사

Ajax 요청으로 데이터를 가져왔는데 가져온 데이터가 올바른 데이터가 아닐 수 있으므로 좀 더 보완해서 검사해야 한다. 그래서 XMLHttpRequest의 현재 상태를 검사한 후에는 HTTP 응답 상태 코드를 검사한다.<br>

````javascript
request.onreadystatechange = function(){
    // 요청의 상태값
    if (request.readyState == 4) {
        // HTTP 응답 상태값
        if (requestequest.status === 200) {
            // 이상 없음!
        } else {
            // 요구를 처리하는 과정에서 문제가 발생되었음
        }
    } else {
        // 아직 준비되지 않음
    }
};
````

| HTTP 응답 상태 코드 | 설명                 | 예                                                           |
| ------------------- | -------------------- | ------------------------------------------------------------ |
| 1XX                 | 정보 응답            | **100 Continue**<br />> 임시 응답. <br />> 클라이언트가 계속 요청 하거나 <br />> 이미 요청 완료한 경우 무시해도 괜찮다. |
| 2XX                 | 성공 응답            | **200 OK**<br />> 요청 성공적 완료.                          |
| 3XX                 | 리다이렉션 메시지    | **301 Moved Permanetly**<br />> 요청한 리소스의 URI가 변경되었다.<br />> 새로운 URI가 응답에서 주어질 수 있다. |
| 4XX                 | 클라이언트 에러 응답 | **403 Forbidden**<br />> 클라이언트는 해당 콘텐츠에 접근 권리가 없다.<br />**404 Not Found**<br />> 서버는 요청받은 리소스를 찾을 수 없다. |
| 5XX                 | 서버 에러 응답       | **500 Internal Server Error**<br />> 서버가 처리 방법을 모르는 상황이 발생했다. |

[HTTP 응답 상태 코드]: https://developer.mozilla.org/ko/docs/Web/HTTP/Status



## Step4. 응답 데이터 접근 옵션

위 두가지 검사를 패스했다면, 서버로부터 받은 데이터를 사용할 수 있다. 그 응답 데이터에 접근해야 한다.

* request.responseText<br>

  서버의 응답을 `텍스트 문자열`로 반환

* request.responseXML<br>

  서버의 응답을 `XMLDocumen` 객체로 반환하며, DOM 함수들을 통해 이 객체를 다룰 수 있다.

* 위 두 방법은 비동기로 진행했을 경우로, open의 3번째 파라미터가 true일 경우에 사용된다.<br>

  동기로 진행할 경우, 함수(nameOfTheFunction)를 명시할 필요 업시 send() 호출에 의해 반환되는 data를 바로 사용할 수 있다. 하지만 동기로 할 경우, 서버 응답이 완료될 때까지 사용자는 빈 화면을 보고 기다리기 때문에 좋은 방법은 아니다.