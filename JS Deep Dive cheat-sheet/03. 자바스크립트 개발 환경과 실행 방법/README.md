# 03. 자바스크립트 개발 환경과 실행 방법

## 📌 3.1 자바스크립트 실행 환경
모든 브라우저는 자바스크립트를 해석하고 실행할 수 있는 **자바스크립트 엔진**을 내장하고 있다.    
`Node.js` 역시 자바스크립트 엔진을 내장하고 있어, 자바스크립트는 `브라우저 환경` 및 `Node.js`에서 동일하게 동작한다.    
하지만 두 자바스크립트 실행 환경은 용도의 차이가 있다.    
### 브라우저 환경과 Node.js의 공통점
- 자바스크립트 엔진 내장
- 자바스크립트의 코어인 ECMAScript를 실행할 수 있음
- ECMAScript 이외에 추가로 제공하는 기능은 호환되지 않음

### 브라우저 환경과 Node.js의 차이점
- **브라우저**
    - HTML, CSS, JavaScript를 실행해 _웹페이지를 브라우저 화면에 렌더링_ 하는 것이 주된 목적
    - 파싱된 HTML 요소를 선택하거나 조작하는 기능의 집합인 DOM API 제공
    - 파일 시스템 제공 X (Web API - FileReader 객체 사용하는 방법 사용 필요)
    - _클라이언트 사이드 web API_ 제공
        - ex. `ECMAScript`, `DOM`, `BOM`, `Canvas`, `XMLHttpRequest`, `fetch`, `requestAnimationFrame`, `SVG`, `Web Storage`, `Web Component`, `Web Worker`, ...  

- **Node.js**
    - _브라우저 외부에서 자바스크립트 실행 환경을 제공_ 하는 것이 주된 목적
    - DOM API 제공 X (브라우저 외부 환경에서 HTML 요소를 파싱해 객체화한 DOM을 직접 다룰 필요가 없으므로)
    - 파일 시스템(파일 생성 및 수정) 기본 제공
    - 클라이언트 사이드 web API 제공 X
    - ECMAScript와 Node.js 고유의 API 제공

<img src="https://user-images.githubusercontent.com/66112716/212527611-9a236055-a1f3-43f5-a535-138ba9c6ecdd.png" width="500" />

## 📌 3.2 웹 브라우저
- **개발자 도구**
    - `Elements` : 로딩된 웹페이지의 DOM과 CSS를 편집해 렌더링된 뷰 확인 가능
    - `Console` : 로딩된 웹페이지의 에러를 확인하거나 자바스크립트 소스코드에 작성한 `console.log` 메서드의 실행 결과 확인 가능
    - `Sources` : 로딩된 웹페이지의 자바스크립트 코드 디버깅 가능
    - `Network` : 로딩된 웹페이지에 관련된 네트워크 요청(request) 정보 및 성능 확인 가능
    - `Application` : 웹 스토리지, 세션, 쿠키 확인 및 관리 가능
- **콘솔**
- **브라우저에서 자바스크립트 실행**
- **디버깅**

## 📌 3.3 Node.js
### Node.js REPL
> `Node.js REPL`(Read Eval Print Loop)로 간단한 자바스크립트 코드를 실행해 결과를 확인해보자.

```cmd
$ node
```
프롬프트가 `>`로 변경되면 자바스크립트 코드를 실행할 수 있다.    
```cmd
Welcome to Node.js v14.3.0.
Type ".help" for more information.
> 1 + 2
3
> Math.max(1, 2, 3)
3
> [1, 2, 3].filter(v => v % 2)
[1, 3]
```
자바스크립트 파일을 실행하려면 `node` 명령어 뒤에 파일 이름을 입력한다.    
파일 확장자 `.js`는 생략해도 된다.    
```cmd
$ node index.js
```

## 📌 3.4 비주얼 스튜디오 코드
- **내장 터미널**
```js
// 예제 03-02
// myapp/index.js
const arr = [1, 2, 3];
arr.forEach(console.log);
```

```cmd
> node index
1 0 [1, 2, 3]
2 1 [1, 2, 3]
3 2 [1, 2, 3]
```
- **Code Runner 확장 플러그인**

<img src="https://user-images.githubusercontent.com/66112716/212529189-ce3ccb55-5e5c-490f-b855-64743be0b5c0.png" width="600" />

- **Live Server 확장 플러그인**

<img src="https://user-images.githubusercontent.com/66112716/212529138-597e13b8-ce22-4c95-b5a3-0b71b7321df3.png" width="600" />
