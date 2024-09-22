# React Query

- 웹사이트에서 필요한 데이터의 개수가 적은 간단한 사이트 => `React` 의 `state` 와 `props`  정도로 해결 가능합니다.
- 우리가 이용하는 대부분의 사이트 ⇒ 매우 다양한 데이터들이 필요합니다.
- `React Context`  : 전역적으로 쓰이는 데이터를 관리하는 방법
    - `prop drilling` 문제를 해결해줍니다.
    - ⇒ but, 만능은 아닌지라 부족하거나 아쉬운 부분에 대해 보완이 필요합니다.
    - ⇒ `Redux` , `Recoil` 등 `React` 와 함께 사용할 수 있는 다양한 상태 관리 라이브러리들이 등장했습니다. ( 클라이언트 상태 데이터들을 잘 관리하기 위해 나타났습니다.)
- 상태를 관리하는 데이터
    - 클라이언트 상태 데이터 : 보통 자기 컴퓨터에서만 사용하는 데이터 입니다.  ( 위에서 언급한 상태 관리 라이브러리들 )
    1️⃣ **웹사이트의 어떤 메뉴가 열렸는지 닫혔는지, 혹은 사용자가 어떤 버튼을 눌렀는지 아닌지와 같은** **UI 상태 값**
    2️⃣ **유저가 입력 폼에 입력한 데이터 등 서버와는 상관없이 웹 브라우저 안에서만 사용하는 데이터입니다.**
    ⇒ 서버 상태 데이터를 관리하기엔 잘 맞지 않는 부분도 있고, 자칫 코드가 복잡해지는 문제가 있습니다.
    - 서버 상태 데이터 : 서버에서 가져온 데이터 입니다
    1️⃣ **예를 들어 네이버에 접속했을 때 우리가 보는 뉴스기사나 각종 글은 모두 서버에서 받아오는 서버 상태 데이터입니다.**

**⇒ `React Query`  : 서버 상태 데이터만을 집중적으로 관리합니다.**

# React Query 기초

## 서버 상태 ( Server State)

- 고려해야될 상황
    - 데이터를 받아오는 중에는 어떤 화면을 보여줄 것인지
    - 로딩과 에러상황은 어떻게 알 수 있을지
    - 서버로부터 받아 온 데이터들은 어떻게 관리할지
    - 데이터를 주기적으로 받아오려면 어떤 식으로 구현해야 할지 ( 초 단위 or  몇 분마다도 고려사항 )
    
      ⇒ `Redux` 같은 러브러리에서는 이런 서버 데이터들의 특성에 맞게 구현하고 데이터들을 관리하는 게 쉽지   
    않았습니다.
    

⇒ 이런 상황을 해결하기 위해 `React Query` 가 등장하였습니다.

## `React Query`

- 데이터를 가져오는 과정에서 로딩과 에러 처리를 쉽게 구현할 수 있도록 여러 값을 제공해줍니다.
- 정해진 시간 혹은 조건마다 서버 상태 데이터를 최신으로 가져오는 작업도 알아서 해줍니다.
- 캐시 ( cache ) 라는 걸 사용해서 매번 서버에서 데이터를 가져올 필요 없이 유저에게 더 빠르게 데이터를 보여주기도 합니다.
- 페이지네이션이나 Infinite Query, Optimistic Update와 같은 웹사이트들에서 자주 사용하는 기능을 손쉽게 구현할 수 있도록 해줍니다.

# 리액트 쿼리 설치하기

- 터미널에서 다음 명령어를 실행하면 됩니다.

```jsx
npm install @tanstack/react-query
```

- `Home.js`  라는 파일을  만들고 `App.js` 파일에 아래와 같이 입력합니다.

`src/HomPage.js`

```jsx
function HomePage() {
  return <div>홈페이지</div>;
}

export default HomePage;
```

`src/App.js`

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import HomePage from './HomePage';

**const queryClient = new QueryClient();**

function App() {
  return (
    **<QueryClientProvider client={queryClient}>**
      <HomePage />
    **</QueryClientProvider>**
  );
}

export default App;
```

- `@tanstack/react-query` 패키지에서 `QueryClient` 를 import해서 새로운 **쿼리 클라이언트**를 만들어주었습니다.
- 그리고 그것을 `QueryClientProvider` 를 통해 App 컴포넌트의 자손 컴포넌트에 전달해 주었습니다.
- `React Context` 와 같은 패턴
    - 데이터를 전역적으로 사용하기 위해서 먼저 `Context Provider`  를 설정
    
      ⇒ 리액트 쿼리에서는 그것이 바로 `QueryClientProvider` 
    

 ⇒ `QueryClientProvider` 를 통해 쿼리 클라이언트를 제공해 줘야 그 아에 있는 자손 컴포넌트에서 리액트 쿼리를 사용할 수 있게 됩니다.

- import를 할 때는 반드시 `react-query` 가 아닌 `@tanstack/react-query` (최신 버전) 에서 합니다.

## 리액트 쿼리 개발자 도구 **( React Query Devtools )**

- 터미널에 다음 명령어를 실행하면 됩니다.

```jsx
npm install @tanstack/react-query-devtools
```

- 그런 다음에 아래와 같이 리액트 쿼리 개발자 도구를 위한 코드를 추가해 줍니다.

`src/App.js`

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactQueryDevtools } from '@tanstack/react-query-devtools';
import HomePage from './HomePage';

const queryClient = new QueryClient();

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <HomePage />
      <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}

export default App;
```

- `initialIsOpen` 은 리액트 쿼리 개발자 도구가 열려있는 채로 실행할 것인가를 선택하는 옵션입니다.
- 일단 `false` 로 하고 터미널에서 `npm start` 를 입력해 앱을 실행해보면 오른쪽 아래에 플로팅 버튼이 떠있습니다.

![플로팅버튼.jpeg](/13th_Week/플로팅버튼.jpeg)

- 버튼을 클릭해보면 개발자 도구 화면이 잘 보이는 것을 확인할 수 있습니다.

![개발자도구.jpeg](/13th_Week/개발자도구.jpeg)

# useQuery로 데이터 받아오기

## 쿼리란?

- ‘문의하다, 질문하다’ 라는 뜻을 가지고 있는 단어입니다.
- 데이터베이스 같은 것에 우리가 필요한 데이터를 요청하는 것을 말합니다.

  ⇒  **`useQuery()` 는 필요한 데이터를 백엔드에 요청해서 받아 오는 React Hook입니다.**

## `useQuery`

- `api.js` : 백엔드에서 데이터를 받아 올 함수
- `getPosts()` :  백엔드로부터 모든 포스트 목록을 받아오는 함수

`src/api.js`

```jsx
const BASE_URL = 'https://learn.codeit.kr/api/codestudit';

export async function getPosts() {
  const response = await fetch(`${BASE_URL}/posts`);
  return await response.json();
}
```

- `HomePage` 컴포넌트에서는 `useQuery()` 를 import한 다음 실행합니다.

`src/HomePage.js`

```jsx
import { useQuery } from '@tanstack/react-query';
import { getPosts } from './api';

function HomePage() {
  const result = useQuery({ queryKey: ['posts'], queryFn: getPosts });
  console.log(result);

  return <div>홈페이지</div>;
}

export default HomePage;
```

- `useQuery()`  의 리턴 값?

![useQuery로 데이터받아오기.jpeg](/13th_Week/useQuery로데이터받아오기.jpeg)

## `useQuery()` 리턴 값 살펴보기

- `useQuery()` 훅의 리턴 값에는 많은 것 들이 있습니다.
- 데이터
    - `data` 에는 우리가 백엔드에서 받아온 데이터들이 들어 있습니다.
    - 위의 예시를 보면 리스폰스 바디로 받은 데이터가 객체로 되어있고, 페이지네이션에 필요한 정보들과 함께 `results` 란 항목에 실제 포스트 데이터가 배열로 들어가 있는 것을 볼 수 있습니다.
- 데이터를 받아온 시간
    - `dataUpdatedAt` : 현재의 데이터를 받아온 시간을 나타내는 항목입니다.
    - 이 시간을 기준으로 언제 데이터를 refetch 할 것인지 등을 정하게 됩니다.
- 다양한 상태 정보
    - `isError` , `isFetched` , `isPending` , `isPaused` , `isSuccess` 와 같은 다양한 상태 정보도 확인할 수 있습니다.
    - `status` 라는 항목에 `success` : 데이터를 성공적으로 받아왔다는 뜻입니다.

# Query Status와 Fetch Status

- 리액트 쿼리 ⇒ 2가지 `status` 존재
- `query status` : 실제로 받아온 `data` 값이 있는지 없는지를 나타내는 상태 값입니다.
    - `useQuery()` 결괏값에서 `status` 값을 통해 확인할 수 있습니다.
- `fetch status` : `queryFn()` 함수가 현재 실행되는 중인지 아닌지를 나타냅니다.
    - `fetchStatus` 값을 통해 확인할 수 있습니다.

## Query Status

- 3가지 상태 값을 가집니다.
- `pending` , `error` , `success` 의 상태값중 하나를 가지게 됩니다.
- `pending` ⇒ `isPending` 와 매칭
    - 아직 데이터를 받아오지 못했을 때를 의미합니다.
- `error` ⇒ `isError` 와 매칭
    - 데이터를 받아오는 중에 에러가 발생했음을 뜻합니다.
- `success` ⇒ `isSuccess` 와 매칭
    - 데이터를 성공적으로 받아왔음을 의미합니다.

<br>

- `useQuery()` 의 결괏값을 콘솔에 찍어보면 결괏값이 두 번 출력됩니다.
- 이 중 첫 번째 결괏값을 살펴보면 `status` 가 `'pending'`  상태임을 볼 수 있습니다.

![QueryStatus값.jpeg](/13th_Week/QueryStatus값.jpeg)

- `'pending'` 상태
: 가장 처음 컴포넌트가 마운트 되고 ( DOM 트리에 추가되고 ) `useQuery()` 가 실행되면서, 데이터를 아직 받아오기 전
- 그 후에 찍힌 두 번째 결괏값 `status` 가 `'success'` 상태

    : `data` 항목에서 실제 데이터들을 확인할 수 있습니다.


![pending,success상태.jpeg](/13th_Week//pending,success상태.jpeg)

- `status` 가 `error` 상태

  : 데이터를 받아오는 중에 에러가 발생

 ⇒ 예를 들어 API 주소를 없는 주소로 바꿔 보거나, 혹은 쿼리 함수에서 일부러 에러를 만들어 throw 해 보면 `status` 가 `'error'`  상태로 바뀌고

⇒ `useQuery()` 결괏값의 `error` 값에서 그 에러를 살펴볼 수 있습니다.

`src/api.js`

```jsx
export async function getPosts() {
  const response = await fetch(`${BASE_URL}/posts`);
  const body = await response.json();

  // 일부러 에러 발생
  throw new Error('An error happened.');
  return body;
}
```

![error상태.jpeg](/13th_Week//error상태.jpeg)

## Fetch Status

- `useQuery()`  를 사용할 때 쿼리 함수라는 걸 `queryFn`  으로 등록해줍니다

  ⇒ 이 쿼리 함수의 실행 상태를 말해주는 값이 바로 `fetch status`  입니다.

- 총 3가지 상태 값을 가집니다.
- `'fetching'`
    - 현재 쿼리 함수가 실행 되는 중입니다.
- `'paused'`
    - 만약 쿼리 함수가 시작은 했는데 실제로 실행되고 있지는 않은 상태입니다.
    : 대표적으로 네트워크가 오프라인 된 경우입니다.
- `'idle'`
    - 쿼리 함수가 어떤 작업도 하고 있지 않은 상황
    - 즉, `'fetching'` 상태도 아니고 `'paused'` 상태도 아닌 상태

## Query Status와 Fetch Status 다이어그램

![QueryStatus와FetchStatus다이어그램jpeg](/13th_Week/QueryStatus와FetchStatus다이어그램.jpeg)

- 먼저 처음으로 컴포넌트가 마운트되어 `useQuery()` 가 실행되면, 데이터를 아직 받아오지 못했기 때문에 query status는 `'pending'` 이 됩니다.
- 그리고 쿼리 함수가 실행되면서 fetch status는 `'fetching'`  상태가 됩니다.

  : 만약 네트워크 상태가 오프라인일 때 쿼리 함수가 실행된다면, fetch status는 `'paused'`  상태가 됩니다.

- 데이터를 성공적으로 받았다면 query status는 `'success'` 가 되고,
- 만약 데이터를 받아오는 과정에서 에러가 발생했다면 `'error'` 상태가 됩니다.

⇒ fetch status는 데이터를 성공적으로 가져왔는지 여부에 상관없이, 쿼리 함수의 실행이 끝나면 `'idle'` 상태가 됩니다.

⇒ 그 이후에 데이터를 서버에서 다시 받아오는 refetch 작업이 발생하면 쿼리 함수가 재실행되면서 다시 `'fetching'` 상태로 가게 됩니다.

## 정리

- `query status` 와 `fetch status`  는 엄연히 독립적인 상태입니다.

⇒ 다양한 조합의 형태로 나타날 수 있습니다.

- 이상적인 상황에서는 `pending & fetching` 상태에서 `success & idle` 상태가 됩니다.
- 에러가 발생하는 경우 `error & idle` 상태가 됩니다.
- `success & idle` 상태에서 데이터를 refetch하게 되면

⇒ `success & fetching`  상태가 되기도 합니다.
⇒ `query status`  와 `fetch status` 값을 잘 활용하면, 다양한 상황에 맞춰 디테일한 구현을 할 수 있습니다.

# 다양한 쿼리 키와 쿼리 함수의 형태

- 홈 피드 ⇒ 모든 포스트를 다 받아와서 보여줍니다.
- 내 피드 ⇒ 사이트에 로그인한 특정 유저의 포스트만 보여줘야 합니다.
    - 백엔드로부터 특정 유저의 포스트만 받아오는 API 함수가 필요합니다.

`src/api.js` 

```jsx
export async function getPostsByUsername(username) {
  const response = await fetch(`${BASE_URL}/**posts?username=${username}**`);
  return await response.json();
}
```

- 이렇게 가져온 데이터를 어떻게 캐시에 저장하면 좋을지? 의 문제

⇒ 전체 포스트를 저장하고 있는 `['posts']` 쿼리 키와는 구분되는 방식으로 특정 유저의 포스트 데이터만 따로 저장하면 됩니다.

⇒ 아래와 같이 계층적으로 쿼리 키를 지정하는 것이 가능합니다.

```jsx
function HomePage() {
  **const username = 'codeit'; // 임의로 username을 지정**
  const { data: postsDataByUsername } = useQuery({
    queryKey: ['posts', **username**],
    queryFn: () => getPostsByUsername(**username**),
  });
  console.log(postsDataByUsername);
  
  return <div>홈페이지</div>;
}
```

- 이렇게 하면 특정 `username`  에 대한 쿼리만 따로 캐싱이 됩니다.

![username쿼리.jpeg](/13th_Week/username쿼리.jpeg)

- 이렇게 배열을 활용해 계층적인 쿼리 키를 설정하는 것이 가능하고, 또 상황에 따라 다양한 파라미터를 활용해 쿼리 키를 설정할 수도 있습니다.
- 만약 포스트를 나만 볼 수 있는 `private`  상태로 지정할 수 있으면 
⇒ 특정 유저의 포스트 중 `private` 상태의 포스트만 받아 와서 다음과 같은 쿼리 키로 저장할 수 있습니다.

```jsx
const { data: postsDataByUsername } = useQuery({ 
    queryKey: ['posts', username, { **status: private** }], 
    **queryFn: () => getPrivatePostsByUsername(username)** 
});
```

- 위의 코드를 보면 `queryFn` 에서 화살표 함수 형태로 바꿔서 아규먼트로 `username`  을 전달해 준 것을 볼 수 있습니다.

⇒ 아규먼트를 전달해주어야 하는 상황에서는 이렇게 화살표 함수로 만들어주면 됩니다.

- 만약 쿼리 키에 있는 값을 아규먼트로 전달하고 싶다면 다음과 같이 사용할 수도 있습니다.
- 쿼리 함수의 파라미터로 `queryKey` 가 전달됩니다.
- 쿼리 키에 있는 **`username`**  이라는 값을 이용하고 싶으면 다음과 같이 `queryKey` 배열의 1번 인덱스 요소를 아규먼트로 넣어주면 됩니다.

```jsx
const { data: postsDataByUsername } = useQuery({ 
    **queryKey**: ['posts', **username**], 
    **queryFn: ({ queryKey }) => getPostsByUserId(queryKey[1])**, 
});
```

- 혹은 다음과 같이 쿼리 키에서 객체의 특정한 값을 가져와 활용할 수도 있습니다.

```jsx
const username = 'codeit';
const { data: postsDataByUsername } = useQuery({ 
    **queryKey**: ['posts', { **username** }], 
    **queryFn: ({ queryKey }) => {
        const [key, { username }] = queryKey; 
        return getPostsByUserId(username);**
    } 
});
```

- 여기서 한 가지 주의할 점이 있습니다.
- 객체를 쿼리 키로 전달하면 그 안에서는 순서에 상관없이 같은 값들을 가지고 있는 객체라면 같은 쿼리 키로 인식하지만,
- 배열을 쿼리 키로 전달하면 요소의 순서가 중요합니다.

⇒ 순서가 달라지면 다른 쿼리 키로 인식하기 때문에 배열의 요소로 쿼리 키를 지정할 경우 순서에 꼭 유의해야합니다.

```jsx
// 다음 세 가지는 모두 같은 쿼리로 인식한다
useQuery({ queryKey: ['posts', { username, userEmail }], ... });
useQuery({ queryKey: ['posts', { userEmail, username }], ... });
useQuery({ queryKey: ['posts', { userEmail, username, other: undefined }], ... });

// 다음 세 가지는 모두 다른 쿼리로 인식한다
useQuery({ queryKey: ['posts', username, userEmail], ... });
useQuery({ queryKey: ['posts', userEmail, username], ... });
useQuery({ queryKey: ['posts', undefined, userEmail, username], ... });
```

# 자주 쓰는 옵션과 리턴 값

- `useQuery()` 에서 리턴하는 객체는 정말 많은 값들이 있습니다.
- 리턴 값들 중 가장 많이 활용하는 것은 백에드에서 받아 온 데이터가 있는 `data` 와 그 데이터를 받아오는 과정에서 보게 되는 `pending` , `error` 와 같은 status 관련 값들입니다.

⇒ status값들을 활용해 상황에 맞는 로딩과 에러 메시지를 보여주는 방법

- 처음 데이터를 받아오는 과정이라 아직 데이터가 없는 상황 ⇒ `query status` 가 `pending` 상태

  :  `useQuery` 의 리턴값들 중에는 현재 `pending` 상태인지 아닌지를 확인하는 `isPending`  존재

⇒ `isPending` 이 `true` 인 상황에서는 로딩 중이라는 메시지를 보여주면 됩니다.

⇒ 또한 쿼리 함수를 실행하던 중에 에러가 발생하면 쿼리가 `error` 상태로 인식됩니다.

`src/api.js`

```jsx

const { error, isError } = useQuery({
  queryKey: ['posts'],
  queryFn: async (key) => {
    throw new Error('An error occurred!');
  },
})
console.log(error);
console.log(isError);
```

- 쿼리 함수 내에서 의도적으로 에러를 발생시켜 에러가 발생하면 `isError` 라는 값이 `true`  가 됩니다.
- `isError`  라는 값이 `true` 가 되면 적절한 메시지를 보여주는 식으로 에러 처리를 구현 가능합니다.

## 로딩과 에러 처리 구현하기

- `isPending` 과 `isError` 값을 이용해서 로딩과 에러 처리를 구현할 수 있습니다.
- 아래와 같이 해당 상황에서 적절한 메시지를 리턴하면 됩니다.
- 리액트 쿼리에서는 에러가 발생하면 기본적으로 3번의 재시돌르 하는데, 테스트 할 때는 `retry`  횟수를 0으로 조정하면 에러 화면을 더 빨리 볼 수 있어서 편하게 테스트 할 수 있습니다.

`scr/HomePage.js`

```jsx
function HomePage() {
  const {
    data: postsData,
    isPending,
    isError,
  } = useQuery({
    queryKey: ['posts'],
    queryFn: getPosts,
    **retry: 0**,
  });

  if (isPending) return '로딩 중입니다...';

  if (isError) return '에러가 발생했습니다.';

  const posts = postsData?.results ?? [];

  return (
    <div>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            {post.user.name}: {post.content}
          </li>
        ))}
      </ul>
    </div>
  );
}
```

- 위의 코드를 실행하고, 크롬 개발자 도구의 네트워크 탭에서 네트워크 속도를 Slow 3G로 바꾸고 새로 고침해보면 다음과 같이 로딩 메세지를 볼 수 있습니다.

![로딩메세지.jpeg](/13th_Week/로딩메세지.jpeg)

- 그리고 쿼리 함수에서 의도적으로 에러를 발생시켜 보면 에러 메시지를 볼 수 있습니다.

`src/api.js`

```jsx
export async function getPosts() {
  const response = await fetch(`${BASE_URL}/posts`);
  throw new Error('An error occurred!');
}
```

![로딩오류메세지.jpeg](/13th_Week/로딩오류메세지.jpeg)

# useMutation으로 데이터 추가하기

- 뮤테이션: ‘형태나 구조상의 변화’

: 사이드 이펙트를 가진 함수를 의미합니다.

: 데이터베이스에 새로운 값을 추가하거나 수정, 삭제 하는 행위

⇒ 사이드 이펙트가 발생하는 경우에 `useMutation()` 이라는 훅을 사용합니다.

## `useMutation()`  vs `useQuery()`

- `useQuery()` 의 쿼리 함수
    - 컴포넌트가 마운트되면서 자동으로 실행됩니다.
- `useMutation()`
    - 실제로 뮤테이션하는 함수를 직접 실행해줘야 합니다.
    - `mutate()` 함수를 통해 `mutationFn` 으로 등록했던 함수를 실행할 수 있고
    
    ⇒ 그래야만 백엔드 데이터를 실제로 수정하게 됩니다.
    
    - `mutate()` 를 하면 백엔드의 데이터는 변경이 되지만, 현재 캐시에 저장된 데이터는 refetch를 하지 않는 이상 기존의 데이터가 그대로 저장되어 있습니다.
    
    ⇒  refetch를 해줘야만 변경된 데이터를 화면에 제대로 반영할 수 있습니다.
    

## `useMutation()` 으로 데이터 추가하기

- 포스트 업로드를 요청하는 API 함수입니다.

`src/api.js`

```jsx
export async function uploadPost(newPost) {
  const response = await fetch(`${BASE_URL}/posts`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(newPost),
  });

  if (!response.ok) {
    throw new Error('Failed to upload the post.');
  }

  return await response.json();
}
```

- 그런 뒤에 다음과 같이 간단한 form을 만들어서 새로운 포스트를 입력받을 수 있게 할 수 있습니다.

`src/HomePage.js`

```jsx
const [content, setContent] = useState('');

// ...

const handleInputChange = (e) => {
  setContent(e.target.value);
}

const handleSubmit = (e) => {
  e.preventDefault();
  const newPost = { username: 'codeit', content };
    // ...
};

return (
  <>
    <div>
      <form onSubmit={handleSubmit}>
        <textarea
          name='content'
          value={content}
          onChange={handleInputChange}
        />
        <button
          disabled={!content}
          type='submit'
        >
          업로드
        </button>
      </form>
    </div>
    <div>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            {post.user.name}: {post.content}
          </li>
        ))}
      </ul>
    </div>
  </>
);
```

- 마지막으로 다음과 같이 `useMutation()` 을 작성하고, 업로드 버튼을 눌렀을 때 `mutate()`  함수를 실행하도록 하면 됩니다.

```jsx
const uploadPostMutation = **useMutation({
  mutationFn: (newPost) => uploadPost(newPost),
});**

const handleSubmit = (e) => {
  e.preventDefault();
  const newPost = { username: 'codeit', content };
  uploadPostMutation.mutate(newPost);
  setContent('');
};
```

- 전체 코드입니다.

`src/HomPage.js`

```jsx
import { useState } from 'react';
import { useMutation, useQuery } from '@tanstack/react-query';
import { getPosts, uploadPost } from './api';

function HomePage() {
  const [content, setContent] = useState('');
  const {
    data: postsData,
    isPending,
    isError,
  } = useQuery({
    queryKey: ['posts'],
    queryFn: getPosts,
    retry: 0,
  });

  const uploadPostMutation = useMutation({
    mutationFn: (newPost) => uploadPost(newPost),
  });

  const handleInputChange = (e) => {
    setContent(e.target.value);
  }

  const handleSubmit = (e) => {
    e.preventDefault();
    const newPost = { username: 'codeit', content };
    uploadPostMutation.mutate(newPost);
    setContent('');
  };

  if (isPending) return '로딩 중입니다...';

  if (isError) return '에러가 발생했습니다.';

  const posts = postsData?.results ?? [];

  return (
    <>
      <div>
        <form onSubmit={handleSubmit}>
          <textarea
            name="content"
            value={content}
            onChange={handleInputChange}
          />
          <button disabled={!content} type="submit">
            업로드
          </button>
        </form>
      </div>
      <div>
        <ul>
          {posts.map((post) => (
            <li key={post.id}>
              {post.user.name}: {post.content}
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}

export default HomePage;
```

- 포스트를 등록합니다.

![포스트등록.jpeg](/13th_Week/포스트등록.jpeg)

❗️업로드 버튼을 눌러도 새로 등록한 포스트가 화면에 보이지 않는 문제 발생 ⇒ `invalidateQueries()` 함수로 해결

![업로드안되는문제.jpeg](/13th_Week//업로드안되는문제.jpeg)

⇒ `mutate()` 를 한다고 해서 캐시에 있는 데이터가 변경이 되는 것이 아니기 때문입니다.

⇒ 따라서 데이터를 refetch해야 새로운 데이터를 보여줄 수 있습니다.

⇒ 해당화면에서 새로고침을 해야 새로운 포스트까지 확인할 수 있습니다.

# `useMutation`  콜백으로 데이터 바로 업데이트 하기

- 전에 `useMutation()` 훅을 이용해 새로운 데이터를 추가할 때의 문제

 ⇒ 캐시에 있는 데이터가 업데이트되지 않아, 새로운 데이터를 확인하려면 새로고침을 해 줬어야 했습니다.

## `invalidateQueries()`  함수

- 클라이언트의 `invalidateQueries()`  라는 함수를 사용하면 업로드가 끝난 이후에 자동으로 refetch를 하도록 설정할 수 있습니다.
- 캐시에 있는 모든 쿼리 혹은 특정 쿼리들을 invalidate 하는 함수입니다.
- `invalidate` : ‘무효화하다’ ⇒ 캐시에 저장된 쿼리를 무효화한다는 의미입니다.
- 쿼리를 invalidate하면 해당 쿼리를 통해 받아 온 데이터를 stale time이 지났는지 아닌지에 상관없이 stale 상태로 만들고, 해당 데이터를 백그라운드에서 refetch 하게 됩니다.

⇒ 쿼리 클라이언트는 `useQueryClient()`  훅을 사용해서 가져올 수 있고 원하는 시점에 `queryClient.invalidateQueries()` 함수를 실행하면 됩니다.

```jsx
import { useQueryClient } from '@tanstack/react-query'

const queryClient = useQueryClient();

// ...

queryClient.**invalidateQueries()**;
```

⇒ 새로 데이터를 추가했을 때, 해당 쿼리를 `invalidate` 하면 데이터를 자동으로 refetch 할 수 있게 됩니다.

⇒ 새롭게 업로드 된 포스트도 바로 보여 줄 수 있습니다.

## `useMutation()` 함수의 콜백 옵션

- `onMutate` , `onSuccess` , `onError` , `onSettled` 와 같은 옵션들이 있어서 뮤테이션 사이클에 따라 적절한 동작을 추가할 수 있습니다.
- `onSuccess` , 즉 뮤테이션이 성공한 시점에 `['post']`  쿼리를 invalidate 해주는 함수를 콜백으로 등록해주면 됩니다.
- 아래 코드를 추가하면 포스트를 업로드 하자마자 업로드된 포스트까지 화면에 잘 보이는 것을 확인할 수 있습니다.

```jsx

const queryClient = useQueryClient();

// ...

const uploadPostMutation = useMutation({
  mutationFn: (newPost) => uploadPost(newPost),
  **onSuccess**: () => {
    queryClient.invalidateQueries({ queryKey: ['posts'] });
  },
});
```

![useMutation입력중.jpeg](/13th_Week/useMutation입력중.jpeg)

![useMutation성공.jpeg](/13th_Week/useMutation성공.jpeg)

## `mutate()` 함수의 콜백 옵션

- 이와 같은 `onSuccess`  , `onError`  , `onSettled`  와 같은 옵션은 `useMutation()`  에서도 사용할 수 있고 `mutate()` 함수에서도 사용할 수 있습니다.
- 이때 `useMutation()`  에 등록한 콜백 함수들이 먼저 실행되고, 그 다음에 `mutate()` 에 등록한 콜백 함수들이 실행됩니다.

```jsx
const uploadPostMutation = useMutation({
  mutationFn: (newPost) => uploadPost(newPost),
  onSuccess: () => {
    console.log('onSuccess in useMutation');
  },
  onSettled: () => {
    console.log('onSettled in useMutation');
  },
});

...

uploadPostMutation.mutate(newPost, {
  onSuccess: () => {
    console.log('onSuccess in mutate');
  },
  onSettled: () => {
    console.log('onSettled in mutate');
  },
});
```

- 여기서 한 가지 주의할 점이 있습니다.
- `useMutation()` 에 등록된 콜백 함수들은 **컴포넌트가 언마운트되더라도 실행되지만**,
- `mutate()` 의 콜백 함수들은 **만약 뮤테이션이 끝나기 전에 해당 컴포넌트가 언마운트되면 실행되지 않는 특징**을 가지고 있습니다.

⇒ 따라서 **`query invalidation` 과 같은 뮤테이션 과정에서 꼭 필요한 로직은 `useMutation()` 을 통해 등록하고**,

⇒ **그 외에 다른 페이지로 리다이렉트 한다든가, 혹은 결과를 토스트로 띄워주는 것과 같이 해당 컴포넌트에 종속적인 로직은 `mutate()` 를 통해 등록해주면 됩니다.**

```jsx
...

const uploadPostMutation = useMutation({
  mutationFn: (newPost) => uploadPost(newPost),
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['posts'] });
  },
});

const handleUploadPost = (newPost) => {
  uploadPostMutation.mutate(newPost, {
    onSuccess: () => {
      toast('포스트가 성공적으로 업로드 되었습니다!');
    },
  });
};
```

## `isPending` 프로퍼티 활용하기

- 포스트가 업로드 되는 중에는 중복해서 업로드 하면 안 되니깐 버튼을 비활성해야 합니다.
- 뮤테이션에는 `isPending`  이라는 값이 있습니다.

  ⇒ 다음과 같이 **`uploadPostMutation.isPending`** 값을 이용하면 간단히 구현할 수 있습니다.

```jsx
const uploadPostMutation = useMutation({
  mutationFn: (newPost) => uploadPost(newPost),
  onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['posts'] });
  },
});

// ...

**<button
  disabled={uploadPostMutation.isPending || !content}
  type='submit'
>**
  업로드
</button>
```

# React Query 기초 정리

## `useQuery()` 훅

```jsx
useQuery({
  queryKey: ['posts', username],
  queryFn: () => getPostsByUsername(username),
  staleTime: 60 * 1000,
});
```

- **`useQuery()`  가 있는 컴포넌트가 마운트되면 `useQuery()`  의 쿼리 함수인 `queryFn()`  이 자동으로 실행되면서 알아서 데이터를 가져옵니다.**
- `queryKey` 를 이용해 받아온 데이터를 캐싱할 수 있습니다.
- 이때 특정 데이터만 따로 캐싱하도록 `queryKey`  배열에 값을 추가할 수도 있었습니다.
- `queryFn` 으로는 백엔드에서부터 데이터를 받아오는 함수를 지정하면 됩니다.
- `staleTime`  옵션을 활용해 데이터가 언제까지 fresh 상태를 유지할 것인지 정할 수 있습니다.

## 로딩, 에러 처리하기

```jsx
const {
  data: postsData,
  isPending,
  isError,
} = useQuery({
  queryKey: postsQueryKey,
  queryFn: postsQueryFn,
});

if (isPending) return '로딩 중입니다...';

if (isError) return '에러가 발생했습니다.';
```

- `useQuery()` 의 리턴 값을 활용해서 로딩과 에러 화면 처리를 간편하게 할 수 있었습니다.

## `useMutation()` 사용법

```jsx
const uploadPostMutation = useMutation({
  mutationFn: (newPost) => uploadPost(newPost),
    onSuccess: () => {
    queryClient.invalidateQueries({ queryKey: ['posts'] });
  },
});

const handleUploadPost = (newPost) => {
  uploadPostMutation.mutate(newPost, {
    onSuccess: () => {
      toast('포스트가 성공적으로 업로드 되었습니다!');
    },
  });
};
```

- 데이터베이스에 값을 추가하거나 변경하는 경우 `useMutation()` 을 활용하면 됩니다.
- 알아서 쿼리 함수를 실행하는 `usQuery()`  와는 달리 `mutate()` 함수를 실행해야 `mutationFn` 이 실행됩니다.

# Dependant Query

## Dependant Query란?

- 어떤 두 쿼리가 의존관계가 있어 어떤 특정 순서대로 실행이 되어야 하는 경우에 사용
    - 첫 번째 쿼리 : 유저의 정보를 받아오고
    - 두 번째 쿼리 : 받아온 유저의 정보에서 아이디를 이용해 해당 유저의 프로젝트를 받아오게 됩니다.

```jsx
const { data: user } = useQuery({
  queryKey: ['user', email],
  queryFn: getUserByEmail,
});

const userId = user?.id

const {
  data: projects,
} = useQuery({
  queryKey: ['projects', userId],
  queryFn: getProjectsByUser,
});
```

- 이런 상황에서 `useQuery()` 의 `enabled` 옵션을 사용할 수 있습니다.
- `enabled`  옵션을 사용하면 `enabled` 값이 `true` 가 되어야만 해당 쿼리가 실행됩니다.

 ⇒ 이렇게 어떤 특정 값이나 조건이 충족된 이후에 실행되는 쿼리를 **Dependant Query**라고 합니다.

- 만약 어떤 쿼리가 `useId` 값이 있을 때만 실행하도록 하고 싶으면 다음과 같이 설정해 주면 됩니다.

 ⇒ 그러면 `userId` 가 있는 경우 `true` , 없는 경우 `false` 로 옵션값이 설정됩니다.

```jsx

const { data: user } = useQuery({
  queryKey: ['user', email],
  queryFn: getUserByEmail,
});

const userId = user?.id

const {
  data: projects,
} = useQuery({
  queryKey: ['projects', userId],
  queryFn: getProjectsByUser,
  enabled: !!userId,
});
```

- 위 상황에서는 두 개의 쿼리를 순서대로 실행하려고 `enabled` 옵션을 사용했지만, `enabled` 옵션 값으로 꼭 앞선 쿼리에서 가져온 데이터가 들어가야만 하는 것은 아닙니다.
- 위의 경우에도 백엔드 API 설계에 따라서 두 번의 쿼리가 아니라 한 번의 쿼리로 끝낼 수도 있습니다.
- 예를 들어 `getProjectsByUserEmail()` 이라는 함수를 통해 유저의 이메일 주소를 백엔드로 전달하고, 그에 맞는 유저의 프로젝트를 받을 수 있는 API가 있다면 한번의 쿼리로 끝낼 수도 있습니다.
- 실제로 `enabled` 옵션은 쿼리의 순서를 정하기 위해 사용하기보다는 어떤 쿼리를 바로 실행하지 않고 특정한 값이 있거나 특정 상황이 되었을 때 실행하도록 하는 등, 다양한 시나리오에서 활용할 수 있습니다.

## `useQuery()` 에서 `enabled` 값 사용하기

- 로그인 버튼을 클릭하면 코드잇이라는 유저로 로그인 되고, 해당 유저의 정보를 백엔드로부터 받아 와서 유저의 이름을 보여줍니다.
- `currentUsername` 이라는 스테이트 값을 만들어 사용합니다.
- 로그인 버튼을 누르면 `currentUsername` 을 `'codeit'` 이라는 값으로 설정하고, `currentUsername`  값이 있을 때 `userInfo` 쿼리를 실행합니다.

`src/api.js`

```jsx
const BASE_URL = 'https://learn.codeit.kr/api/codestudit';

// ...

export async function getUserInfo(username) {
  const response = await fetch(`${BASE_URL}/users/${username}`);
  return await response.json();
}
```

`HomePage.js` 

```jsx
import { useState } from 'react';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { getPosts, uploadPost, getUserInfo } from './api';

function HomePage() {
  const [currentUsername, setCurrentUsername] = useState('');

  // ...

  const { data: userInfoData, isPending: isUserInfoPending } = useQuery({
    queryKey: ['userInfo'],
    queryFn: () => getUserInfo(currentUsername),
    enabled: !!currentUsername,
  });
  
    // ...

  const handleLoginButtonClick = () => {
    setCurrentUsername('codeit');
  };  

  const loginMessage = isUserInfoPending
    ? '로그인 중입니다...'
    : `${userInfoData?.name}님 환영합니다!`;

  if (isPending) return '로딩 중입니다...';

  if (isError) return '에러가 발생했습니다.';

  const posts = postsData?.results ?? [];

  return (
    <>
      <div>
        {currentUsername ? (
          loginMessage
        ) : (
          <button onClick={handleLoginButtonClick}>codeit으로 로그인</button>
        )}
        <form onSubmit={handleSubmit}>
          <textarea
            name="content"
            value={content}
            onChange={handleInputChange}
          />
          <button disabled={!content} type="submit">
            업로드
          </button>
        </form>
      </div>
      <div>
        <ul>
          {posts.map((post) => (
            <li key={post.id}>
              {post.user.name}: {post.content}
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}

export default HomePage;
```

- 실행해보면 처음에는 `['userInfo']` 쿼리가 `disabled` 되어있습니다.

![userInfo상태disabled.jpeg](/13th_Week/userInfo상태disabled.jpeg)

- 로그인 버튼을 누르면 `currentUsername`  이 설정이 되고, `['userInfo']`  쿼리가 실행되어 데이터를 가져옵니다.

![currentUsername설정.jpeg](/13th_Week/currentUsername설정.jpeg)

- `userInfo` 데이터를 가져오고 나면 유저 이름이 잘 보입니다.

![userInfo데이터가져오기.jpeg](/13th_Week/userInfo데이터가져오기.jpeg)

# Paginated Query

- 기존
    - 모든 포스트를 다 한번에 불러왔습니다.
- 이번
    - 3 개씩 끊어서 불러오고, 다음 페이지 버튼을 누르면 그 다음 데이터를 3 개씩 받아오도록 합니다.
- 먼저 현재 포스트를 받아오는 API 함수를 `page` 와 `limit`  값을 받도록 변경합니다.
- 참고로 백엔드 API에 쿼리 파라미터로 `page` 와 `limit` 를 넘겨주면 **해당하는 `page`  의 데이터를 `limit` 개수만큼 보내주도록 설계**가 되어있습니다.

`src/api.js`

```jsx
export async function getPosts**(page = 0, limit = 10)** {
  const response = await fetch(`${BASE_URL}/posts?page=${page}&limit=${limit}`);
  return await response.json();
}
```

- 그 다음에는 `useState()` 를 사용해 `page` 변수를 만들어줍니다.
- `useQuery()` 에서 다음과 같이 `queryKey` 에 `page` 를 추가해 `page`  별로 데이터를 캐싱합니다.
- 그리고 쿼리 함수 호출 부분에 `page` 를 추가해줍니다.

`src/HomePage.js`

```jsx
import { useState } from 'react';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { getPosts, uploadPost, getUserInfo } from './api';

**const PAGE_LIMIT = 3;**

function HomePage() {
  // ...

  const [page, setPage] = useState(0);
  const {
    data: postsData,
    isPending,
    isError,
  } = useQuery({
    queryKey: ['posts', page],
    queryFn: () => getPosts(page, PAGE_LIMIT),
  });

  // ...

  const posts = postsData?.results ?? [];

  return (
    <>
      <div>
        {currentUsername ? (
          loginMessage
        ) : (
          <button onClick={handleLoginButtonClick}>codeit으로 로그인</button>
        )}
        <form onSubmit={handleSubmit}>
          <textarea
            name="content"
            value={content}
            onChange={handleInputChange}
          />
          <button disabled={!content} type="submit">
            업로드
          </button>
        </form>
      </div>
      <div>
        <ul>
          {posts.map((post) => (
            <li key={post.id}>
              {post.user.name}: {post.content}
            </li>
          ))}
        </ul>
      </div>
    </>
  );
}

export default HomePage;
```

![페이지데이터3개.jpeg](/13th_Week/페이지데이터3개.jpeg)

- 결과를 확인해 보면 이제 첫 페이지 ( `page`  값 0 ) 에 해당하는 3개의 데이터만 보이게 됩니다.

![hasMore값true.jpeg](/13th_Week/hasMore값true.jpeg)

- 이제 페이지를 변경할 수 있게끔 버튼을 추가합니다.
- 리액트 쿼리 개발자 도구로 `posts` 데이터를 살펴보면 `hasMore` 라는 값이 있습니다.
- 백엔드에서 그다음 페이지가 있을 때 `hasMore`  값을 `true` 로 보내 주게 됩니다.
- 이 `hasMore` 라는 값으로 다음 페이지 버튼을 비활성화할지 말지를 결정할 수 있습니다.

`src/HomePage.js`

```jsx
import { useState } from 'react';
import { useMutation, useQuery, useQueryClient } from '@tanstack/react-query';
import { getPosts, uploadPost, getUserInfo } from './api';

const PAGE_LIMIT = 3;

function HomePage() {
  // ...
  const [page, setPage] = useState(0);
  const {
    data: postsData,
    isPending,
    isError,
  } = useQuery({
    queryKey: ['posts', page],
    queryFn: () => getPosts(page, PAGE_LIMIT),
  });
  
  // ...

  const posts = postsData?.results ?? [];

  return (
    <>
      <div>
        {currentUsername ? (
          loginMessage
        ) : (
          <button onClick={handleLoginButtonClick}>codeit으로 로그인</button>
        )}
        <form onSubmit={handleSubmit}>
          <textarea
            name="content"
            value={content}
            onChange={handleInputChange}
          />
          <button disabled={!content} type="submit">
            업로드
          </button>
        </form>
      </div>
      <div>
        <ul>
          {posts.map((post) => (
            <li key={post.id}>
              {post.user.name}: {post.content}
            </li>
          ))}
        </ul>
        <div>
          <button
            disabled={page === 0}
            onClick={() => setPage((old) => Math.max(old - 1, 0))}
          >
            &lt;
          </button>
          <button
            disabled={!postsData?.**hasMore**}
            onClick={() => setPage((old) => old + 1)}
          >
            &gt;
          </button>
        </div>
      </div>
    </>
  );
}

export default HomePage;
```

![다음페이지버튼비활성화전.jpeg](/13th_Week/다음페이지버튼비활성화전.jpeg)

- 마지막 페이지로 가면 다음 페이지 버튼이 비활성화됩니다.

![다음페이지비활성화.jpeg](/13th_Week/다음페이지버튼비활성화.jpeg)

- 다음 페이지로 넘어갈 때마다 아래와 같이 매번 로딩 메시지가 뜹니다.
- 새로운 페이지에 해당하는 쿼리를 보낼 때마다 완전히 새로운 쿼리로 인식을 하기 때문에 계속 `pending` 상태가 되기 때문입니다.

![다음페이지버튼시로딩화면.jpeg](/13th_Week/다음페이지버튼시로딩화면.jpeg)

- 리액트 쿼리에서는 좀 더 부드러운 UI 전환을 위해 `placeholderData` 라는 것을 설정해줄 수 있습니다.
- `useQuery()` 에서 `placeholderData` 옵션에 `keepPreviousData` 혹은 `(prevData) => prevData` 를 넣어주면 페이지가 새로 바뀌더라도 매번 `pending`  상태가 되지 않고,
- 이전의 데이터를 유지해서 보여주다가 새로운 데이터 fetch가 완료되면 자연스럽게 새로운 데이터로 바꿔서 보여주게 됩니다.

```jsx
import {
  // ...
  keepPreviousData,
} from '@tanstack/react-query';

const {
  data: postsData,
  isPending,
  isError,
} = useQuery({
    queryKey: ['posts', page],
    queryFn: () => getPosts(page, PAGE_LIMIT),
    **placeholderData: keepPreviousData,**
});
```

![placeholderData데이터전.jpeg](/13th_Week/placeholderData데이터전.jpeg)

- 다음 버튼을 누르면 `fetching` 상태이지만 로딩 메시지가 뜨지 않고 이전 페이지의 내용이 그대로 보입니다.

![placeholderData상태_데이터전.jpeg](/13th_Week/placeholderData_fetching상태_데이터전.jpeg)

- 그러다가 데이터를 모두 받아 오면 다음 페이지의 내용으로 바뀝니다.

![placeholderData상태_데이터완료후.jpeg](/13th_Week/placeholderData_fetching상태_데이터완료후.jpeg)

- 그런데 이때 중간 과정에서 다음 페이지 버튼이 활성화된 채로 있습니다.
- 현재 보이는 데이터가 이전 데이터, 즉 `placeholderData` 라면 다음 페이지 버튼을 비활성화해줘야합니다.
- 그렇지 않으면 유저가 다음 페이지 버튼을 마구 누르는 경우, 존재하지 않는 페이지로 리퀘스트가 갈 수도 있습니다.

 ⇒ `userQuery()` 의 리턴 값에서 `isPlaceholderData` 값을 활용하면 됩니다.

```jsx
const {
  data: postsData,
  isPending,
  isError,
  isPlaceholderData,
} = useQuery({
  queryKey: ['posts', page],
  queryFn: () => getPosts(page, PAGE_LIMIT),
  placeholderData: keepPreviousData,
});

...

return (
  ...
    <div>
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{`${post.user.name}: ${post.content}`}</li>
      ))}
    </ul>
    <div>
      <button
        disabled={page === 0}
        onClick={() => setPage((old) => Math.max(old - 1, 0))}
      >
        &lt;
      </button>
      <button
        disabled={**isPlaceholderData** || !postsData?.hasMore}
        onClick={() => setPage((old) => old + 1)}
      >
        &gt;
      </button>
    </div>
  </div>
  
);
```

- 이렇게 페이지네이션을 완성해보았습니다.
- 만약 좀 더 심리스 ( seamless ) 한 유저 인터페이스를 만들고 싶다면 ?
    - 데이터를 prefetch하는 방법이 존재합니다.
    - 쿼리 클라이언트의 `prefetchQuery` 함수를 이용하면
    
     ⇒ 현재 내가 보고 있는 페이지가 1 페이지여도 백그라운드에서 2 페이지 데이터를 미리 fetch해 놓기 때문에 다음 페이지로 갈 때 전혀 어색함이나 끊김이 없어 2 페이지의 데이터들을 보여줄 수 있습니다.
    
    ```jsx
    ...
    
    useEffect(() => {
      if (!isPlaceholderData && postsData?.hasMore) {
        queryClient.prefetchQuery({
          queryKey: ['posts', page + 1],
          queryFn: () => getPosts(page + 1, PAGE_LIMIT),
        });
      }
    }, [isPlaceholderData, postsData, queryClient, page]);
    ...
    ```

![prefetchQuery함수이용.jpeg](/13th_Week/prefetchQuery함수이용.jpeg)

# Infinite Query

- 리액트 쿼리에서 제공하는 `useInfiniteQuery` 를 이용해서 더 불러오기 버튼을 쉽게 구현할 수 있습니다.
- 먼저 페이지네이션 버튼 대신, “더 불러오기” 버튼을 렌더링 합니다.

```jsx
...

return (
  <>
    <div>
      <form onSubmit={handleSubmit}>
        <textarea
          name='content'
          value={content}
          onChange={handleInputChange}
        />
        <button
          disabled={uploadPostMutation.isPending || !content}
          type='submit'
        >
          업로드
        </button>
      </form>
    </div>
    <div>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{`${post.user.id}: ${post.content}`}</li>
        ))}
      </ul>
      <div>
        <button>더 불러오기</button>
      </div>
    </div>
  </>
);
```

![image.png](./더불러오기%20버튼jpeg)

## `useIfnfiniteQuery()`  사용하기

- 기존에 사용하던 `useQuery()` 를 `useInfiniteQuery()` 로 바꿔줍니다.
- 이때 `useQuery()` 와는 달리 `useInfiniteQuery()` 에서는 `initialPageParam` 과 `getNextPageParam`  옵션을 설정해줘야 합니다.
- 페이지지네이션과는 달리 페이지 별로 데이털르 별도로 저장하지 않고 전체 포스트를 한 번에 관리할 것이기 때문에 쿼리 키는 다시 `['posts']` 로 변경해 줍니다.
- 쿼리함수도 `pageParam` 이라는 값을 받아서 백엔드에 전달할 페이지 값으로 사용하도록 변경해줍니다.

```jsx
import {
  // ...
  useInfiniteQuery,
} from '@tanstack/react-query';

//

const {
  data: postsData,
  isPending,
  isError,
} = useInfiniteQuery({
  queryKey: ['posts'],
  queryFn: ({ pageParam }) => getPosts(pageParam, PAGE_LIMIT),
  initialPageParam: 0,
  getNextPageParam: (lastPage, allPages, lastPageParam, allPageParams) =>
    lastPage.hasMore ? lastPageParam + 1 : undefined,
});
```

- `useQuery()` 에서는 `data` 가 백엔드에서 받아 온 하나의 페이지 정보만 담고 있습니다.
- `useInfiniteQuery()` 에서는 `data.pages` 에 배열의 형태로 모든 페이지의 정보를 담고 있게 됩니다.
- 페이지네이션
    - 첫 번째 페이지 (0페이지) 의 데이터를 백엔드로부터 받아와 해당 데이터만 화면에 보여줍니다
    - 첫 번째 페이지의 데이터는 `['posts' , 0]` 의 쿼리키로 해싱합니다.
    - 두 번째 페이지 (1페이지) 의 데이터는 `['posts' , 1]` 의 쿼리키로 해싱합니다.
- `useInfiniteQuery()`
    - `data.pages` 라는 배열 안에 지금까지 받아 온 모든 페이지의 데이터가 담기게 됩니다.
    - 맨 처음 페이지의 데이터를 받아오면 `data.pages` 배열의 0번 인덱스의 해당 데이터가 저장됩니다.
    - 두 번째 페이지로 넘어가면 `data.pages`  배열의 1번 인덱스에 데이터가 저장됩니다.
    - 하나의 배열의 두 페이지의 데이터들이 모두 담겨있으므로 첫 번째와 두 번째 데이터를 한 번에 화면에 보여 줄 수 있습니다.
    
      ⇒  이런 식으로 더 불러오기를 구현할 수 있습니다.
    
      ⇒ 이 데이터들은 `['posts']`  라는 하나의 쿼리 키로 캐싱됩니다.
    

## `pageParam`  사용하기

```jsx
**initialPageParam**: 0,
getNextPageParam: (lastPage, allPages, lastPageParam, allPageParams) =>
  lastPage.hasMore ? lastPageParam + 1 : undefined,
```

- `initailPageParam` 으로는 초기 페이지 설정값을 정합니다.
- API 에서는 0 페이지가 초기 페이지이므로 값을 0으로 설정합니다.
- `getNextPageParam()`  함수로는 다음 페이지의 설정값을 정하게 됩니다.
    - `lastPage`  : 현재까지 중 가장 마지막 페이지의 데이터가 전달됩니다.
    - `allPages`  : 모든 페이지의 데이터가 전달됩니다.
    - `lastPageParam` : 현재까지 중 가장 마지막 페이지의 설정값을 말합니다.
    - `allPageParmas` : 모든 페이지의 각각의 페이지 설정값을 가지고 있게 됩니다.
- `getNextPageParam()` 에서는 파라미터로 받은 값들의 정보를 이용해 그다음 페이지 값인 `pageParam` 을 리턴해야 합니다.
- 현재 0페이지라면 그다음 값은 1이 되어야합니다.
- 0 페이지의 데이터에서 `hasMore` 값을 확인 합니다.
    - `true`  인 경우 `lasPageParam` 의 값인 0에 1을 더한 값   을 1을 리턴하도록 했습니다.
    - `false` 인 경우 `undefined` 나 `null` 을 리턴해 주면 됩니다. ⇒ 다음 페이지가 없다는 것을 의미합니다.
- 이 `pageParam`  값은 쿼리 함수의 파라미터로 전달되므로, 이 값을 이용해 백엔드에 해당 페이지에 해당하는 데이터를 요청할 수 있습니다.
- 따라서 쿼리 함수를 아래처럼 수정할 수 있습니다.

```jsx

queryFn: ({ pageParam }) => getPosts(pageParam, PAGE_LIMIT)
```

- 위처럼 코드를 수정하고 실행해 보면, 아래처럼 `['posts']` 쿼리 키에 다음과 같이 `pages`  배열의 0번 인덱스에는 첫 번째 페이지 (0 페이지) 의 데이터가,
- `pageParams`  배열의 0번 인덱스에는 첫 번째 페이지 값인 0이 들어있는 것을 확인할 수 있습니다.

![image.png](/13th_Week/pageParam사용하기jpeg)

## 다음 페이지 불러오기

- 다음 페이지를 불러오려면 `usInfiniteQuery()` 의 리턴 값 중 하나인 `fetchNextPage()`  함수를 이용하면 됩니다.
- `fetchNextPage()` 함수를 실행하면 `getNextPageParam()`  함수의 리턴 값이 `undefined` 나 `null`  이 아닌 경우 ⇒ 해당 리턴 값을 쿼리 함수의 `pageParam()` 으로 전달해 그 다음 페이지 데이터를 가져옵니다.
- 따라서 우리는 “더 불러오기” 버튼의 `onClick()` 함수로 `fetchNextPage()` 함수를 등록해주면 됩니다.

```jsx
const {
  data: postsData,
  isPending,
  isError,
  fetchNextPage,
} = useInfiniteQuery({
  queryKey: ['posts'],
  queryFn: ({ pageParam }) => getPosts(pageParam, PAGE_LIMIT),
  initialPageParam: 0,
  getNextPageParam: (lastPage, allPages, lastPageParam, allPageParams) =>
    lastPage.hasMore ? lastPageParam + 1 : undefined,
});

// ...

return (
  ...
    <div>
      <button onClick={fetchNextPage}>더 불러오기</button>
    </div>
  ...
);
```

## 페이지 데이터를 구조에 맞게 정리하기

- 기존에는 `postsData` 에 페이지 하나당 하나에 해당하는 포스트 데이터만 들어있었습니다.
- 이제는 `postData` 안에 `pages` 라는 배열에 모든 페이지의 포스트 데이터가 담겨 있습니다.
- 따라서 `pages` 배열을 `Array.map()`  함수를 통해 돌면서, 각 페이지에 해당하는 포스트 데이터를 모두 보여주도록 변경해봅니다.
- `Post`  컴포넌트를 새로 만들어 포스트 관련 내용은 해당 컴포넌트로 옮겨주었습니다.

`src/HomePage.js` 

```jsx
// ...

const postsPages = postsData?.pages ?? [];

return (
    ...
        <div>
      <ul>
        {postsPages.map((postPage) =>
          postPage.results.map((post) => <Post key={post.id} post={post} />)
        )}
      </ul>
    <div>
    ...
);
```

`src/Post.js`

```jsx
function Post({ post }) {
  return (
    <li key={post.id}>
      {post.user.name}: {post.content}
    </li>
  );
}

export default Post;
```

- 그러면 버튼을 클릭할 때마다 다음과 같이 데이터를 한 번에 세 개씩 불러와서 잘 보여 주는 걸 확인할 수 있습니다.

![image.png](/13th_Week/페이지를데이터구조에맞게정리하기.jpeg)

- 더 불러오기 버튼을 누르면 세 개씩 불러옵니다.

![image.png](/13th_Week/더불러오기버튼3개씩데이터가져오기.jpeg)

## 버튼 비활성화 처리하기

- 더 이상 불러올 데이터가 없거나 다음 데이터를 불러오는 중일 때에는 “더 불러오기” 버튼을 비활성화합니다.
- `useInfiniteQuery()` 의 리턴 값 중 `hasNextPage`  와 `isFetchingNextPage` 를 이용하면 다으모가 같이 간단히 구현할 수 있습니다.

```jsx
const {
  data: postsData,
  isPending,
  isError,
  hasNextPage,
  fetchNextPage,
  isFetchingNextPage,
} = useInfiniteQuery({
  queryKey: ['posts'],
  queryFn: ({ pageParam }) => getPosts(pageParam, PAGE_LIMIT),
  initialPageParam: 0,
  getNextPageParam: (lastPage, allPages, lastPageParam, allPageParams) =>
    lastPage.hasMore ? lastPageParam + 1 : undefined,
});

// ...

return (
    ...
        <button
      onClick={fetchNextPage}
      disabled={!hasNextPage || isFetchingNextPage}
    >
      더 불러오기
    </button>
    ...
);
```

![image.png](/13th_Week/버튼비활성화1.jpeg)

- 다음 데이터를 불러오는 중이면 버튼이 비활성화됩니다.

![image.png](/13th_Week/버튼비활성화2.jpeg)

- 더 이상 불러올 데이터가 없으면 버튼이 비활성화 됩니다.

# 다양한 쿼리 사용하기 정리

## Dependant Query

- 어떤 특정 값을 먼저 받아오거나 어떤 조건이 되었을 때 쿼리 함수를 실행하려면 다음과 같이 `enabled` 옵션을 사용하면 됩니다.

```jsx
const { data: userInfoData } = useQuery({
  queryKey: queryKey,
  queryFn: queryFn,
  enabled: !!username,
});
```

## Paginated Query

- 쿼리 키에 페이지 정보를 포함해서 페이지네이션을 구현할 수 있습니다.
- `placeholderData` 옵션을 활용하면, 새로운 페이지를 보여줄 때 이전의 데이터를 보여주다가 새로운 데이터가 오면 자연스럽게 전환할 수 있습니다.

```jsx
const {data: postsData } = useQuery({
    queryKey: ['posts', page],
    queryFn: () => getPosts(page, PAGE_LIMIT),
    placeholderData: keepPreviousData,
});
```

- `prefetchQuery()`  함수를 사용하면 다음 페이지의 데이터를 미리 fetch하도록 구현할 수도 있습니다.

```jsx
useEffect(() => {
  if (!isPlaceholderData && postsData?.hasMore) {
    queryClient.prefetchQuery({
      queryKey: ['posts', page + 1],
      queryFn: () => getPosts(page + 1, PAGE_LIMIT),
    });
  }
}, [isPlaceholderData, postsData, queryClient, page]);
```

## Infinite Query

```jsx
const {
  data: postsData,
  isPending,
  isError,
  hasNextPage,
  fetchNextPage,
  isFetchingNextPage,
} = useInfiniteQuery({
  queryKey: ['posts'],
  queryFn: ({ pageParam }) => getPosts(pageParam, LIMIT),
  initialPageParam: 0,
  getNextPageParam: (lastPage, allPages, lastPageParam, allPageParams) =>
    lastPage.hasMore ? lastPageParam + 1 : undefined,
});
```

- `useInfiniteQuery()` 훅에서는 `page param` 값을 활용하여 페이지를 더 불러옵니다.
- `useQuery()` 에서는 `data` 에 한 페이지에 해당하는 데이터만 담고 있었지만,
- `useInfiniteQuery()` 에서는 `data` 에 모든 페이지의 데이터가 `pages` 라는 프로퍼티로 배열에 담겨있습니다.
- `getNextPageParam()`  함수에서 다음 페이지가 있는 경우 다음 `page param` 값을 쿼리 함수로 전달해 다음 페이지의 데이터를 받아옵니다.
- 만약 `getNextPageParams()` 함수에서 `undefined`  나 `null`  값을 리턴하면 다음 페이지가 없는 것으로 간주해 `fetchNextPage()` 함수를 실행해도 더 이상 데이터를 받아오지 않고, `hasNextPage` 의 값도 `false` 가 됩니다.

## Optimistic updates

- Optimistic updates는 서버가 제대로 동작할 것을 낙관적으로 기대하며, 서버로부터의 리스폰스를 기다리지 않고 유저에게 바로 피드백을 주는 방식을 말합니다.
- `useMutation()` 의 `onMutate` , `onError` , `onSettled` 옵션을 활용해 Optimstic updates를 구현할 수 있습니다.

```jsx
const likeMutation = useMutation({
  mutationFn: async ({ postId, username, userAction }) => {
    if (userAction === USER_ACTION.LIKE_POST) {
      await likePost(postId, username);
    } else {
      await unlikePost(postId, username);
    }
  },
  onMutate: async ({ postId, username, userAction }) => {
    await queryClient.cancelQueries({
      queryKey: [QUERY_KEYS.LIKE_STATUS, postId],
    });
    await queryClient.cancelQueries({
      queryKey: [QUERY_KEYS.NUM_OF_LIKES, postId],
    });

    const prevLikeStatus = queryClient.getQueryData([
      QUERY_KEYS.LIKE_STATUS,
      postId,
      username,
    ]);
    const prevLikeCount = queryClient.getQueryData([
      QUERY_KEYS.LIKE_COUNT,
      postId,
    ]);

    queryClient.setQueryData(
      [QUERY_KEYS.LIKE_STATUS, postId, username],
      () => userAction === USER_ACTION.LIKE_POST
    );
    queryClient.setQueryData([QUERY_KEYS.LIKE_COUNT, postId], (prev) => {
      userAction === USER_ACTION.LIKE_POST ? prev + 1 : prev - 1;
    });

    return { prevLikeStatus, prevLikeCount };
  },
  onError: (err, { postId, username }, context) => {
    queryClient.setQueryData(
      [QUERY_KEYS.LIKE_STATUS, postId, username],
      context.prevLikeStatus
    );
    queryClient.setQueryData(
      [QUERY_KEYS.LIKE_COUNT, postId],
      context.prevLikeCount
    );
  },
  onSettled: (data, err, { postId, username }) => {
    queryClient.invalidateQueries({
      queryKey: [QUERY_KEYS.LIKE_STATUS, postId, username],
    });
    queryClient.invalidateQueries({
      queryKey: [QUERY_KEYS.LIKE_COUNT, postId],
    });
  },
});
```

### `onMutate`

1. 우리가 옵티미스틱 업데이트를 통해 변경하려고 하는 데이터가 refetch로 인해 덮어씌워지는 것을 막기 위해 `cancelQueryies()`  를 실행하여, 좋아요 관련 데이터를 받아오지 않도록 쿼리를 취소해줍니다.
2. 에러가 발생했을 때는 이전의 데이터로 롤백해 줘야 하는데요. 롤백용 데이터를 따로 저장해줍니다.
3. 우리가 원하는 값으로 쿼리 데이터를 미리 변경합니다.
4. 마지막으로 롤백용 데이터를 리턴해주면 됩니다.

### `onError`

- 롤백용 데이터를 세 번째 파라미터인 `context`  로 받아옵니다. `context` 값으로 쿼리 데이터를 변경해 줍니다.

### `onSettled`

- 에러 여부와 상관없이 백엔드 서버와 데이터를 동기화해주기 위하여 좋아요 관련 데이터 쿼리를 invalidate 해 줍니다.

# Next.js에서 React Query 사용하기

- 리액트 쿼리는 Next.js와 같은 서버 사이드 렌더링을 제공하는 프레임워크와 함께 사용할 수도 있습니다.
- 서버에서 리액트 쿼리를 사용하면 클라이언트에서 쿼리를 실행해서 데이터를 가져올 때까지 기다리는 것이 아니라, 서버에서 쿼리를 실행해 데이터를 prefetch할 수 있습니다.

## 초기 설정

`pages/_app.js`

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

export default function App({ Component, pageProps }) {
  const [queryClient] = React.useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
            // 보통 SSR에서는 staleTime을 0 이상으로 해줌으로써
            // 클라이언트 사이드에서 바로 다시 데이터를 refetch 하는 것을 피한다.
            staleTime: 60 * 1000,
          },
        },
      })
  );

  return (
    <QueryClientProvider client={queryClient}>
      <Component {...pageProps} />
    </QueryClientProvider>
  );
}
```

- 먼저 `App` 컴포넌트에서 초기 설정을 해 줍니다.
- Next.js에서는 기존의 리액트 프로젝트와는 다르게 `App`  컴포넌트 안에 새로운 `QueryClient` 를 `useState()`  를 사용해 state로 선언해 줘야 합니다.
- `App`  컴포넌트 바깥에 선언하게 되면 서버 렌더링시 쿼리 캐시가 다른 사용자들과 리퀘스트 간에 공유가 될 수 있기 때문에 반드시 `App` 컴포넌트 내부에 선언을 해줍니다.
- Next.js에서는 페이지를 이동하면 `App`  컴포넌트부터 새롭게 렌더링되기 때문에 쿼리 클라이언트가 매번 새롭게 생성되는 것을 막기 위하여 state로 저장해줍니다.

## Prefetch 구현하기

- 리액트 쿼리에서는 두 가지 방법으로 prefetching을 지원합니다.

## `initialData`  를 사용한 prefetching

```jsx
export async function getServerSideProps() {
  const posts = await getPosts()
  return { props: { posts } }
}

function Posts(props) {
  const { data } = useQuery({
    queryKey: ['posts'],
    queryFn: getPosts,
    initialData: props.posts,
  })

  // ...
}
```

- 이 방법은 Next.js에서 정적 생성, 서버 사이드 렌더링을 하면서 prefetch한 데이터를 `useQuery()`  의 `initialData` 로 설정해 주는 방법입니다.
- 사용 방법은 매우 간단하며 prefetching 단계에서는 리액트 쿼리를 전혀 사용하지 않아도 된다는 장점이 있습니다.
- 다만 몇 가지 단점들도 있습니다.
    - `getStaticProps()` , `getServerSideProps()`  는 `pages` 폴더 안에서만 동작하기 때문에 `useQuery()`  를 사용하려는 컴포넌트까지 prefetch한 데이터를 props drilling으로 내려 줘야만 합니다.
    - 또한 같은 쿼리의 `useQuery()` 를 여러 군데서 사용한다면, 모든 `useQuery()`  에 똑같은 `initialData` 를 설정해줘야 하는 문제도 있습니다.
    - 쿼리가 서버로부터 언제 fetch 되었는지 정확히 알 수 없기 때문에, `dataUpdatedAt`  의 시간이나 쿼리 refetching이 필요한지 여부는 페이지가 로드된 시점으로부터 계산된다는 한게점도 있습니다.
    - 추가적으로 만약 어떤 쿼리 키로 캐싱된 데이터가 이미 있다면 `initailData`  는 해당 데이터를 절대 덮어쓰지 않습니다.
    - 따라서 이미 캐싱된 데이터가 더 오래 된 것이더라도, `getServerSideProps()` 함수로 받아 온 데이터는 `initialData` 로 설정이 되기 때문에 새로운 데이터로 업데이트할 수 없다는 단점이 있습니다.

## Hydration API 사용하기

```jsx
import { dehydrate, HydrationBoundary, QueryClient, useQuery } from '@tanstack/react-query'

export async function getStaticProps() {
  const queryClient = new QueryClient()

  await queryClient.prefetchQuery({
    queryKey: ['posts'],
    queryFn: getPosts,
  })

  return {
    props: {
      dehydratedState: dehydrate(queryClient),
    },
  }
}

function Posts() {
  const { data } = useQuery({ queryKey: ['posts'], queryFn: getPosts })

  // 이 쿼리는 서버에서 prefetch하지 않는 데이터. 
    // prefetch하는 데이터와 아닌 데이터를 자유롭게 섞어서 활용할 수 있다.
  const { data: commentsData } = useQuery({
    queryKey: ['posts-comments'],
    queryFn: getComments,
  })

  // ...
}

export default PostsRoute({ dehydratedState }) {
  return (
    <HydrationBoundary state={dehydratedState}>
      <Posts />
    </HydrationBoundary>
  )
}
```

- Hydration은 이미 렌더링된 HTML과 리액트를 연결하는 작업을 말합니다.
- 정적인 HTML을 리액트 코드와 연결해서 동적인 상태로 바꿔 주는 걸 수분을 보충한다는 의미로 hydrate라고 표현합니다.
- dehydrate은 hydrate의 반대되는 말입니다. 동적인 것을 다시 정적인 상태로 만드는 작업을 말합니다.
- 위 코드에서는 prefetch한 결괏값이 담긴 `queryClient` 를 dehydrate해서 클라이언트로 보내 주었습니다.
- 이렇게 하면 초기 설정 코드가 늘어나지만 `initialData` 를 이용하면서 발생하는 여러 단점을 모두 해결할 수 있습니다.

