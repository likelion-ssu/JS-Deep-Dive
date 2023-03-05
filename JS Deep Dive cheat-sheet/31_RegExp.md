# 31. RegExp

## 📌 31.1 정규 표현식이란?

**정규 표현식**은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어이다. 정규표현식은 문자열을 대상으로 특정 패턴과 일치하는 문자열을 검색, 추출, 치환하는 **패턴 매칭 기능**을 제공한다.

```js
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = "010-1234-567팔";

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트(확인)한다.
regExp.test(tel); // -> false
```

위와 같이 패턴을 정의하고 테스트 하는 것으로 문자열을 간단히 체크할 수 있다.

## 📌 31.2 정규 표현식의 생성

정규 표현식 객체를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.

<img src= "https://user-images.githubusercontent.com/87116017/218711258-30837aaa-061a-4fd4-b160-b5d84780319f.png" alt ="regexp" width="400px"/><br>
_[image reference](https://velog.io/@jiseong/%EC%A0%95%EA%B7%9C-%ED%91%9C%ED%98%84%EC%8B%9D)(정규 표현식 리터럴)_

-   정규 표현식 리터럴 사용하기

    ```js
    const target = "Is this all there is?";

    // 패턴: is
    // 플래그: i => 대소문자를 구별하지 않고 검색한다.
    const regexp = /is/i;

    // test 메서드는 target 문자열에 대해 정규표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
    regexp.test(target); // -> true
    ```

-   RegExp 생성자 함수 사용하기

    ```js
    const target = "Is this all there is?";

    const regexp = new RegExp(/is/i); // ES6
    // const regexp = new RegExp(/is/, 'i');
    // const regexp = new RegExp('is', 'i');

    regexp.test(target); // -> true
    ```

    RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수 있다.

    ```js
    const count = (str, char) => (str.match(new RegExp(char, "gi")) ?? []).length;

    count("Is this all there is?", "is"); // -> 3
    count("Is this all there is?", "xx"); // -> 0
    ```

## 📌 31.3 RegExp 메서드

### ▶️ RegExp.prototype.exec

`exec` 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 **배열**로 반환한다. 매칭 결과가 없는 경우 `null`을 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

### ▶️ RegExp.prototype.test

`test` 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 **불리언 값**으로 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); // -> true
```

### ▶️ String.prototype.match

`String` 표준 빌트인 객체가 제공하는 `match` 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 **배열**로 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp); // -> ["is", "is"]
```

`exec` 메서드는 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭 결과만 반환한다. 하지만 `match` 메서드는 g플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/g;

target.match(regExp); // -> ["is", "is"]
```

## 📌 31.4 플래그

**플래그**는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.
| 플래그 | 의미 | 설명 |
| --- | --- | --- |
| i | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다. |
| g | Global | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m | Multi line | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다. |

플래그는 선택적으로 사용할 수 있으며 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.    
그리고 문자열에 패턴 검색 매칭이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

```js
const target = "Is this all there is?";

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색한다.
target.match(/is/);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색한다.
target.match(/is/i);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색한다.
target.match(/is/g);
// -> ["is", "is"]

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색한다.
target.match(/is/gi);
// -> ["Is", "is", "is"]
```

## 📌 31.5 패턴

**패턴**은 문자열의 일정한 규칙을 표현하기 위해 사용한다. <br>
패턴은 /로 열고 닫으며 문자열의 따옴표는 생략한다.

### ▶️ 문자열 검색

정규 표현식의 패턴에 문자열을 지정하면 검색 대상 문자열에서 지정한 문자열을 검색한다.

```js
const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴. 플래그가 생략되었으므로 대소문자를 구별한다.
const regExp = /is/;

// target과 정규 표현식이 매치하는지 테스트한다.
regExp.test(target); // -> true

// target과 정규 표현식의 매칭 결과를 구한다.
target.match(regExp);
// -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

대소문자를 구별하지 않고 검색하려면 플래그 `i`를 사용한다.

```js
const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴. 플래그 i를 추가하면 대소문자를 구별하지 않는다.
const regExp = /is/i;

target.match(regExp);
// -> ["Is", index: 0, input: "Is this all there is?", groups: undefined]
```

검색 대상 문자열에서 패턴에 맞는 모든 문자열을 전역 검색하려면 플래그 `g`를 사용한다.

```js
const target = "Is this all there is?";

// 'is' 문자열과 매치하는 패턴.
// 플래그 g를 추가하면 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
const regExp = /is/gi;

target.match(regExp); // -> ["Is", "is", "is"]
```

### ▶️ 임의의 문자열 검색

`.`은 임의의 문자 한 개를 의미한다.

```js
const target = "Is this all there is?";

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp); // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### ▶️ 반복 검색

`{m,n}`은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다. 콤마 뒤에 공백이 없어야 한다.

```js
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;

target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]
```

`{n}`은 앞선 패턴이 n번 반복되는 문자열을 의미한다. 즉, {n,n}과 같다.

```js
const target = "A AA B BB Aa Bb AAA";

// 'A'가 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{2}/g;

target.match(regExp); // -> ["AA", "AA"]
```

`{n,}`은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.

```js
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A{2,}/g;

target.match(regExp); // -> ["AA", "AAA"]
```

`+`는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. 즉 {1,}과 같다.

```js
const target = "A AA B BB Aa Bb AAA";

// 'A'가 최소 한 번 이상 반복되는 문자열('A, 'AA', 'AAA', ...)을 전역 검색한다.
const regExp = /A+/g;

target.match(regExp); // -> ["A", "AA", "A", "AAA"]
```

`?`는 앞선 패턴이 최대 한 번 이상 반복되는 문자열을 의미한다. 즉 {0,1}과 같다.

```js
const target = "color colour";

// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색한다.
const regExp = /colou?r/g;

target.match(regExp); // -> ["color", "colour"]
```

### ▶️ OR 검색

`|`은 or의 의미를 갖는다.

```js
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'를 전역 검색한다.
const regExp1 = /A|B/g;

target.match(regExp1); // -> ["A", "A", "A", "B", "B", "B", "A", "B"]

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp2 = /A+|B+/g;

target.match(regExp2); // -> ["A", "AA", "B", "BB", "A", "B"]
```

`[]`내의 문자는 or로 동작한다.

```js
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g;

target.match(regExp); // -> ["A", "AA", "B", "BB", "A", "B"]
```

범위를 지정하려면 `[]`내에 `-`를 사용한다.

```js
const target = "A AA BB ZZ Aa Bb 12,345";

// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ... ~ 또는 'Z', 'ZZ', 'ZZZ', ...
const regExp1 = /[A-Z]+/g;

target.match(regExp1); // -> ["A", "AA", "BB", "ZZ", "A", "B"]

// 'A' ~ 'Z' 또는 'a' ~ 'z'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp2 = /[A-Za-z]+/g;

target.match(regExp2); // -> ["AA", "BB", "Aa", "Bb"]

// '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp3 = /[0-9]+/g;

target.match(regExp3); // -> ["12", "345"]

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp4 = /[0-9,]+/g;

target.match(regExp4); // -> ["12,345"]
```

### ▶️ OR 검색 간단한 표현 ([메타문자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes))

| 메타문자 | 의미                                 |
| -------- | ------------------------------------ |
| \d       | 숫자                                 |
| \D       | 숫자가 아닌 문자                     |
| \w       | 알파벳, 숫자, 언더스코어             |
| \W       | 알파벳, 숫자, 언더스코어가 아닌 문자 |

```js
const target = "AA BB 12,345";

// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\d,]+/g;

target.match(regExp); // -> ["12,345"]

// '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D,]+/g;

target.match(regExp); // -> ["AA BB ", ","]
```

```js
const target = "Aa Bb 12,345 _$%&";

// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\w,]+/g;

target.match(regExp); // -> ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\W,]+/g;

target.match(regExp); // -> [" ", " ", ",", " ", "$%&"]
```

### ▶️ NOT 검색

`[...]`내의 `^`은 not의 의미를 갖는다.

```js
const target = "AA BB 12 Aa Bb";

// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^0-9]+/g;

target.match(regExp); // -> ["AA BB ", " Aa Bb"]
```

### ▶️ 시작 위치로 검색

`[...]` 밖의 `^`은 문자열의 시작을 의미한다.

```js
const target = "https://poiemaweb.com";

// 'https'로 시작하는지 검사한다.
const regExp = /^https/;

regExp.test(target); // -> true
```

### ▶️ 마지막 위치로 검색

`$`는 문자열의 마지막을 의미한다.

```js
const target = "https://poiemaweb.com";

// 'com'으로 끝나는지 검사한다.
const regExp = /com$/;

regExp.test(target); // -> true
```

## 📌 31.6 자주 사용하는 정규표현식

### ▶️ 특정 단어로 시작하는지 검사

```js
const url = "https://example.com";

// 'http://' 또는 'https://'로 시작하는지 검사한다.
/^https?:\/\//.test(url); // -> true
// 다음과 동일하다.
/^(http|https):\/\//.test(url); // -> true
```

### ▶️ 특정 단어로 끝나는지 검사

```js
const fileName = "index.html";

// 'html'로 끝나는지 검사한다.
/html$/.test(fileName); // -> true
```

### ▶️ 숫자로만 이루어진 문자열인지 검사

```js
const target = "12345";

// 숫자로만 이루어진 문자열인지 검사한다.
/^\d+$/.test(target); // -> true
```

### ▶️ 하나이상의 공백으로 시작하는지 검사

\s는 여러가지 공백 문자(스페이스, 탭 등)를 의미한다. 즉, \s는 `[\t\r\n\v\f]`와 같은 의미다.

```js
const target = " Hi!";

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // -> true
```

### ▶️ 아이디로 사용 가능한지 검사

```js
const id = "abc123";

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4,10}$/.test(id); // -> true
```

### ▶️ 메일 주소 형식에 맞는지 검사

```js
const email = "ungmo2@gmail.com";

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // -> true
```

### ▶️ 핸드폰 번호 형식에 맞는지 검사

```js
const cellphone = "010-1234-5678";

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
```

### ▶️ 특수 문자 포함 여부 검사

```js
const target = "abc#123";

// A-Za-z0-9 이외의 문자가 있는지 검사한다.
/[^A-Za-z0-9]/gi.test(target); // -> true
```

특수문자를 선택적으로 검사하기 위해서는 다음 방식을 사용한다.

```js
/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi.test(target); // -> true
```

특수 문자를 제거할 때는 String.prototype.replace 메서드를 사용한다.

```js
target.replace(/[^A-Za-z0-9]/gi, ""); // -> abc123
```
