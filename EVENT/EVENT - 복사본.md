# EVENT

사용자(or 애플리케이션)가 클릭하거나 키보드를 누르는 것처럼 다양한 동작이 일어나는데 이런 것들을 이벤트라고 부른다.<br>

이벤트는 키보드를 이용해 엔터를 치거나 마우스 클릭과 같이 다른 것에 영향을 미치는 것을 의미한다.<br>

이벤트는 사용자가 발생시킬 수도 있고 애플리케이션 스스로 발생시킬 수도 있다.

## 이벤트 관련 용어 정리

* **이벤트 이름**<br>사용자(or 애플리케이션)에 의해 일어난 동작의 이름

* **이벤트 속성**<br>특정 요소(태그)에 이벤트를 직접 연결할 때 활용하는 속성

* **이벤트 리스너와 이벤트 핸들러**<br>특정 요소(태그)에 이벤트 속성에 연결된 함수<br>공식적으로 어느 것이 맞다고 정해진 것은 없다.<br>

  > 여러 사이트들을 검색해본 결과 두가지 의견이 있다.
  >
  > + 이벤트 리스너는 이벤트를 watch하고 있다가 이벤트가 발생하면 알려준다. 이벤트 핸들러는 이벤트 리스너가 발생했다고 알려준 이벤트를 처리한다. (이 의견이 더 많다.)
  > + 이벤트 리스너와 이벤트 핸들러는 동의어다. 이벤트 리스너는 이벤트 추가하는데 사용하는 addEventListener에서 파생된 용어이고, 이벤트 핸들러는 이벤트 처리기로 더 많이 사용되는 용어이며 의미론적으로 더 정확하다.

* **디폴트 이벤트**<br>요소(태그)가 가지고 있는 기본적인 이벤트<br>"a태그를 클릭하면 href 속성에 정의한 곳으로 이동한다." 등

* **이벤트를 연결**<br>객체의 속성에 함수 자료형을 할당하는 것

* **이벤트 모델**<br>이벤트를 연결하는 방법<br>모던 브라우저는 '표준 이벤트 모델, 인라인 이벤트 모델, 기본 이벤트 모델'을 지원한다.<br>구버전의 IE 브라우저(~8)는 '마이크로소프트 인터넷 익스플로러 이벤트 모델, 인라인 이벤트 모델, 기본 이벤트 모델'을 지원한다.

## 이벤트 타입

* 마우스 이벤트
* 키보드 이벤트
* HTML 프레임 이벤트
* HTML 입력 양식 이벤트
* 유저 인터페이스 이벤트
* 구조 변화 이벤트
* 터치 이벤트

[Event reference]: https://developer.mozilla.org/en-US/docs/Web/Events	"MDN"

## 이벤트 모델

| DOM Level   | 이벤트 모델                                                  |
| ----------- | ------------------------------------------------------------ |
| DOM Level 0 | 인라인 이벤트 모델, 기본 이벤트 모델                         |
| DOM Level 2 | 마이크로소프트 인터넷 익스플로러 이벤트 모델, 표준 이벤트 모델 |

### DOM Level 0

지정한 대상에 on<...> 이벤트 리스너를 바로 연결하는 방법<br>

하나의 이벤트에 하나의 이벤트 리스너를 연결할 수 있다.<br>

> 현재 읽고 있는 책(모던 웹을 위한 JavaScript + jQuery 입문(3판))에서 나온 용어인데, 둘 다 표준으로 정해진 이름이 아닌 것 같다. 무엇인지 어떤 방법인지만 알고 있으면 될 것 같다.
>
> 개인적인 생각으로 동적임을 담당하는 자바스크립트와 html 구조는 분리하는 게 맞다고 생각하기 때문에 지양하는 방법이다.

[DOM on-event handlers]: https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Event_handlers	"MDN"

#### - 인라인 이벤트 모델

HTML attribute에 바로 이벤트를 연결하는 방법

````html
<h1 onclick="alert('클릭');">Click</h1>

<h2 onclick="var alpha=10; alert(alpha);">Click</h2>
````

#### - 기본 이벤트 모델

자바스크립트 property에 바로 이벤트를 연결하는 방법

````javascript
document.getElementById('myBtn').onclick = function(event) { alert('클릭'); }

var myBtn = document.getElementById('myBtn');
myBtn.onclick = function(event) { alert('클릭'); }
````

#### - 디폴트 이벤트 제거

몇몇 HTML태그는 기본적으로 이벤트 속성을 가지고 있다.<br>

`a태그` 클릭 시 다른 페이지로 이동하는 것, from의 `submit` 버튼 클릭 시 자동으로 form을 제출하고 페이지를 새로고침하는 동작하는 것이 디폴트 이벤트이다.<br>

* a태그의 디폴트 이벤트를 제거하는 방법은 여러가지가 있다.

  + void 연산자 사용

    ````javascript
    <a href="javascript:void(0);"> Click </a>
    ````

    void 연산자는 주어진 식을 실행하고, undefined를 반환한다.<br>

    따라서 undefined를 반환하도록 설계되어 있는 곳에 사용한다.<br>

    void(0);처럼 0을 매개변수로 넘길 경우, 0을 무효로 설정한다는 뜻이기 때문에 단순히 undefined 원시값을 사용할 수 있다.<br>

    void의 반환 값이 undefined가 아닐 경우, 페이지 내용을 반환된 값으로 대체한다.<br>

    *ex) `<a href="javascript:void( alert('hello') );">` 는 반환 값이 alert이므로 alert 창을 띄운다.*

    > `javascript:SCRIPT` 는 자바스크립트 프로토콜로 해당 구문(SCRIPT)을 자바스크립트처럼 실행하라는 뜻이다. 하지만 어디까지나 이벤트 핸들러의 대안이므로, 적극적으로 사용하지 않는 것이 좋다.

  + #(해시태그) 사용

    ````html
    <a href="#"> Click </a>
    ````

    클릭 시 URI에 #(해시태그)가 붙는 문제점이 있다.

  + 비워두기

    ````html
    <a href=""> Click </a>
    ````

    클릭 시 현재 창의 맨위로 이동한다.

* submit 버튼의 이벤트 제거

  + 인라인 이벤트 모델(HTML attribute에 바로 이벤트를 연결하는 방법)

    입력폼의 onsubmit 속성의 값으로 return false; 를 추가한다.

    ````html
    <form onsubmit="alert('test'); return false;">  </form>
    ````

  + 기본 이벤트 모델(자바스크립트 property에 바로 이벤트를 연결하는 방법)

    이벤트 리스너에 return false; 를 추가한다.

    ````html
    <form id="my-form">  </form>
    
    <script>
        document.getElementById('my-form').onsubmit = function(){
            return false;
        }
    </script>
    ````

  + 위의 두가지 방법을 합친 방법(책 내용과는 다르게 해석했다.)

    입력폼의 onsubmit 속성의 값으로 실행할 리스너를 return한다. 해당 리스너에 return false; 를 추가한다.

    ````html
    <form onsubmit="return startSubmit();">  </form>
    
    <script>
        function startSubmit(){
            return false;
        }
    </script>
    ````

#### - 이벤트 객체와 이벤트 발생 객체

추후 추가 예정입니다.

#### - 이벤트 강제 실행

추후 추가 예정입니다.

### DOM Level 2

메서드를 이용해서 이벤트를 연결하거나 제거하는 방법<br>

하나의 이벤트만 연결할 수 있는 DOM Level 0의 이벤트 모델의 단점을 보완하기 위해 나왔다.<br>

여러 개의 이벤트를 추가할 수 있다. 이벤트는 연결한 순서대로 작동한다.

#### - 인터넷 익스플로러 이벤트 모델 (IE8까지 지원)

object.attachEvent(`event`, `listener`)<br>

object.detachEvent(`event`, `listener`)

+ attachEvent()와 detachEvent()는 오직 두개의 매개변수(이벤트 속성, 이벤트 리스너)만 받는다.
  * `event` => on 접두사와 함께 사용되는 이벤트 속성. onclick, onload, onmousedown 등
  * `listener` => 지정된 이벤트 속성이 이 요소에 전달될 때 호출되는 이벤트 리스너 함수이다.

+ attachEvent()로 연결된 이벤트 리스너는 이벤트 객체 매개변수가 없다. 대신 윈도우 객체의 이벤트 속성을 참조한다. 이벤트 리스너에서 `this 키워드`는 window 객체를 가리킨다. 

+ 이벤트 발생 객체(e)를 사용하려면 이벤트 객체의 srcElement 속성을 사용해야 한다.

  > attachEvent()에서는 Event.target 속성 대신 IE에서만 사용할 수 있는 Event.srcElement 속성을 가지고 있고, 다른 브라우저에서 지원하는 Event.target 와 동일한 의미를 가진다.
  >
  > Event.target은 이벤트에 연결된 객체에 대한 참조를 가리킨다.

+ 익명함수를 이용하여 이벤트를 제거할 수 없다. 이벤트를 제거할 때 사용하는 detachEvent() 메서드는 어떤 이벤트 리스너를 제거할지 명확하게 알려주어야 하기 때문이다.

+ 인터넷 익스플로러 이벤트 모델은 캡쳐링(`useCapture`)을 지원하지 않는다. 

#### - 표준 이벤트 모델 (모던 브라우저 + IE9부터 지원)

W3C에서 공식적으로 지정한 DOM Level 2 모델<br>

이벤트 캡쳐링 지원한다.(`useCapture`)<br>

이벤트 사용을 위해 EventTarget을 사용한다.

> EventTarget은 이벤트들을 받을 수 있고, 이벤트를 위한 이벤트 리스너를 가질 수 있는 객체에 의해 구현된 인터페이스 도구이다.
> Element, document, window는 가장 일반적인 이벤트 타겟이다.
> XHR이나 AudioNode, AudioContext 그 외의 다른 객체들도 이벤트 타겟이 될 수 있다.

EventTarget 객체는 세가지 메서드를 지원한다.

+ EventTarget.addEventListener(`type`, `listener`[, `options`])

  지정한 이벤트(type)가 타겟에 전달될 때 마다 호출될 함수(listener)를 설정한다.

  * `type` => listen할 이벤트 타입을 string으로 나타낸다. click, keydown, error 등
  * `listener` => 지정된 이벤트가 발생했을 때, 함수(listener)를 호출한다. listener는 EventListener 
  * `options` => 이벤트 리스너에 대한 특성을 지정하는 옵션 객체
    * useCapture => DOM <u>**트리의 하단에 있는**</u> EventTarget으로 전송하기 전에, 등록된 listener로 이 타입의 이벤트의 전송여부를 나타내는 Boolean 이다. 트리에서 <u>위쪽으로</u> 버블링되는 이벤트는 캡처를 사용하도록 하고, 지정된 listener를 트리거하지 않는다. 
    * once => 리스너를 추가한 후 한 번 이상 은 호출해야 함을 나타내느 Boolean이다. true이면 호출할 때 listener가 자동으로 삭제된다.
    * passive => true일 경우 listener에서 지정한 함수가 preventDefault()를 호출하지 않음을 나타내느 Boolean이다. passive listener가 preventDefault()를 호출하면 user agent는 콘솔 경고를 생성하는 것 외의 작업은 수행하지 않는다.

+ EventTarget.removeEventListener(`type`, `listener`[, `options`])
  EventTarget.addEventListener()로 등록된 이벤트 리스너를 EventTarget에서 제거하는 메서드이다. 제거할 이벤트 리스너는 이벤트 타입, 이벤트 리스너 기능 자체, 일치하는 과정에 영향을 줄 수 있는 다양한 옵션들을 복합적으로 식별한다.

  * type => 제거할 이벤트 타입을 string으로 나타낸다. click, keydown, error 등
  * listener => 

+ EventTarget.dispatchEvent

전달된 이벤트에 대한 참조이다. 이벤트를 받고 그 이벤트 수신기(listener)를 가질 수 있는 객체에 의해 구현된 인터페이스이다.

#### - 디폴트 이벤트 제거

Event.preventDefault

Event.returnValue

ie9 이하 버전에서는 event.returnValue를 false로 해야 한다. 

## 이벤트 전파 방법

엘리먼트가 해당 이벤트에 대한 핸들(함수)를 등록한 경우, 다른 요소 내에 중첩된 요오에서 발생하는 이벤트를 전파하는 방법

### 이벤트 버블링

### 이벤트 캡쳐링













