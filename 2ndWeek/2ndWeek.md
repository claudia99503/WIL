# 모던 자바스크립트

# 자바스크립트의 동작 원리

## 자바스크립트의 데이터 타입

### 1. number

### 2. string

### 3. boolean

### 4. undefined

### 5. null

### 6. object

### 7. symbol

### 8. bigint

## 자바스크립트의 유연한 데이터 타입

- 자바스크립트는 데이터 타입이 유연한 프로그래밍 언어입니다.

## Truthy 값과 Falsy 값

- if, for, while 등 불린 타입의 값이 요구되는 맥락에서는 조건식이나 불린 타입의 값 뿐만 아니라 다른 타입의 값도 불린 값처럼 평가될 수 잇습니다.
- 이 때, false 처럼 평가되는 값을 falsy 값
    
    : `false` , `null` , `undefined` , `0` , `NaN` , `''(빈 문자열)` 
    
- true 처럼 평가되는 값을 truthy 값
    
    : `falsy` 값을 제외한 값들
    

⇒ `falsy` 값과 `truthy` 값을 명확하게 확인하고 싶다면 `Boolean` 함수를 사용해서 직접 boolean 타입으로 형 변환을 해볼 수 있습니다.

```jsx
// falsy
Boolean(false);
Boolean(null);
Boolean(undefined);
Boolean(0);
Boolean(NaN);
Boolean('');

// truthy
Boolean(true);
Boolean('codeit');
Boolean(123);
Boolean(-123);
Boolean({});
Boolean([]);

```

## 논리 연산자

- 자바스크립트에서 `AND` 와 `OR` 연산자는 무조건 불린 값을 리턴하는게 아니라,

 ⇒ **왼쪽 피연산자 값의 유형에 따라**서 두 피연산자 중 하나를 리턴하는 방식으로 동작합니다.

- `AND` 연산자

⇒ 왼쪽 피연산자가 falsy 값일 때 왼쪽 피연산자를, 
    **왼쪽 피연산자**가 truthy 값일 때 오른쪽 피연산자를 리턴

- `OR` 연산자

⇒ **왼쪽 피연산자**가 falsy일 때 오른쪽 피연산자를,
    **왼쪽 피연산자**가 truthy일 때 왼쪽 피연산자를 리턴

```jsx
console.log(null && undefined); // null
console.log(0 || true); // true
console.log('0' && NaN); // NaN
console.log({} || 123); // {}

```

# 자바스크립트의 다양한 변수 선언 방식

- 자바스크립트가 처음 등장할 때부터 사용되던 `var` 와,
- `var` 의 부족함을 채우기위해 ES2015에서 새롭게 등장한 `let` 과 `const` 가 있습니다.

### `var`

1. 변수 이름 중복 선언 가능

2. 변수 선언 전에 가능(호이스팅)

3. 함수 스코프

: 중복된 이름으로 선언이 가능했던 특징은 여러 사람이 협업할 때 자주 문제가 생깁니다.

⇒ 이런 문제를 개선하기 위해 ES2015에서 `let` 과 `const` 가 등장했습니다.

### `let` , `const`

1. 변수 이름 중복 선언 불가 (SyntaxError 발생)
2. 변수 선언 전에 사용 불가 (ReferenceError 발생)
3. 블록 스코프

덧붙여 `const` 키워드는 `let` 키워드와 다르게 값을 재할당할 수 없다는 특징도 있습니다.

## 함수 스코프(function scope)와 블록 스코프(block scope)

- `var` 키워드로 선언한 변수는 함수 스코프
- `let` 과 `const` 키워드로 선언한 변수는 블록 스코프를 가집니다.
- 함수 스코프 : 함수를 기준으로 스코프를 구분한다

 ⇒ 함수 안에서 선언한 변수는 함수 안에서만 유효합니다.

```jsx
function sayHi() {
  var userName = 'codeit';
  console.log(`Hi ${userName}!`);
}

console.log(userName); // ReferenceError

```

- 하지만 함수를 제외한 for, if, while 등과 같은 문법 안에서 선언한 변수는 그 문법 밖에서도 계속 유효 했었기 때문에 중복선언등의 문제가 생길 수도 있습니다.

  ⇒ 이런 문제를 해결하기 위해 `let` 과 `const` 키워드와 함께 블록 스코프가 등장했습니다.

```jsx
for (var i = 0; i < 5; i++) {
  console.log(i);
}

console.log(i); // 5
```

- 블록 스코프는 중괄호로 감싸진 코드 블록에 따라 유효 범위를 구분하게 됩니다.

```jsx
function sayHi() {
  const userName = 'codeit';
  console.log(`Hi ${userName}!`);
}

for (let i = 0; i < 5; i++) {
  console.log(i);
}

{
  let language = 'JavaScript';
}

console.log(userName); // ReferenceError
console.log(i); // ReferenceError
console.log(language); // ReferenceError

```

<br><br>


# 함수 다루기

## 함수 선언

- 자바스크립트에서 함수는 다양한 방식으로 선언할 수 있습니다.
- 가장 일반적인 방법은 `function` 키워드를 통해 함수를 선언하는 방식입니다.

```jsx
// 함수 선언
function sayHi() {
  console.log('Hi!');
}
```

⇒ 이렇게 작성하는 방식을 **함수 선언 (function declaration)** 이라고 합니다.

## 함수 표현식

- 자바스크립트에서 함수는 값으로 취급될 수도 있기 때문에 변수에 할당해서 함수를 선언할 수도 있습니다.

```jsx
// 함수 표현식
const sayHi = function () {
  console.log('Hi!');
}
```

⇒ 이렇게 함수를 값으로 다루는 방식을 함수 표현식 (function expression) 이라고 합니다.

## 다양한 함수의 형태

- 자바스크립트에서 함수는 값으로 취급됩니다

 ⇒ 코드를 작성할 때 다양한 형태로 활용할 수 있습니다.

```jsx
// 변수에 할당해서 활용
const printJS = function () {
  console.log('JavaScript');
};

// 객체의 메소드로 활용
const codeit = {
  printTitle: function () {
    console.log('Codeit');
  }
}

// 콜백 함수로 활용
myBtn.addEventListener('click', function () {
  console.log('button is clicked!');
});

// 고차 함수로 활용
function myFunction() {
  return function () {
    console.log('Hi!?');
  };
};
```

## 파라미터의 기본값

- 자바스크립트에서 함수의 파라미터는 기본값을 가질 수 있습니다.
- 기본값이 있는 파라미터는 함수를 호출할 때 아규먼트를 전달하지 않으면,  함수 내부의 동작은 이. ㅏ라미터의 기본값을 가지고 동작하게 됩니다.

```jsx
function sayHi(name = 'Codeit') {
  console.log(`Hi! ${name}`);
}

sayHi('JavaScript'); // Hi! JavaScript
sayHi(); // Hi! Codeit
```

## arguments 객체

- 자바스크립트 함수 안에는 `argument` 라는 독특한 객체가 존재합니다.
- `argument` 객체는 함수를 호출할 때 전달한 아규먼트들을 배열의 형태로 모아둔 유사 배열 객체입니다.
- 함수를 호출할 때 전달되는 아규먼트의 개수가 불규칙적일 때 유용하게 활용될 수 있습니다.

```jsx
function printArguments() {
  // arguments 객체의 요소들을 하나씩 출력
  for (const arg of arguments) {
    console.log(arg); 
  }
}

printArguments('Young', 'Mark', 'Koby');
```

## Rest Parameter

- `argument` 객체를 이용하는 것 말고도 불규칙적으로 전달되는 아규먼트를 다루는 방법이 있습니다.
- 파라미터 앞에 마침표 세 개를 붙여주면, 여러 개로 전달되는 아규먼트들을 배열로 다룰 수가 있게 됩니다.
- `arguments` 객체는 유사 배열이기 때문에 배열의 메소드를 활용할 수 없는 반면,
- `rest parameter` 는 배열이기 때문에 배열의 메소드를 자유롭게 사용할 수 있다는 장점이 있습니다.

```jsx
function printArguments(...args) {
  // args 객체의 요소들을 하나씩 출력
  for (const arg of args) {
    console.log(arg); 
  }
}

printArguments('Young', 'Mark', 'Koby');
```

- `rest parameter` 는 다른 일반 파라미터들과 함께 사용될 수 있습니다.

```jsx
function printRankingList(first, second, **...others**) {
  console.log('코드잇 레이스 최종 결과');
  console.log(`우승: ${first}`);
  console.log(`준우승: ${second}`);
  for (const arg of others) {
    console.log(`참가자: ${arg}`);
  }
}

printRankingList('Tommy', 'Jerry', 'Suri', 'Sunny', 'Jack');
```

- 이름 그대로 앞에 정의된 이름 그대로 앞에 정의된 파라미터에 `argument` 를 먼저 할당하고 나머지 `argument` 를 배열로 묶는 역할을 합니다.
- ⇒ 일반 파라미터와 함께 사용할 때는 반드시 가장 마지막에 작성해야 합니다.

## Arrow Function

- arrow function은 익명 함수를 좀 더 간결하게 표현할 수 있도록 ES2015에서 새롭게 등장한 함수 선언 방식입니다.
- 아래 코드와 같이 표현식으로 함수를 정의할 때 활용될 수도 있고 콜백 함수로 전달할 때 활용할 수도 있습니다.

```jsx
// 화살표 함수 정의
const getTwice = (number) => {
  return number * 2;
};

// 콜백 함수로 활용
myBtn.addEventListener('click', () => {
  console.log('button is clicked!');
});
```

- 화살표 함수는 다양한 상황에 따라 축약형으로 작성될 수 있습니다.

```jsx
// 1. 함수의 파라미터가 하나 뿐일 때
const getTwice = (number) => {
  return number * 2;
};

// 파라미터를 감싸는 소괄호 생략 가능
const getTwice = number => {
  return number * 2;
};

// 2. 함수 동작 부분이 return문만 있을 때
const sum = (a, b) => {
  return a + b;
};

// return문과 중괄호 생략 가능
const sum = (a, b) => a + b;
```

Arrow function이 일반함수와 몇 가지 차이점이 있습니다.

⇒ 가장 대표적인 차이점은 `arguments` 객체가 없고, this가 가리키는 값이 일반 함수와 다르다는 점입니다.

## this

- 웹 브라우저에서 this가 사용될 때는 전역 객체, Window 객체를 가지게 됩니다.
- 객체의 메소드를 정의하기 위한 함수 안에선 **메소드를 호출한 객체**를 가리키게 됩니다.

```jsx
const user = {
  firstName: 'Tess',
  lastName: 'Jang',
  getFullName: function () {
    return `${this.firstName} ${this.lastName}`;
  },
};

console.log(user.getFullName()); // getFullName 안에서의 this는 getFullName을 호출한 user객체가 담긴다!

```

<br><br>


# 이름이 있는 함수 표현식

## Named Function Expression (기명 함수 표현식)

- 함수 표현식으로 함수를 만들 때는 선언하는 함수에 이름을 붙여줄 수 있습니다.
- 이름이 있는 함수 표현식, 즉 **기명 함수 표현식**이라고 부릅니다.
- 함수 표현식으로 함수가 할당된 변수에는 자동으로 name이라는 프로퍼티를 가지게 됩니다.

```jsx
const sayHi = function () {
  console.log('Hi');
};

console.log(sayHi.name); // sayHi
```

- 이렇게 이름이 없는 함수를 변수에 할당할 때는 변수의 name 프로퍼티는 변수 이름 그 자체를 문자열로 가지게 됩니다.
- 하지만 함수에 이름을 붙여주게 되면, name 속성은 함수 이름을 문자열로 갖게 됩니다.

```jsx
const sayHi = function printHiInConsole() {
  console.log('Hi');
};

console.log(sayHi.name); // printHiInConsole
```

- 이 함수 이름은 함수 내부에서 함수 자체를 가리킬 때 사용할 수 있고 함수를 외부에서 함수를 호출할 때 사용할 수 없습니다.

```jsx
const sayHi = function printHiInConsole() {
  console.log('Hi');
};

printHiInConsole(); // ReferenceError
```

- 기명 함수 표현식은 일반적으로 함수 내부에서 함수 자체를 가리킬 때 사용됩니다.

```jsx
let countdown = function(n) {
  console.log(n);

  if (n === 0) {
    console.log('End!');
  } else {
    countdown(n - 1);
  }
};

countdown(5);
```

<br><br>


# 즉시 실행 함수 (IIFE)

### 함수 선언 ≠ 함수 실행

```jsx
function sayHi() {
  console.log('Hi!');
}
  
sayHi();
```

- 일반적으로는 이렇게 함수를 먼저 선언한 다음, 선언된 함수 이름 뒤에 소괄호를 붙여서 함수를 실행합니다.
- 그런데 때로는 함수가 선언된 순간에 바로 실행을 할 수도 있습니다.

## 즉시 실행 함수

```jsx
(function () {
  console.log('Hi!');
})();
```

- 함수선언 부분을 소괄호로 감싼 다음에 바로 뒤에 함수를 실행하는 소괄호를 한번 더 붙여주는 방식입니다.

 ⇒  함수가 선언된 순간 바로 실행이 됩니다.

- 이렇게 함수 선언과 동시에 즉시 실행되는 함수를 가리켜 **즉시 실행 함수 (표현)** 이라고 부릅니다.
- 영어로는 **Immediately Invoked Function Expression**, 줄여서 **IIFE**라고 부릅니다.

```jsx
(function (x, y) {
  console.log(x + y);
})(3, 5);
```

- 그리고 즉시 실행 함수도 일반 함수처럼 파라미터를 작성하고, 함수를 호출할 때 아규먼트를 전달할 수도 있습니다.
- 한 가지 주의할 점은 즉시 실행 함수는 함수에 이름을 지어주더라도 외부에서 재사용할 수 없습니다.

```jsx
(function sayHi() {
  console.log('Hi!');
})();

sayHi(); // ReferenceError
```

- 그래서 일반적으로는 이름이 없는 익명 함수를 사용합니다.
- 다만, 함수 내부에서 자기 자신을 호출하는 재귀적인 구조를 만들고자 할 땐 이름이 필요할 수도 있습니다.

```jsx
(function countdown(n) {
  console.log(n);
  if (n === 0) {
    console.log('End!');
  } else {
    countdown(n - 1);
  }
})(5);
```

## 즉시 실행 함수의 활용

- 즉시 실행 함수는 말 그대로 선언과 동시에 실행이 이뤄지기 때문에 일반적으로 프로그램 **초기화 기능**에 많이 활용됩니다.

```jsx
(function init() {
  // 프로그램이 실행 될 때 기본적으로 동작할 코드들..
})();
```

- 혹은 재사용이 필요 없는, 일회성 동작을 구성할 때 활용하기도 합니다.

```jsx
const firstName = 'Young';
const lastName = 'Kang';

const greetingMessage = (function () {
  const fullName = `${firstName} ${lastName} `;

  return `Hi! My name is ${fullName}`;
})();
```

이렇게 함수의 리턴값을 바로 변수에 할당하고 싶을 때 활용할 수 있습니다.


<br><br>

# 자바스크립트의 문법과 표현

## 조건부 연산자 (Conditional operator)

- 삼항 연산자 (Ternary operator)라고도 불리는 이 연산자는 자바스크립트에서 세 개의 피연산자를 가지는 유일한 연산자 입니다.
- `if` 문과 같은 원리로 조건에 따라 값을 결정할 때 활용됩니다.

```jsx
const cutOff = 80;

const passChecker = (score) => score > cutOff ? '합격입니다!' : '불합격입니다!';

console.log(passChecker(75));
```

- 간단한 조건식의 경우에는 `if` 문 보다 훨씬 더 간결하게 표현할 수 있는 장점이 있지만 내부에 변수나 함수를 선언한다거나 반복문 같은 표현식이 아닌 문장은 작성할 수 없다는 한계가 존재합니다.

## Spread 구문

- 여러개의 값을 묶어놓은 배열이나 객체와 같은 값은 바로 앞에 마침표 세 개를 붙여서 펼칠 수가 있습니다.

```jsx
const webPublishing = ['HTML', 'CSS'];
const interactiveWeb = [...webPublishing, 'JavaScript'];

console.log(webPublishing);
console.log(interactiveWeb);

const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const arr3 = [...arr1, ...arr2];
console.log(arr3);
```

- `spread` 구문은 배열이나 객체를 복사하거나 혹은 복사해서 새로운 요소들을 추가할 때 유용하게 활용 될 수 있습니다.
- 참고로 배열은 객체로 펼칠 수 있지만 객체는 배열로 펼칠 수 없습니다.

```jsx
const members = ['태호', '종훈', '우재'];
const newObject = { ...members };

console.log(newObject); // {0: "태호", 1: "종훈", 2: "우재"}

const topic = {
  name: '모던 자바스크립트',
  language: 'JavaScript', 
}
const newArray = [...topic]; // TypeError!
```

## 모던한 프로퍼티 표기법

- ES2015 이후부터는 자바스크립트에서 변수나 함수를 활용해서 프로퍼티를 만들 때 프로퍼티 네임과 변수나 함수 이름이 같다면 다음과 같이 축약해서 사용할 수 있습니다.

```jsx
function sayHi() {
  console.log('Hi!');
}

const title = 'codeit';
const birth = 2017;
const job = '프로그래밍 강사';

const user = {
  title, 
  birth, 
  job, 
  sayHi,
};

console.log(user); // {title: "codeit", birth: 2017, job: "프로그래밍 강사", sayHi: ƒ}
```

- 그리고 메소드를 작성할 때도 다음과 같이 `function` 키워드를 생략할 수가 있습니다.

```jsx
const user = {
  firstName: 'Tess',
  lastName: 'Jang',
  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  },
};

console.log(user.getFullName()); // Tess Jang
```

- 뿐만 아니라 아래 코드와 같이 대괄호를 활용하면 다양한 표현식으로 프로퍼티 네임을 작성할 수 있습니다.

```jsx
const propertyName = 'birth';
const getJob = () => 'job';

const codeit = {
  ['topic' + 'name']: 'Modern JavaScript',
  [propertyName]: 2017,
  [getJob()]: '프로그래밍 강사',
};

console.log(codeit);
```

## 구조 분해 Destructuring

- 배열과 객체와 같이 내부에 여러 값을 담고 있는 데이터 타입을 다룰 때 Destructuring 문법을 활용하면,

  ⇒  배열의 요소나 객체의 프로퍼티 값을 개별적인 변수에 따로 따로 할당해서 다룰 수가 있습니다.

```jsx
// Array Destructuring
const members = ['코딩하는효준', '글쓰는유나', '편집하는민환'];
const [macbook, ipad, coupon] = members;

console.log(macbook); // 코딩하는효준
console.log(ipad); // 글쓰는유나
console.log(coupon); // 편집하는민환

// Object Destructuring
const macbookPro = {
  title: '맥북 프로 16형',
  price: 3690000,
};

const { title, price } = macbookPro;

console.log(title); // 맥북 프로 16형
console.log(price); // 3690000
```

- 함수에서 default parater, rest paraameter를 다루듯이 Destructuring 문법을 활용할 때도 기본값과 rest 문법을 활용할 수 있습니다.

```jsx
// Array Destructuring
const members = ['코딩하는효준', '글쓰는유나', undefined, '편집하는민환', '촬영하는재하'];
const [macbook, ipad, airpod = '녹음하는규식', ...coupon] = members;

console.log(macbook); // 코딩하는효준
console.log(ipad); // 글쓰는유나
console.log(airpod); // 녹음하는규식
console.log(coupon); // (2) ["편집하는민환", "촬영하는재하"]

// Object Destructuring
const macbookPro = {
  title: '맥북 프로 16형',
  price: 3690000,
  memory: '16 GB 2667 MHz DDR4',
  storage: '1TB SSD 저장 장치',
};

const { title, price, color = 'silver', ...rest } = macbookPro;

console.log(title); // 맥북 프로 16형
console.log(price); // 3690000
console.log(color); // silver
console.log(rest); // {memory: "16 GB 2667 MHz DDR4", storage: "1TB SSD 저장 장치"}
```

## 에러와 에러 객체

- 자바스크립트에서 에러가 발생하면 그 순간 프로그램 자체가 멈춰버리고 이후의 코드가 동작하지 않습니다.
- 그리고 에러가 발생하면 에러에 대한 정보를 `name` 과 `message` 라는 프로퍼티로 담고 있는 **에러 객체**가 만들어집니다.

 ⇒ 대표적인 에러 객체는 SyntaxError, ReferenceError, TypeError 입니다.

- 에러 객체는 직접 만들 수도 있습니다.

 ⇒ `new` 키워드와 에러 객체 이름을 딴 함수를 통해 에러 객체를 만들 수 있고, `throw` 키워드로 에러를 발생시킬 수 있습니다.

```jsx
throw new TypeError('타입 에러가 발생했습니다.');

```

## try…catch문

- `try...catch` 문은 자바스크립트에서 대표적인 에러 처리 방법입니다.

```jsx
try {
  // 실행할 코드
} catch (error) {
  // 에러 발생 시 동작할 코드
}
```

- `try` 문 안에서 실행할 코드를 작성하고, `try` 문에서 에러가 발생한 경우에 실행할 코드를 `catch` 문 안에 작성하면 됩니다.
- 이 때 `try` 문에서 발생한 에러 객체가 `catch` 문의 첫 번째 파라미터로 전달됩니다.
- 만약, `try` 문에서 에러가 발생하지 않을 경우 `catch` 문의 코드는 동작하지 않습니다.
- 그리고, `try...catch` 문에서 에러의 유무와 상관없이 항상 동작해야할 코드가 필요하다면 `finally` 문을 활용할 수 있습니다.

```jsx
try {
  // 실행할 코드
} catch (error) {
  // 에러가 발상했을 때 실행할 코드
} finally {
  // 항상 실행할 코드
}
```

## finally문

```jsx
try {
  // 실행할 코드
} catch (err) {
  // 에러가 발생했을 때 실행할 코드
} finally {
  // 항상 실행할 코드
}
```

- `try` 문에서 에러가 발생하지 않는다면 `try` 문의 코드가 모두 실행된 다음에,
- `try` 문에서 에러가 발생한다면 `catch` 문의 코드가 모두 실행된 다음 실행할 코드를 `finally` 문에 작성하면 됩니다.

⇒ 다시 말해 **try문에서 어떤 코드를 실행할 때 에러 여부와 상관 없이 항상 실행할 코드를 작성**하는 것입니다.

```jsx
function printMembers(...members) {
  for (const member of members) {
    console.log(member);
  }
}

try {
  printMembers('영훈', '윤수', '동욱');
} catch (err) {
  alert('에러가 발생했습니다!');
  console.error(err);
} finally {
  const end = new Date();
  const msg = `코드 실행을 완료한 시각은 ${end.toLocaleString()}입니다.`;
  console.log(msg);
}
```

- 위 코드에서도 에러 유무와 관계없이 코드 실행 시각을 알 수 있습니다.

## finally문에서의 에러처리

- `finally` 문에서 에러가 발생한 경우에는 다시 그 위에 있는 `catch`  문으로 넘어가진 않습니다.
- 만약 `finally` 문에서도 에러 처리가 필요한 경우에는 아래 처럼 `try...catch` 문을 중첩해서 활용하는 방법이 있습니다.

```jsx
try {
  try {
    // 실행할 코드
  } catch (err) {
    // 에러가 발생했을 때 실행할 코드
  } finally {
    // 항상 실행할 코드
  }
} catch (err) {
  // finally문에서 에러가 발생했을 때 실행할 코드
}
```

<br><br>

# 문장과 표현식

## 문장 (Statements)

- 우리가 작성하는 모든 자바스크립트 코드는 모두 문장과 표현식으로 구성되어 있습니다.
- 문장은 **어떤 동작이 일어나도록 작성된 최소한의 코드 덩어리**를 가리킵니다.

```jsx
let x; 
x = 3;

if (x < 5) {
  console.log('x는 5보다 작다');
} else {
  console.log('x는 5와 같거나 크다');
}

for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

- 이 코드의 첫 번째 줄도 `x` 라는 변수를 선언하는 동작이 일어나는 하나의 문장이고,
- 두 번째 줄도 `x` 에 3이라는 값을 할당하는 동작이 일어나는 하나의 문장입니다.
- 그리고 4번줄 부터 8번줄 까지도 하나의 문장입니다.
- 그리고 10번줄 부터 12번줄 까지도 반복 동작을 하는 문장의 예시라고 볼 수 있습니다.

 ⇒ 선언문, 할당문, 조건문, 반복문 .. 이렇게 끝에 문이라고 붙은 이유가 모두 **동작을 수행하는 문장**이기 때문입니다.

## 표현식 (expressions)

- 표현식은 **결과적으로 하나의 값이 되는 모든 코드**를 가리킵니다.

```jsx
5 // 5

'string' // string
```

- 어떤 하나의 값을 그대로 작성하는 것도 표현식이지만,

```jsx
5 + 7 // 12

'I' + ' Love ' + 'Codeit' // I Love Codeit

true && null // null
```

- 이렇게 연산자를 이용한 연산식도 결국 하나의 값이 됩니다.

```jsx
const title = 'JavaScript';
const codeit = {
  name: 'Codeit'
};
const numbers = [1, 2, 3];

typeof codeit // object
title // JavaScript
codeit.name // Codeit
numbers[3] // undefined
```

- 위 코드의 마지막 네 줄처럼 선언된 변수를 호출하거나, 객체의 프로퍼티에 접근하는 것도 결국에는 하나의 값으로 평가됩니다.

⇒ 길이와 상관없이 결과적으로 하나의 값이 되는 코드를 모두 **표현식**이라고 할 수 있습니다.


<br><br>

## 객체 Spread 하기

```jsx
const codeit = { 
  name: 'codeit', 
};

const codeitClone = { 
  ...codeit, // spread 문법!
};

console.log(codeit); // {name: "codeit"}
console.log(codeitClone); // {name: "codeit"}
```

- 이렇게 중괄호 안에서 객체를 spread 하게 되면, 해당 객체의 프로퍼티들이 펼쳐지면서 객체를 복사할 수 있습니다.

```jsx
const latte = {
  esspresso: '30ml',
  milk: '150ml'
};

const cafeMocha = {
  ...latte,
  chocolate: '20ml',
}

console.log(latte); // {esspresso: "30ml", milk: "150ml"}
console.log(cafeMocha); // {esspresso: "30ml", milk: "150ml", chocolate: "20ml"}
```

- 다른 객체가 가진 프로퍼티에 다른 프로퍼티를 추가해서 새로운 객체를 만들 수도 있습니다.

## 주의 사항

- 배열을 Spread 하면 새로운 배열을 만들거나 함수의 아규먼트로 쓸 수 있었지만, 객체로는 새로운 배열을 만들거나 함수의 아규먼트로 사용할 수는 없습니다.

```jsx
const latte = {
  esspresso: '30ml',
  milk: '150ml'
};

const cafeMocha = {
  ...latte,
  chocolate: '20ml',
}

[...latte]; // Error

(function (...args) {
  for (const arg of args) {
    console.log(arg);
  }
})(...cafeMocha); // Error
```

⇒ 객체를 spread 할 때는 **반드시 객체를 표현하는 중괄호 안에서 활용**해야 합니다.


<br><br>


# 자바스크립트의 유용한 내부 기능

## forEach 메소드

- 배열의 요소를 하나씩 살펴보면서 반복 작업을 하는 메소드입니다.
- `forEach` 메소드는 첫 번째 아규먼트로 콜백함수를 전달받습니다.
- 콜백 함수의 파라미터에는 가각 배열의 요소, index, 메소드를 호출한 배열이 전달됩니다. (index와 array는 생략 가능)

```jsx
const numbers = [1, 2, 3];

numbers.forEach((element, index, array) => {
  console.log(element); // 순서대로 콘솔에 1, 2, 3이 한 줄씩 출력됨.
});
```

## map 메소드

- `forEach` 와 비슷하게 배열의 요소를 하나씩 살펴보면서 반복 작업을 하는 메소드입니다.
- 단, 첫 번째 아규먼트로 전달하는 콜백 함수가 매번 리턴하는 값들을 모아서 새로운 배열을 만들어 리턴하는 특징이 있습니다.

```jsx
const numbers = [1, 2, 3];
const twiceNumbers = numbers.map((element, index, array) => {
  return element * 2;
});

console.log(twiceNumbers); // (3) [2, 4, 6]
```

## filter 메소드

- `filter` 메소드는 배열의 요소를 하나씩 살펴보면서 콜백함수가 리턴하는 조건과 일치하는 요소만 모아서 새로운 배열을 리턴하는 메소드입니다.

```jsx
const devices = [
  {name: 'GalaxyNote', brand: 'Samsung'},
  {name: 'MacbookPro', brand: 'Apple'},
  {name: 'Gram', brand: 'LG'},
  {name: 'SurfacePro', brand: 'Microsoft'},
  {name: 'ZenBook', brand: 'Asus'},
  {name: 'MacbookAir', brand: 'Apple'},
];

const apples = devices.filter((element, index, array) => {
  return element.brand === 'Apple';
});

console.log(apples); // (2) [{name: "MacbookPro", brand: "Apple"}, {name: "MacbookAir", brand: "Apple"}]
```

## find 메소드

- `find` 메소드는 `filter` 메소드와 비슷하게 동작하지만, 배열의 요소들을 반복하는 중에 콜백함수가 리턴하는 조건과 일치하는 가장 첫번째 요소를 리턴하고 반복을 종료하는 메소드입니다.

```jsx
const devices = [
  {name: 'GalaxyNote', brand: 'Samsung'},
  {name: 'MacbookPro', brand: 'Apple'},
  {name: 'Gram', brand: 'LG'},
  {name: 'SurfacePro', brand: 'Microsoft'},
  {name: 'ZenBook', brand: 'Asus'},
  {name: 'MacbookAir', brand: 'Apple'},
];

const myLaptop = devices.find((element, index, array) => {
  console.log(index); // 콘솔에는 0, 1, 2까지만 출력됨.
  return element.name === 'Gram';
});

console.log(myLaptop); // {name: "Gram", brand: "LG"}
```

## some 메소드

- `some` 메소드는 배열 안에 콜백함수가 리턴하는 조건을 만족하는 요소가 1개 이상 있는지를 확인하는 메소드 입니다.
- 배열을 반복하면서 모든 요소가 콜백함수가 리턴하는 조건을 만족하지 않는다면 `false` 를 리턴하고,
- 배열을 반복하면서 콜백함수가 리턴하는 조건을 만족하는 요소가 등장한다면 바로 `true` 를 리턴하고 반복을 종료합니다.

```jsx
const numbers = [1, 3, 5, 7, 9];

// some: 조건을 만족하는 요소가 1개 이상 있는지
const someReturn = numbers.some((element, index, array) => {
  console.log(index); // 콘솔에는 0, 1, 2, 3까지만 출력됨.
  return element > 5;
});

console.log(someReturn); // true;
```

## every 메소드

- `every` 메소드는 배열 안에 콜백 함수가 리턴하는 조건을 만족하지 않는 요소가 1개 이상 있는지를 확인하는 메소드 입니다.
- 배열을 반복하면서 모든 요소가 콜백함수가 리턴하는 조건을 만족한다면 `true` 를 리턴하고,
- 배열을 반복하면서 콜백함수가 리턴하는 조건을 만족하지 않는 요소가 등장한다면 바로 `false` 를 리턴하고 반복을 종료합니다.

```jsx
const numbers = [1, 3, 5, 7, 9];

// every: 조건을 만족하지 않는 요소가 1개 이상 있는지
const everyReturn = numbers.every((element, index, array) => {
  console.log(index); // 콘솔에는 0까지만 출력됨.
  return element > 5;
});

console.log(everyReturn); // false;
```

## reduce 메소드

- `reduce` 메소드는 누적값을 계산할 때 활용하는 조금 독특한 메소드 입니다.
- `reduce` 메소드는 일반적으로 두 개의 파라미터를 활용합니다
- 첫 번째는 반복동작할 콜백함수입니다.
- **매번 실행되는 콜백함수의 리턴값이 다음에 동작할 콜백함수의 첫번째 파라미터로 전달됩니다.**
- 결과적으로 **마지막 콜백함수가 리턴하는 값이 `reduce` 메소드의 최종 리턴값이 됩니다.**

  ⇒ `reduce` 메소드의 두 번째 파라미터로 전달한 초기값이 첫 번째로 실행될 콜백함수의 가장 첫 번째 파라미터로 전달됩니다.

```jsx
const numbers = [1, 2, 3, 4];

// reduce
const sumAll = numbers.reduce((accumulator, element, index, array) => {
  return accumulator + element;
}, 0);

console.log(sumAll); // 10
```

## sort 메소드

- 배열에서 `sort` 라는 메소드를 활용하면 배열을 정렬할 수 있습니다.
- `sort` 메소드에 아무런 아규먼트도 전달하지 않을 때는 기본적으로 유니코드에 정의된 문자열 순서에 따라 정렬됩니다.

```jsx
const letters = ['D', 'C', 'E', 'B', 'A'];
const numbers = [1, 10, 4, 21, 36000];

letters.sort();
numbers.sort();

console.log(letters); // (5) ["A", "B", "C", "D", "E"]
console.log(numbers); // (5) [1, 10, 21, 36000, 4]
```

⇒ `numbers` 에 `sort` 메소드를 사용한 것처럼, 숫자를 정렬할 때는 우리가 상식적으로 이해하는 오름차순이나 내림차순 정렬이 되지 않습니다.

⇒ `sort` 메소드에 다음과 같은 콜백함수를 아규먼트로 작성해주면 됩니다.

```jsx
const numbers = [1, 10, 4, 21, 36000];

// 오름차순 정렬
numbers.sort((a, b) => a - b);
console.log(numbers); // (5) [1, 4, 10, 21, 36000]

// 내림차순 정렬
numbers.sort((a, b) => b - a);
console.log(numbers); // (5) [36000, 21, 10, 4, 1]
```

- `sort` 메소드를 사용할 때 한 가지 주의해야 될 부분은 `메소드를 실행하는 원본 배열의 요소들을 정렬` 한다는 점입니다.
- 한 번 정렬하고 나면 정렬하기전에 순서로 다시 되돌릴 수 없으니까, 원본 배열의 순서가 필요하다면 미리 다른 변수에 복사해두는 것이 좋습니다.

## reverse 메소드

- `reverse` 메소드는 말 그대로 배열의 순서를 뒤집어 주는 메소드 입니다.
- `reverse` 메소드는 별도의 파라미터가 존재하지 않기 때문에 단순히 메소드를 호출해주기만 하면 배열의 순서가 뒤집힙니다.
- **`sort` 메소드와 마찬가지로 원본 배열의 요소들을 뒤집어 버립니다.**

```jsx
const letters = ['a', 'c', 'b'];
const numbers = [421, 721, 353];

letters.reverse();
numbers.reverse();

console.log(letters); // (3) ["b", "c", "a"]
console.log(numbers); // (3) [353, 721, 421]
```

## Map

- `Map` 은 이름이 있는 데이터를 저장한다는 점에서 **객체와 비슷**합니다.
- 하지만, 할당연산자를 통해 값을 추가하고 점 표기법이나 대괄호 표기법으로 접근하는 일반 객체와 다르게 `Map` 은 **메소드를 통해서 값을 추가하거나 접근**할 수 있습니다.
- `new` 키워드를 통해서 `Map` 을 만들 수 있고 아래와 같은 메소드를 통해 `Map` 안의 여러 값들을 다룰 수 있습니다.

- map.set(key, value): key를 통해 value를 **추가**하는 메소드
- map.get(key): key에 해당하는 **값을 얻**는 메소드. key가 존재하지 않으면 undefined를 반환
- map.has(key): key가 존재하면 true, 존재하지 않으면 false를 반환하는 메소드
- map.delete(key): key에 **해당하는 값**을 삭제하는 메소드
- map.clear(): Map 안의 **모든 요소**를 제거하는 메소드
- map.size: 요소의 개수를 반환하는 프로퍼티. (메소드가 아닌 점 주의! 배열의 length 프로퍼티와 같은 역할)

```jsx
// Map 생성
const codeit = new Map();

// set 메소드
codeit.set('title', '문자열 key');
codeit.set(2017, '숫자형 key');
codeit.set(true, '불린형 key');

// get 메소드
console.log(codeit.get(2017)); // 숫자형 key
console.log(codeit.get(true)); // 불린형 key
console.log(codeit.get('title')); // 문자열 key

// has 메소드
console.log(codeit.has('title')); // true
console.log(codeit.has('name')); // false

// size 프로퍼티
console.log(codeit.size); // 3

// delete 메소드
codeit.delete(true);
console.log(codeit.get(true)); // undefined
console.log(codeit.size); // 2

// clear 메소드
codeit.clear();
console.log(codeit.get(2017)); // undefined
console.log(codeit.size); // 0
```

- 문자열과 심볼 값만 key(프로퍼티 네임)로 사용할 수 있는 일반 객체와는 다르게 Map 객체는 메소드를 통해 값을 다루기 때문에,
- 다양한 자료형을 key로 활용할 수 있다는 장점이 있습니다.

## Set

- `Set` 은 여러 개의 값을 순서대로 저장한다는 점에서 배열과 비슷합니다.
- 하지만, 배열의 메소드는 활용할 수 없고 `Map` 과 비슷하게 `Set` 만의 메소드를 통해서 값을 다루는 특징이있습니다.
- `Map` 과 마찬가지로 `new` 키워드로 `Set` 으로 만들 수 있고 아래와 같은 메소드를 통해 `Set` 안의 여러 값들을 다룰 수 있습니다.

- set.add(value): 값을 추가하는 메소드. (메소드를 호출한 자리에는 추가된 값을 가진 `Set` 자신을 반환)
- set.has(value): `Set` 안에 값이 존재하면 true, 아니면 false를 반환하는 메소드.
- set.delete(value): 값을 제거하는 메소드. (메소드를 호출한 자리에는 셋 내에 값이 있어서 제거에 성공하면 true, 아니면 false를 반환.)
- set.clear(): `Set` 안의 모든 요소를 제거하는 메소드
- set.size: 요소의 개수를 반환하는 프로퍼티. (메소드가 이닌 점 주의! 배열의 length 프로퍼티와 같은 역할)

```jsx
// Set 생성
const members = new Set();

// add 메소드
members.add('영훈'); // Set(1) {"영훈"}
members.add('윤수'); // Set(2) {"영훈", "윤수"}
members.add('동욱'); // Set(3) {"영훈", "윤수", "동욱"}
members.add('태호'); // Set(4) {"영훈", "윤수", "동욱", "태호"}

// has 메소드
console.log(members.has('동욱')); // true
console.log(members.has('현승')); // false

// size 프로퍼티
console.log(members.size); // 4

// delete 메소드
members.delete('종훈'); // false
console.log(members.size); // 4
members.delete('태호'); // true
console.log(members.size); // 3

// clear 메소드
members.clear();
console.log(members.size); // 0
```

- 한가지 특이한 점은 일반 객체는 프로퍼티 네임으로, Map은 `get` 메소드로, 그리고 배열은 `index` 를 통해서 개별값에 접근할 수 있습니다.

⇒ **Set에는 개별 값에 바로 접근하는 방법이 없습니다.**

```jsx
// Set 생성
const members = new Set();

// add 메소드
members.add('영훈'); // Set(1) {"영훈"}
members.add('윤수'); // Set(2) {"영훈", "윤수"}
members.add('동욱'); // Set(3) {"영훈", "윤수", "동욱"}
members.add('태호'); // Set(4) {"영훈", "윤수", "동욱", "태호"}

for (const member of members) {
  console.log(member); // 영훈, 윤수, 동욱, 태호가 순서대로 한 줄 씩 콘솔에 출력됨.
}
```

- 위 코드와 같이 반복문을 통해서 전체요소를 한꺼번에 다룰 때 반복되는 그 순간에 개별적으로 접근할 수 있습니다.

 ⇒ 중복을 허용하지 않는 값을 모을 때 유용하게 사용됩니다.

- `Set` 객체에 요소를 추가할 때 이미 `Set` 객체 안에 있는 값(중복된 값)을 추가하려고 하면 그 값은 무시되는 특징이 있습니다.
- 처음 `Set` 을 생성할 때 아규먼트로 배열을 전달할 수 있읍니다.

 ⇒ 이런 특징을 활용해서 배열 내에서 중복을 제거한 값들의 묶음을 만들 때 `Set` 을 활용할 수 있습니다.

```jsx
const numbers = [1, 3, 4, 3, 3, 3, 2, 1, 1, 1, 5, 5, 3, 2, 1, 4];
const uniqNumbers = new Set(numbers);

console.log(uniqNumbers); // Set(5) {1, 3, 4, 2, 5}
```

