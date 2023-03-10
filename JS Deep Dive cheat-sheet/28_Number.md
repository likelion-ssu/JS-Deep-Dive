# 28. Number

`Number`는 표준 빌트인 객체로, 원시 타입인 숫자를 다룰 대 유용한 프로퍼티와 메서드를 제공한다.

## 📌 28.1 Number 생성자 함수

`Number` 객체는 **생성자 함수 객체**이다.

따라서 `new` 연산자와 함께 호출하여 `Number` 인스턴스를 생성한다.

### 1️⃣ 인수 없이 생성하기

1. 인수 없이 `new` 연산자를 호출한다.
2. `[[NumberData]]` 내부 슬롯에 **0을 할당한** Number 래퍼 객체를 생성한다.
   - `[[PrimitiveValue]]`와 동일하다.

### 2️⃣ 인수와 함께 생성하기

1. 인수로 숫자를 전달하면서 `new` 연산자를 호출한다.
2. `[[NumberData]]` 내부 슬롯에 **인수로 전달받은 숫자를 할당한** Number 래퍼 객체를 생성한다.

### ❌ 올바르지 않은 인수와 사용하기

1. `Number` 생성자 함수의 인수로 숫자가 아닌 값을 전달한다.
2. 인수를 숫자로 강제로 변환한다.
3. `[[NumberData]]` 내부 슬롯에 **변환된 숫자를 할당한** Number 래퍼 객체를 생성한다.
   - 변환이 불가능한 경우, `NaN`을 삽입한다.

### ✅ 생성자 말고 Number를 사용하기

- [명시적 타입 변환](https://github.com/likelion-ssu/JS-Deep-Dive/blob/main/JS%20Deep%20Dive%20cheat-sheet/09_%ED%83%80%EC%9E%85%EB%B3%80%ED%99%98%EA%B3%BC-%EB%8B%A8%EC%B6%95%ED%8F%89%EA%B0%80.md#-93-%EB%AA%85%EC%8B%9C%EC%A0%81-%ED%83%80%EC%9E%85-%EB%B3%80%ED%99%98)에서 본 것처럼, `new` 연산자 없이 생성자 함수를 호출하면 숫자를 반환한다.
- 이를 이용하여 명시적으로 타입을 변환하기도 한다.

## 📌 28.2 Number 프로퍼티

### 1️⃣ Number.EPSILON

- `ES6`에서 도입되었다.
- 1과 1보다 큰 숫자 중 가장 작은 숫자와의 차이이다.
- 현재 `Number.EPSIOLON`은 _2.2204460492503130808472633361816 \* 10^-16_ 이다.
- 부동소수점 문제를 해결하기 위해 사용한다.
  - 문제 되는 상황
    ```javascript
    0.1 + 0.2; // 0.300000...0004
    0.1 + 0.2 === 0.3; // false
    //부동 소수점으로 인해 오차가 발생한다.
    ```
  - 해결방법
    ```javascript
    function isEqual(a, b) {
      //a와 b의 차가 Number.EPSILON보다 작으면 같은 수로 인정한다.
      return Math.abs(a - b) < Number.EPSILON;
    }
    ```

### 2️⃣ Number.MAX_VALUE

- 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다.
  - _1.7976931348623157 \* 10^308_
  - 이것보다 큰 수는 `Infinity` 이다.

### 3️⃣ Number.MIN_VALUE

- 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.
  - _5 \* 10^-324_
  - 이것보다 작은 수는 0이다.

### 4️⃣ Number.MAX_SAFE_INTEGER

- 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다.
  - _9007199254740991_

### 5️⃣ Number.MIN_SAFE_INTEGER

- 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다.
- _9007199254740991_

### 6️⃣ Number.POSITIVE_INFINITY

- 양의 무한대를 나타내는 숫자갑 `Infinity`와 같다.

### 7️⃣ Number.NEGATIVE_INFINITY

- 음의 무한대를 나타내는 숫자값 `-Infinity`와 같다.

### 8️⃣ Number.NaN

- '숫자가 아님'을 나타내는 숫자값이다.
- `window.NaN`과 같다.

## 📌 28.3 Number 메서드

### 1️⃣ Number.isFinite

- `ES6`에서 도입되었다.
- 인수로 전달받은 숫자가 정상적인 유한수인지를 검사한다.
- 인수가 `NaN`이면 항상 `false`를 반환한다.
- 빌트인 전역함수 `isFinite`와 구분해야한다.
  - 전역함수 `isFinite`는 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행한다.
  - `Number.isFinite`는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.

```javascript
Number.isFinite(null); // false
isFinite(null); //true
```

### 2️⃣ Number.isInteger

- `ES6`에 도입되었다.
- 인수로 전달된 숫자 값이 정수인지 검사한다.
- 검사 전에 인수를 숫자로 암묵적 타입 변환 하지 않는다.

### 3️⃣ Number.isNaN

- `ES6`에 도입되었다.
- 인수로 전달된 숫자 값이 정수인지 검사한다.
- 빌트인 전역함수 `isNaN`과 구분해야한다.
  - 전역함수 `isNaN`은 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행한다.
  - `Number.isNaN`은 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.

### 4️⃣ Number.isSafeInteger

- `ES6`에 도입되었다.
- 인수로 전달된 숫자 값이 안전한 정수인지 검사한다.
- 안전한 정수 값은 _-(2^53 - 1)_ 과 _2^53 - 1_ 사이의 정수 값이다.

### 5️⃣ Number.prototype.toExponential

- 숫자를 _지수 표기법_ 으로 변환하여 **문자열**로 반환한다.
  - 지수 표기법이란, 캐우 크거나 작은 숫자를 표기할 떄 e 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다.
- 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용할 경우 에러가 발생한다.
  ```javascript
  77.toExponential(); // 에러
  77.1234.toExponential(); //"7.71234e + 1"
  (77).toExponential(); //"7.7e + 1"
  77 .toExponential(); //"7.7e + 1"
  ```

### 6️⃣ Number.prototype.toFixed

- 숫자를 반올림하여 문자열로 반환한다.
- 반올림하는 소수점 이하 자릿수를 인수로 전달할 수 있다.
  - 인수는 0 ~ 20 사이 숫자이다.
  - 생략시 기본값 0이 지정된다.

### 7️⃣ Number.prototype.toPrecision

- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.
- 수가 너무 크거나 작으면, 지수 표기법으로 결과를 반환한다.
- 인수를 생략하면 기본 값 0으로 지정된다.

### 8️⃣ Number.prototype.toString

- 숫자를 문자열로 변환하여 반환한다.
- 진법을 나타내는 2 ~ 36 사이의 정수값을 인수로 전달할 수 있다.
  - 인수를 생략하면, 기본값 10진법이 지정된다.
