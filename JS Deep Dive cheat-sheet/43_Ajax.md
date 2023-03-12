# 43. Ajax

## 📌 43.1 Ajax란?

Ajax란 자바스크립트를 사용하여 브라우저가 서버에게 **비동기 방식으로 데이터를 요청** 하고, 서버가 응답한 데이터를 수신하여 **웹페이지를 동적으로 갱신** 하는 프로그래밍 방식이다.

- 브라우저에서 제공하는 Web API인 `XMLHttpRequest` 객체를 기반으로 동작한다.  
  `XMLHttpRequest`는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공한다.
- 1999년 마이크로소프트가 개발한 `XMLHttpRequest`는 주목받지 못했으나 2005년 발표된 구글 맵스를 통해 가능성을 확인했다.

### 전통적인 방식

이전의 웹페이지는 html 태그로 시작해서 html 태그로 끝나는 완전한 HTML을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다.

**전통적인 방식의 단점**

- 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 **불필요한 데이터 통신** 이 발생한다.
- 변경할 필요가 없는 부분까지 **처음부터 다시 렌더링** 하여, 화면 전환이 일어나면 순간적으로 깜빡이는 현상이 발생한다.
- 클라이언트와 서버와의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 **블로킹** 된다.

### Ajax

**Ajax의 장점**

- 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 때문에 **불필요한 데이터 통신이 발생하지 않는다.**
- 변경할 필요가 없는 부분은 **다시 렌더링하지 않는다.** 따라서 화면이 순간적으로 깜빡이는 현상이 발생하지 않는다.
- 클라이언트와 서버의 통신이 비동기 방식으로 동작하므로, 요청 이후 블로킹이 발생하지 않는다.

<br/>

## 📌 43.2 JSON

`JSON(JavaScript Object Notation)`은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.  
자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로 대부분의 프로그래밍 언어에서 사용할 수 있다.

### JSON 표기 방식

- JSON은 키와 값으로 구성된 순수한 텍스트다.
- **키** 는 반드시 큰따옴표로 묶어야한다.
- **값** 은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있고, 문자열은 반드시 큰따옴표로 묶어야한다.

```js
{
  "name": "Lee",
  "age": 20,
  "alive": true,
  "hobby": ["traveling", "tennis"]
}
```

### JSON.stringfy

- `JSON.stringfy` 메서드는 객체를 `JSON` 포맷의 문자열로 변환한다.
- 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는데 이를 직렬화(serializing)라 한다.

  <details>
    <summary>`JSON.stringfy`의 객체 직렬화</summary>

  ```js
  const obj = {
    name: 'Lee',
    age: 20,
    alive: true,
    hobby: ['traveling', 'tennis'],
  };

  // 객체를 JSON 포맷의 문자열로 변환한다.
  const json = JSON.stringify(obj);
  console.log(typeof json, json);
  // string {"name":"Lee","age":20,"alive":true,"hobby":["traveling","tennis"]}

  // 객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
  const prettyJson = JSON.stringify(obj, null, 2);
  console.log(typeof prettyJson, prettyJson);
  /*
  string {
    "name": "Lee",
    "age": 20,
    "alive": true,
    "hobby": [
      "traveling",
      "tennis"
    ]
  }
  */

  // replacer 함수. 값의 타입이 Number이면 필터링되어 반환되지 않는다.
  function filter(key, value) {
    // undefined: 반환하지 않음
    return typeof value === 'number' ? undefined : value;
  }

  // JSON.stringify 메서드에 두 번째 인수로 replacer 함수를 전달한다.
  const strFilteredObject = JSON.stringify(obj, filter, 2);
  console.log(typeof strFilteredObject, strFilteredObject);
  /*
  string {
    "name": "Lee",
    "alive": true,
    "hobby": [
      "traveling",
      "tennis"
    ]
  }
  */
  ```

  </details>

  <details>
    <summary>`JSON.stringfy`의 배열 직렬화</summary>

  ```js
  const todos = [
    { id: 1, content: 'HTML', completed: false },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'Javascript', completed: false },
  ];

  // 배열을 JSON 포맷의 문자열로 변환한다.
  const json = JSON.stringify(todos, null, 2);
  console.log(typeof json, json);
  /*
  string [
    {
      "id": 1,
      "content": "HTML",
      "completed": false
    },
    {
      "id": 2,
      "content": "CSS",
      "completed": true
    },
    {
      "id": 3,
      "content": "Javascript",
      "completed": false
    }
  ]
  */
  ```

  </details>

### JSON.parse

- `JSON.parse` 메서드는 JSON 포맷의 문자열을 객체로 변환한다.
- 서버로부터 클라이언트에게 전송된 JSON 데이터를 객체로서 사용하려면 문자열을 객체화해야한다. 이를 **역직렬화(deserializing)** 라 한다.
- JSON 배열의 요소가 객체인 경우 배열의 요소까지 객체로 변환한다.

  <details>
    <summary>`JSON.parse`의 객체 역직렬화</summary>

  ```js
  const obj = {
    name: 'Lee',
    age: 20,
    alive: true,
    hobby: ['traveling', 'tennis'],
  };

  // 객체를 JSON 포맷의 문자열로 변환한다.
  const json = JSON.stringify(obj);

  // JSON 포맷의 문자열을 객체로 변환한다.
  const parsed = JSON.parse(json);
  console.log(typeof parsed, parsed);
  // object {name: "Lee", age: 20, alive: true, hobby: ["traveling", "tennis"]}
  ```

  </details>

  <details>
    <summary>`JSON.parse`의 배열 역직렬화</summary>

  ```js
  const todos = [
    { id: 1, content: 'HTML', completed: false },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'Javascript', completed: false },
  ];

  // 배열을 JSON 포맷의 문자열로 변환한다.
  const json = JSON.stringify(todos);

  // JSON 포맷의 문자열을 배열로 변환한다. 배열의 요소까지 객체로 변환된다.
  const parsed = JSON.parse(json);
  console.log(typeof parsed, parsed);
  /*
  object [
    { id: 1, content: 'HTML', completed: false },
    { id: 2, content: 'CSS', completed: true },
    { id: 3, content: 'Javascript', completed: false }
  ]
  */
  ```

  </details>

<br/>

## XMLHttpRequest

- 브라우저는 주소창이나 HTML의 `form` 태그 또는 `a` 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다.
- 자바스크립트를 사용하여 HTTP 요청을 전송하려면 `XMLHttpRequest` 객체를 사용한다.
- Web API인 `XMLHttpRequest` 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

### XMLHttpRequest 객체 생성

`XMLHttpRequest` 객체는 `XMLHttpReqeust` 생성자 함수를 호출하여 생성한다.  
_(브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 정상적으로 실행된다.)_

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();
```

### XMLHttpReqeust 객체의 프로퍼티와 메서드

XMLHttpReqeust 객체는 다양한 프로퍼티와 메서드를 제공한다.

<br/>

#### XMLHttpReqeust 객체의 프로토타입 프로퍼티

<!-- prettier-ignore -->
<table>
  <tr>
    <th>readyState</th>
    <td>
      HTTP 요청의 현재 상태를 나타내는 정수. 다음과 같은 XMLHttpReqeust의 정적 프로퍼티를 값으로 갖는다.<br/>
      <ul>
        <li>UNSENT : 0</li>
        <li>OPENED : 1</li>
        <li>HEADERS_RECEIVED : 2</li>
        <li>LOADING : 3</li>
        <li>DONE : 4</li>
      </ul>
    </td>
  </tr>
  <tr>
    <th>status</th><td>HTTP 요청에 대한 응답 상태(HTTP 상태 코드)를 나나태는 정수<br/>(ex. 200)</td>
  </tr>
  <tr>
    <th>statusText</th><td>HTTP 요청에 대한 응답 메시지를 나타내는 문자열<br/>(ex. "OK")</td>
  </tr>
  <tr>
    <th>responseType</th><td>HTTP 응답 타입<br/>(ex. document, json, text, blob, arraybuffer)</td>
  </tr>
  <tr>
    <th>response</th><td>HTTP 요청에 대한 응답 몸체(response body). responseType에 따라 타입이 다르다.</td>
  </tr>
  <tr>
    <th>responseText</th><td>서버가 전송한 HTTP 요청에 대한 응답 문자열</td>
  </tr>
</table>

<br/>

#### XMLHttpReqeust 객체의 이벤트 핸들러 프로퍼티

<!-- prettier-ignore -->
<table>
  <tr>
    <th>onreadystatechange</th><td>readyState 프로퍼티 값이 변경된 경우</td>
  </tr>
  <tr>
    <th>onloadstart</th><td>HTTP 요청에 대한 응답을 받기 시작한 경우</td>
  </tr>
  <tr>
    <th>onprogress</th><td>HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생</td>
  </tr>
  <tr>
    <th>onabort</th><td>abort 메서드에 의해 HTTP 요청이 중단된 경우</td>
  </tr>
  <tr>
    <th>onerror</th><td>HTTP 요청에 에러가 발생한 경우</td>
  </tr>
  <tr>
    <th>onload</th><td>HTTP 요청이 성공적으로 완료한 경우</td>
  </tr>
  <tr>
    <th>ontimeout</th><td>HTTP 요청 시간이 초과한 경우</td>
  </tr>
  <tr>
    <th>onloadend</th><td>HTTP 요청이 완료한 경우. HTTP 요청이 성공 또는 실패하면 발생</td>
  </tr>
</table>

<br/>

#### XMLHttpReqeust 객체의 메서드

<!-- prettier-ignore -->
<table>
  <tr>
    <th>open</th><td>HTTP 요청 초기화</td>
  </tr>
  <tr>
    <th>send</th><td>HTTP 요청 전송</td>
  </tr>
  <tr>
    <th>abort</th><td>이미 전송된 HTTP 요청 중단</td>
  </tr>
  <tr>
    <th>setReqeustHeader</th><td>특정 HTTP 요청 헤더의 값을 설정</td>
  </tr>
  <tr>
    <th>getResponseHeader</th><td>특정 HTTP 요청 헤더의 값을 문자열로 반환</td>
  </tr>
</table>

<br/>

#### XMLHttpReqeust 객체의 정적 프로퍼티

<!-- prettier-ignore -->
<table>
  <tr>
    <th>UNSENT</th><td>0</td><td>open 메서드 호출 이전</td>
  </tr>
  <tr>
    <th>OPENED</th><td>1</td><td>open 메서드 호출 이후</td>
  </tr>
  <tr>
    <th>HEADERS_RECEIVED</th><td>2</td><td>send 메서드 호출 이후</td>
  </tr>
  <tr>
    <th>LOADING</th><td>3</td><td>서버 응답 중(응답 데이터 미완성 상태)</td>
  </tr>
  <tr>
    <th>DONE</th><td>4</td><td>서버 응답 완료</td>
  </tr>
</table>

### HTTP 요청 전송

다음과 같은 순서로 HTTP 요청을 전송한다.

1. `XMLHttpReqeust.prototype.open` 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 `XMLHttpReqeust.prototype.setReqeustHeader` 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. `XMLHttpReqeust.prototype.send` 메서드로 HTTP 요청을 전송한다.

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```

<br/>

#### XMLHttpReqeust.prototype.open

open 메서드는 서버에 전송할 HTTP 요청을 초기화한다.

```js
xhr.open(method, url[, async])
```

<table>
  <tr>
    <th>매개변수</th>
    <td>설명</td>
  </tr>
  <tr>
    <th>method</th>
    <td>HTTP 요청 메서드("GET", "POST", "PUT", "DELETE" 등)</td>
  </tr>
  <tr>
    <th>url</th>
    <td>HTTP 요청을 전송할 URL</td>
  </tr>
  <tr>
    <th>async</th>
    <td>비동기 요청 여부. 옵션으로 기본값은 true이며, 비동기 방식으로 동작한다.</td>
  </tr>
</table>

- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다.
- 주로 5가지 요청 메서드(GET, POST, PUT, PATCH, DELETE 등)를 사용하여 CRUD를 구현한다.

<table>
  <tr>
    <th>HTTP 요청 메서드</th>
    <td>종류</td>
    <td>목적</td>
    <td>페이로드</td>
  </tr>
  <tr>
    <th>GET</th>
    <td>index/retrieve</td>
    <td>모든/특정 리소스 취득</td>
    <td>X</td>
  </tr>
  <tr>
    <th>POST</th>
    <td>create</td>
    <td>리소스 생성</td>
    <td>O</td>
  </tr>
  <tr>
    <th>PUT</th>
    <td>replace</td>
    <td>리소스의 전체 교체</td>
    <td>O</td>
  </tr>
  <tr>
    <th>PATCH</th>
    <td>modify</td>
    <td>리소스의 일부 수정</td>
    <td>O</td>
  </tr>
  <tr>
    <th>DELETE</th>
    <td>delete</td>
    <td>모든/특정 리소스 삭제</td>
    <td>X</td>
  </tr>
</table>

#### XMLHttpReqeust.prototype.send

send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다. 기본적으로 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.

- GET 요청 메서드의 경우 데이터를 URL의 일부분인 쿼리 문자열(query string)로 서버에 전송한다.
- POST 요청 메서드의 경우 데이터(페이로드)를 요청 몸체(reqeust body)에 담아 전송한다.

#### XMLHttpReqeust.prototype.setReqeustHeader

### HTTP 응답 처리

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가
// 변경될 때마다 발생한다.
xhr.onreadystatechange = () => {
  // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
  // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 상태다.
  // 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
  if (xhr.readyState !== XMLHttpRequest.DONE) return;

  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
```

```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
// https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

// HTTP 요청 전송
xhr.send();

// load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
  // status 프로퍼티는 응답 상태 코드를 나타낸다.
  // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
  // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
  // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
  if (xhr.status === 200) {
    console.log(JSON.parse(xhr.response));
    // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
  } else {
    console.error('Error', xhr.status, xhr.statusText);
  }
};
```
