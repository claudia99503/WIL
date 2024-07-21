# 자바스크립트 모듈

# 모듈 사용법 정리

- Node.js 환경에서 모듈 문법을 사용하려면 파일을 `.mjs` 확장자로 저장해야 합니다.
- 모듈 문법을 사용하면 파일에서 필요한 것을 내보내고 다른 파일에서 가져올 수 있습니다.

## export(내보내기)

- export는 named export 와 default export 2가지가 있습니다.
- 파일에서 **여러 함수와 변수**를 export하고 싶을 때는 `named export` 를 씁니다.
- 파일에서 **하나의 메인 함수나 변수**를 export하고 싶을 때는 `default export` 을 씁니다.

## named export

`calculator.mjs`

```jsx
export const PI = 3.14;

export function add(a, b) { 
  return a + b;
}

export function subtract(a, b) { 
  return a - b;
}

export function multiply(a, b) { 
  return a * b;
}

export function divide(a, b) { 
  return a / b;
}
```

- 내보내고 싶은 것 앞에 `export` 키워드를 사용하면 됩니다.
- 참고로 `const` 혹은 `function` 으로 선언하는 것 외에 `let` 이나 `class` 로 선언하는 것도 모두 export할 수 있습니다.

## default export

`calculator.mjs`

```jsx
const PI = 3.14;

function add(a, b) { 
  return a + b;
}

function subtract(a, b) { 
  return a - b;
}

function multiply(a, b) { 
  return a * b;
}

function divide(a, b) { 
  return a / b;
}

const calculator = {
  PI,
  add,
  subtract,
  multiply,
  divide,
}

export default calculator;
```

- `export` 뒤에 `default` 키워드를 사용하면 default export를 할 수 있습니다.
- `function` 과 `class` 는 선언과 동시에 `export default` 를 할 수 있지만
- `const` 와 `let` 은 선언 이후에 `export default` 를 해야 합니다.

```jsx
// const
const foo = 1;
export default foo;
```

```jsx
// let
let foo = 1;
export default foo;
```

```jsx
// function
export default function foo() {
}
```

```jsx
// class
export default class Foo {
}
```

- `default export` 는 **파일당 한 번만** 할 수 있습니다.

## import(가져오기)

- `named export` 를 한 것은 `named import` 로 가져옵니다.
- `default export` 를 한 것은 `default import` 로 가져옵니다.

## named import

`calculator.mjs`

```jsx
export const PI = 3.14;

export function add(a, b) { 
  return a + b;
}

export function subtract(a, b) { 
  return a - b;
}

export function multiply(a, b) { 
  return a * b;
}

export function divide(a, b) { 
  return a / b;
}
```

`main.mjs`

```jsx
import { add, subtract } from './calculator.mjs';

console.log(add(1, 2));
console.log(subtract(1, 2));
```

```jsx
3
-1
```

- `import` 키워드를 사용하고 중괄호 안에 내보낸 함수나 변수 이름을 그대로 써 줘야 합니다.
- 그리고 가져오는 파일의 경로를 써 주면 됩니다.

## default import

`calculator.mjs`

```jsx
const PI = 3.14;

function add(a, b) { 
  return a + b;
}

function subtract(a, b) { 
  return a - b;
}

function multiply(a, b) { 
  return a * b;
}

function divide(a, b) { 
  return a / b;
}

const calculator = {
  PI,
  add,
  subtract,
  multiply,
  divide,
}

export default calculator;
```

`main.mjs`

```jsx
import calculator from './calculator.mjs';

console.log(calculator.add(1, 2));
console.log(calculator.subtract(1, 2));
```

```jsx
3
-1
```

- `default import` 는 중괄호를 쓰지 않고 사용할 이름 (위 코드에서 `calculator` ) 만 써 주면 됩니다.
- 이름은 자유롭게 정할 수 있습니다.

## 이름 바꾸기

- `as` 키워드를 사용하면 named import를 할 때 이름을 바꿀 수 있습니다.
- 아래 예시는 `main.mjs` 파일에서 `add` 함수 이름을 바꿔서 `addNumbers` 로 import하는 예시입니다.

`calculator.mjs`

```jsx
export const PI = 3.14;

export function add(a, b) {
  return a + b;
}
```

`main.mjs`

```jsx
import { PI, add as addNumbers } from './calculator.mjs';

console.log(addNumbers(1, 2));
```

```jsx
3
```

## named export 한꺼번에 하기

- export를 하고 싶은 것마다 앞에 `export` 키워드를 쓰지 않고 중괄호로  묶어서 `export` 할 수도 있습니다.
- 아래 코드는 `PI`, `add` , `subtract` , `multiply` , `divide` 를 모두 named export하는 것과 같습니다.

`calculator.mjs`

```jsx
const PI = 3.14;

function add(a, b) { 
  return a + b;
}

function subtract(a, b) { 
  return a - b;
}

function multiply(a, b) { 
  return a * b;
}

function divide(a, b) { 
  return a / b;
}

export { PI, add, subtract, multiply, divide };
```

- impoort할 때는 이전과 똑같이 필요한 것을 import하면 됩니다.

`main.mjs`

```jsx
import { PI, add, subtract, multiply, divide } from './calculator.mjs';
```

- 아래처럼 export 할 때는 `as` 키워드를 사용할 수 있습니다.
- 아예 내보낼 때 이름을 바꿔 주는 겁니다.

`calculator.mjs`

```jsx
// ...

export { PI, **add as addNumbers**, subtract, multiply, divide };
```

`main.mjs`

```jsx
import { PI, **addNumbers**, subtract, multiply, divide } from './calculator.mjs';
```

- 그러면 `add` 대신 `addNumbers` 라는 이름으로 import해야 합니다.
- 참고로 `export default { ... }` 를 할 수도 있습니다.

`calculator.mjs`

```jsx
// ...

export default { PI, add, subtract, multiply, divide };
```

- 그러면 `{ ... }` 부분이 객체로 인식되고, 그 객체를 default export하게 됩니다. 아래 코드와 비슷합니다.

```jsx
// ...

const calculator = { PI, add, subtract, multiply, divide };

export default calculator;
```

## named import 한꺼번에 하기

- 별표( `*` ) 문자를 사용하면 파일에서 named export되는 모든 것을 한꺼번에 import 할 수 있습니다.

`calculator.mjs`

```jsx
const PI = 3.14;

function add(a) { 
  return a + b;
}

function subtract(a) { 
  return a - b;
}

function multiply(a) { 
  return a * b;
}

function divide(a) { 
  return a / b;
}

export { PI, add, subtract, multiply, divide };
```

`main.mjs`

```jsx
import * as calculator from './calculator.mjs';

console.log(calculator.PI);
console.log(calculator.add(1, 2));
console.log(calculator.subtract(1, 2));
console.log(calculator.multiply(1, 2));
console.log(calculator.divide(1, 2));
```

```jsx
3.14
3
-1
2
0.5
```

- 이런 방식의 import를 namespace import(네임스페이스 임포트)라고도 부릅니다.
- `as` 키워드를 사용해서 이름을 지정해 주고, 이름 뒤에 점 표기법을 이용해서 변수나 함수에 접근할 수 있습니다.

