# EVENT

사용자(or 애플리케이션)가 클릭하거나 키보드를 누르는 것처럼 다양한 동작이 일어나는데 이런 것들을 이벤트라고 부른다.<br>

이벤트는 키보드를 이용해 엔터를 치거나 마우스 클릭과 같이 다른 것에 영향을 미치는 것을 의미한다.<br>

이벤트는 사용자가 발생시킬 수도 있고 애플리케이션 스스로 발생시킬 수도 있다.

## 이벤트 관련 용어 정리

````javascript

````

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

## 이벤트 종류

* 마우스 이벤트
* 키보드 이벤트
* HTML 프레임 이벤트
* HTML 입력 양식 이벤트
* 유저 인터페이스 이벤트
* 구조 변화 이벤트
* 터치 이벤트

## 이벤트 모델 종류

| DOM Level   | 이벤트 모델                                                  |
| ----------- | ------------------------------------------------------------ |
| DOM Level 0 | 인라인 이벤트 모델, 기본 이벤트 모델                         |
| DOM Level 2 | 마이크로소프트 인터넷 익스플로러 이벤트 모델, 표준 이벤트 모델 |

### DOM Level 0

지정한 대상에 on<...> 이벤트 핸들러를 바로 연결하는 방법<br>

하나의 이벤트에 하나의 이벤트 핸들러를 연결할 수 있다.<br>

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

#### - 이벤트 객체와 이벤트 발생 객체

이벤트 객체는 

#### - 이벤트 강제 실행

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

    이벤트 핸들러에 return false; 를 추가한다.

    ````html
    <form id="my-form">  </form>
    
    <script>
        document.getElementById('my-form').onsubmit = function(){
            return false;
        }
    </script>
    ````

  + 위의 두가지 방법을 합친 방법(책 내용과는 다르게 해석했다.)

    입력폼의 onsubmit 속성의 값으로 실행할 핸들러를 return한다. 해당 핸들러에 return false; 를 추가한다.

    ````html
    <form onsubmit="return startSubmit();">  </form>
    
    <script>
        function startSubmit(){
            return false;
        }
    </script>
    ````

### DOM Level 2

메서드를 이용해서 이벤트를 연결하거나 제거하는 방법<br>

하나의 이벤트만 연결할 수 있는 DOM Level 0의 이벤트 모델의 단점을 보완하기 위해 나왔다.<br>

여러 개의 이벤트를 추가할 수 있다. 이벤트는 연결한 순서대로 작동한다.



[EventTarget.addEventListener()]: https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener	"MDN"





EventTarget



### - 인터넷 익스플로러 이벤트 모델 (IE8까지 지원)

익명 함수에 이벤트 모델을 연결(제거)할 수 없다. 대상화된 이름이 없어서



IE에서만 사용할 수 있는 Event.srcElement은 표준 Event.target 속성으로 웹의 호환성을 위해 다른 브라우저에서도 지원합니다.

### - 표준 이벤트 모델 (모던 브라우저 + IE9부터 지원)

W3C에서 공식적으로 지정한 DOM Level 2 모델<br>

이벤트 캡쳐링 지원(책에서는 사실상? 사용하지 않으므로 다루지 않았다.)



### - 디폴트 이벤트 제거



Event.preventDefault

Event.returnValue

ie9 이하 버전에서는 event.returnValue를 false로 해야 한다. 











[EVENT]: https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events	"MDN"

