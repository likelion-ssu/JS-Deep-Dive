# 37. Set과 Map

## 📌 37.1 Set
**`Set` 객체는 중복되지 않는 유일한 값들의 집합이다.**   

- 배열과 `Set`의 차이

**구분**|**배열**|**`Set` 객체**
:---:|:---|:---:
동일한 값을 중복하여 포함할 수 있다.|O|X
요소 순서에 의미가 있다.|O|X
인덱스로 요소에 접근할 수 있다.|O|X

위와 같은 `Set` 객체의 특성은 수학적 _집합_ 의 특성과 일치한다.   
`Set`은 수학적 집합을 구현하기 위한 자료구조로, 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.   

### ▶️ `Set` 객체의 생성
`Set` 객체는 `Set` 생성자 함수로 생성하며, 생성자 함수에 인수를 전달하지 않을 경우 빈 `Set` 객체가 생성된다.   
```js
const set = new Set();
console.log(set); // Set(0) {}
```
`Set` 생성자 함수는 **이터러블을 인수로 전달받아 `Set` 객체를 생성**한다.   
이터러블의 중복된 값은 `Set` 객체에 요소로 저장되지 않는다.   
<br>

또한 `Set`은 중복을 허용하지 않기에, 이 특성을 활용해 배열에서 중복된 요소를 제거할 수 있다.   
```js
// 배열의 중복 요소 제거
const uniq = array => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set을 사용한 배열의 중복 요소 제거
const uniq = array => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

### ▶️ `size` : 요소 개수 확인
- `Set.prototype.size` : `Set` 객체의 요소 개수 확인   
    - `size`프로퍼티는 `setter`함수 없이 `getter` 함수만 존재하는 접근자 프로퍼티이다.   
    - `size` 프로퍼티에 숫자를 할당해 `Set` 객체의 요소 개수를 변경할 수 없다.   
```js
const set = new Set([1, 2, 3]);

console.log(Object.getOwnPropertyDescriptor(Set.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

set.size = 10; // 무시된다.
console.log(set.size); // 3
```

### ▶️ `add()` : 요소 추가
- `Set.prototype.add` : `Set` 객체에 요소를 추가할 때 사용하는 메서드
    - `add` 메서드는 새로운 요소가 추가된 `Set` 객체를 반환한다.   
    - `add` 메서드를 호출한 후 `add` 메서드를 연속적으로 호출할 수 있다.   
```js
const set = new Set();
console.log(set); // Set(0) {}

set.add(1);
console.log(set); // Set(1) {1}
```

<details>
<summary><b>💡 요소 추가와 관련된 추가 사항 및 예제코드</b></summary>

`Set` 객체에 중복된 요소의 추가는 허용되지 않는다.   
위의 상황이 발생할 경우 에러가 발생하지 않고 무시된다.   
```js
const set = new Set();

set.add(1).add(2);
console.log(set); // Set(2) {1, 2}
```
```js
const set = new Set();

set.add(1).add(2).add(2);
console.log(set); // Set(2) {1, 2}
```
<br>

일치 비교 연산자 ===를 사용할 경우, `NaN`과 `NaN`을 다르다고 평가한다.   
그러나 `Set` 객체의 경우 `NaN`과 `NaN`을 같다고 평가하여 중복 추가를 허용하지 않는다.   
또한 +0과 -0 역시 일치 비교 연산자 ===와 마찬가지로 같다고 평가하여 중복 추가를 허용하지 않는다.   
```js
const set = new Set();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(NaN).add(NaN);
console.log(set); // Set(1) {NaN}

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
set.add(0).add(-0);
console.log(set); // Set(2) {NaN, 0}
```
<br>

`Set` 객체의 경우 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.   
```js
const set = new Set();

set
  .add(1)
  .add('a')
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([]);

console.log(set); // Set(7) {1, "a", true, undefined, null, {}, []}
```
</details>

### ▶️ `has()` : 요소 존재 여부 확인
- `Set.prototype.has` : `Set` 객체에 특정 요소가 존재하는지 확인하는 메서드
    - `has` 메서드는 특정 요소의 존재 여부를 나타내는 `boolean` 값을 반환한다.   
```js
const set = new Set([1, 2, 3]);

console.log(set.has(2)); // true
console.log(set.has(4)); // false
```

### ▶️ `delete()` : 요소 삭제
- `Set.prototype.delete` : `Set` 객체의 특정 요소를 삭제하는 메서드
    - `delete` 메서드를 사용할 경우 삭제 성공 여부를 나타내는 `boolean`값을 반환한다.
    - `delete` 메서드에는 ~~인덱스~~가 아닌 **삭제하려는 요소 값**을 인수로 전달해야 한다.   
        - `Set` 객체는 순서에 의미가 없다.   
        - 배열과 같이 인덱스를 가지지 않는다.   
```js
const set = new Set([1, 2, 3]);

// 요소 2를 삭제한다.
set.delete(2);
console.log(set); // Set(2) {1, 3}

// 요소 1을 삭제한다.
set.delete(1);
console.log(set); // Set(1) {3}
```

<details>
<summary><b>💡 요소 삭제와 관련된 추가 사항 및 예제코드</b></summary>

존재하지 않는 `Set` 객체 요소를 삭제하려 할 경우 에러 없이 무시된다.   
```js
const set = new Set([1, 2, 3]);

// 존재하지 않는 요소 0을 삭제하면 에러없이 무시된다.
set.delete(0);
console.log(set); // Set(3) {1, 2, 3}
```
<br>

`delete` 메서드는 삭제 성공 여부를 나타내는 `boolean` 값을 반환하므로,   
`Set.prototype.add`와 같이 연속적으로 호출할 수 없다.   
```js
const set = new Set([1, 2, 3]);

// delete는 불리언 값을 반환한다.
set.delete(1).delete(2); // TypeError: set.delete(...).delete is not a function
```
</details>

### ▶️ `clear()` : 요소 일괄 삭제
- `Set.prototype.clear` : `Set` 객체의 모든 요소를 일괄 삭제하는 메서드
    - `clear` 메서드는 언제나 `undefined`를 반환한다.   
```js
const set = new Set([1, 2, 3]);

set.clear();
console.log(set); // Set(0) {}
```

### ▶️ `forEach()` : 요소 순회
- `Set.prototype.forEach` : `Set` 객체의 요소를 순회하는 메서드
    - `Array.prototype.forEach`와 유사하게 콜백 함수와 `forEach` 메서드의 콜백 함수 내부에서 `this`로 사용될 객체를 인수로 전달한다.   

|**`Set.prototype.forEach`의 인수**|
|:---:|
|**첫번째 인수** : 현재 순회 중인 요소 값|
|**두번째 인수** : 현재 순회 중인 요소 값|
|**세번째 인수** : 현재 순회 중인 `Set` 객체 자체|

```js
const set = new Set([1, 2, 3]);

set.forEach((v, v2, set) => console.log(v, v2, set));
/*
1 1 Set(3) {1, 2, 3}
2 2 Set(3) {1, 2, 3}
3 3 Set(3) {1, 2, 3}
*/
```
- 첫번째 인수와 두번째 인수는 같은 값이다. 
    - `Array.prototype.forEach` 메서드와 인터페이스를 통일하기 위함이다.(다른 의미는 없다.)
- `Array.prototype.forEach`메서드의 콜백 함수 : 두번째 인수로 현재 순회 중인 요소의 인덱스를 전달받는다.
    - `Set` 객체는 순서에 의미가 없기에 배열과 같이 인덱스를 가지지 않는다.
<br>

**`Set` 객체는 이터러블이다.**   
따라서 `for...of` 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.   
<details>
<summary><b>💡 예제 코드 37-17</b></summary>

```js
const set = new Set([1, 2, 3]);

// Set 객체는 Set.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set); // true

// 이터러블인 Set 객체는 for...of 문으로 순회할 수 있다.
for (const value of set) {
  console.log(value); // 1 2 3
}

// 이터러블인 Set 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...set]); // [1, 2, 3]

// 이터러블인 Set 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, ...rest] = [...set];
console.log(a, rest); // 1, [2, 3]
```
</details>

### ▶️ 집합 연산
`Set` 객체는 수학적 집합을 구현하기 위한 자료구조이므로,   
`Set` 객체를 통해 교집합, 합집합, 차집합 등을 구현할 수 있다.   

<details>
<summary><b>1️⃣ A 교집합 B 구현 </b></summary>

- 구현 방법 1
```js
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

- 구현 방법 2
```js
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```
</details>

<details>
<summary><b>2️⃣ A 합집합 B 구현 </b></summary>

- 구현 방법 1
```js
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

- 구현 방법 2
```js
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```
</details>

<details>
<summary><b>3️⃣ A 차집합 B 구현 </b></summary>

- 구현 방법 1
```js
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

- 구현 방법 2
```js
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```
</details>

<details>
<summary><b>4️⃣ 부분 집합과 상위 집합 </b></summary>

- 구현 방법 1
```js
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

- 구현 방법 2
```js
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```
</details>

## 📌 37.2 Map
**`Map` 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.**   
`Map`객체와 객체는 유사하나, 다음과 같은 차이가 있다.   

**구분**|**객체**|**`Map` 객체**
:---:|:---:|:---:
키로 사용할 수 있는 값|문자열 또는 심벌 값|객체를 포함한 모든 값
이터러블|X|O
요소 개수 확인|`Object.keys(obj).length`|`map.size`

### ▶️ `Map` 객체의 생성
`Map` 객체는 `Map` 생성자 함수로 생성한다.   
`Map` 생성자 함수에 인수를 전달하지 않을 경우 빈 `Map` 객체가 생성된다.   
```js
const map = new Map();
console.log(map); // Map(0) {}
```
<br>

**`Map` 생성자 함수는 이터러블을 인수로 전달받아 `Map` 객체를 생성**한다.   
이 때 **인수로 전달되는 이터러블은 `키와 값`의 쌍으로 이루어진 요소로 구성되어야 한다.**   
```js
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```
※ `Map` 생성자 함수의 인수로 전달한 이터러블에 **중복된 키**를 가지는 요소가 존재할 경우, 값이 덮어씌워진다.     
→ `Map` 객체에는 중복된 키를 가지는 요소가 존재할 수 없다.   

### ▶️ `size` : 요소 개수 확인
- `Map.prototype.size` : `Map` 객체의 요소 개수를 확인하는 프로퍼티
    - `size` 프로퍼티는 `setter`함수 없이 `getter` 함수만 존재하는 접근자 프로퍼티이다. 
    - `size` 프로퍼티에 숫자를 할당해 `Map` 객체 요소 개수를 변경할 수 없다.   
```js
const { size } = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(size); // 2
```
```js
const map = new Map([['key1', 'value1'], ['key2', 'value2']]);

console.log(Object.getOwnPropertyDescriptor(Map.prototype, 'size'));
// {set: undefined, enumerable: false, configurable: true, get: ƒ}

map.size = 10; // 무시된다.
console.log(map.size); // 2
```

### ▶️ `set()` : 요소 추가
- `Map.prototype.set` : `Map` 객체에 요소를 추가하는 메서드
    - `set` 메서드는 새로운 요소가 추가된 `Map` 객체를 반환한다.
    - `set` 메서드 호출 후 `set` 메서드를 연속적으로 호출할 수 있다.
```js
const map = new Map();
console.log(map); // Map(0) {}

map.set('key1', 'value1');
console.log(map); // Map(1) {"key1" => "value1"}
```
```js
const map = new Map();

map
  .set('key1', 'value1')
  .set('key2', 'value2');

console.log(map); // Map(2) {"key1" => "value1", "key2" => "value2"}
```
<details>
<summary><b>💡 요소 추가와 관련된 추가 사항 및 예제코드</b></summary>

`Map` 객체에는 중복된 키를 가지는 요소가 존재할 수 없다.   
따라서 중복된 키를 가지는 요소가 추가될 경우 값이 덮어씌워진다.   
이 때 에러는 발생하지 않는다.   
```js
const map = new Map();

map
  .set('key1', 'value1')
  .set('key1', 'value2');

console.log(map); // Map(1) {"key1" => "value2"}
```
<br>

일치 비교 연산자 ===의 사용 시 `NaN`과 `NaN`을 다르다고 평가하지만,   
`Map` 객체의 경우 `NaN`과 `NaN`을 같다고 평가하여 중복 추가를 허용하지 않는다.   
또한 +0과 -0 역시 일치 비교 연산자 ===와 마찬가지로 같다고 평가해 중복 추가를 허용하지 않는다.   
```js
const map = new Map();

console.log(NaN === NaN); // false
console.log(0 === -0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(NaN, 'value1').set(NaN, 'value2');
console.log(map); // Map(1) { NaN => 'value2' }

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(0, 'value1').set(-0, 'value2');
console.log(map); // Map(2) { NaN => 'value2', 0 => 'value2' }
```
<br>

- `객체`는 **문자열 또는 심벌 값만 키로 사용**할 수 있다.    
- `Map`는 **키 타입에 제한이 없다.**   
따라서 객체를 포함하여 모든 값을 키로 사용할 수 있다.   
```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

// 객체도 키로 사용할 수 있다.
map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map);
// Map(2) { {name: "Lee"} => "developer", {name: "Kim"} => "designer" }
```
</details>

### ▶️ `get()` : 요소 취득
- `Map.prototype.get` : `Map` 객체에서 특정 요소를 취득하는 메서드
    - `get` 메서드의 인수로 키를 전달할 경우, `Map` 객체에서 인수로 전달한 `키`를 가지는 `값`을 반환한다. 
    - 인수로 전달한 `키`를 가지는 요소가 존재하지 않을 경우 `undefined`를 반환한다. 
```js
const map = new Map();

const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

map
  .set(lee, 'developer')
  .set(kim, 'designer');

console.log(map.get(lee)); // developer
console.log(map.get('key')); // undefined
```

### ▶️ `has()` : 요소 존재 여부 확인
- `Map.prototype.has` : `Map` 객체에 특정 요소가 존재하는지 확인하는 메서드
    - `has` 메서드는 특정 요소의 존재 여부를 나타내는 `boolean` 값을 반환한다.
```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

console.log(map.has(lee)); // true
console.log(map.has('key')); // false
```

### ▶️ `delete()` : 요소 삭제
- `Map.prototype.delete` : `Map` 객체의 요소를 삭제하는 메서드
    - `delete` 메서드는 삭제 성공 여부를 나타내는 `boolean` 값을 반환한다.
```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(kim);
console.log(map); // Map(1) { {name: "Lee"} => "developer" }
```
<details>
<summary><b>💡 요소 삭제와 관련된 추가 사항 및 예제코드</b></summary>

존재하지 않는 키로 `Map` 객체의 요소를 삭제하려 할 경우 에러 없이 무시된다.   
```js
const map = new Map([['key1', 'value1']]);

// 존재하지 않는 키 'key2'로 요소를 삭제하려 하면 에러없이 무시된다.
map.delete('key2');
console.log(map); // Map(1) {"key1" => "value1"}
```
<br>

`delete` 메서드는 삭제 성공 여부를 나타내는 `boolean` 값을 반환하므로,    
`set` 메서드와 달리 연속적으로 호출할 수 없다.   
```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.delete(lee).delete(kim); // TypeError: map.delete(...).delete is not a function
```
</details>

### ▶️ `clear()` : 요소 일괄 삭제
- `Map.prototype.clear` : `Map` 객체의 요소를 일괄 삭제하는 메서드
    - `clear` 메서드는 언제나 `undefined`를 반환한다.
```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.clear();
console.log(map); // Map(0) {}
```

### ▶️ `forEach()` : 요소 순회
- `Map.prototype.forEach` : `Map` 객체의 요소를 순회하는 메서드
    - `Array.prototype.forEach`와 유사하게 콜백 함수와 `forEach` 메서드의 콜백 함수 내부에서 `this`로 사용될 객체를 인수로 전달한다.   

|**`Map.prototype.forEach`의 인수**|
|:---:|
|**첫번째 인수** : 현재 순회 중인 요소 값|
|**두번째 인수** : 현재 순회 중인 요소 키|
|**세번째 인수** : 현재 순회 중인 `Map` 객체 자체|

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
*/
```

**`Map` 객체는 이터러블이다.**   
따라서 `for...of` 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수 있다.   
<details>
<summary><b>💡 예제 코드 37-43 & 37-44</b></summary>

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// Map 객체는 Map.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in map); // true

// 이터러블인 Map 객체는 for...of 문으로 순회할 수 있다.
for (const entry of map) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}

// 이터러블인 Map 객체는 스프레드 문법의 대상이 될 수 있다.
console.log([...map]);
// [[{name: "Lee"}, "developer"], [{name: "Kim"}, "designer"]]

// 이터러블인 Map 객체는 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [a, b] = map;
console.log(a, b); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
```

```js
const lee = { name: 'Lee' };
const kim = { name: 'Kim' };

const map = new Map([[lee, 'developer'], [kim, 'designer']]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
for (const key of map.keys()) {
  console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const value of map.values()) {
  console.log(value); // developer designer
}

// Map.prototype.entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const entry of map.entries()) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}
```
</details>

**`Map` 메서드**|**설명**
:---|:---
`Map.prototype.keys`|`Map` 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환
`Map.prototype.values`|`Map` 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환
`Map.prototype.entries`|`Map` 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환
