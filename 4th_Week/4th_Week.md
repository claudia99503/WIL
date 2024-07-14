# 자바스크립트로 리퀘스트 보내기

# fetch 정리

## `fetch()` 기본 문법

```jsx
const res = await fetch(url);

// 리스폰스 상태 코드
res.status;

// 리스폰스 헤더
res.headers;

// 리스폰스 바디
await res.json(); // JSON 문자열을 파싱해서 자바스크립트 객체로 변환
await res.text(); // 문자열을 그대로 가져옴
```

- `res.json()`  메소드는 바디의 JSON 문자열을 파싱해서 자바스크립트 객체로 변환해줍니다.
- `res.text()`  메소드는 바디의 내용을 문자열 그대로 가져옵니다.

 ⇒ 바디의 내용이 JSON 형식이 아닌데 `res.json()` 메소드를 사용하면 파싱 오류가 납니다.

## `fetch()` 사용 예시

```jsx
const res = await fetch('https://learn.codeit.kr/api/color-surveys');
const data = await res.json();
console.log(data);
```

```jsx
{
  count: 51,
  next: 'https://learn.codeit.kr/api/color-surveys/?offset=10&limit=10',
  previous: null,
  results: [
    {
      createdAt: 1688448542000,
      updatedAt: 1688448542000,
      colorCode: '#FFFFFF',
      id: 51,
      mbti: 'ENTJ'
    },
    {
      createdAt: 1688448539000,
      updatedAt: 1688448539000,
      colorCode: '#FFFFFF',
      id: 50,
      mbti: 'ENTJ'
    },
    {
      createdAt: 1688438808000,
      updatedAt: 1688438808000,
      colorCode: '#000000',
      id: 49,
      mbti: 'ISTJ'
    },
    {
      createdAt: 1688377640000,
      updatedAt: 1688377640000,
      colorCode: '#123456',
      id: 48,
      mbti: 'ISFP'
    },
    {
      createdAt: 1688290276000,
      updatedAt: 1688290276000,
      colorCode: '#55497E',
      id: 47,
      mbti: 'ENFP'
    },
    {
      createdAt: 1688205649000,
      updatedAt: 1688205649000,
      colorCode: '#591DD3',
      id: 46,
      mbti: 'INFJ'
    },
    {
      createdAt: 1688205637000,
      updatedAt: 1688205637000,
      colorCode: '#000000',
      id: 45,
      mbti: 'INFJ'
    },
    {
      createdAt: 1688191191000,
      updatedAt: 1688191191000,
      colorCode: '#DB59F1',
      id: 44,
      mbti: 'ESTJ'
    },
    {
      createdAt: 1688190719000,
      updatedAt: 1688190719000,
      colorCode: '#FD1B6C',
      id: 43,
      mbti: 'ESTJ'
    },
    {
      createdAt: 1688104686000,
      updatedAt: 1688104686000,
      colorCode: '#ABCD00',
      id: 42,
      mbti: 'ENFJ'
    }
  ]
}
```

## `URL` 객체

- 쿼리 파라미터를 보낼 때는 URL 객체를 사용할 수 있습니다.

```jsx
const params = { offset: 10, limit: 10 };
const url = new URL('https://learn.codeit.kr/api/color-surveys');
Object.keys(params).forEach((key) => url.searchParams.append(key, params[key]));
const res = await fetch(url);
const data = await res.json();
console.log(data);
```

## `fetch()` 옵션

### 리퀘스트 설정을 할 수 있습니다.

- `method` (메소드): `'GET'` , `'POST'` , `'PATCH'` , `'DELETE'` 같은 값으로 설정할 수 있습니다.

 ⇒ `method` 를 설정하지 않으면 기본 값이 `'GET'` 입니다.

- `headers` (헤더) : 자주 설정하는 헤더로는 `'Content-Type'` 가 있습니다.
- `body` (바디) : 자바스크립트 객체는 그대로 전달할 수 없기 때문에 JSON 문자열로 바꿔줘야 합니다.

## POST 리퀘스트 예시

```jsx
const surveyData = {
  mbti: 'ENFP',
  colorCode: '#ABCDEF',
    password: '0000',
};

const res = fetch('https://learn.codeit.kr/api/color-surveys', { 
  method: 'POST' ,
  body: JSON.stringify(surveyData),
  headers: {
    'Content-Type': 'application/json',
  },
});
const data = await res.json();
console.log(data);
```

```jsx
{
  createdAt: 1688477153992,
  updatedAt: 1688477153992,
  colorCode: '#ABCDEF',
  id: 52,
  mbti: 'ENFP',
  password: '0000'
}
```

## API 함수 만들기

- 똑같은 리퀘스트를 보내는 코드가 반복된다면 함수로 만들어주는 것이 좋습니다.
- 보통 웹 개발을 할 때는 API를 호출하는 함수들을 따로 모아 두고 필요할 때 import해서 씁니다.
- 예를 들어 `api.js` 라는 파일에 아래와 같은 함수들을 만들어 놓을 수 있습니다.

`api.js`

```jsx
export async function getColorSurveys(params = {}) {
  const url = new URL('https://learn.codeit.kr/api/color-surveys');
  Object.keys(params).forEach((key) =>
    url.searchParams.append(key, params[key])
  );
  const res = await fetch(url);
  const data = await res.json();
  return data;
}

export async function getColorSurvey(id) {
  const res = await fetch(`https://learn.codeit.kr/api/color-surveys/${id}`);
  const data = await res.json();
  return data;
}

export async function createColorSurvey(surveyData) {
  const res = await fetch('https://learn.codeit.kr/api/color-surveys', {
    method: 'POST',
    body: JSON.stringify(surveyData),
    headers: {
      'Content-Type': 'application/json',
    },
  });
  const data = await res.json();
  return data;
}

```

- 리퀘스트마다 바뀔 수 있는 부분을 함수 파라미터 받아서 활용하면 됩니다.

## `fetch()` 오류 처리

리퀘스트를 보낼 때 발생하는 오류는 크게 두 가지가 있습니다.

1. URL이 이상하거나 헤더 정보가 이상해서 리퀘스트 자체가 실패하는 경우
2. 리퀘스트는 성공적이지만 상태 코드가 실패를 나타내는 경우(4XX, 5XX)
- `fetch()` 함수는 첫 번째 경우에만 리턴하는 Promise를 reject합니다.

 ⇒ `fetch()` 에 대한 오류 처리를 할 때는 언제 PRomise가 reject되는지, 어떤 내용이 리스폰스 바디로 돌아오는지를 잘 생각해야 합니다.

- `fetch()` 오류를 깔끔하게 처리하는 방법은 두 번째 경우에도 오류를 `throw` 하는 것입니다.
- 리스폰스 상태 코드의 성공 여부는 `.ok`  프로퍼티로 확인할 수 있습니다.

```jsx
export async function getColorSurvey(id) {
  const res = await fetch(`https://learn.codeit.kr/api/color-surveys/${id}`);

  if (!res.ok) {
    throw new Error('데이터를 불러오는데 실패했습니다.');
  }

  const data = await res.json();
  return data;
}
```

- 그러면 리퀘스트가 완전히 성공하지 않는 이상 오류를 `throw` 하기 때문에 쉽게 처리할 수 있습니다.
- 여기서 리퀘스트가 완전히 성공한다는 건, 리퀘스트가 잘 전달되고 리스폰스도 성공을 나타내는 상황을 말합니다.

