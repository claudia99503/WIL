# React 웹 개발 시작하기

# JSX 문법

## JSX란?

- JSX는 자바스크립트의 확장 문법입니다.
- 리액트로 코드를 작성할 때 HTML 문법과 비슷한 이 JSX 문법을 활용하면 훨씬 더 편리하게 화면에 나타낼 코드를 작성할 수 있습니다.

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    <h1>안녕 리액트!</h1>
);
```

## JSX 문법

- JSX는 자바스크립트로 HTML과 같은 문법을 사용할 수 있도록 만들어주는 편리한 문법이지만, 그만큼 꼭 지켜야 할 규칙들도 있습니다.

## HTML과 다른 속성명

### 1. 속성명은 카멜 케이스로 작성하기!

- JSX 문법에서도 태그에 속성을 지정해줄 수 있습니다.
- 단, 여러 단어가 조합된 몇몇 속성들을 사용할 때는 반드시 카멜 케이스(Camel Case)로 작성해야 합니다.
- 예를 들면 `onclick` , `onblur` , `onfocus` 등과 같은 이벤트 속성이나, `tabindex` 같은 속성들이 있습니다.

⇒ **이런 속성들은 모두 `onClick` , `onBlur` , `onFocus` , `onMouseDown` , `onMouseOver` , `tabIndex` 처럼 작성해야 됩니다.**

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <button **onClick**= ... >클릭!</button>,
);
```

- 단, 예외적으로 HTML에서 비표준 속성을 다룰 때 활용하는 `data-*` 속성은 카멜 케이스(Camel Case)가 아니라 기존의 HTML 문법 그대로 작성해야 됩니다.

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <div>
    상태 변경: 
    <button className="btn" **data-status**="대기중">대기중</button>
    <button className="btn" **data-status**="진행중">진행중</button>
    <button className="btn" **data-status**="완료">완료</button>
  </div>,
);
```

### 2. 자바스크립트 예약어와 같은 속성명은 사용할 수 없다!

- JSX 문법도 결국은 자바스크립트 문법이기 때문에, `for` 나 `class` 처럼 자바스크립트의 문법에 해당하는 예약어와 똑같은 이름의 속성명은 사용할 수 없습니다.
- HTML의 `for` 의 경우에는 자바스크립트의 반복문 키워드 `for` 와 겹치기 때문에 `htmlFor` 로,
- HTML의 `class` 속성도 자바스크립트의 클래스 키워드 `class` 와 겹치기 때문에 `className` 으로 작성해야 합니다.

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <form>
    <label **htmlFor**="name">이름</label>
    <input id="name" **className**="name-input" type="text" />
  </form>,
);
```

## 반드시 하나의 요소로 감싸기 - Fragment

- JSX 문법을 활용할 때는 반드시 하나의 요소로 감싸주어야 합니다.
- 아래 코드처럼 여러 개의 요소를 작성하면 오류가 발생합니다.

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <p>안녕</p>
  <p>리액트!</p>,
);
```

- 이럴 때는 아래 코드처럼 여러 태그를 감싸는 부모 태그를 만들어 하나의 요소로 만들어 주어야 합니다.

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <div>
    <p>안녕</p>
    <p>리액트!</p>
  </div>,
);
```

- 하지만 이렇게 작성한다면 때로는 꼭 필요하지 않은 부모 태그가 작성될 수 있습니다.

⇒ `Fragment` 로 감싸주면 의미 없는 부모 태그를 만들지 않아도 여러 요소를 작성할 수 있습니다.

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Fragment>
    <p>안녕</p>
    <p>리액트!</p>
  </Fragment>,
);
```

- 참고로 `Fragment` 는 아래 코드처럼 빈 태그로 감싸는 단축 문법으로 활용할 수도 있습니다.

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <>
    <p>안녕</p>
    <p>리액트!</p>
  </>,
);
```

## 자바스크립트 표현식 넣기

- JSX 문법에서 **중괄호({})**를 활용하면 자바스크립트 표현식을 넣을 수 있습니다.

```jsx
import ReactDOM from 'react-dom/client';

const product = '맥북';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <h1>나만의 {product} 주문하기</h1>,
);
```

- 중괄호 안에서 문자열을 조합할 수도 있습니다.
- 변수에 이미지 주소를 할당해서 `img` 태그의 `src` 속성값을 전달해 줄 수도 있고, 이벤트 핸들러를 좀 더 편리하게 등록할 수도 있습니다.

```jsx
import ReactDOM from 'react-dom/client';

const **product** = 'MacBook';
const **model** = 'Air';
const **imageUrl** = 'https://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/MacBook_with_Retina_Display.png/500px-MacBook_with_Retina_Display.png'

function **handleClick**(e) {
  alert('곧 도착합니다!');
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <>
    <h1>**{product + ' ' + model}** 주문하기</h1>
    <img src=**{imageUrl}** alt="제품 사진" />
    <button onClick=**{handleClick}**>확인</button>
  </>,
);
```

- 단, JSX 문법에서 중괄호는 자바스크립트 표현식을 다룰 때 활용하기 때문에, 중괄호 안에서 `for` , `if` 문 등의 문장은 다룰 수 없습니다.
- 그런데도 만약 JSX 문법을 활용할 때 조건문이 꼭 필요하다면 조건 연산자를, 반복문이 꼭 필요하다면 배열의 반복 메소드를 활용해 볼 수 있습니다.

# 컴포넌트 문법

## 리액트 엘리먼트

- JSX 문법으로 작성한 요소는 결과적으로 자바스크립트 객체가 됩니다.

```jsx
import ReactDOM from 'react-dom/client';

const element = <h1>안녕 리액트!</h1>;
console.log(element);

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
 element,
);
```

```jsx
{$$typeof: Symbol(react.element), type: "h1", key: null, ref: null, props: {…}, …}
```

- 이런 객체를 리액트 엘리먼트라고 부릅니다.
- 이 리액트 엘리먼트를 `ReactDOM.render` 함수의 아규먼트로 전달하게 되면, 리액트가 객체 형태의 값을 해석해서 HTML 형태로 브라우저에 띄어줍니다.
- 리액트 엘리먼트는 리액트로 화면을 그려내는데 가장 기본적인 요소입니다.

## 리액트 컴포넌트

- 리액트 컴포넌트는 리액트 엘리먼트를 조금 더 자유롭게 다루기 위한 하나의 문법입니다.
- 컴포넌트를 만드는 가장 간단한 방법은 자바스크립트의 함수를 활용하는 것입니다.
- 아래 코드에서 JSX 문법으로 작성된 **하나의 요소를 리턴하는 `Hello` 함수가 바로 하나의 컴포넌트** 입니다.

```jsx
import ReactDOM from 'react-dom/client';

function **Hello**() {
  return <h1>안녕 리액트</h1>;
}

const element = (
  <>
    **<Hello />
    <Hello />
    <Hello />**
  </>
);
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
 element,
);
```

- **이렇게 컴포넌트를 작성하면, 위 코드에서 `element` 변수 안의 JSX 코드에서 볼 수 있듯 컴포넌트 함수 이름을 통해 하나의 태그처럼 활용할 수가 있습니다.**
- 이런 특성을 모듈 문법으로 활용하면 훨씬 더 독립적으로 컴포넌트 특성에 집중해서 코드를 작성할 수가 있습니다.

`Dice.js`

```jsx
import diceBlue01 from './assets/dice-blue-1.svg';

function **Dice**() {
  return <img src={diceBlue01} alt="주사위" />;
}

export default Dice;
```

`App.js`

```jsx
import Dice from './Dice';

function App() {
  return (
    <div>
      <**Dice** />
    </div>
  );
}

export default App;
```

- 한 가지 주의해야 할 부분은, 리액트 컴포넌트의 이름은 반드시 **첫 글자를 대문자**로 작성해야 한다는 것입니다.

# Props 정리하기

## Props

- JSX 문법에서 컴포넌트를 작성할 때 컴포넌트에도 속성을 지정할 수 있습니다.
- 리액트에서 이렇게 컴포넌트에 지정한 속성들을 **Props**라고 부릅니다.
- **Props는 Properties의 약자** 입니다.

⇒ 컴포넌트에 속성을 지정해주면 **각 속성이 하나의 객체로 모여서 컴포넌트를 정의한 함수의 첫 번째 파라미터로 전달됩니다.**

`App.js`

```jsx
import Dice from './Dice';

function App() {
  return (
    <div>
      <**Dice** color="blue" />
    </div>
  );
}

export default App;
```

`Dice.js`

```jsx
import diceBlue01 from './assets/dice-blue-1.svg';

function **Dice**(**props**) {
  console.log(**props**)
  return <img src={diceBlue01} alt="주사위" />;
}

export default Dice;
```

- 위 코드들 처럼 `App` 함수에서 사용하는 `Dice` 컴포넌트에 `color` 라는 속성을 `blue` 라고 지정해주고,
- `Dice` 함수 내부에서 `props` 라는 파라미터를 하나 만들어 출력해보면 브라우저 콘솔에는 다음과 같은 출력 결과가 나타납니다.

```jsx
{ color: "blue" }
```

**⇒ 컴포넌트를 활용할 때 속성값을 다양하게 전달하고 이 `props` 값을 활용하면, 똑같은 컴포넌트라도 전달된 속성값에 따라 서로 다른 모습을 그려낼 수도 있게 됩니다.**

`App.js`

```jsx
import Dice from './Dice';

function App() {
  return (
    <div>
      <Dice color="red" num={2} />
    </div>
  );
}

export default App;
```

`Dice.js`

```jsx
import diceBlue01 from './assets/dice-blue-1.svg';
import diceBlue02 from './assets/dice-blue-2.svg';
// ...
import diceRed01 from './assets/dice-red-1.svg';
import diceRed02 from './assets/dice-red-2.svg';
// ...

const DICE_IMAGES = {
  blue: [diceBlue01, diceBlue02],
  red: [diceRed01, diceRed02],
};

function Dice(props) {
  const src = DICE_IMAGES[**props.color**][**props.num - 1**];
  const alt = `${**props.color**} ${**props.num**}`;
  return <img src={src} alt={alt} />;
}

export default Dice;
```

- 참고로, 이렇게 `props` 가 객체 형태를 띠고 있으니 Destructuring 문법을 활용해서 조금 더 간결하게 코드를 작성할 수 있습니다.

```jsx
import diceBlue01 from './assets/dice-blue-1.svg';
import diceBlue02 from './assets/dice-blue-2.svg';
// ...
import diceRed01 from './assets/dice-red-1.svg';
import diceRed02 from './assets/dice-red-2.svg';
// ...

const DICE_IMAGES = {
  blue: [diceBlue01, diceBlue02],
  red: [diceRed01, diceRed02],
};

function Dice({ color = 'blue', num = 1 }) {
  const src = DICE_IMAGES[**color**][**num - 1**];
  const alt = `${**color**} ${**num**}`;
  return <img src={src} alt={alt} />;
}

export default Dice;
```

## Children

- `props` 에는 `children` 이라는 조금 특별한 프로퍼티(prop, 프롭)가 있습니다.
- JSX 문법으로 컴포넌트를 작성할 때 컴포넌트를 **단일 태그가 아니라 여는 태그와 닫는 태그의 형태로 작성하면**

**⇒ 그 안에 작성된 코드가 바로 이 `children` 값에 담기게 됩니다.**

`Button.js` 

```jsx
function Button({ children }) {
  return <button>{children}</button>;
}

export default Button;

```

`App.js`

```jsx
import Button from './Button';
import Dice from './Dice';

function App() {
  return (
    <div>
      <div>
        <Button>던지기</Button>
        <Button>처음부터</Button>
      </div>
      <Dice color="red" num={2} />
    </div>
  );
}

export default App;
```

- JSX 문법으로 컴포넌트를 작성할 때 어떤 정보를 전달할 때는 일반적인 `props` 의 속성값을 주로 활용합니다.

⇒ **화면에 보여질 모습을 조금 더 직관적인 코드로 작성하고자 할 때 `children` 값을 활용할 수 있습니다.**

- 참고로 이 `children` 을 활용하면 단순히 텍스트만 작성하는 걸 넘어서 컴포넌트 안에 컴포넌트를 작성할 수도 있고,
- 컴포넌트 안에 복잡한 태그들을 더 작성할 수도 있습니다.

# State 정리하기

## State

- state는 리액트에서 화면을 그려내는 데 굉장히 중요한 역할을 합니다.
- State라는 단어는 한국어로 ‘상태’라는 뜻이 있는데, 리액트에서의 state도 그 의미가 다르지 않습니다.

⇒ 상태가 바뀔 때마다 화면을 새롭게 그려내는 방식으로 동작을 합니다.

- 리액트에서 state를 만들고, state를 바꾸기 위해서는 일단 `useState`  라는 함수를 활용해야 합니다.

```jsx
import { useState } from 'react';

// ...

  const [num, setNum] = useState(1);

// ...
```

- 보통 이렇게 Destructuring 문법으로 작성합니다.

: `useState` 함수가 초깃값을 아규먼트로 받고 그에 따른 실행 결과로 요소 2개를 가진 배열의 형태로 리턴하기 때문입니다.

⇒ **첫 번째 요소가 바로 state이고, 두 번째 요소가 이 state를 바꾸는 setter 함수입니다.**

- 참고로 위 코드에서도 볼 수 있듯 **첫 번째 변수는 원하는 state의 이름( `num` )을 지어주고,**
- **두 번째 변수에는 state 이름 앞에 set을 붙인 다음 카멜 케이스로 이름을 지어주는 것( `setNum` )이 일반적**입니다.
- state는 변수에 새로운 값을 할당하는 방식으로 변경하는 것이 아니라 이 setter 함수를 활용해야 합니다.

**⇒ setter 함수는 호출할 때 전달하는 아규먼트 값으로 state 값을 변경해줍니다.**

```jsx
import { useState } from 'react';
import Button from './Button';
import Dice from './Dice';

function App() {
  const [num, setNum] = useState(1);

  const handleRollClick = () => {
    setNum(3); // num state를 3으로 변경!
  };

  const handleClearClick = () => {
    setNum(1); // num state를 1로 변경!
  };

  return (
    <div>
      <Button onClick={handleRollClick}>던지기</Button>
      <Button onClick={handleClearClick}>처음부터</Button>
      <Dice color="red" num={num} />
    </div>
  );
}

export default App;

```

- setter 함수를 활용해서 이벤트 핸들러를 등록해두면, 이벤트가 발생할 때마다 상태가 변하면서 화면이 새로 그려집니다.

## 참조형 State

- 자바스크립트의 자료형은 크게 `기본형(Primitive type)` 과 `참조형(Reference type)` 로 나눌 수 있습니다.
- 특히 참조형 값들은 조금 독특한 특성을 가지고 있어서 변수로 다룰 때도 조금 주의해야 할 부분들이 있었는데, state를 활용할 때도 마찬가지입니다.

```jsx
// ... 

  const [gameHistory, setGameHistory] = useState([]);

  const handleRollClick = () => {
    const nextNum = random(6);
    gameHistory.push(nextNum);
    setGameHistory(gameHistory); // **state가 제대로 변경되지 않는다!**
  };

// ...
```

- 위 코드에서 볼 수 있듯 배열 값을 가진 `gameHistory` 에 `push` 메소드를 이용해서 배열의 값을 변경한 다음, 변경된 배열을 setter 함수로 state를 변경하려고 하면 코드가 제대로 동작하지 않습니다.
- `gameHistory` state는 배열 값 자체를 가지고 있는 게 아니라 그 배열의 주솟값을 참조하고 있습니다.

⇒ **`push` 메소드로 배열 안에 요소를 변경했다고 하더라도 결과적으로 참조하는 배열의 주솟값은 변경된 것이 아닙니다.**

⇒ **결과적으로 리액트 입장에서는 `gameHistory` state가 참조하는 주솟값은 여전히 똑같기 때문에 상태(state)가 바뀌었다고 판단하지 않는 것입니다.**

⇒ **그래서 참조형 state를 활용할 때는 반드시 새로운 참조형 값을 만들어 state를 변경해야 합니다.**

- 가장 간단한 방법은 Spread 문법( `...` )을 활용하는 것입니다.

```jsx
// ...

  const [gameHistory, setGameHistory] = useState([]);

  const handleRollClick = () => {
    const nextNum = random(6);
    setGameHistory([...gameHistory, nextNum]); // state가 제대로 변경된다!
  };

// ...
```

- 이 참조형 state의 특성을 이해하지 못하면, 간혹 state가 제대로 변경되지 않는 버그가 발생했을 때 원인을 제대로 찾지 못하는 경우가 발생할 수도 있습니다.

⇒ **참조형 state를 활용할 땐 반드시 새로운 참조형 값을 만들어서 state를 변경해야 한다는 점**

