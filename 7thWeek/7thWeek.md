# React로 데이터 다루기

# 배열 랜더링 하기

`pockemon.js`

```jsx
[
  {
    "id": 1,
    "name": "이상해씨",
    "types": [
      "풀",
      "독"
    ]
  },
  {
    "id": 2,
    "name": "이상해풀",
    "types": [
      "풀",
      "독"
    ]
  },
  {
    "id": 3,
    "name": "이상해꽃",
    "types": [
      "풀",
      "독"
    ]
  }
 ]
```

## `map` 으로 렌더링하기

- 배열 메소드 `map` 에서 콜백 함수의 리턴 값으로 리액트 엘리먼트를 리턴하면 됩니다.

```jsx
import items from './pokemons';

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  return (
    <ul>
      **{**items.**map**((item) => (
        <li key={item.id}>
          <Pokemon item={item} />
        </li>
      ))**}**
    </ul>
  );
}
 
export default App;
```

- 참고로 반드시 JSX의 중괄호 안에서 `map` 함수를 써야 하는 것은 아닙니다.
- 예를 들어서 아래처럼 `renderedItems` 라는 **변수에 `map` 의 결과를 지정해도 똑같이 렌더링** 하게 됩니다.

⇒ `renderedItems` 의 계산된 값이 결국 리액트 엘리먼트의 배열이기 때문입니다.

```jsx
import items from './pokemons';

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  const renderedItems = items.**map**((item) => (
    <li key={item.id}>
      <Pokemon item={item} />
    </li>
  ));

  return (
    <ul>
      {renderedItems}
    </ul>
  );
}
 
export default App;
```

## `sort` 로 정렬하기

- 배열 메소드의 `sort` 메소드를 사용하면 배열을 정렬할 수 있습니다.
- **정렬한 배열을 렌더링** 할 수 있습니다.
- 아래 코드는 `id` 순서대로 / 반대로 정렬하는 예시입니다.

```jsx
import { useState } from 'react';
import items from './pokemons';

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  const [direction, setDirection] = useState(1);

  const handleAscClick = () => setDirection(1);

  const handleDescClick = () => setDirection(-1);

  const sortedItems = items.**sort**((a, b) => direction * (a.id - b.id));

  return (
    <div>
      <div>
        <button onClick={handleAscClick}>도감번호 순서대로</button>
        <button onClick={handleDescClick}>도감번호 반대로</button>
      </div>
      <ul>
        {sortedItems.map((item) => (
          <li key={item.id}>
            <Pokemon item={item} />
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

## `filter` 로 삭제하기

- 배열 메소드 중 `filter` 와 배열형 스테이트를 활용하면 삭제 기능을 간단히 구현할 수 있습니다.

```jsx
import { useState } from 'react';
import mockItems from './pokemons';

function Pokemon({ item, onDelete }) {
  const handleDeleteClick = () => onDelete(item.id);

  return (
    <div>
       No.{item.id} {item.name}
      <button onClick={handleDeleteClick}>삭제</button>
    </div>
  );
}

function App() {
  const [items, setItems] = useState(mockItems);

  const handleDelete = (id) => {
    const nextItems = items.filter((item) => item.id !== id);
    setItems(nextItems);
  };

  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>
          <Pokemon item={item} onDelete={handleDelete} />
        </li>
      ))}
    </ul>
  );
}

export default App;
```

## 반드시 `key` 를 내려주자

- 각 요소를 렌더링 할 때는 `key` Prop을 내려줘야 합니다.
- 이때 **가장 바깥쪽에 있는 (최상위) 태그에다가 `key`  Prop을 지정**하면 됩니다.
- 앞에서 `id` 는 각 요소를 구분할 수 있는 고유한 값이기 때문에 사용했었습니다.
- 반드시 `id` 일 필요는 없고 **포켓몬 이름**처럼(참고로 포켓몬 이름은 고유합니다)

⇒ **각 데이터를 구분할 수 있는 고유한 값이면 무엇이든 `key` 로 활용**해도 상관없습니다.

```jsx
import items from './pokemons';

function Pokemon({ item }) {
  return (
    <div>
      No.{item.id} {item.name}
    </div>
  );
}

function App() {
  return (
    <ul>
      {items.map((item) => (
        <li **key**={**item.name**}> // **포켓몬 이름**
          <Pokemon item={item} />
        </li>
      ))}
    </ul>
  );
}

export default App;
```

# 데이터 가져오기

## useEffect

- `useEffect` 를 사용해서 초기 데이터를 불러오고 정렬을 바꿀 때마다 데이터를 불러옵니다.

## 처음 한 번만 실행하기

```jsx
useEffect(() => {
  // 실행할 코드
}, []);
```

- 컴포넌트가 처음 렌더링 되고 나면 리액트가 콜백 함수를 기억해뒀다가 실행합니다.

⇒ 그 이후로는 콜백 함수를 실행하지 않습니다.

## 값이 바뀔 때마다 실행하기

```jsx
useEffect(() => {
  // 실행할 코드
}, **[dep1, dep2, dep3, ...]**);
```

- 컴포넌트가 처음 렌더링 되고 나면 리액트가 콜백 함수를 기억해뒀다가 실행합니다.
- 그 이후로 렌더링 할 때는 **디펜던시 리스트에 있는 값들을 확인해서 하나라도 바뀌면** 콜백 함수를 기억해뒀다가 실행합니다.

# 조건부 렌더링

## 논리 연산자 활용하기

## `AND` 연산자

```jsx
import { useState } from 'react';

function App() {
  const [show, setShow] = useState(false);

  const handleClick = () => setShow(!show);

  return (
    <div>
      <button onClick={handleClick}>토글</button>
      **{**show **&&** <p>보인다 👀</p>**}**
    </div>
  );
}

export default App;
```

- `show` 값이 **`true` 이면 렌더링** 하고, `false` 이면 렌더링 하지 않습니다.

## `OR` 연산자

```jsx
import { useState } from 'react';

function App() {
  const [hide, setHide] = useState(true);

  const handleClick = () => setHide(!hide);

  return (
    <div>
      <button onClick={handleClick}>토글</button>
      **{**hide **||** <p>보인다 👀</p>**}**
    </div>
  );
}

export default App;
```

- `hide` 값이 `true` 이면 렌더링 하지 않고, **`false` 이면 렌더링** 합니다.

## 삼항 연산자 활용

```jsx
import { useState } from 'react';

function App() {
  const [toggle, setToggle] = useState(false);

  const handleClick = () => setToggle(!toggle);

  return (
    <div>
      <button onClick={handleClick}>토글</button>
      **{toggle ? <p>✅</p> : <p>❎</p>}**
    </div>
  );
}

export default App;
```

- 삼항 연산자를 사용하면 **참, 거짓일 경우에 다르게 렌더링**해줄 수 있습니다.
- `toggle` 의 값이 참일 경우에는 `✅` 을, 거짓일 경우에는 `❎` 를 렌더링합니다.

## 랜더링 되지 않는 값들

```jsx
function App() {
  const nullValue = null;
  const undefinedValue = undefined;
  const trueValue = true;
  const falseValue = false;
  const emptyString = '';
  const emptyArray = [];

  return (
    <div>
      <p>**{nullValue}**</p>
      <p>**{undefinedValue}**</p>
      <p>**{trueValue}**</p>
      <p>**{falseValue}**</p>
      <p>**{emptyString}**</p>
      <p>**{emptyArray}**</p>
    </div>
  );
}

export default App;
```

- 위 컴포넌트에서 중괄호 안에 있는 값들은 모두 아무것도 렌더링하지 않습니다.

```jsx
function App() {
  const zero = 0;
  const one = 1;

  return (
    <div>
      <p>**{zero}**</p>
      <p>**{one}**</p>
    </div>
  );
}

export default App;
```

- 반면에 이 값들은 각각 **숫자 0과 1을 렌더링** 합니다.

## 조건부 렌더링을 사용할 때 주의할 점

`문제 있는 코드`

```jsx
import { useState } from 'react';

function App() {
  const [num, setNum] = useState(0);

  const handleClick = () => setNum(num + 1);

  return (
    <div>
      <button onClick={handleClick}>더하기</button>
      {**num** && <p>num이 0 보다 크다!</p>}
    </div>
  );
}

export default App;
```

- `num` 값이 0일 때는 `false` 로 계산되니깐 뒤의 값을 계산하지 않기 때문에 아무것도 렌더링 하지 않는 코드 같습니다.
- 하지만 앞에서 살펴봤듯이 **`숫자 0`  은 `0` 으로 렌더링** 됩니다.

⇒ 처음 실행했을 때 숫자 0이 렌더링 되고 '더하기' 버튼을 눌러서 `num` 값이 증가하면 `num이 0 보다 크다!` 가 렌더링 됩니다.

⇒ 이런 경우엔 아래처럼 보다 명확한 논리식을 써주는 게 안전합니다.

: `true` 나 `false` 값은 리액트에서 렌더링 하지 않기 때문입니다.

```jsx
import { useState } from 'react';

function App() {
  const [num, setNum] = useState(0);

  const handleClick = () => setNum(num + 1);

  return (
    <div>
      <button onClick={handleClick}>더하기</button>
      {**(num > 0)** && <p>num이 0 보다 크다!</p>}
    </div>
  );
}

export default App;
```

# useState

## 초기값 지정하기

```jsx
const [state, setState] = useState(**initialState**);
```

- `useState` 함수에 값을 전달하면 초깃값으로 지정할 수 있었습니다.

## 콜백으로 초깃값 지정하기

```jsx
const [state, setState] = useState(() => {
  // 초기값을 계산
  return **initialState**;
});
```

- 초깃값을 계산해서 넣는 경우 이렇게 콜백을 사용하면 좋습니다.

`예시` 

```jsx
function ReviewForm() {
  **const savedValues = getSavedValues()**; // ReviewForm을 렌더링할 때마다 실행됨
  const [values, setValues] = useState(savedValues);
  // ...
}
```

- `getSavedValues` 라는 함수를 통해서 컴퓨터에 저장된 초깃값을 가져오는 상황

문제점 : `savedValues` 라는 값은 처음 렌더링 한 번만 계산하면 되는데 매 렌더링 때마다 불필요하게 `getSavedValues` 함수를 실행해서 저장된 값을 가져옵니다.

```jsx
function ReviewForm() {
  const [values, setValues] = useState(() => {
    const savedValues = getSavedValues(); // 처음 렌더링할 때만 실행됨
    **return savedValues**
  });
  // ...
}
```

- 이럴 때는 이렇게 콜백 형태로 초깃값을 지정해주면

⇒ **처음 렌더링 할 때 한 번만 콜백을 실행해서 초깃값을 만듭니다**.

⇒ **그 이후는 콜백을 실행하지 않기 때문**에 `getSavedValues` 를 불필요하게 실행하지 않습니다.

주의할 점: 이 콜백 함수가 리턴할 때까지 리액트가 렌더링하지 않고 기다립니다.

⇒ **콜백 함수의 실행이 오래 걸릴 수록 초기 렌더링이 늦어집니다**.

## Setter 함수 사용하기

## 기본

```jsx
const [state, setState] = useState(0);

const handleAddClick = () => {
  setState(state + 1);
}
```

- `Setter` 함수에다가 값을 전달하면, 해당하는 값으로 변경되었습니다.

주의점: **배열이나 객체 같은 참조형은 반드시 새로운 값을 만들어서 전달해야 한다는 것**입니다.

### 참조형 State 사용의 잘못된 예

```jsx
const [**state**, setState] = useState({ count: 0 });

const handleAddClick = () => {
  **state**.count += 1; // 참조형 변수의 프로퍼티를 수정
  setState(**state**); // 참조형이기 때문에 변수의 값(레퍼런스)는 변하지 않음
}
```

### 참조형 State 사용의 올바른 예

```jsx
const [**state**, setState] = useState({ count: 0 });

const handleAddClick = () => {
  setState({ **...state,** count: state.count + 1 }); // 새로운 객체 생성
}
```

## 콜백으로 State 변경

```jsx
setState((prevState) => {
  // 다음 State 값을 계산
  **return nextState**;
});
```

- 만약 이전 State 값을 참조하면서 State를 변경하는 경우,
- **비동기 함수에서 State를 변경하게 되면 최신 값이 아닌 State 값을 참조하는 문제**가 있었습니다.

⇒ **콜백을 사용해서 처리**할 수 있습니다.

- 파라미터로 올바른 State 값을 가져와서 사용할 수 있습니다.

⇒ **이전 State 값으로 새로운 State를 만드는 경우에는 항상 콜백 형태를 사용하는게 좋습니다.**

### 콜백으로 State를 변경하는 예시

```jsx
const [count, setCount] = useState(0);

const handleAddClick = async () => {
  await addCount();
  setCount(**(prevCount) => prevCount + 1**);
}
```

# 입력 폼 다루기

## 입력 폼

## HTML과 다른 점

### onChange

- 리액트에선 순수 HTML과 다르게
- `onChange`  **Prop을 사용하면 입력 값이 바뀔 때마다 핸들러 함수를 실행**합니다.
- `oninput` 이벤트와 같습니다.

⇒ `onChange` 라는 Prop을 사용해야 합니다.

### htmlFor

- `<label />` 태그에서 사용하는 속성인 `for` 은 자바스크립트 반복문 키워드인 `for` 와 겹치기 때문에
- 리액트에서는 `htmlFor` 를 사용합니다.

## 폼을 다루는 기본적인 방법

- 스테이트를 만들고 `target.value` 값을 사용해서 값을 사용해서 값을 변경해 줄 수 있습니다.
- 이때 `value` Prop으로 스테이트 값을 내려주고, `onChange` Prop으로 핸들러 함수를 넘겨줍니다.

```jsx
function TripSearchForm() {
  const [location, setLocation] = useState('Seoul');
  const [checkIn, setCheckIn] = useState('2022-01-01');
  const [checkOut, setCheckOut] = useState('2022-01-02');

  const handleLocationChange = (e) => setLocation(e.target.value);

  const handleCheckInChange = (e) => setCheckIn(e.target.value);

  const handleCheckOutChange = (e) => setCheckOut(e.target.value);
    
  return (
    <form>
      <h1>검색 시작하기</h1>
      <label htmlFor="location">위치</label>
      <input id="location" name="location" value={location} placeholder="어디로 여행가세요?" onChange={handleLocationChange} />
      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" name="checkIn" value={checkIn} onChange={handleCheckInChange} />
      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" name="checkOut" value={checkOut} onChange={handleCheckOutChange} />
      <button type="submit">검색</button>
    </form>
  )
}
```

## 폼 값을 객체 하나로 처리하기

- 이벤트 객체의 `target.name` 과 `target.value` 값을 사용해서 값을 변경해 줄 수도 있었습니다.

⇒ 객체형 스테이트 하나만 가지고도 값을 처리할 수 있었습니다.

```jsx
function TripSearchForm() {
  const [values, setValues] = useState({
    location: 'Seoul',
    checkIn: '2022-01-01',
    checkOut: '2022-01-02',
  })

  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues((prevValues) => ({
      ...prevValues,
      [name]: value,
    }));
  }
    
  return (
    <form>
      <h1>검색 시작하기</h1>
      <label htmlFor="location">위치</label>
      <input id="location" name="location" value={values.location} placeholder="어디로 여행가세요?" onChange={handleChange} />
      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" name="checkIn" value={values.checkIn} onChange={handleChange} />
      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" name="checkOut" value={values.checkOut} onChange={handleChange} />
      <button type="submit">검색</button>
    </form>
  )
}
```

## 기본 submit 동작 막기

- HTML 폼의 기본 동작은 `submit` 타입의 버튼을 눌렀을 때 페이지를 이동하는 겁니다.
- 이벤트 객체의 `preventDefault` 를 사용하면 이 동작을 막을 수 있었습니다.

```jsx
const handleSubmit = (e) => {
  e.preventDefault();
  // ...
}
```

## 제어 컴포넌트

- 인풋 태그의 `value` 속성을 지정하고 사용하는 컴포넌트입니다.
- 리액트에서 인풋의 값을 제어하는 경우로 리액트에서 지정한 값과 실제 인풋 `value` 의 값이 항상 같습니다.

⇒ 값을 예측하기가 쉽고 인풋에 쓰는 값을 여러 군데서 쉽게 바꿀 수 있다는 장점이 있어서 리액트에서 권장하는 방법입니다.

⇒ 이때 `State` 냐 `Prop` 이냐는 중요하지 않고, 리액트로 `value` 를 지정한다는 것이 핵심입니다.

## 예시1

```jsx
function TripSearchForm() {
  const [values, setValues] = useState({
    location: 'Seoul',
    checkIn: '2022-01-01',
    checkOut: '2022-01-02',
  })

  const handleChange = (e) => {
    const { name, value } = e.target;
    setValues((prevValues) => ({
      ...prevValues,
      [name]: value,
    }));
  }
    
  return (
    <form>
      <h1>검색 시작하기</h1>
      <label htmlFor="location">위치</label>
      <input id="location" name="location" **value={values.location}** placeholder="어디로 여행가세요?" onChange={handleChange} />
      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" name="checkIn" **value={values.checkIn}** onChange={handleChange} />
      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" name="checkOut" **value={values.checkOut}** onChange={handleChange} />
      <button type="submit">검색</button>
    </form>
  )
}
```

## 예시2

```jsx
function TripSearchForm({ values, onChange }) {
  return (
    <form>
      <h1>검색 시작하기</h1>
      <label htmlFor="location">위치</label>
      <input id="location" name="location" **value={values.location}** placeholder="어디로 여행가세요?" onChange={onChange} />
      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" name="checkIn" **value={values.checkIn}** onChange={onChange} />
      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" name="checkOut" **value={values.checkOut}** onChange={onChange} />
      <button type="submit">검색</button>
    </form>
  )
}
```

## 비제어 컴포넌트

- 인풋 태그의 `value` 속성을 리액트에서 지정하지 않고 사용하는 컴포넌트입니다.

```jsx
function TripSearchForm({ onSubmit }) {
  return (
    <form **onSubmit={onSubmit}** >
      <h1>검색 시작하기</h1>
      <label htmlFor="location">위치</label>
      <input id="location" name="location" placeholder="어디로 여행가세요?" />
      <label htmlFor="checkIn">체크인</label>
      <input id="checkIn" type="date" name="checkIn" />
      <label htmlFor="checkOut">체크아웃</label>
      <input id="checkOut" type="date" name="checkOut" />
      <button type="submit">검색</button>
    </form>
  )
}
```

- 참고로 위처럼 작성해도 `onSubmit` 함수에서는 폼 태그를 참조할 수 있습니다.

- 값들을 참조하려면 이벤트 객체의 `target` 활용해서 이렇게 할 수도 있습니다.

```jsx
const handleSubmit = (e) => {
  e.preventDefault();
  const **form** = **e.target**;
  const location = **form**['location'].value;
  const checkIn = **form**['checkIn'].value;
  const checkOut = **form**['checkOut'].value;
  // ....
}
```

- 폼 태그로 곧바로 `FormData` 를 바로 만드는 것도 가능합니다.

```jsx
const handleSubmit = (e) => {
  e.preventDefault();
  const form = e.target;
  const formData = new FormData(form);
  // ...
}
```

- 만약 이렇게 제어 컴포넌트랑 비제어 컴포넌트 모두 쓸 수 있는 경우라면

⇒ 제어 컴포넌트를 사용하는 게 좋습니다.

하지만 반드시 비제어 컴포넌트로 만들어야만 하는 경우도 있습니다.

# ref와 useRef

## Ref 객체 생성

```jsx
import { useRef } from 'react';

// ...

const ref = **useRef**();
```

- `useRef` 함수로 Ref 객체를 만들 수 있었습니다.

## `ref` Prop 사용하기

```jsx
const **ref** = useRef();

// ...

<div ref={**ref**}> ... </div>
```

- `ref` Prop에다가 앞에서 만든 Ref 객체를 내려주면 됩니다.

## Ref 객체에서 DOM 노드 참조하기

```jsx
const node = ref.current;
if (node) {
  // node 를 사용하는 코드
}
```

- Ref 객체의 `current` 라는 프로퍼티를 사용하면 DOM 노드를 참조할 수 있었습니다.
- `current` 값은 없을 수도 있으니까 반드시 값이 존재하는지 검사하고 사용해야 됩니다.

## 예시: 이미지 크기 구하기

- 다음 코드는 `img` 노드의 크기를 `ref` 를 활용해서 출력하는 예시입니다.
- `img`  노드에는 너비 값인 `width` 와 높이 값인 `height`  라는 속성이 있습니다.

⇒ **Ref 객체의 `current` 로 DOM 노드를 참조해서 두 속성 값을 가져왔습니다**.

```jsx
import { useRef } from 'react';

function Image({ src }) {
  const imgRef = useRef();

  const handleSizeClick = () => {
    const **imgNode** = **imgRef**.**current**;
    if (!imgNode) return;

    const { width, height } = **imgNode**;
    console.log(`${width} x ${height}`);
  };

  return (
    <div>
      <img src={src} ref={imgRef} alt="크기를 구할 이미지" />
      <button onClick={handleSizeClick}>크기 구하기</button>
    </div>
  );
}
```

# 사이드 이펙트와 useEffect

## 사이드 이펙트(Side Effect)란?

- 한국어로는 '부작용'이라는 뜻입니다.
- 일상생활에서는 주로 안 좋은 것들을 부작용이라고 부르지만
- 프로그래밍에선 말 그대로 **외부에 부수적인 작용**을 하는걸 말합니다.

```jsx
**let count = 0;**

function add(a, b) {
  const result = a + b;
  **count += 1; // 함수 외부의 값을 변경**
  return result;
}

const val1 = add(1, 2);
const val2 = add(-4, 5);
```

- 위 코드에서 `add` 함수는 `a` , `b` 를 파라미터로 받아서 더한 값을 리턴합니다.
- 함수 코드 중에서 함수 바깥에 있는 `count` 라는 변수의 값을 변경하는 코드가 있습니다.

⇒ **`add` 함수는 실행하면서 함수 외부의 상태 (`count` 변수)가 바뀌기 때문**에, 이런 함수를 **"사이드 이펙트가 있다"**고 합니다.

- 우리에게 친숙한 예로는 `console.log`  함수가 있습니다.
- `console.log` 함수를 사용하면 값을 계산해서 리턴하는 게 아니라 웹 브라우저 콘솔 창에 문자열을 출력합니다.

⇒ 외부 상태를 변경해서 문자열을 출력하는 것입니다.

⇒ **함수 안에서 함수 바깥에 있는 값이나 상태를 변경하는 걸 '사이드 이펙트'라고 부릅니다.**

## 사이드 이펙트와 useEffect

- `useEffect` 는 리액트 컴포넌트 함수 안에서 **사이드 이펙트를 실행하고 싶을 때** 사용하는 함수입니다.
- 예를 들면 DOM 노드를 직접 변경한다거나, 브라우저에 데이터를 저장하고, 네트워크 리퀘스트를 보내는 것 처럼입니다.

⇒ **주로 리액트 외부에 있는 데이터나 상태를 변경할 때 사용합니다.**

## 페이지 정보 변경

```jsx
useEffect(() => {
  document.title = title; // 페이지 데이터를 변경
}, [title]);
```

## 네트워크 요청

```jsx
useEffect(() => {
  fetch('https://example.com/data') // 외부로 네트워크 리퀘스트
    .then((response) => response.json())
    .then((body) => setData(body));
}, [])
```

## 데이터 저장

```jsx
useEffect(() => {
  **localStorage**.setItem('theme', theme); // 로컬 스토리지에 테마 정보를 저장
}, [theme]);
```

참고: `localStorage` 는 웹 브라우저에서 데이터를 저장할 수 있는 기능입니다.

## 타이머

```jsx
useEffect(() => {
  const timerId = setInterval(() => {
    setSecond((prevSecond) => prevSecond + 1);
  }, 1000); // 1초마다 콜백 함수를 실행하는 타이머 시작
  
  return () => {
    clearInterval(timerId);
  }
}, []);
```

참고: `setInterval` 이라는 함수를 쓰면 일정한 시간마다 콜백 함수를 실행할 수 있습니다.

## useEffectf를 쓰면 좋은 경우

- `useEffect` 는 '동기화’에 쓰면 유용한 경우가 많습니다.
- 여기서 동기화는 컴포넌트 안에 데이터와 리액트 바깥에 있는 데이터를 일치시키는 걸 말합니다.
- 아래 `App` 컴포넌트는 인풋 입력에 따라 페이지 제목을 바꾸는 컴포넌트입니다.

## 핸들러 함수만 사용한 예시

```jsx
import { useState } from 'react';

const INITIAL_TITLE = 'Untitled';

function App() {
  const [**title**, setTitle] = useState(INITIAL_TITLE);

  const **handleChange** = (e) => {
    const nextTitle = e.target.value;
    setTitle(nextTitle);
    **document.title = nextTitle;**
  };

  const **handleClearClick** = () => {
    const nextTitle = INITIAL_TITLE;
    setTitle(nextTitle);
    **document.title = nextTitle;**
  };

  return (
    <div>
      <input value={title} onChange={handleChange} />
      <button onClick={handleClearClick}>초기화</button>
    </div>
  );
}

export default App;
```

- `handleChange`  함수와 `handleClearClick` 함수 모두 `title` 스테이트를 변경한 후에 `document.title` 도 함께 변경해주고 있습니다.

⇒ `documetn.title` 값을 바꾸는 건 외부의 상태를 변경하는 거니까 **사이드 이펙트**입니다.

- 만약 새로 함수를 만들어서 `setTitle` 을 사용하는 코드를 추가할 때마다 `document.title` 값도 변경해야 한다는 걸 기억해뒀다가 관련된 코드를 작성해야 한다는 점이 아쉽습니다.

⇒ 동료 개발자가 나중에 컴포넌트를 수정하거나 1년 뒤의 내가 컴포넌트를 수정할 때 빠뜨리기 좋습니다.

## useEffect를 사용한 예시

```jsx
import { useEffect, useState } from 'react';

const INITIAL_TITLE = 'Untitled';

function App() {
  const [title, setTitle] = useState(INITIAL_TITLE);

  const handleChange = (e) => {
    const nextTitle = e.target.value;
    setTitle(nextTitle);
  };

  const handleClearClick = () => {
    setTitle(INITIAL_TITLE);
  };

  **useEffect(() => {
    document.title = title;
  }, [title]);**

  return (
    <div>
      <input value={title} onChange={handleChange} />
      <button onClick={handleClearClick}>초기화</button>
    </div>
  );
}

export default App;
```

- `useEffect` 를 사용한 예시에서는 `document` 를 다루는 사이드 이펙트 부분만 따로 처리를 하고 있습니다.
- 이렇게 하면 `setTItle` 함수를 쓸 때마다 `document.title` 을 변경하는 코드를 신경 쓰지 않아도 되니까 편리합니다.
- 게다가 처음 렌더링 되었을 때  'Untitled' 라고 페이지 제목을 변경하는 효과까지 낼 수 있습니다.
- 이 코드를 보는 누구든 이 컴포넌트는 `title` 스테이트 값을 가지고 항상 `doucument.title` 에 반영해줄 것이라고 쉽게 예측할 수 있습니다.

⇒ **`useEffect` 는 리액트 안과 밖의 데이터를 일치시키는데 활용하면 좋습니다.**

**⇒ `useEffect` 를 사용하면 반복되는 코드를 줄이고, 동작을 쉽게 예측할 수 있는 코드를 작성할 수 있습니다.**

## 정리 함수 (Cleanup Function)

```jsx
useEffect(() => {
  // 사이드 이펙트

  return () => {
    // 사이드 이펙트에 대한 정리
  }
}, [dep1, dep2, dep3, ...]);
```

- `useEffect` 의 콜백 함수에서도 사이드 이펙트를 만들면 정리가 필요한 경우가 있습니다.
- 이럴 때 **콜백 함수에서 리턴 값**으로 **정리하는 함수를 리턴**할 수 있었습니다.
- 리턴한 정리 함수에서는 사이드 이펙트에 대한 뒷정리를 합니다.
- 예를 들면 이미지 **파일 미리보기를 구현할 때 Object URL을 만들어서 브라우저의 메모리를 할당 ( `createObjectURL` )** 했었는데요.
- **정리 함수**에서는 **이때 할당한 메모리를 다시 해제 ( `revokeObjectURL` )** 해줬었습니다.

## 정리 함수가 실행되는 시점

⇒ **콜백을 한 번 실행했으면, 정리 함수도 반드시 한 번 실행된다**고 생각하면 됩니다.

- 정확히는 새로운 콜백 함수가 호출되기 전에 실행되거나(앞에서 실행한 콜백의 사이드 이펙트를 정리)
- 컴포넌트가 화면에서 사라지기 전에 실행됩니다 (맨 마지막으로 실행한 콜백의 사이드 이펙트를 정리)

## 예시: 타이머

```jsx
import { useEffect, useState } from 'react';

function Timer() {
  const [second, setSecond] = useState(0);

  useEffect(() => {
    const timerId = **setInterval**(() => {
      console.log('타이머 실행중 ... ');
      setSecond((prevSecond) => prevSecond + 1);
    }, 1000);
    console.log('타이머 시작 🏁');

    **return () => {
      clearInterval(timerId);
      console.log('타이머 멈춤 ✋');
    }; // 정리함수**
  }, []);

  return <div>{second}</div>;
}

function App() {
  const [show, setShow] = useState(false);

  const handleShowClick = () => setShow(true);
  const handleHideClick = () => setShow(false);

  return (
    <div>
      {show && <Timer />}
      <button onClick={handleShowClick}>보이기</button>
      <button onClick={handleHideClick}>감추기</button>
    </div>
  );
}

export default App;
```

- 일정한 시간 간격마다 콜백 함수를 실행하는 `setInterval` 이라는 함수도 정리가 필요한 사이드 이펙트입니다.
- 렌더링이 끝나면 타이머를 시작하고, 화면에서 사라지면 타이머를 멈춥니다.
- 사용자가 '보이기' 버튼을 눌렀을 때 `show` 값이 참으로 바뀌면서 다시 렌더링 됩니다.
- 조건부 렌더링에 의해서 `Timer` 컴포넌트를 랜더링합니다.

⇒ `Timer` 컴포넌트에서는 `useEffect` 에서 타이머를 시작하고, 정리함수를 리턴합니다. (콘솔에는 '타이머 시작 🏁'이 출력됩니다.)

⇒ 다시 사용자가 감추기' 버튼을 누르면 `show` 값이 거짓으로 바뀌면서 다시 렌더링 됩니다.

: 조건부 렌더링에 의해서 이제 `Timer` 컴포넌트를 렌더링 하지 않습니다.

⇒ 리액트에선 마지막으로 앞에서 기억해뒀던 정리 함수를 실행해줍니다.

⇒ 타이머를 멈추고 콘솔에는 '타이머 멈춤 ✋'이 출력됩니다.

## useCallback으로 함수 재사용하기

```jsx
import { useCallback, useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [num, setNum] = useState(0);

  const **addCount** = useCallback(() => {
    setCount((c) => c + 1);
    console.log(`num: $**{num}**`);
  }, **[num]**);

  const addNum = () => setNum((n) => n + 1);

  **useEffect(()** => {
    console.log('timer start');
    const timerId = setInterval(() => {
      **addCount()**; // **num 값이 바뀔 때만 새로 실행**
    }, 1000);

    return () => {
      clearInterval(timerId);
      console.log('timer end');
    };
  }, [addCount]);

  return (
    <div>
      <button onClick={addCount}>count: {count}</button>
      <button onClick={addNum}>num: {num}</button>
    </div>
  );
}

export default App;
```

- `useCallback` 을 사용하면 함수를 매번 생성하는 게 아니라 리액트에다 함수를 기억해둘 수 있습니다.

⇒ 리액트는 **`useCallback` 의 디펜던시 리스트 값이 바뀔 때만 함수를 새로 만들어줍니다.**

- `addCount` 함수에서는 `num` 이라는 스테이트를 참조하고 있으니까 이 값을 디펜던시 리스트에 추가했습니다.

⇒ 리액트는 **`num` 값이 바뀔 때만 `addCount` 함수를 새로 만듭니다.**

⇒ `useEffect` 의 디펜던시로 `addCount` 가 들어가 있으니까, 결론적으로 **`useEffect` 의 콜백은 `num` 값이 바뀔 때만 새로 실행됩니다.**

⇒ 이런 식으로 **컴포넌트 안에서 만든 함수를 디펜던시 리스트에 사용할 때는 `useCallback` 혹으로 매번 함수를 새로 생성하는 걸 막을 수 있습니다.**

## 되도록이면 파라미터를 활용하기

- 위에 코드를 조금 더 개선하기
- `addCount` 라는 함수에서 `num` 값을 꼭 참조할 필요는 없습니다.

⇒ `useCallback` 을 쓰지 않고, 아래처럼 파라미터로 받아오게 할 수 있습니다.

⇒ 이렇게 하면 `addCount`  함수 자체만 놓고 보면 **바깥에 있는 스테이트 값을 직접적으로 참조하지 않기 때문에 오래된 스테이트 값을 참조할 염려가 없습니다.**

```jsx
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [num, setNum] = useState(0);

  const addCount = (log) => {
    setCount((c) => c + 1);
    console.log(log);
  }

  const addNum = () => setNum((n) => n + 1);

  useEffect(() => {
    console.log('timer start');
    const timerId = setInterval(() => {
      addCount(`num ${num}`);
    }, 1000);

    return () => {
      clearInterval(timerId);
      console.log('timer end');
    };
  }, [num]);

  return (
    <div>
      <button onClick={addCount}>count: {count}</button>
      <button onClick={addNum}>num: {num}</button>
    </div>
  );
}

export default App;
```

# 리액트 Hook 정리

## Hook의 규칙

- 반드시 리액트 컴포넌트 함수(Functional Component) 안에서 사용해야 합니다.
- 컴포넌트 함수의 최상위에서만 사용 (조건문, 반복문 안에서 못 씀)

## useState

## State 사용하기

```jsx
const [state, setState] = useState(**initialState**);
```

## 콜백으로 초깃값 지정하기

- 초깃값을 계산하는 코드가 복잡한 경우에 활용

```jsx
const [state, setState] = useState(() => {
  // ...
  **return initialState;**
});
```

## State 변경

```jsx
setState(**nextState**);
```

## 이전 State를 참조해서 State 변경

- 비동기 함수에서 최신 State 값을 가져와서 새로운 State 값을 만들 때

```jsx
setState((**prevState**) => {
  // ...
  **return nextState**
});
```

## useEffect

- 컴포넌트 함수에서 **사이드 이펙트 (리액트 외부의 값이나 상태를 변경할 때) 에 활용**하는 함수

## 처음 렌더링 후에 한 번만 실행

```jsx
useEffect(() => {
  // ...
}, []);
```

## 렌더링 후에 특정 값이 바뀌었으면 실행

- 참고로 처음 렌더링 후에도 한 번 실행됩니다.

```jsx
useEffect(() => {
  // ...
}, **[dep1, dep2, dep3, ...]**);
```

## 사이드 이펙트 정리 (Cleanup) 하기

```jsx
useEffect(() => {
  // 사이드 이펙트

  **return () => {
    // 정리
  }**
}, [dep1, dep2, dep3, ...]);
```

## useRef

## 생성하고 DOM 노드에 연결하기

```jsx
const **ref** = **useRef**();

// ...

return <div ref={**ref**}>안녕 리액트!</div>;
```

## DOM 노드 참조하기

```jsx
const **node** = ref.current;
if (**node**) {
  // node를 사용하는 코드
}
```

## useCallback

- 함수를 매번 새로 생성하는 것이 아니라 디펜던시 리스트가 변경될 때만 함수를 생성

```jsx
const handleLoad = useCallback((option) => {
  // ...
}, **[dep1, dep2, dep3, ...]**);
```

## Custom Hook

- 자주 사용하는 Hook 코드들을 모아서 함수로 만들 수 있었습니다.
- 이때 `use000` 처럼 반드시 맨 앞에 `use` 라는 단어를 붙여서 다른 개발자들이 Hook이라는 걸 알 수 있게 해줘야합니다.

## useAsync

- 비동기 함수의 로딩, 에러 처리를 하는 데 사용할 수 있는 함수입니다.
- 함수를 `asyncFunction` 이라는 파라미터로 추상화해서 `wrappedFunction` 이라는 함수를 만들어 사용하는 방식이 좋습니다.

```jsx
function **useAsync**(**asyncFunction**) {
  const [pending, setPending] = useState(false);
  const [error, setError] = useState(null);

  const **wrappedFunction** = useCallback(async (...args) => {
    setPending(true);
    setError(null);
    try {
      return await **asyncFunction**(...args);
    } catch (error) {
      setError(error);
    } finally {
      setPending(false);
    }
  }, **[asyncFunction]**);

  return [pending, error, **wrappedFunction**];
}
```

## useToggle

- `toggle` 함수를 호출할 때마다 `value` 값이 참/거짓으로 번갈아가며 바뀝니다.
- ON/OFF스위치를 만들 때 유용합니다.

```jsx
function useToggle(initialValue = false) {
  const [**value**, **setValue**] = useState(initialValue);
  const toggle = () => **setValue((prevValue) => !prevValue)**;
  return [value, toggle];
}
```

## useTimer

- `start` 를 실행하면 `callback` 이라는 파라미터를 넘겨 준 함수를 `timeout` 밀리초 마다 실행하고, `stop` 을 실행하면 멈춥니다.
- `setInterval` 이란 함수는 웹 브라우저에 함수를 등록해서 일정한 시간 간격마다 실행합니다.

⇒ 실행할 때마다 사이드 이펙트를 만들고, 사용하지 않으면 정리를 해줘야 합니다.

⇒ `clearInterval` 이라는 함수를 실행해서 사이드 이펙트를 정리합니다.

```jsx
function useTimer(callback, timeout) {
  const [isRunning, setIsRunning] = useState(false);

  const start = () => setIsRunning(true);

  const stop = () => setIsRunning(false);

  useEffect(() => {
    if (!isRunning) return;

    **const timerId = setInterval(callback, timeout); // 사이드 이펙트 발생**
    **return () => {
      clearInterval(timerId); // 사이드 이펙트 정리
    };**
  }, [isRunning, callback, timeout]);
  
  return [start, stop];
}
```

# Contetxt 정리

## 리액트 Context

- Props만으로 리액트를 개발을 하다 보면 여러 곳에 쓰이는 데이터를 내려주고 싶을 때가 있습니다.

⇒ 컴포넌트의 단계가 많다면 여러 번 반복해서 Prop을 내려줘야 합니다. : `프롭 드릴링 (Prop Drilling)` 문제

⇒ 프롭 드릴링 (Prop Drilling) 을 해결하기 위해 사용하는 기능입니다.

## Context 만들기

- Context는 `createContext` 라는 함수를 통해 만들 수 있습니다.

```jsx
import { createContext } from 'react';

const LocaleContext = **createContext**();
```

- 참고로 이때 아래처럼 기본값을 넣어 줄 수 있습니다.

```jsx
import { createContext } from 'react';

const LocaleContext = createContext(**'ko'**);
```

## Context 적용하기

- Context를 쓸 때는 반드시 값을 공유할 범위를 정하고 사용해야 합니다.
- 이때 범위는 Context 객체에 있는 `Provider` 라는 컴포넌트로 정해줄 수 있습니다.
- 이때 `Provider` 의 `value`  prop으로 공유할 값을 내려주면 됩니다.

```jsx
import { createContext } from 'react';

const LocaleContext = createContext('ko');

function App() {
  return (
    <div>
       ... 바깥의 컴포넌트에서는 LocaleContext 사용불가

       <LocaleContext.Provider value="en">
          ... Provider 안의 컴포넌트에서는 LocaleContext 사용가능
       </LocaleContext.Provider>
    </div>
  );
}
```

## Context 값 사용하기

- `useContext` 라는 Hook을 사용하면 값을 가져와 사용할 수 있습니다.
- 이때 아규먼트로는 사용할 Context를 넘겨주면 됩니다.

```jsx
import { createContext, useContext } from 'react';

const LocaleContext = createContext('ko');

function Board() {
  const **locale** = **useContext(LocaleContext)**;
  return <div>언어: **{locale}**</div>;
}

function App() {
  return (
    <div>
       <**LocaleContext.Provider** **value="en"**>
          <Board />
       </**LocaleContext.Provider**>
    </div>
  );
}
```

## State, Hook와 함께 활용하기

- Provider 역할을 하는 컴포넌트를 하나 만들고, 여기서 State를 만들어서 `value` 로 넘겨줄 수 있습니다.
- 그리고 아래의 `useLocale` 와 같이 `useContext` 를 사용해서 값을 가져오는 커스텀 Hook을 만들 수도 있습니다.

⇒ 이렇게 하면 Context에서 사용하는 State 값은 반드시 우리가 만든 함수를 통해서만 쓸 수 있기 때문에 안전한 코드를 작성하는데 도움이 됩니다.

```jsx
import { createContext, useContext, useState } from 'react';

**const LocaleContext = createContext({});**

export function LocaleProvider({ children }) {
  const [locale, setLocale] = useState();
  return (
    <LocaleContext.Provider **value={{ locale, setLocale }**}>
      {children}
    </LocaleContext.Provider>
  );
}

export function **useLocale()** {
  const context = **useContext(LocaleContext)**;

  if (!context) {
    throw new Error('반드시 LocaleProvider 안에서 사용해야 합니다');
  }

  const { locale } = context;
  return locale;
}

export function useSetLocale() {
  **const context = useContext(LocaleContext);**

  if (!context) {
    throw new Error('반드시 LocaleProvider 안에서 사용해야 합니다');
  }

  const { setLocale } = context;
  return setLocale;
}
```
<br>

# React로 웹사이트 만들기

# 리액트 라우터 정리

## 리액트 라우터란?

- 리액트에서 경로에 따라 페이지를 나누도록 해주는 라이브러리입니다.
- 리액트스러운 방법으로 컴포넌트를 사용해서 페이지를 나누는 것이 특징입니다.

## 라우터

- 리액트 라우터를 사용하려면 반드시 라우터라는 컴포넌트가 필요합니다.
- `BrowserRouter` 를 최상위 컴포넌트에서 감싸주면 모든 곳에서 사용할 수 있습니다.

```jsx
import { BrowserRouter } from 'react-router-dom';

function App() {
  return <BrowserRouter> ... </BrowserRouter>;
}
```

## 페이지 나누는 방법

- `Routes` 컴포넌트 안에다가 `Route` 컴포넌트를 배치해서 각 페이지를 나눌 수 있습니다.
- 이때 `Routes` 안에서는 위에서부터 차례대로 `Route` 를 검사합니다.
- 현재 경로와 `path` prop이 일치하는 `Route`  를 찾습니다.

```jsx
<Routes>
  <Route path="/" element={<HomePage />} />
  <Route path="posts" element={<PostListPage />} />
  <Route path="posts/1" element={<PostPage />} />
</Routes>
```

## 링크

- 리액트 라우터에서는 `<a>` 태그 대신에 `Link`  컴포넌트를 사용합니다.
- `to` 라는 prop으로 이동할 경로를 정해주면 됩니다.

```jsx
<**Link** to="/posts">블로그<**/Link**>
```

## 하위 페이지 나누기

- `Route` 컴포넌트 안에다가 `Route` 컴포넌트를 배치하면 됩니다.
- 이때 하위 페이지에서 **최상위 경로**에 해당하는 경로는 `path` prop이 아니라 `index` 라는 prop을 사용하면 됩니다.

```jsx
<Routes>
  <Route path="/"><HomePage /></Route>
  <Route path="posts" element={<PostLayout />} >
    <Route **index** element={<PostListPage />}  />
    <Route path="1" element={<PostPage />}  />
  </Route>
</Routes>
```

- 이때 부모 `Route` 컴포넌트에 `element` 를 지정하고,
- 아래처럼 `Outlet` 이라는 컴포넌트를 활용하면 공통된 레이아웃을 지정해줄 수 있습니다.

```jsx
import { **Outlet** } from 'react-router-dom';

function PostLayout() {
  return (
    <div>
      <h1>블로그</h1>
      <hr />
      <Outlet />
    </div>
  );
}

export default PostLayout;
```

## 동적인 경로 다루기

- 콜론( `:` ) 으로 시작하는 문자열을 사용하면 경로에 파라미터를 지정할 수 있습니다.
- 예를 들어서 아래처럼 `/post/:postId` 라는 경로는 `/posts/123` 이라던지
- `/posts/abc` 라는 주소로 접속하면 `123`  이나 `abc` 라는 값을 `postId` 라는 파라미터로 받습니다.

```jsx
<Routes>
  <Route path="/"><HomePage /></Route>
  <Route path="posts" element={<PostLayout />} >
    <Route index element={<PostListPage />}  />
    <Route path="**:postId**" element={<PostPage />}  />
  </Route>
</Routes>
```

- 경로 파라미터를 사용하려면 `useParams` 라는 훅을 사용하면 됩니다.

```jsx
function PostPage() {
  const { **postId** } = **useParams**();
  // ...
}
```

## 쿼리 사용하기

- `useSearchParams` 라는 Custom hook으로 `searchParams`  객체를 받아올 수 있습니다.
- 이 hook은 `SearchParams` 객체와 Setter 함수를 배열형으로 리턴합니다.
- 이때 쿼리 값은 `SearchParams`  의 `get` 함수로 가져옵니다.

```jsx
import { useSearchParams } from 'react-router-dom';

function PostListPage() {
  const [searchParams, setSearchParams] = useSearchParams();
  const filterQuery = searchParams.get('filter');

  // ...
}
```

- 만약 쿼리 값을 변경하고 주소를 이동하고 싶다면 Setter 함수에 객체를 넘겨주면 됩니다.
- 이때 객체의 프로퍼티로 쿼리 값을 지정할 수 있습니다.
- 아래 예시는 `?filter=react` 라는 쿼리로 이동하는 예시입니다.

```jsx
setSearchParams({
  filter: 'react',
});
```

## 페이지 이동하기

## Navigate 컴포넌트

- 리턴값으로 `Navigate` 컴포넌트를 리턴하면 `to` prop으로 지정한 경로로 이동합니다.

```jsx
function PostPage() {
  // ...

  const post = getPost(postId);

  // post가 없는 경우 /posts 페이지로 이동
  if (!post) {
    return **<Navigate to="/posts" />;**
  }

  // ...
}
```

## useNavigate Hook

- `useNavigate` 라는 hook으로 `navigate` 함수를 가져오면 이 함수를 통해 페이지를 이동할 수 있습니다.

```jsx
const navigate = useNavigate();

const handleClick = () => {
  // ... 어떤 작업을 한 다음에 페이지를 이동
  **navigate('/wishlist');**
}
```

## Link, Navigate, useNavigate의 쓰임새

- 3가지 모두 다 페이지를 이동할 때 쓸 수 있다는 점에서 비슷합니다.

## Link

- **사용자가 클릭해서 페이지를 이동하도록 할 때** 사용하면 됩니다.
- 하이퍼링크 텍스트나 페이지를 이동하는 버튼, 이미지 등에 사용하면 됩니다.
- 대부분의 경우 `Link`  만으로도 충분합니다.

## Navigate

- 특정 경로에서 **렌더링 시점에 다른 페이지로 이동시키고 싶을 때** 사용하면 됩니다.

예시:

- 쇼핑몰의 회원 전용 페이지에 로그인 없이 들어와서 로그인 페이지로 리다이렉트 하는 경우
- 쇼핑몰의 상품 상세 페이지에서 제품이 품절되었거나 삭제되어서 다른 페이지로 이동시키는 경우

## useNavigate

- 특정한 **코드의 실행이 끝나고 나서 페이지를 이동시키고 싶을 때** 사용하면 됩니다.

예시:

- 쇼핑몰에서 장바구니에 담기를 눌렀을 때 리퀘스트를 보내고 장바구니 페이지로 이동시키는 경우
- 쇼핑몰에서 결제하기 버튼을 누르고 나서 모든 결제가 완료된 후에 페이지를 이동시키는 경우
- 리다이렉트된 로그인 페이지에서 로그인을 완료한 후에 처음 진입했던 페이지로 돌아가는 경우

