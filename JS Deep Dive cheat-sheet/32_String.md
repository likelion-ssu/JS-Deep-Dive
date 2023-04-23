# 32. String

`String`은 **표준 빌트인 객체** 로, 원시타입인 문자열을 다룰 때 유용한 프로퍼티와 메서드를 제공한다.

## 📌 32.1 String 생성자 함수

### new String()

표준 빌트인 객체 `String`은 생성자 함수 객체로, `new` 연산자와 함께 호출하여 인스턴스를 생성할 수 있다.  
이때 생성되는 객체는 String [래퍼 객체](https://github.com/likelion-ssu/JS-Deep-Dive/blob/main/JS%20Deep%20Dive%20cheat-sheet/21_빌트인-객체.md#-213-원시값과-래퍼-객체)이며, \[\[StringData]] 내부 슬롯을 갖는다.

- \[\[StringData]]에는 생성자 함수의 인수로 전달받은 문자열을 할당한다.

  ```js
  const strObj1 = new String(); // String {length: 0, [[PrimitiveValue]]: ""}
  const strObj2 = new String('Lee'); // String {0: "L", 1: "e", 2: "e", length: 3, [[PrimitiveValue]]: "Lee"}

  // 문자열이 아닌 값을 전달하면 강제 변환한다.
  const strObj3 = new String(null); // String {0: "n", 1: "u", 2: "l", : "l", length: 4, [[PrimitiveValue]]: "null"}
  ```

<br/>

### String()

new 연산자를 사용하지 않고 String 생성자 함수를 호출하면, 인스턴스가 아닌 **문자열** 을 반환한다.

- 이를 이용해 명시적으로 타입 변환을 할 수 있다.

  ```js
  String(1); // -> "1"
  String(NaN); // -> "NaN"
  String(true); // -> "true"
  ```

<br/>

## 📌 32.2 String 프로퍼티

### length 프로퍼티

`length` 프로퍼티는 문자열의 문자 개수를 반환한다.

```js
// 문자열 원시값을 객체처럼 접근하면 래퍼 객체가 임시로 생성된다.
'Hello'.length; // 5
'안녕하세요!'.length; // 6
```

### 인덱스 프로퍼티 키와 값

String 래퍼 객체는 인덱스를 나타내는 숫자를 **프로퍼티 키** 로, 각 문자를 **프로퍼티 값** 으로 갖는 **유사배열 객체** 이다.

- 유사 배열이므로 인덱스를 사용하여 문자에 접근할 수 있다.

  ```js
  console.log(strObj[0]); // L
  ```

- 단, 문자열은 원시 값이므로 변경할 수 없다. 에러가 발생하지 않음에 유의한다.
  ```js
  strObj[0] = 'S';
  console.log(strObj); // 'Lee'
  ```

<br/>

## 📌 32.3 String 메서드

문자열은 변경 불가능한 원시 값이기 때문에, String 래퍼 객체도 **읽기 전용(read only)** 객체로 제공된다.

- String 객체의 메서드는 원본 String 래퍼 객체를 직접 변경하지 않는다.
- String 객체의 메서드는 언제나 새로운 문자열을 반환한다.

  ```js
  const strObj = new String('Lee');
  console.log(Object.getOwnPropertyDescriptors(strObj));
  /* String 래퍼 객체는 읽기 전용 객체다. 즉, writable 프로퍼티 어트리뷰트 값이 false다.
  {
    '0': { value: 'L', writable: false, enumerable: true, configurable: false },
    '1': { value: 'e', writable: false, enumerable: true, configurable: false },
    '2': { value: 'e', writable: false, enumerable: true, configurable: false },
    length: { value: 3, writable: false, enumerable: false, configurable: false }
  }
  */
  ```

<br/>

### ✅ String.prototype.indexOf

<details>
  <summary>문자열을 검색하여 첫 번째 인덱스를 반환한다.</summary>

- indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다.

  ```js
  const str = 'Hello World';
  str.indexOf('l'); // -> 2
  str.indexOf('or'); // -> 7
  ```

- 검색에 실패하면 `-1`을 반환한다. 특정 문자열이 존재 여부를 확인하는데 활용할 수 있다.
  ```js
  str.indexOf('x'); // -> -1
  if (str.indexOf('Hello') !== -1) {
    // 포함되어 있는 경우에 처리할 내용
  }
  ```
- 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
  ```js
  // 문자열 str의 인덱스 3부터 'l'을 검색하여 첫 번째 인덱스를 반환한다.
  str.indexOf('l', 3); // -> 3
  ```

</details>
<br/>

### ✅ String.prototype.search

<details>
  <summary>문자열을 검색하여 인덱스를 반환하며, 정규표현식을 사용한 검색이 가능하다.</summary>

- search 메서드는 대상 문자열에서 인수로 전달받은 문자열, 정규 표현식과 매치하는 문자열을 검색하여, 일치하는 문자열의 인덱스를 반환한다.
- 검색에 실패하면 `-1`을 반환한다.

  ```js
  const str = 'Hello world';

  // 문자열 str에서 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.
  str.search(/o/); // -> 4
  str.search(/x/); // -> -1
  ```

</details>

<br/>

### ✅ String.prototype.includes

<details>
  <summary>문자열이 포함되어 있는지 확인하여 true/false 값을 반환한다.</summary>

- ES6에서 도입되었다.
- 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 그 결과를 `true` 또는 `false`로 반환한다.
- 두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

  ```js
  const str = 'Hello world';

  str.includes('Hello'); // -> true
  str.includes(''); // -> true
  str.includes('x'); // -> false
  str.includes(); // -> false

  str.includes('l', 3); // -> true
  str.includes('H', 3); // -> false
  ```

</details>
<br/>

### ✅ String.prototype.startsWith

<details>
  <summary>특정 문자열로 시작하는지 확인한다.</summary>

- ES6에서 도입되었다.
- 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 true/false 값을 반환한다.
- 두 번째 인수로 검색을 시작할 인덱스를 지정할 수 있다.

  ```js
  const str = 'Hello world';

  // 문자열 str이 'He'로 시작하는지 확인
  str.startsWith('He'); // -> true
  // 문자열 str이 'x'로 시작하는지 확인
  str.startsWith('x'); // -> false

  // 문자열 str의 인덱스 5부터 시작하는 문자열이 ' '로 시작하는지 확인
  str.startsWith(' ', 5); // -> true
  ```

</details>
<br/>

### ✅ String.prototype.endsWith

<details>
  <summary>특정 문자열로 끝나는지 확인한다.</summary>

- ES6에서 도입되었다.
- 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 그 결과를 true / false 값을 반환한다.

  ```js
  const str = 'Hello world';

  // 문자열 str이 'ld'로 끝나는지 확인
  str.endsWith('ld'); // -> true
  // 문자열 str이 'x'로 끝나는지 확인
  str.endsWith('x'); // -> false

  // 문자열 str의 처음부터 5자리까지('Hello')가 'lo'로 끝나는지 확인
  str.endsWith('lo', 5); // -> true
  ```

  </details>
  <br/>

### ✅ String.prototype.charAt

<details>
  <summary>특정 인덱스의 문자를 반환한다.</summary>

- 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.
- 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환한다.
- 유사한 문자열 메서드로 `String.prototype.charCodeAt`, `String.prototype.codePointAt`가 있다.

  ```js
  const str = 'Hello';

  for (let i = 0; i < str.length; i++) {
    console.log(str.charAt(i)); // H e l l o
  }

  str.charAt(5); // -> ''
  ```

  </details>
  <br/>

### ✅ String.prototype.substring

<details>
  <summary>지정한 범위의 문자열을 반환한다.</summary>

- 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.
- 두 번째 인수를 생략하면, 첫 번째 인수의 인덱스 부터 마지막 문자까지 부분 문자열을 반환한다.

  ```js
  const str = 'Hello World';

  str.substring(1, 4); // -> ell
  str.substring(1); // -> 'ello World'
  ```

- 첫 번째 인수가 두 번째 인수보다 작지 않을 경우 다음과 같이 동작한다.

  - 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
  - 인수 < `0` 또는 `NaN`인 경우 `0`으로 취급된다.
  - 인수 > 문자열 길이(str.length)인 경우 인수는 문자열의 길이로 취급된다.

  ```js
  const str = 'Hello World'; // str.length == 11

  // 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
  str.substring(4, 1); // -> 'ell'

  // 인수 < 0 또는 NaN인 경우 0으로 취급된다.
  str.substring(-2); // -> 'Hello World'

  // 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)으로 취급된다.
  str.substring(1, 100); // -> 'ello World'
  str.substring(20); // -> ''
  ```

  - 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득할 수 있다.

  ```js
  const str = 'Hello World';
  // 스페이스를 기준으로 앞에 있는 부분 문자열 취득
  str.substring(0, str.indexOf(' ')); // -> 'Hello'

  // 스페이스를 기준으로 뒤에 있는 부분 문자열 취득
  str.substring(str.indexOf(' ') + 1, str.length); // -> 'World'
  ```

</details>
<br/>

### ✅ String.prototype.slice

<details>
  <summary>substring과 동일하게 동작하며, 인수로 음수를 사용할 수 있다는 차이가 있다.</summary>

- slice 메서드는 substring과 동일하게 동작한다.
- 음수인 인수를 전달할 수 있으며, 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```js
const str = 'hello world';

// substring과 slice 메서드는 동일하게 동작한다.
// 0번째부터 5번째 이전 문자까지 잘라내어 반환
str.substring(0, 5); // -> 'hello'
str.slice(0, 5); // -> 'hello'

// 인덱스가 2인 문자부터 마지막 문자까지 잘라내어 반환
str.substring(2); // -> 'llo world'
str.slice(2); // -> 'llo world'

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-5); // -> 'hello world'
// slice 메서드는 음수인 인수를 전달할 수 있다. 뒤에서 5자리를 잘라내어 반환한다.
str.slice(-5); // ⟶ 'world'
```

</details>
<br/>

### ✅ String.prototype.toUpperCase

<details>
  <summary>대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.</summary>

```js
const str = 'Hello World!';
str.toUpperCase(); // -> 'HELLO WORLD!'
```

</details>
<br/>

### ✅ String.prototype.toLowerCase

<details>
  <summary>대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.</summary>

```js
const str = 'Hello World!';
str.toLowerCase(); // -> 'hello world!'
```

</details>
<br/>

### ✅ String.prototype.trim

<details>
  <summary>대상 문자열 앞뒤에 공백 문자를 제거한 문자열을 반환한다.</summary>

```js
const str = '   foo  ';
str.trim(); // -> 'foo'
```

- `String.prototype.trimStart`, `String.prototype.trimEnd` 를 사용하면 문자열 앞뒤의 공백을 제거할 수 있다.  
  _2021.01 기준 stage 4에 제안되어있다._

  ```js
  const str = '   foo  ';

  // String.prototype.{trimStart,trimEnd} : Proposal stage 4
  str.trimStart(); // -> 'foo  '
  str.trimEnd(); // -> '   foo'
  ```

- 정규표현식을 사용하여 검색 후 치환할 수 있다.

  ```js
  const str = '   foo  ';

  // 첫 번째 인수로 전달한 정규 표현식에 매치하는 문자열을 두 번째 인수로 전달한 문자열로 치환한다.
  str.replace(/\s/g, ''); // -> 'foo'
  str.replace(/^\s+/g, ''); // -> 'foo  '
  str.replace(/\s+$/g, ''); // -> '   foo'
  ```

</details>
<br/>

### ✅ String.prototype.repeat

<details>
  <summary>문자열을 반복해 연결한 문자열을 반환한다.</summary>

- ES6에서 도입되었다.
- 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다.
- 인수가 0이면 빈 문자열을, 음수이면 `RangeError`를 발생시킨다.
- 인수의 default 값은 0이다.

  ```js
  const str = 'abc';
  str.repeat(); // -> ''
  str.repeat(0); // -> ''
  str.repeat(1); // -> 'abc'
  str.repeat(2); // -> 'abcabc'
  str.repeat(2.5); // -> 'abcabc' (2.5 → 2)
  str.repeat(-1); // -> RangeError: Invalid count value
  ```

  </details>
  <br/>

### ✅ String.prototype.replace

<details>
  <summary>문자열을 검색 후 치환한 문자열을 반환한다.</summary>

- 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.

  ```js
  const str = 'Hello world';
  // str에서 첫 번째 인수 'world'를 검색하여 두 번째 인수 'Lee'로 치환한다.
  str.replace('world', 'Lee'); // -> 'Hello Lee'
  ```

- 검색된 문자열이 여럿 존재할 경우 첫 번째로 검색된 문자열만 치환한다.

  ```js
  const str = 'Hello world world';
  str.replace('world', 'Lee'); // -> 'Hello Lee world'
  ```

- 특수한 교체 패턴을 사용할 수 있다. 자세한 내용은 MDN의 [함수 설명](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/replace)을 참고하자.
  ```js
  const str = 'Hello world';
  // 특수한 교체 패턴을 사용할 수 있다. ($& => 검색된 문자열)
  str.replace('world', '<strong>$&</strong>');
  ```
- 첫 번째 인수로 정규표현식을 사용할 수 있다.

  ```js
  const str = 'Hello Hello';
  // 'hello'를 대소문자를 구별하지 않고 전역 검색한다.
  str.replace(/hello/gi, 'Lee'); // -> 'Lee Lee'
  ```

- 두 번째 인수로 치환 함수를 전달할 수 있다.

  ```js
  // 카멜 케이스를 스네이크 케이스로 변환하는 함수
  function camelToSnake(camelCase) {
    // /.[A-Z]/g는 임의의 한 문자와 대문자로 이루어진 문자열에 매치한다.
    // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
    return camelCase.replace(/.[A-Z]/g, (match) => {
      console.log(match); // 'oW'
      return match[0] + '_' + match[1].toLowerCase();
    });
  }

  const camelCase = 'helloWorld';
  camelToSnake(camelCase); // -> 'hello_world'

  // 스네이크 케이스를 카멜 케이스로 변환하는 함수
  function snakeToCamel(snakeCase) {
    // /_[a-z]/g는 _와 소문자로 이루어진 문자열에 매치한다.
    // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
    return snakeCase.replace(/_[a-z]]/g, (match) => {
      console.log(match); // '_w'
      return match[1].toUpperCase();
    });
  }

  const snakeCase = 'hello_world';
  snakeToCamel(snakeCase); // -> 'helloWorld'
  ```

</details>
<br/>

### ✅ String.prototype.split

<details>
  <summary>특정 문자열을 기준으로 분리한 문자열 배열을 반환한다.</summary>

- 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환한다.
- 인수로 빈 문자열을 전달하면 각 문자를 모두 분리한다.
- 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.

  ```js
  const str = 'How are you doing?';

  // 공백으로 구분(단어로 구분)하여 배열로 반환한다.
  str.split(' '); // -> ["How", "are", "you", "doing?"]

  // \s는 여러 가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉, [\t\r\n\v\f]와 같은 의미다.
  str.split(/\s/); // -> ["How", "are", "you", "doing?"]

  // 인수로 빈 문자열을 전달하면 각 문자를 모두 분리한다.
  str.split(''); // -> ["H", "o", "w", " ", "a", "r", "e", " ", "y", "o", "u", " ", "d", "o", "i", "n", "g", "?"]

  // 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
  str.split(); // -> ["How are you doing?"]
  ```

- 두 번째 인수로 배열의 길이를 지정할 수 있다.
  ```js
  // 공백으로 구분하여 배열로 반환한다. 단, 배열의 길이는 3이다
  str.split(' ', 3); // -> ["How", "are", "you"]
  ```
- 배열을 반환하므로, `Array.prototype.reverse`, `Array.prototype.join` 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.

  ```js
  // 인수로 전달받은 문자열을 역순으로 뒤집는다.
  function reverseString(str) {
    return str.split('').reverse().join('');
  }

  reverseString('Hello world!'); // -> '!dlrow olleH'
  ```

  </details>
