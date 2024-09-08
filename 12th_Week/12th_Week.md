# Next.js로 웹 사이트 만들기

# 라우팅 정리

## `<Link>`  컴포넌트

- Next.js 에서는 링크를 연결하는데 `<a>` 태그 대신에 `<Link>` 컴포넌트를 사용합니다.
- `<a>`  태그를 사용하면 페이지를 이동할 때 페이지 전체를 다시 로딩하기 때문에 속도가 느리고, 빈 화면이 잠깐 보이면서 깜빡거림이 생깁니다.
- `<Link>` 컴포넌트는 Next.js 에서 내부적으로 여러 가지 최적화를 해주기 때문에 빠르고 부드러운 페이지 전환이 가능합니다.

```jsx
import Link from 'next/link';

export default Page() {
  return <Link href="/">홈페이지로 이동</Link>;
}
```

## `useRouter()`  Hook

## 쿼리 사용하기

- `router.query` 값을 사용하면 페이지 주소에서 Params 값이나 쿼리스트링 값을 참조할 수 있습니다.
- 예를 들면 `pages/products/[id].js` 페이지에서 `router.query['id']` 값으로 Params `id` 에 해당하는 값을 가져올 수 있습니다.

`pages/products/[id].js`

```jsx
import { useRouter } from 'next/router';

export default function Product() {
  const router = useRouter();
  const id = **router.query['id']**;

  return <>Product #{id} 페이지</>;
}
```

- `/search?q=티셔츠`  와 같은 주소로 들어왔을 때 `router.query['q']` 값으로 쿼리스트링 `q` 에 해당하는 값을 가져올 수 있었습니다.

`pages/search.js` 

```jsx
import { useRouter } from 'next/router';

export default function Search() {
  const router = useRouter();
  const q = **router.query['q']**;

  return <>{q} 검색 결과</>;
}
```

## 페이지 이동하기

- `router.push()`  함수를 사용하면 코드로 페이지를 이동할 수 있었습니다.

```jsx
import { useState } from 'react';
import { useRouter } from 'next/router';

export default function SearchForm() {
  const [value, setValue] = useState();
  const router = useRouter();

  function handleChange(e) {
    setValue(e.target.value);
  }

  function handleSubmit(e) {
    e.preventDefault();
    if (!value) {
      return **router.push('/')**;
    }
    return **router.push(`/search?q=${value}`)**;
  }

  return (
    <form onSubmit={handleSubmit}>
      <input name="q" value={value} onChange={handleChange} />
      <button>검색</button>
    </form>
  );
}
```

## 리다이렉트

- `next.config.js`  파일을 수정하면 특정 주소에 대해서 리다이렉트할 주소를 지정할 수 있습니다.
- 예를 들어서 **`/products/:id` 라는 주소로 들어오면 `/items/:id` 라는 주소로 이동시켜 줄 수 있습니다.**
- 이때 `permanent` 라는 속성으로 상태코드를 정할 수 있습니다.
- `permanent: false`  로 하면 307 Temporary Redirect를 하고,
- `permanent: true` 로 하면 브라우저에 리다이렉트 정보를 저장하는 308 Permanent Redirect를 할 수 있습니다.

```jsx

/** @type {import('next').NextConfig} */
const nextConfig = {
  async redirects() {
    return [
      {
        **source: '/products/:id',
        destination: '/items/:id',**
        permanent: true,
      },
    ];
  },
}

module.exports = nextConfig;
```

## 커스텀 404 페이지

- 존재하지 않는 주소로 들어올 경우에 Next.js 에서는 기본적으로 404 페이지를 보여줍니다.
- 내가 원하는 404 페이지를 보여주려면 `pages/404.js` 파일을 만들고 일반적인 페이지처럼 구현하면 됩니다.

## 커스텀 App

- 모든 페이지에 공통적으로 코드를 적용하고 싶다면 커스텀 `App` 컴포넌트를 수정하면 됩니다.
- **`pages/_app.js` 파일에 있는 컴포넌트입니다.**
- **이 컴포넌트에 사이트 전체에서 보여 줄 컴포넌트나 전체적으로 적용할 리액트 컨텍스트를 적용할 수 있습니다.**
- **그리고 사이트 전체에 적용할 CSS 파일도 여기서 임포트 할 수 있습니다.**
- 커스텀 `App`  컴포넌트의 Props는 `Component`  와 `pageProps`  가 있습니다.
- 우리가 만든 페이지들이 `Component`  Prop으로 전달되고 여기에 내부적으로 필요한 Props는 `pageProps` 라는 값으로 전달됩니다.

`pages/_app.js`

```jsx
import Header from '@/components/Header';
import { ThemeProvider } from '@/lib/ThemeContext';
import '@/styles/globals.css';

export default function App(**{ Component, pageProps }**) {
  return (
    <ThemeProvider>
      <Header />
      **<Component {...pageProps} />**
    </ThemeProvider>
  );
}
```

## 커스텀 Document

- `pages/_document.js`  파일에 있는 `Document`  컴포넌트는 HTML 코드의 뼈대를 수정하는 용도로 사용합니다.
- 코드는 React 컴포넌트이지만 일반적인 컴포넌트처럼 동작하지는 않기 때문에 **`useState` 나 `useEffect` 처럼 브라우저에서 실행이 필요한 기능들은 사용할 수 없습니다.**

`pages/_document.js`

```jsx
import { Html, Head, Main, NextScript } from 'next/document'

export default function Document() {
  return (
    <Html lang="ko">
      <Head />
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  )
}
```

<br>

# 사이트 완성하기 정리

## `<Image>` 컴포넌트

- Next.js 에서는 이미지를 사용할 때  **Next.js 서버를 한 번 거쳐서 이미지 최적화를 한 다음 사용할 수 있도록 해줍니다.**
- 그래서 되도록이면 `<img>`  태그보다는 `<Image>` 컴포넌트를 사용하는 걸 권장합니다.
- `<Image>` 컴포넌트는 `next/image` 에서 불러와 사용합니다.

  **⇒ 이때 반드시 크기가 있어야 합니다.**

- `width` 와 `height` 값을 지정하거나 또는 `fill`  이라는 Prop을 사용할 수 있습니다.

## `width` 와 `height`  를 사용하는 경우

- `<img>`  태그와 마찬가지로 너비와 높이를 지정해 줍니다.

```jsx
import Image from 'next/image';

export default function Page() {
  return (
    <>
      <Image
        src="/images/product.jpeg"
        width={300}
        height={300}
        alt="상품 이미지"
      />
    </>
  );
}
```

## `fill` 을 사용하는 경우

- 이미지 크기를 유연하게 지정해야 할 때는 `fill` 을 사용합니다.
- **이때 부모 요소에서 `position: relative`  와 같이 포지셔닝 해야합니다.**
- `fill` 을 사용할 경우 `absolute` 포지션으로 배치되기 때문에,
- “가장 가까운 포지셔닝이 된” 조상 요소에 꽉 차도록 이미지가 배치됩니다.
- **만약 이미지의 비율이 깨지는 것을 막고 싶다면 `object-fit: cover`  속성으로 이미지를 크롭하시면 됩니다.**

```jsx
import Image from 'next/image';

export default function Page() {
  return (
    <>
     <div
        style={{
          position: 'relative',
          width: '100%',
          height: '300px',
        }}
      >
        <Image
          src="/images/product.jpeg"
          fill
          alt="상품 이미지"
          style={{
            **objectFit: 'cover',**
          }}
        />
      </div>
    </>
  );
}
```

## `<Head>`  컴포넌트

- `next/head` 에서 불러와서 `<Head>` 컴포넌트 안에 `<head>` 태그에 넣고 싶은 코드를 작성하면 됩니다.

```jsx
import Head from 'next/head';

export default function Page() {
  return (
    <>
      <Head>
        <title>페이지 제목</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>
      ...
    </>
  );
}
```

- 만약 사이트 전체에 공통으로 적용하고 싶다면 `/pages/_app.js`  파일에서 `<Head>` 컴포넌트를 활용하면 됩니다.

`pages/_app.js` 

```jsx
import Head from 'next/head';

export default function App({ Component, pageProps }) {
  return (
    <>
      <Head>
        <title>사이트 기본 제목</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>
      <Component {...pageProps} />
    </>
  );
}
```

## 개발 모드, 빌드, 실행 그리고 배포

## 개발 서버 켜기

- 프로젝트 폴더에서 터미널을 연 다음, 아래 명령을 입력해서 개발 서버를 실행할 수 있습니다.
- 개발 모드이기 때문에 새로고침 없이도 수정 사항을 그때 그때 확인할 수 있습니다.

```jsx
npm run dev
```

## 빌드하기

- Next.js 프로젝트를 배포하려면 우선 실행 가능한 형태로 코드를 변환한 다음에, 서버에서 실행을 해야 합니다.
- 이때 코드를 변환하는 과정을 “빌드” 라고 합니다.
- 빌드를 하려면 프로젝트 폴더에서 터미널을 연 다음, 아래 명령어를 입력합니다.

```jsx
npm run build
```

## 실행 하기

- 프로젝트를 빌드했다면 이제 실행할 수 있습니다.
- 마찬가지로 프로젝트 폴더에서 터미널을 연 다음, 아래 명령어를 입력합니다.

```jsx
npm run start
```

# 정적 생성 vs SSR

## 홈페이지 ( `/pages/index.js` )

- 상품 데이터가 자주 업데이트되지 않는다고 가정 ⇒ `getStaticProps()` 함수를 구현하고 데이터를 불러와서  정적 생성
- 관리자가 상품을 자주 업데이트하거나, 항상 최신의 상품을 보여 줘야 한다면 서버사이드 렌더링

## 검색 페이지 ( `/pages/search.js` )

- 주소에 있는 쿼리스트링 값에 따라서 다른 검색 결과를 보여 줘야하기 때문에 ⇒ 서버사이드 렌더링
- `getServerSideProps()` 함수를 구현해서 쿼리스트링에 따라 다른 데이터를 가지고 서버사이드 렌더링

## 상품 상세 페이지 ( `/pages/items/[id].js` )

- 상품 상세 페이지에서는 항상 최신의 리뷰 데이터를 보여준다 ⇒ 서버사이드 렌더링
- `getServerSideProps()`  함수를 구현해서 아이디 값에 따라 다른 데이터를 가지고 서버사이드 렌더링을 하도록 구현
- 리퀘스트가 들어올 때마다 매번 리뷰 데이터를 불러와서 렌더링하기 때문에, 항상 최신의 리뷰 데이터로 프리렌더링 할 수 있습니다.

## 설정 페이지 ( `/pages/settings.js`  )

- 설정 페이지에서는 딱히 데이터를 사용하지 않았기 때문에, Next.js에서 기본적으로 정적 생성을 하고 있습니다.

⇒ 이렇게 페이지마다 데이터를 어떻게 보여줄 것인가에 따라 서버사이드 렌더링, 정적 생성을 다르게 해줍니다.

⇒ Next.js 에서는 특별한 경우가 아니라면 되도록이면 정적 생성으로 구현할 것을 권장하고 있습니다.

⇒ 리퀘스트가 들어올 때마다 매번 렌더링을 하는 것보다 미리 렌더링을 해서 저장해 둔 것을 보내 주는 게 훨씬 빠르기 때문입니다.

## 서버사이드 렌더링을 고려하면 좋은 경우

- 항상 최신 데이터를 보여 줘야 하는 경우
- 데이터가 자주 바뀌는 경우
- 리퀘스트의 데이터를 사용해야 하는 경우 (예 : 헤더, 쿼리스트링, 쿠키 등)

  ⇒ **그 외에 특별한 이유가 없다면 되도록 정적 생성을 하는 걸 권장합니다.**

# 프리 렌더링 정리

## 프리 렌더링 (Pre-rendering)

- 웹 브라우저가 페이지를 로딩하기 이전에 렌더링하는 걸 말합니다.
- 크게 정적 생성 (Static Generation) 과 서버 사이드 렌더링 (Sever-sid Rendering) 으로 나뉩니다.
- Next.js에서는 기본적으로 모든 페이지를 정적 생성합니다.

## 정적 생성 (Static Generation)

- 프로젝트를 빌드하는 시점에 미리 HTML을 렌더링 하는 걸 말합니다.

  ⇒ 기본적으로 Next.js에서는 모든 페이지를 정적 생성합니다.

### `getStaticProps()`  함수

- 정적 생성할 때 필요한 데이터를 받아와서 렌더링 하고 싶다면 `getStaticProps()`  함수를 구현하고 export하면 됩니다.
- 객체의 `props` 프로퍼티로 넘겨줄 Props 객체를 지정하고, 이것을 페이지 컴포넌트에서 사용할 수 있습니다.

```jsx
export async function **getStaticProps()** {
  const res = await axios('/products/');
  const products = res.data;

  return {
    props: {
      products,
    },
  };
}

export default function Home({ products }) {
  return (
    <ProductList products={products} />
  );
}
```

### `getStaticPaths() 함수`

- `/pages/products/[id].js` 와 같이 다이나믹 라우팅을 하는 페이지를 정적 생성을 할 때에는 어떤 페이지를 정적 생성할지 지정해줘야 합니다.
- `getStaticPaths()` 라는 함수를 구현하고 export해서 정해줄 수 있습니다.
- `getStaticPaths()` 함수에서는 리턴 값으로 객체를 리턴하는데, `paths` 라는 배열에서 각 페이지에 해당하는 정보를 넘겨줄 수 있습니다.
- 예를 들어서 `id` 값이 `'1'` 인 페이지를 정적 생성하려면 `{ params: { id: '1' } }` 과 같이 쓸 수 있습니다.
- 그리고 `fallback` 이라는 속성을 사용해서 정적 생성되지 않은 페이지를 처리해 줄 것인지 지정할 수 있습니다.
- `fallback: true`  라고 하면 생성되지 않은 페이지로 접속했을 때 `getStaticProps()` 함수를 실행해 페이지를 만들어서 보여줍니다.

```jsx
export async function getStaticPaths() {
  return {
    paths: [
      { params: { id: '1' }},
      { params: { id: '2' }},
    ],
    fallback: true,
  };
}
```

- `getStaticProps()` 함수에서는 `context` 파라미터를 사용해서 필요한 Params ( `context.params` ) 값이나 쿼리스트링 ( `context.query` ) 값을 참조할 수 있습니다.
- 그리고 `fallback: true` 라고 지정했다면, 필요한 데이터가 존재하지 않을 수 있기 때문에 적절한 예외처리를 해야합니다.
- `{ notFound: true }`  를 리턴하면 데이터를 찾을 수 없을 때 404 페이지로 이동시킬 수 있습니다.

```jsx
export async function getStaticProps(context) {
  const productId = context.params['id'];

  let product;

  try {
    const res = await axios(`/products/${productId}`);
    product = res.data;
  } catch {
    return {
      notFound: true,
    };
  }

  return {
    props: {
      product,
    },
  };
}
```

- 만약 `fallback: true` 를 설정했다면 `getStaticProps()` 를 실행하는 동안 보여 줄 로딩 페이지를 구현해야 합니다.
- 페이지 컴포넌트에서 필요한 데이터가 존재하지 않을 때를 처리해 주면 됩니다.

```jsx

export default function Product({ product }) {
  if (!product) {
    return <>로딩 중 ...</>
  }
  return <>상품 이름: {product.name}</>;
}
```

## 서버사이드 렌더링 **(Server-side Rendering)**

- Next.js 서버에 리퀘스트가 도착할 때마다 페이지를 렌더링해서 보내주는 방식입니다.
- `getServerSideProps()` 함수를 구현하고 export하면 됩니다.
- 이때 리턴 값으로는 객체를 리턴합니다.
- 정적 생성때와 마찬가지로 `props` 프로퍼티로 Props 객체를 넘겨주면 페이지 컴포넌트에서 받아서 사용할 수 있습니다.

```jsx
export async function getServerSideProps() {
  const res = await axios('/products/');
  const products = res.data;

  return {
    **props: {
      products,
    },**
  };
}

export default function Home({ products }) {
  return (
    <ProductList products={products} />
  );
}
```
<br>

# App Router란?

- 지금까지는 라우팅과 프리렌더링
- Next.js 13.4 이후 버전부터는 새로운 방식의 라우팅 ⇒ App Router
- App Router의 가장 큰 차이는 `/pages` 폴더가 아닌 `/app` 폴더에 페이지 컴포넌트들을 추가한다는 겁니다.
- 그리고 이 페이지 컴포넌트들은 기본적으로 리액트 서버 컴포넌트 (React Server Component) 입니다.

  ⇒ 서버에서만 렌더링 되는 컴포넌트

## React Server Component (RSC) 란?

- 리액트 서버 컴포넌트는 서버에서만 렌더링하는 컴포넌트입니다.
- 2023년 5월을 기준으로 리액트에서는 아직까지는 실험적인 기능
- 기존 (리액트 클라이언트 컴포넌트)
    - 클라이언트에서 리퀘스트를 보내서 데이터를 받아오거나
    - Next.js에서 프리렌더링을 한다면 데이터를 Props로 내려받습니다.
- 현재 ( 리액트 서버 컴포넌트)
    - 컴포넌트를 `async / await` 함수로 만들 수 있고, 함수 최상위 (top-level) 에서 `await`  로 데이터를 가져올 수 있습니다.

⇒ `async / await` 를 사용해서 컴포넌트 함수를 작성하기 때문에 훨씬 직관적인 문법으로 컴포넌트를 개발할 수 있습니다.

⇒ 서버에서 모든 데이터를 가져온 다음 렌더링까지 해서 보내주기 때문에 서버와 클라이언트가 여러 번 리퀘스트를 주고받을 때보다 빠르게 페이즐 보여줄 수 있습니다.

⇒ 서버 컴포넌트 렌더링에 필요한 자바스크립트는 서버에서만 실행하기 때문에 클라이언트가 다운로드 해야 할 자바스크립트 코드의 양도 줄어듭니다.

```jsx
async function getData() {
  const res = await fetch('https://api.example.com/...');
  return res.json();
}

export default async function Page() {
  const data = await getData();

  return <main> ... </main>;
}
```

⇒ 정적 생성, 서버사이드 렌더링 뿐만 아니라, 서버 컴포넌트까지 활용해서 최적화된 웹사이트를 만들 수 있습니다.

