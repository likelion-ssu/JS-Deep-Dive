# 29. Math

표준 빌트인 객체인 Math는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다.

Math는 생성자 함수가 아니므로, 정적 프로퍼티와 정적 메서드만 제공한다.

## 📌 29.1 Math 프로퍼티

### 1. Math.PI
- **원주율 PI 값을 반환**한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.PI; // -> 3.141592653589793
    ```

    </div>
    </details>


## 📌 29.2 Math 메서드

### 1. Math.abs
- 인수로 전달된 **숫자의 절대값(Absolute Value)을 반환**한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.abs(-1);        // -> 1
    Math.abs('-1');      // -> 1
    Math.abs('');        // -> 0
    Math.abs([]);        // -> 0
    Math.abs(null);      // -> 0
    Math.abs(undefined); // -> NaN
    Math.abs({});        // -> NaN
    Math.abs('string');  // -> NaN
    Math.abs();          // -> NaN
    ```

    </div>
    </details>

### 2. Math.round
- 인수로 전달된 숫자의 소수점 이하를 **반올림한 정수를 반환**한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.round(1.4);  // -> 1
    Math.round(1.6);  // -> 2
    Math.round(-1.4); // -> -1
    Math.round(-1.6); // -> -2
    Math.round(1);    // -> 1
    Math.round();     // -> NaN
    ```

    </div>
    </details>

### 3. Math.ceil
- 인수로 전달된 숫자의 소수점 이하를 **올림한 정수를 반환**한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.ceil(1.4);  // -> 2
    Math.ceil(1.6);  // -> 2
    Math.ceil(-1.4); // -> -1
    Math.ceil(-1.6); // -> -1
    Math.ceil(1);    // -> 1
    Math.ceil();     // -> NaN
    ```

    </div>
    </details>

### 4. Math.floor
- 인수로 전달된 숫자의 소수점 이하를 **내림한 정수를 반환**한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.floor(1.9);  // -> 1
    Math.floor(9.1);  // -> 9
    Math.floor(-1.9); // -> -2
    Math.floor(-9.1); // -> -10
    Math.floor(1);    // -> 1
    Math.floor();     // -> NaN
    ```

    </div>
    </details>

### 5. Math.sqrt
- 인수로 전달된 숫자의 **제곱근을 반환**한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.sqrt(9);  // -> 3
    Math.sqrt(-9); // -> NaN
    Math.sqrt(2);  // -> 1.414213562373095
    Math.sqrt(1);  // -> 1
    Math.sqrt(0);  // -> 0
    Math.sqrt();   // -> NaN
    ```

    </div>
    </details>

### 6. Math.random
- 0에서 1미만의 **임의의 난수(랜덤 숫자)를 반환**한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.random(); // 0에서 1 미만의 랜덤 실수(0.8208720231391746)

    /*
    1에서 10 범위의 랜덤 정수 취득
    1) Math.random으로 0에서 1 미만의 랜덤 실수를 구한 다음, 10을 곱해 0에서 10 미만의
    랜덤 실수를 구한다.
    2) 0에서 10 미만의 랜덤 실수에 1을 더해 1에서 10 범위의 랜덤 실수를 구한다.
    3) Math.floor로 1에서 10 범위의 랜덤 실수의 소수점 이하를 떼어 버린 다음 정수를 반환한다.
    */
    const random = Math.floor((Math.random() * 10) + 1);
    console.log(random); // 1에서 10 범위의 정수
    ```

    </div>
    </details>

### 7. Math.pow
- 첫 번째 인수를 밑(Base)으로, 두 번째 인수를 지수(Exponent)로 **거듭제곱한 결과를 반환**한다.
- ES7에서 도입된 지수 연산자를 사용하면 가독성이 더 좋다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.pow(2, 8);  // -> 256
    Math.pow(2, -1); // -> 0.5
    Math.pow(2);     // -> NaN

    // ES7 지수 연산자
    2 ** 2 ** 2; // -> 16
    Math.pow(Math.pow(2, 2), 2); // -> 16
    ```

    </div>
    </details>

### 8. Math.max
- **전달받은 인수 중에서 가장 큰 수를 반환**하며, 인수가 전달되지 않는 경우 -Infinity를 반환한다.
- 배열을 인수로 전달받아 배열의 요소 중에서 최대값을 구하려면 **Function.prototype.apply 메서드** 또는 **스프레드 문법** 을 사용해야 한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.max(1); // -> 1
    Math.max(1, 2); // -> 2
    Math.max(1, 2, 3); // -> 3
    Math.max(); // -> -Infinity

    // 배열 요소 중에서 최대값 취득
    Math.max.apply(null, [1, 2, 3]); // -> 3

    // ES6 스프레드 문법
    Math.max(...[1, 2, 3]); // -> 3
    ```

    </div>
    </details>

### 8. Math.max
- **전달받은 인수 중에서 가장 작은 수를 반환**하며, 인수가 전달되지 않는 경우 Infinity를 반환한다.
- 배열을 인수로 전달받아 배열의 요소 중에서 최소값을 구하려면 **Function.prototype.apply 메서드** 또는 **스프레드 문법** 을 사용해야 한다.
    <details>
    <summary>예제코드 보기</summary>
    <div markdown="1">

    ```js
    Math.min(1); // -> 1
    Math.min(1, 2); // -> 1
    Math.min(1, 2, 3); // -> 1
    Math.min(); // -> Infinity

    // 배열 요소 중에서 최소값 취득
    Math.min.apply(null, [1, 2, 3]); // -> 1

    // ES6 스프레드 문법
    Math.min(...[1, 2, 3]); // -> 1
    ```

    </div>
    </details>
