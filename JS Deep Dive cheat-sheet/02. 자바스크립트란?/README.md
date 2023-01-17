# 02. 자바스크립트란?

<br/>

## 2.1 자바스크립트의 탄생

- 1995년, 넷스케이프 커뮤니케이션즈(Netscape communications)가 자사의 브라우저에서 동작하는 경량 프로그래밍 언어 도입을 결졍하였다.
- 이에 브렌던 아이크(Brendan Eich)가 자바스크립트를 개발해 발표하였다.
- 1996년 3월, 넷스케이프 내비게이터(Netscape Navigator)2에 탑재 및 "모카(Mocha)"로 명명되었다.
- 1996년 9월, "라이브스크립트(LiveScript)"로 명명되었다.
- 1996년 12월, "자바스크립트(JavaScript)"로 최종 명명되었다.
- 현재, 자바스크립트는 현재 모든 브라우저의 표준 프로그래밍 언어로 자리매김하였다.

<br/>

## 2.2 자바스크립트의 표준화

- 1996년 8월, 마이크로소프트가 "JScript"를 인터넷 익스플로러(Internet Explorer)3.0에 탑재하였다.
- 마이크로소프트와 넷스케이프 커뮤니케이션 간 시장 점유율 경쟁이 발생하였다.
- 브라우저에 따라 웹페이지가 정상적으로 동작하지 않는 크로스 브라우징 이슈를 야기하였다.
- 1996년 11월, 컴퓨터 시스템 표준화 비영리 기구 ECMA 인터네셔널이 넷스케이프 커뮤니케이션으로부터 자바스크립트 표준화 의뢰를 받았다.
- 1997년 7월, ECMA-262 사양 완성되었고 상표권 문제로 ECMAScript로 명명되었다.

|버전|출시 연도|특징|
|---|---|---|
|ES1|1997|초판|
|ES2|1998|ISO/IEC 16262 국제 표준과 동일한 규격을 적용|
|ES3|1999|정규 표현식, try ... catch|
|ES5|2009|HTML5와 함께 출현한 표준안. <br/> JSON, strict mode, 접근자 프로퍼티, 프로퍼티 어트리뷰트 제어, 향상도긴 배열 조작 기능(forEach, map, filter, reduce, some, every)|
|ES6(ECMAScript 2015)|2015|let/const, 클래스, 화살표 함수, 템플릿 리터럴, 디스트럭처링 할당, 스프레드 문법, rest 파라미터, 심벌, 프로미스, Map/Set, 이터러블, for ... of, 제너레이터, Proxy, 모듈 import, export|
|ES7(ECMAScript 2016)|2016|지수(**) 연산자, Array.Prototype.includes, String.prototype.includes|
|ES8(ECMAScript 2017)|2017|async/await, Object 정적 메서드(Object.values, Object.entries, Object.getOwnPropertyDescriptors)|
|ES9(ECMAScript 2018)|2018|Object rest/spread 프로퍼티, Promise.prototype.finally, async generator, for await ... of|
|ES10(ECMAScript 2019)|2019|Object.fromEntries, Promise.prototype.flat, Array.prototype.flatMap, optional catch binding|
|ES11(ECMAScript 2020)|2020|String.prototype.matchAll, BigInt, globalThis, Promise.allSettled, null 병합 연산자, 옵셔널 체이닝 연산자, for ... in enumeration order|

<br/>

## 2.3 자바스크립트 성장의 역사
초장기 자바스크립트는 웹페이지의 보조적인 기능을 수행하기 위한 한정적인 용도로 사용되었다. 대부분의 로직은 주로 웹서버에서 실행되었고, 브라우저는 서버로부터 전달받은 HTML과 CSS를 렌더링하는 수준이었다.

### 2.3.1 Ajax
- 1999년, 서버와 브라우저가 비동기(Asynchronous) 방식으로 데이터를 교환할 수 있는 통신 기능인 Ajax(Asynchronous JavaScript and XML)가 XMLHttpRequest라는 이름으로 등장하였다.
- 서버로부터 필요한 데이터만 전송받아 변경해야 하는 부분만 한정적으로 렌더링하는 방식을 통해, 웹 브라우저에서도 데스크톱 애플리케이션과 유사한 빠른 성능과 부드러운 화면 전환이 가능해졌다.
- 이전에는 모든 HTML 코드를 서버로부터 다시 전송받아 불필요한 데이터 통신과 전체 렌더링으로 인한 성능 문제, 화면전환으로 인한 깜빡임 등의 한계가 있었다.
- 2005년, 구글 맵스(Google Maps)는 웹 애플리케이션 프로그래밍 언어로서 자바스크립트의 가능성을 보여준 대표적인 사례이다.

### 2.3.2 JQuery
- 2006년, JQuery로 손쉬운 DOM(Document Object Model) 제어와 크로스 브라우징 이슈를 상당 수준 해결할 수 있었다.

### 2.3.3 V8 자바스크립트 엔진
- 자바스크립트 웹 애플리케이션을 구축하려는 시도에 발맞춰 자바스크립트 엔진의 필요성 대두되었다.
- V8 자바스크립트 엔진으로 데스크톱 애플리케이션과 유사한 사용자 경험(UX: user experience)을 제공할 수 있었다.
- 과거 웹 서버에서 수행되던 로직들이 대거 클라이언트(브라우저)로 옮겨졌다.
- 웹 애플리케이션 개발에서 프런트엔드 영역이 주목받는 계기가 되었다.

### 2.3.4 Node.js
- 2009년, 라이언 달(Ryan Dahl)이 발표한 Node.js는 구글 V8 자바스크립트 엔진으로 빌드된 자바스크립트 런타임 환경이다.
- 다시 말해, 자바스크립트 엔진을 브라우저에서 독립시킨 자바스크립트 실행 환경이다.
- 서버 사이드 애플리케이션 개발에 주로 사용하며, 이에 필요한 모듈, 파일 시스템, HTTP 등 내장 API를 제공한다.
- 프런트엔드와 백엔드 영역에서 자바스크립트를 사용할 수 있다는 동형성(Isomorphic)으로 학습 코스트 감소시킬 수 있다.
- 비동기 I/O를 지원하고, 단일 스레드(Single Thread) 이벤트 루프 기반으로 동작해 요청(Request) 처리 성능이 좋다.

### 2.3.5 SPA 프레임워크
CBD(Compoenet Based Development) 방법론을 기반으로 한 SPA(Single Page Application)가 대중화되며 Angular, React, Vue.js, Svelte 등 다양한 프레임워크/라이브러리가 등장하였다.


<br/>

## 2.4 자바스크립트와 ECMAScript
- EMCAScript는 자바스크립트의 표준 사양인 ECMA-262를 말하며, 프로그래밍 언어의 값, 타입, 객체와 프로퍼티, 함수, 표준 빌트인 객체(Standar Built-in Obejct) 등 핵심 문법 규정되어 있다.
- 자바스크립트는 EMCAScript와 클라이언트 사이드 Web API(DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, WEb Storage, Web Component, Web Worker 등)를 포함하는 개념이다.
- 클라이언트 사이드 Web API는 월드 와이드 웹 콘소시엄(World Wide Web Consortium; W3C)에서 별도의 사양으로 관리하고 있다.


<br/>

## 2.5 자바스크립트의 특징
- HTML, CSS와 함께 웹을 구성하는 요소 중 하나로 웹 브라우저에서 동작하는 유일한 프로그래밍 언어이다.
- 개발자가 별도의 컴파일 작업을 수행하지 않는 인터프리터 언어(Interpreter Language)이다.
- 대부분의 모던 자바스크립트 엔진은 인터프리터와 컴파일러의 장점을 결합해 비교적 처리 속도가 느린 인터프리터의 단점을 해결하였다.
- 명령형(Imperative), 함수형(Functional), 프로토타입 기반(Prototype-based) 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어이다.

**인터프리터 언어 vs. 컴파일러 언어**
|컴파일러 언어|인터프리터 언어|
|---|---|
|코드가 실행되기 전 단계인 컴파일 타임에 소스코드 전체를 한번에 머신 코드(CPU가 바로 실행할 수 있는 기계어)로 변환한 후 실행|코드가 실행되는 단계인 런타임에 문 단위로 한 줄씩 중간 코드(InterMediate Code)인 바이트코드로 변환한 후 실행|
|실행 파일을 생성|실행 파일을 생성하지 않음|
|컴파일 단계와 실행 단계가 분리되어 있고, 명시적인 컴파일 단계를 거쳐 명시적으로 실행 파일을 실행|인터프리트 단계와 실행 단계가 분리되어 있지 않고, 한 줄씩 바이트코드로 변환하고 즉시 실행|
|실행에 앞서 컴파일은 단 한번 수행|코드가 실행될 때마다 인터프리트 과정이 반복 수행|
|컴파일과 실행 단계가 분리되어 있으므로 코드 실행 속도가 빠름|인터프리트 단계와 실행 단계가 분리되어 있지 않고 반복 수행되므로 코드 실행 속도가 비교적 느림|


<br/>

## 2.6 ES6 브라우저 지원 현황
대부분의 모던 브라우저는 ES6를 지원하지만 100%는 아니며, Node.js는 v4부터 ES6를 지원했다. 따라서 브라우저에서 아직 지원하지 않는 최신 기능을 사용하거나 인터넷 익스플로러나 구형 브라우저를 고려해야 하는 상황이라면 바벨(Babel)과 같은 트랜스파일러를 사용해야 한다.



<br/>
