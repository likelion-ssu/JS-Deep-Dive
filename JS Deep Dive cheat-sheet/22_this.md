# 22. this

## 📌 22.1 this 키워드
객체는 상태(state)를 나타내는 `프로퍼티`와 동작(behavior)을 나타내는 `메서드`를 하나의 논리적 단위로 묶은 복합적 자료구조이다.    
`메서드`는 자신이 속한 객체의 상태, `프로퍼티`를 참조하고 변경할 수 있어야 한다.   
메서드가 자신이 속한 객체의 프로퍼티를 참조하려면,   
💡 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.**   
> 객체 리터럴 방식으로 생성한 객체의 경우,    
> 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.      

<br>

참조 표현식이 평가되는 시점은 **메서드가 호출되어 함수 몸체가 실행되는 시점**이다.   
생성자 함수 내부에서는 프로퍼티 또는 메서드의 추가를 위해 **자신이 생성할 인스턴스를 참조**할 수 있어야 한다.   
- 생성자 함수에 의한 객체 생성 방식은 `new` 연산자와 함께 생성자 함수를 호출하는 단계가 추가적으로 필요하다.   
- 생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이다.   
- 따라서 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.    

이를 위해 자바스크립트에서 `this`라는 특수한 식별자를 제공한다.    

|💡 **`this` 특징**|
|:---------------|
|자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 **자기 참조 변수**|
|`this`를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 **프로퍼티 & 메서드 참조 가능**|
|자바스크립트 엔진에 의해 암묵적으로 생성|
|코드 어디에서든 참조 가능|
|자바스크립트 엔진은 함수 호출 시 `arguments 객체`와 `this`를 암묵적으로 함수 내부에 전달|
|`this`가 가리키는 값, `this` 바인딩<sup>*</sup>은 **함수 호출 방식에 의해 동적으로 결정**|  

> 바인딩 : 식별자와 값을 연결하는 과정   
>   ex) 변수 선언 : 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것    
> `this` 바인딩 : `this`가 가리킬 객체를 바인딩하는 것   

<details>
<summary><b>💡 this 식별자를 적용한 코드 예시 확인</b></summary>

```js
function Circle(radius) {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  ????.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
  return 2 * ????.radius;
};

// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const circle = new Circle(5);
```
위 코드에 `this` 식별자를 적용해 수정해보자.   
```js
// 객체 리터럴
const circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  }
};

console.log(circle.getDiameter()); // 10
```
`this`는 상황에 따라 가리키는 객체가 다르다.   
```js
// 생성자 함수
function Circle(radius) {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
}

Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * this.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```
- 생성자 함수 내부의 `this`는 생성자 함수가 생성할 인스턴스를 가리킨다.
- 자바, C++와 같은 클래스 기반 언어에서는 항상 클래스가 생성하는 인스턴스를 가리키는 용도로 `this`를 사용한다.
- 자바스크립트의 `this` **함수가 호출되는 방식에 따라 `this`에 바인딩될 값으로 결정**되기에 동적으로 값이 결정된다.
- strict mode 또한 `this` 바인딩에 영향을 준다.
</details>

`this`는 코드 어디에서든 참조가 가능하다. 즉, 전역에서도 함수 내부에서도 참조할 수 있다.   
```js
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  }
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person('Lee');
```
`this`는 **객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수**이다.     
따라서 일반적으로 **객체의 메서드 내부 또는 생성자 함수 내부에서만 의미**가 있다.   
strict mode가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩 된다.   
(일반 함수 내부에서 this를 사용할 일이 없기 때문이다.)   

## 📌 22.2 함수 호출 방식과 this 바인딩
**`this` 바인딩은 함수 호출 방식(함수가 어떻게 호출되었는지)에 따라 동적으로 결정된다.**   

**렉시컬 스코프**와 **this 바인딩**은 결정 시기가 다르다.   
- 렉시컬 스코프 (함수의 상위 스코프를 결정하는 방식) : 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프 결정
- this 바인딩 : 함수 호출 시점에 결정
<br />

- **함수 호출 방식**
    1. 일반 함수 호출
    2. 메서드 호출
    3. 생성자 함수 호출
    4. `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출

<details>
<summary><b>💡 함수 호출 방식에 따른 예제 코드</b></summary>

```js
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: 'bar' };

foo.call(bar);   // bar
foo.apply(bar);  // bar
foo.bind(bar)(); // bar
```

</details>

### ▶️ 일반 함수 호출
기본적으로 **`this`에는 전역 객체(global object)가 바인딩**된다.   
중첩함수를 **일반 함수로 호출하면 함수 내부의 `this`에는 전역 객체가 바인딩** 된다.   
단, 객체를 생성하지 않는 일반 함수에서 `this`는 의미가 없다.     
<details>
<summary><b>💡 일반 함수 호출을 통한 this 바인딩 예제 코드</b></summary>

```js
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티다.
var value = 1;
// const 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티가 아니다.
// const value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this);  // {value: 100, foo: ƒ}
    console.log("foo's this.value: ", this.value); // 100

    // 메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }

    // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
    bar();
  }
};

obj.foo();
```
```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: ƒ}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this: ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  }
};

obj.foo();
```
</details>

콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 `this`에도 전역 객체가 바인딩된다.   
**일반 함수로 호출된 모든 함수(중첩, 콜백 함수 포함) 내부의 `this`에는 전역 객체가 바인딩된다.**     
&nbsp;&nbsp;&nbsp; ※ 중첩함수나 콜백 함수는 헬퍼 함수의 역할을 하므로 `this`가 전역 객체를 바인딩하는 것은 문제가 있다.   
&nbsp;&nbsp;&nbsp; ※ 메서드의 this 바인딩과 일치시켜줄 필요가 있다.   

|**`this`를 명시적으로 바인딩할 수 있는 메서드**|    
|:---|
|`Function.prototype.apply`|
|`Fucntion.prototype.call`|
|`Function.prototype.bind`|

### ▶️ 메서드 호출
메서드 내부의 `this`에는 **메서드를 호출한 객체가 바인딩**된다.   
&nbsp;&nbsp;&nbsp; ※ **메서드 내부의 `this`** : ❌ ~~메서드를 소유한 객체~~가 아닌, ⭕️ **메서드를 호출한 객체에 바인딩**된다.   
```js
const person = {
  name: 'Lee',
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  }
};

// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee
```
`person` 객체의 `getName` 프로퍼티가 가리키는 함수 객체는 **독립적으로 존재하는 별도의 객체**다.

<img src="https://user-images.githubusercontent.com/66112716/218306941-3db82f95-c257-41aa-b72a-5c30cd1ff305.png" width="700" />

[image reference](https://velog.io/@gavri/%EB%AA%A8%EB%8D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Deep-Dive-2223-this%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8)

```js
const anotherPerson = {
  name: 'Kim'
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```
메서드 내부의 `this`는 **메서드를 호출한 객체에 바인딩**된다.   
(프로퍼티로 메서드를 가리키고 있는 객체와는 관계가 없다.)   

<img src="https://user-images.githubusercontent.com/66112716/218307594-ac1a777c-2873-4ab4-9196-4da30a99b516.png" width="700" />

[image reference](https://velog.io/@gavri/%EB%AA%A8%EB%8D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-Deep-Dive-2223-this%EC%8B%A4%ED%96%89-%EC%BB%A8%ED%85%8D%EC%8A%A4%ED%8A%B8)


```js
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

const me = new Person('Lee');

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // ① Lee

Person.prototype.name = 'Kim';

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // ② Kim
```
`object.prototype`도 객체이므로 메서드를 직접 호출할 수 있다.

### ▶️ 생성자 함수 호출
생성자 함수 내부의 `this`는 **생성자 함수가 (미래에) 생성할 인스턴스**가 바인딩된다.   
생성자 함수는 이름 그대로 **객체(인스턴스)** 를 생성하는 함수이다.    
일반 함수와 동일한 방법으로 생성자 함수를 정의하고, `new` 연산자와 함께 호출할 경우 해당 함수는 생성자 함수로 동작한다.   
```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

### ▶️ `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출
`apply`, `call`, `bind` 메서드는 `Function.prototype`의 메서드이다.    
즉 모든 함수가 상속받아 사용할 수 있다.   

<img src="https://user-images.githubusercontent.com/66112716/218308763-902f4ac0-39fa-4ff1-b1c4-c0803047633c.png" width="700" />

[image reference](https://velog.io/@kozel/%EB%AA%A8%EB%8D%98-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-22%EC%9E%A5-this#2224-functionprototypeapplycallbind-%EB%A9%94%EC%84%9C%EB%93%9C%EC%97%90-%EC%9D%98%ED%95%9C-%EA%B0%84%EC%A0%91-%ED%98%B8%EC%B6%9C)

<details>
<summary><b>💡 apply와 call 메서드 사용 예제 코드</b></summary>

```js
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */

Function.prototype.apply(thisArg[, argsArray])

/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param arg1, arg2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */

 Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])
```
</details>

`apply`와 `call`의 본질적인 기능은 함수를 호출하는 것이다.    
첫 번째 인수로 전달한 특정 객체를 호출한 함수의 `this`에 바인딩한다.   
apply와 call 메서드는 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작한다.   
```js
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

`apply`와 `call`메서드의 대표적 용도는 `arguments 객체`와 같은 **유사 배열 객체에 배열 메서드를 사용하는 경우**이다.   
- `apply` : 배열로 담은 arguments를 통째로 전달
- `call` : `,`로 구분하는 여러개의 인수를 하나의 배열로 묶어서 전달

## 📌 함수 호출 방식에 따른 this 바인딩의 차이 한 눈에 보기

|**함수 호출 방식**|**`this` 바인딩**|
|:---------:|:--------:|
|함수 호출 방식|this 바인딩|
|일반 함수 호출|전역 객체|
|메서드 호출|메서드를 호출한 객체|
|생성자 함수 호출|생성자 함수가 (미래에) 생성할 인스턴스|
|`Function.prototype.apply/call/bind` 메서드에 의한 간접 호출|`apply/call/bind` 메서드에 첫 번째 인수로 전달한 객체|
