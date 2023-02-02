# 16. 프로퍼티 어트리뷰트

## 1️⃣ 16.1 내부 슬롯과 내부 메서드

> 프로퍼티 어트리뷰트를 이해하기 위해, `내부 슬롯`과 `내부 메서드`에 대한 개념을 알고있어야한다.

_내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 **ECMAScript 사양에서 사용하는 `의사 프로퍼티(pseudo property)`와 `의사 메서드(pseudo method)`이다.**_

- ECMAScript 사양에 정의된 대로 구현되어, 자바스크립트 엔진에서 실제로 동작한다.
- 원칙적으로 개발자가 직접 접근하거나 호출할 수 없다.
  - 외부로 공개된 객체의 프로퍼티가 아니기 때문이다.
  - 자바스크립트 엔진의 내부 로직이다.
- 일부는 간접적으로 접근할 수 있다.

  - 예를들어, 모든 객체는 `[[Prototype]]`이라는 `내부 슬롯`을 갖는다.
  - 해당 `내부 슬롯`에 `__proto__`를 통하여 간접적으로 접근할 수 있다.

  ```javascript
      const o = {};

      o.[[Prototype]] //접근 불가능
      o.__proto__ //접근 가능 => Object.prototype
  ```

## 2️⃣ 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

**자바스크립트 엔진은 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.**

- 그렇다면, **프로퍼티의 상태**란?
  - 프로퍼티의 값 (value)
  - 값의 갱신 가능 여부 (writable)
  - 열거 가능 여부 (enumerable)
  - 재정의 가능 여부 (configurable)
- 그렇다면, **프로퍼티 어트리뷰트**란?
  - 자바스크립트 엔진이 관리하는 내부 상태값 (meta-property)인 내부 슬롯
    - `[[Value]]`
    - `[[Writable]]`
    - `[[Enumerable]]`
    - `[[Configurable]]`
- 따라서, 프로퍼티 어트리뷰트에 직접 접근할 수 없다.

  - 하지만, `Object.getOwnPropertyDescriptor` 메서드를 통해 간접 확인은 가능하다.

  ```javascript
  const person = {
    name: "Lee",
  };

  // 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
  console.log(Object.getOwnPropertyDescriptor(person, "name"));
  // {value: "Lee", writable: true, enumerable: true, configurable: true}
  ```

- `Object.getOwnPropertyDescriptor` 메서드의 파라미터
  1. 객체의 참조
  2. 프로퍼티의 키 (문자열 형태로)
- 메서드가 반환하는 `프로퍼티 디스크립터 객체`란?
  - 프로퍼티 어트리뷰트 정보를 제공한다.
  - 존재하지 않는 프로퍼티 or 상속받은 프로퍼티의 경우에는 `undefined`를 반환한다.
- `Object.getOwnPropertyDescriptors` 메서드는 모든 프로퍼티 디스크립터 객체들을 반환한다.

  ```javascript
  const person = {
    name: "Lee",
  };

  //프로퍼티 동적 생성
  person.age = 20;

  // 프로퍼티 어트리뷰트 정보를 제공하는 모든 프로퍼티 디스크립터 객체들을 반환한다.
  console.log(Object.getOwnPropertyDescriptors(person));
  /**
      {
          name : {value: "Lee", writable: true, enumerable: true, configurable: true},
          age : {value: 20, writable: true, enumerable: true, configurable: true}
      }
  */
  ```

## 3️⃣ 16.3 데이터 프로퍼티와 접근자 프로퍼티

### ✅ 데이터 프로퍼티

- 키와 값으로 구성된 일반적인 프로퍼티를 의미한다.
- 지금까지 살펴본 모든 프로퍼티가 `데이터 프로퍼티`였다.
- 데이터 프로퍼티 어트리뷰트는 `자바스크립트 엔진`이 프로퍼티를 생성할 때 기본 값을 정의된다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터<br/>객체의 프로퍼티 | 설명                              |
| ------------------- | --------------------------------------- | --------------------------------- |
| [[Value]]           | `value`                                 | 프로퍼티의 값이다.                |
| [[Writable]]        | `writable`                              | 변경 가능 여부의 `boolean`이다.   |
| [[Enumerable]]      | `enumerable`                            | 열거 가능 여부의 `boolean`이다.   |
| [[Configurable]]    | `configurable`                          | 재정의 가능 여부의 `boolean`이다. |

```javascript
const person = {
  name: "Lee",
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {value: "Lee", writable: true, enumerable: true, configurable: true}
```

- 위 코드를 분석해보자.
  - `value`가 "Lee"이다.
    - 프로퍼티 어트리뷰트 [[Value]]가 "Lee"인 것을 의미한다.
  - `writable` `enumerable` `configurable` 모두 `true`이다.
    - [[Writable]] [[Enumerable]] [[Configurable]] 모두 `true`이다.
- 프로퍼티를 동적으로 추가하여, 디스크립터를 받아오는 `Object.getOwnPropertyDescriptors`도 동일하게 분석한다.

### ✅ 접근자 프로퍼티

- 자체적으로는 값을 갖지 않는다.
- 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 `접근자 함수(accessor function)`로 구성된 프로퍼티이다.

| 프로퍼티 어트리뷰트 | 프로퍼티 디스크립터<br/>객체의 프로퍼티 | 설명                                                                               |
| ------------------- | --------------------------------------- | ---------------------------------------------------------------------------------- |
| [[Get]]             | `get`                                   | 접근자 프로퍼티를 통해 프로퍼티의 값을 읽을 때<br/>호출되는 **접근자 함수**이다.   |
| [[Set]]             | `set`                                   | 접근자 프로퍼티를 통해 프로퍼티의 값을 저장할 때<br/>호출되는 **접근자 함수**이다. |
| [[Enumerable]]      | `enumerable`                            | 열거 가능 여부의 `boolean`이다.                                                    |
| [[Configurable]]    | `configurable`                          | 재정의 가능 여부의 `boolean`이다.                                                  |

- 접근자 함수는 `getter/setter 함수` 라고도 부른다.
  - 접근자 프로퍼티는 `getter`와 `setter` 함수 모두 정의하거나 하나만 정의할 수도 있다.

```javascript
const person = {
  // 데이터 프로퍼티
  firstName: "Junkyu",
  lastName: "Lee",

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  // setter 함수
  set fullName(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
};

person.fullName = "Bokyeol Ko"; //setter 함수가 실행된다.
console.log(person); // {firstName: "Bokyeol", lastName: "Ko"}

console.log(person.fullName); // Bokyeol Ko

//firstName은 데이터 프로퍼티 => 데이터 프로퍼티 어트리뷰트 출력됨
console.log(Object.getOwnPropertyDescriptor(person, "firstName"));
//fullName은 접근자 프로퍼티 => 접근자 프로퍼티 어트리뷰트 출력됨
//getter와 setter은 function
console.log(Object.getOwnPropertyDescriptor(person, "fullName"));
```

### ✅ 데이터 프로퍼티와 접근자 프로퍼티를 구별하는 방법

```javascript
// 일반 객체의 __proto__는 접근자 프로퍼티다.
Object.getOwnPropertyDescriptor(Object.prototype, "__proto__");
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 함수 객체의 prototype은 데이터 프로퍼티다.
Object.getOwnPropertyDescriptor(function () {}, "prototype");
// {value: {...}, writable: true, enumerable: false, configurable: false}
```

프로퍼티 어트리뷰트를 객체로 표현한 프로퍼티 `디스크립터 객체`가 확연히 다르다는 것을 알 수 있다.

> 작성자의 추신😁 \_ 사실상 `value` `getter/setter`의 여부만 확인하면 된다.

## 4️⃣ 16.4 프로퍼티 정의

_새로운 프로퍼티를 추가하면서 **프로퍼티 어트리뷰트를 명시적으로 정의**하거나, 기존 프로퍼티의 **프로퍼티 어트리뷰트를 재정의**하는 것을 말한다._

가령, 아래와 같은 것들을 결정하는 과정이라고 할 수 있다.

- `writable` 프로퍼티 값을 갱신 가능하도록 할 것인가?
- `enuberable` 프로퍼티 값을 열거 가능하도록 할 것인가?
- `configurable` 프로퍼티 값을 재정의 가능하도록 할 것인가?

```javascript
const person = {};
Object.defineProperty(person, "lastName", {
  value: "Lee",
});

// 디스크립터 객체의 프로퍼티를 누락시키면 undefined, false가 기본값이다.
descriptor = Object.getOwnPropertyDescriptor(person, "lastName");
console.log("lastName", descriptor);
// lastName {value: "Lee", writable: false, enumerable: false, configurable: false}

// [[Enumerable]]의 값이 false인 경우
// 해당 프로퍼티는 for...in 문이나 Object.keys 등으로 열거할 수 없다.
// lastName 프로퍼티는 [[Enumerable]]의 값이 false이므로 열거되지 않는다.
console.log(Object.keys(person)); // ["firstName"]

// [[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없다.
// lastName 프로퍼티는 [[Writable]]의 값이 false이므로 값을 변경할 수 없다.
// 이때 값을 변경하면 에러는 발생하지 않고 무시된다.
person.lastName = "Kim";

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 삭제할 수 없다.
// lastName 프로퍼티는 [[Configurable]]의 값이 false이므로 삭제할 수 없다.
// 이때 프로퍼티를 삭제하면 에러는 발생하지 않고 무시된다.
delete person.lastName;

// [[Configurable]]의 값이 false인 경우 해당 프로퍼티를 재정의할 수 없다.
Object.defineProperty(person, "lastName", { enumerable: true });
// Uncaught TypeError: Cannot redefine property: lastName
```

_예제코드가 너무 길어, 중요한 부분만 첨부하였다. 코드 전문을 확인하고 싶다면 16-08을 확인하면 된다._

위와 같이, `Object.defineProperty` 메서드로 프로퍼티를 정의할 때 디스크립터 객체의 프로퍼티를 일부 생략할 수 있다.

이 때 적용되는 기본 값은 아래와 같다.

| 프로퍼티 디스크립터<br/>객체의 프로퍼티 | 대응하는 프로퍼티 어트리뷰트 | 기본값      |
| --------------------------------------- | ---------------------------- | ----------- |
| `value`                                 | [[Value]                     | `undefined` |
| `get`                                   | [[Get]]                      | `undefined` |
| `set`                                   | [[Set]]                      | `undefined` |
| `enumerable`                            | [[Enumerable]]               | `false`     |
| `configurable`                          | [Configurable]]              | `false`     |

> `Object.defineProperties` 메서드를 통해 **여러개의 프로퍼티**를 한 번에 정의할 수 있다.

## 5️⃣ 16.5 객체 변경 방지

- 객체는 변경 가능한 값이기에 재할당 없이 직접 변경할 수 있다.
  - 프로퍼티 추가 및 삭제
  - 프로퍼티 값 갱신
  - `Object.defineProperty` or `Object.defineProperties` 메서드를 통한 프로퍼티 어트리뷰트 재정의
- 때문에 객체 변경을 방지하는 메서드를 제공한다.
  - 변경 금지 강도에 따라 메서드가 나뉜다.

| 구분           | 메서드                     | 프로퍼티<br/>추가 | 프로퍼티<br/>삭제 | 프로퍼티 값<br/>읽기 | 프로퍼티 값<br/>쓰기 | 프로퍼티 어트리뷰트<br/>재정의 |
| -------------- | -------------------------- | ----------------- | ----------------- | -------------------- | -------------------- | ------------------------------ |
| 객체 확장 금지 | `Object.preventExtensions` | [-]               | [x]               | [x]                  | [x]                  | [x]                            |
| 객체 밀봉      | `Object.seal`              | [-]               | [-]               | [x]                  | [x]                  | [-]                            |
| 객체 동결      | `Object.freeze`            | [-]               | [-]               | [x]                  | [-]                  | [-]                            |

### ✅ 객체 확장 금지

- `Object.preventExtensions`
- 확장이 금지된 객체는 프로퍼티 추가가 모두 금지된다.
  - 동적 추가 ❌
  - `Object.defineProperty` ❌
- 확장 가능 여부는 `Object.isExtensible` 메서드로 확인이 가능하다.

```javascript
const person = { name: "Lee" };

// person 객체는 확장이 금지된 객체가 아니다.
console.log(Object.isExtensible(person)); // true

// person 객체의 확장을 금지하여 프로퍼티 추가를 금지한다.
Object.preventExtensions(person);

// person 객체는 확장이 금지된 객체다.
console.log(Object.isExtensible(person)); // false

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 추가는 금지되지만 삭제는 가능하다.
delete person.name;
console.log(person); // {}

// 프로퍼티 정의에 의한 프로퍼티 추가도 금지된다.
Object.defineProperty(person, "age", { value: 20 });
// TypeError: Cannot define property age, object is not extensible
```

### ✅ 객체 밀봉

- `Object.seal`
- 프로퍼티 추가 및 삭제와 프로퍼티 어트리뷰트 재정의를 금지한다.
  - **밀봉된 객체는 읽기와 쓰기만 가능하다.**
- 밀봉 여부는 `Object.isSealed` 메서드로 확인이 가능하다.

```javascript
const person = { name: "Lee" };

// person 객체는 밀봉(seal)된 객체가 아니다.
console.log(Object.isSealed(person)); // false

// person 객체를 밀봉(seal)하여 프로퍼티 추가, 삭제, 재정의를 금지한다.
Object.seal(person);

// person 객체는 밀봉(seal)된 객체다.
console.log(Object.isSealed(person)); // true

// 밀봉(seal)된 객체는 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
    {
    name: {value: "Lee", writable: true, enumerable: true, configurable: false},
    }
    */

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 값 갱신은 가능하다.
person.name = "Kim";
console.log(person); // {name: "Kim"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, "name", { configurable: true });
// TypeError: Cannot redefine property: name
```

### ✅ 객체 동결

- `Object.freeze`
- 프로퍼티 추가 및 삭제 금지
- 프로퍼티 어트리뷰트 재정의 금지
- 프로퍼티 값 갱신 금지
- **동결된 객체는 읽기만 가능하다.**

```javascript
const person = { name: "Lee" };

// person 객체는 동결(freeze)된 객체가 아니다.
console.log(Object.isFrozen(person)); // false

// person 객체를 동결(freeze)하여 프로퍼티 추가, 삭제, 재정의, 쓰기를 금지한다.
Object.freeze(person);

// person 객체는 동결(freeze)된 객체다.
console.log(Object.isFrozen(person)); // true

// 동결(freeze)된 객체는 writable과 configurable이 false다.
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: false, enumerable: true, configurable: false},
}
*/

// 프로퍼티 추가가 금지된다.
person.age = 20; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 삭제가 금지된다.
delete person.name; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 값 갱신이 금지된다.
person.name = "Kim"; // 무시. strict mode에서는 에러
console.log(person); // {name: "Lee"}

// 프로퍼티 어트리뷰트 재정의가 금지된다.
Object.defineProperty(person, "name", { configurable: true });
// TypeError: Cannot redefine property: name
```

### ✅ 불변 객체

- `Object.freeze의 재귀적 호출`
- 기존까지 변경 방지 메서드는, 중첩 객체까지는 영향을 주지 못했다.

  ```javascript
  const person = {
    name: "Lee",
    address: { city: "Seoul" },
  };

  // 얕은 객체 동결
  Object.freeze(person);

  // 직속 프로퍼티만 동결한다.
  console.log(Object.isFrozen(person)); // true
  // 중첩 객체까지 동결하지 못한다.
  console.log(Object.isFrozen(person.address)); // false

  person.address.city = "Busan";
  console.log(person); // {name: "Lee", address: {city: "Busan"}}
  ```

- `불변 객체`는 중첩 객체까지 동결한 **읽기 전용의 객체**이다.
  - 객체를 값으로 갖는 모든 프로퍼티를 재귀적으로 동결한다.

```javascript
function deepFreeze(target) {
  // 객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결한다.
  if (target && typeof target === "object" && !Object.isFrozen(target)) {
    Object.freeze(target);
    /*
      모든 프로퍼티를 순회하며 재귀적으로 동결한다.
    */
    Object.keys(target).forEach((key) => deepFreeze(target[key]));
  }
  return target;
}

const person = {
  name: "Lee",
  address: { city: "Seoul" },
};

// 깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결한다.
console.log(Object.isFrozen(person.address)); // true

person.address.city = "Busan";
console.log(person); // {name: "Lee", address: {city: "Seoul"}}
```
