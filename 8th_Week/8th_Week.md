# 자바스크립트 백엔드 개발 시작하기

# Express로 API 만들기 정리

## Express 뼈대 코드

```jsx
import express from 'express';

const app = express();

// 라우트 정의

app.listen(3000, () => console.log('Server Started'));
```

## 라우트(Route) 정의

- Express 라우트는 아래와 같이 정의할 수 있습니다.

```jsx
app.method(path, handler)
```

- `method` : HTTP 메소드 이름
- `path` : 엔드포인트 경로
- `handler` : 리퀘스트 로직을 처리하고 리스폰스를 돌려주는 핸들러 함수. 첫 번째 파라미터로 리퀘스트 객체, 두 번째 파라미터로 리스폰스 객체를 받습니다.

```jsx
// Arrow Function 핸들러

app.get('/some/path', (req, res) => {
  // 리퀘스트 처리
});
```

```jsx
// 핸들러 함수 선언

function handler(req, res) {
  // 리퀘스트 처리
}

app.get('/some/path', handler);
```

## 리퀘스트 객체

### `req.query`

- **쿼리스트링 파라미터**를 프로퍼티를 담고 있는 객체입니다. 파라미터는 항상 문자열입니다.
- 예시 : `GET /some/path?foo=1&bar=2`

```jsx
app.get('/some/path', (req, res) => {
  console.log(req.query);  // { foo: '1', bar: '2' }
  // ...
});
```

### `req.params`

- **URL 파라미터**를 프로퍼티로 담고 있는 객체입니다. 파라미터는 항상 문자열입니다.
- 예시 : `GET /some/1/path/james`

```jsx
app.get('/some/:id/path/:name', (req, res) => {
  console.log(req.params);  // { id: '1', name: 'james' }
  // ...
});
```

### `req.body`

- **리퀘스트 바디** 내용을 담고 있는 객체입니다. 바디 내용을 **`req.body` 로 접근하려면 `express.json()` 이라는 함수를 이용**해야 합니다.
- 예시 : `POST /some/path`

```jsx
{
  "field1": "value1",
  "field2": "value2"
}
```

## 리스폰스 객체

### `res.send()`

- 리스폰스를 보냅니다. 아규먼트로 전달되는 값에 따라 `Content-Type` 헤더를 설정하고 적절한 바디 내용으로 변환해 줍니다.
- API 서버를 만들 때는 주로 객체나 배열을 전달하는데요. 그러면 `Content-Tyle`  헤더를 `application/json` 으로 설정하고 객체나 배열을 JSON 문자열로 바꿔서 전달해 줍니다.
- 디폴트 상태 코드는 `200` (OK) 입니다.

```jsx
app.get('/some/path', (req, res) => {
  res.send({ field1: 'value1', field2: 'value2' });
});
```

```jsx
GET /some/path

HTTP/1.1 200 OK
Content-Type: application/json

{
  "field1": "value1",
  "field2": "value2"
}
```

### `res.status()`

- 리스폰스의 상태 코드를 설정합니다.

```jsx
app.get('/some/path', (req, res) => {
  // ...
  res.status(404).send(...);
});
```

### `res.sendStatus()`

```jsx
app.get('/some/path', (req, res) => {
  // ...
  res.sendStatus(204);
});
```

<br>

# MongoDB 데이터베이스 사용하기

## MongoDB와 Mongoose

- `MongoDB` 는 데이터를 문서 형태로 저장하는 데이터베이스 입니다.
- 데이터, 즉 문서 하나를 ‘도큐먼트’ 라고 부르고 도큐먼트의 모음을 ‘컬렉션’ 이라고 부릅니다.
- `MongoDB` 가 제공하는 클라우드 서비스, `Atlas` 를 이용하면 쉽게 데이터베이스를 셋업하고 사용할 수 있습니다.

## 데이터베이스 접속

```jsx
import mongoose from 'mongoose';

mongoose.connect('mongodb+srv://...');
```

- `mongodb+src://...` 은 `Atlas` 에서 복사한 URL 입니다.
- 이런 URL은 별도의 파일이나 환경 변수 `.env` 파일에 저장해두는 것이 좋습니다.

## 스키마와 모델

- ‘스키마’ 는 데이터(도큐먼트)의 틀입니다.
- 이 스키마를 기반으로 데이터를 생성하고 조회하고 수정, 삭제할 수 있는 인터페이스를 ‘모델’ 이라고 합니다.
- `Mongoose` 로 어떤 데이터를 다루려면 항상 스키마와 모델을 가장 먼저 정의해야 합니다.

`models/Task.js` 

```jsx
import mongoose from 'mongoose';

const TaskSchema = new mongoose.Schema(
  {
    title: {
      type: String,
      required: true,
      maxLength: 30,
    },
    description: {
      type: String,
    },
    isComplete: {
      type: Boolean,
      required: true,
      default: false,
    },
  },
  {
    timestamps: true,
  }
);

const Task = mongoose.model('Task', TaskSchema);

export default Task;
```

- 위와 같이 필드 이름과 필드에 대한 정보를 객체 형태로 정의하면 됩니다.
- 자주 사용하는 필트 타입은 `String` , `Number` , `Boolean` , `Date` 이고 `default` 프로퍼티로 기본값을 설정할 수 있습니다.
- `id` 필드는 정의하지 않아도 `Mongoose` 가 알아서 관리합니다.
- `timestamps: true` 옵션을 사용하면 `createdAt` , `updatedAt` 필드를 `Mongoose` 가 알아서 생성하고 관리합니다.
- 유효성 검사에 대한 정보도 스키마에 정의합니다. 자주 사용하는 프로퍼티는 아래와 같습니다.
    - `required` : 데이터를 생성할 때 꼭 있어야 하는 필드입니다.
    - 문자열 필드 : `maxLength` (최대 길이), `minLength` (최소 길이), `enum`  (특정 값 중 하나여야 할 때), `match` (특정 패턴이어야 할 때)
    - 숫자형 필드 : `min` (최소), `max` (최대)
- 커스텀 `Validator` 를 사용할 수도 있습니다.

```jsx
{
  title: {
    type: String,
    required: true,
    maxLength: 30,
    validate: {
       validator: function (title) {
         return title.split(' ').length > 1;
       },
       message: 'Must contain at least 2 words.',
     },
  },
  // ...
}
```

## CRUD 연산

## 생성 (Create)

- `.create()` 메소드를 사용해서 객체를 생성할 수 있습니다.

```jsx
app.post('/tasks', async (req, res) => {
  const newTask = await Task.create(req.body);
  res.status(201).send(newTask);
});
```

- 모든 모델 메소드는 비동기로 실행되기 때문에 결과를 가져오려면 `await` 을 사용해야 합니다.

## 조회 (Read)

- 여러 객체를 조회할 때는 `.find()` 메소드를 사용합니다.

```jsx
app.get('/tasks', async (req, res) => {
  const tasks = await Task.find();
  res.send(tasks);
});
```

- `id` 로 특정 객체를 조회할 때는 `.findById()` 를 사용합니다.

```jsx
app.get('/tasks/:id', async (req, res) => {
  const task = await Task.findById(req.params.id);
  res.send(task);
  } else {
    res.status(404).send({ message: 'Cannot find given id.' });
  }
});
```

`id` 외에 다른 조건으로 필터를 하고 싶을 때는 **`쿼리 필터`**  를 사용하면 됩니다.

## `쿼리 필터`

- `Model.find()`  메소드에 필터 객체를 전달하면 조건을 만족하는 데이터만 조회할 수 있습니다.
- `persons` 라는 컬렉션에 아래와 같은 데이터가 있고 이를 이루는 `Person` 모델이 있다고 가정합니다.

```jsx
[
  {
    "_id": "645094e4670add1f1f973f68",
    "name": "James",
    "email": "james@gmail.com",
    "age": 26
  },
  {
    "_id": "645094e4670add1f1f973f67",
    "name": "Charlie",
    "email": "charlie@naver.com",
    "age": 30
  },
  {
    "_id": "645094e4670add1f1f973f66",
    "name": "Alice",
    "email": "alice@gmail.com",
    "age": 21
  },
  {
    "_id": "645094e4670add1f1f973f65",
    "name": "Paul",
    "email": "paul@naver.com",
    "age": 40
  },
  {
    "_id": "645094e4670add1f1f973f64",
    "name": "Hannah",
    "email": "hannah@gmail.com",
    "age": 34
  }
]
```

## 일치

- 가장 많이 사용하는 필터는 필드의 값이 특정 값과 일치하는지 확인하는 겁니다.

```jsx
Person.find({ name: 'James' });
```

```jsx
[
  {
    "_id": "645094e4670add1f1f973f68",
    "name": "James",
    "email": "james@gmail.com",
    "age": 26
  }
]
```

```jsx
Person.find({ age: 21 });
```

```jsx
[
  {
    "_id": "645094e4670add1f1f973f66",
    "name": "Alice",
    "email": "alice@gmail.com",
    "age": 21
  }
]
```

## 비교 연산자

- 필드가 특정 값을 초과하는지 확인하려면 `$gt` 연산자, 특정 값 미만인지 확인하려면 `$lt`  연산자를 사용할 수 있습니다.
- 연산자를 사용하는 경우 새로운 객체 안에 네스팅(nesting)합니다.

```jsx
Person.find({ age: { $gt: 35 } });
```

```jsx
[
  {
    "_id": "645094e4670add1f1f973f65",
    "name": "Paul",
    "email": "paul@naver.com",
    "age": 40
  }
]
```

```jsx
Person.find({ age: { $lt: 35 } });
```

```jsx
[
  {
    "_id": "645094e4670add1f1f973f68",
    "name": "James",
    "email": "james@gmail.com",
    "age": 26
  },
  {
    "_id": "645094e4670add1f1f973f67",
    "name": "Charlie",
    "email": "charlie@naver.com",
    "age": 30
  },
  {
    "_id": "645094e4670add1f1f973f66",
    "name": "Alice",
    "email": "alice@gmail.com",
    "age": 21
  },
  {
    "_id": "645094e4670add1f1f973f64",
    "name": "Hannah",
    "email": "hannah@gmail.com",
    "age": 34
  }
]
```

## **Regex 연산자**

- 문자열 필드가 특정 패턴을 가지고 있는지 확인합니다. `$regex` 연산자를 사용하면 됩니다.

```jsx
Person.find({ email: { $regex: 'gmail\.com$' } });
```

```jsx
[
  {
    "_id": "645094e4670add1f1f973f68",
    "name": "James",
    "email": "james@gmail.com",
    "age": 26
  },
  {
    "_id": "645094e4670add1f1f973f66",
    "name": "Alice",
    "email": "alice@gmail.com",
    "age": 21
  },
  {
    "_id": "645094e4670add1f1f973f64",
    "name": "Hannah",
    "email": "hannah@gmail.com",
    "age": 34
  }
]
```

## AND 연산자

- 여러 조건을 모두 만족하는 결과만 필터하고 싶다면 하나의 객체 안에 여러 조건을 작성하면 됩니다.

```jsx
Person.find({ age: { $lt: 32 }, email: { $regex: 'gmail\.com$' } });
```

```jsx

[
  {
    "_id": "645094e4670add1f1f973f68",
    "name": "James",
    "email": "james@gmail.com",
    "age": 26
  },
 
  {
    "_id": "645094e4670add1f1f973f66",
    "name": "Alice",
    "email": "alice@gmail.com",
    "age": 21
  }
]
```

## OR 연산자

- 여러 조건 중 하나라도 만족하는 결과를 필터 하고 싶다면 `$or` 연산자를 사용하면 됩니다.

```jsx
Person.find({ $or: [{ age: { $lt: 32 } }, { email: { $regex: 'gmail\.com$' } }] });
```

```jsx
[
  {
    "_id": "645094e4670add1f1f973f68",
    "name": "James",
    "email": "james@gmail.com",
    "age": 26
  },
  {
    "_id": "645094e4670add1f1f973f67",
    "name": "Charlie",
    "email": "charlie@naver.com",
    "age": 30
  },
  {
    "_id": "645094e4670add1f1f973f66",
    "name": "Alice",
    "email": "alice@gmail.com",
    "age": 21
  },
  {
    "_id": "645094e4670add1f1f973f64",
    "name": "Hannah",
    "email": "hannah@gmail.com",
    "age": 34
  }
]
```

## `.findOne()` 메소드

- **`.find()`** 는 여러 객체를 가져오는 데 사용하고
- **`.findById()`** 는 `_id` 에 해당하는 객체 하나를 가져오는데 사용합니다.
- **`findOne()`** 는 `_id` 대신 다른 조건을 만족하는 객체 하나를 가져올 때 사용합니다.
- 조건을 만족하는 객체가 여러 개일 경우 처음으로 매칭된 객체 하나만 리턴합니다.

```jsx
Person.findOne({ name: 'James' });
```

```jsx
{
  "_id": "645094e4670add1f1f973f68",
  "name": "James",
  "email": "james@gmail.com",
  "age": 26
}
```

```jsx
Person.findOne({ email: { $regex: 'gmail\.com$' } });
```

```jsx

{
  "_id": "645094e4670add1f1f973f68",
  "name": "James",
  "email": "james@gmail.com",
  "age": 26
}
```

- `_id` 를 객체로 가져올 때는 `.findById()` 를 사용하고
- 다른 조건에 해당하는 객체 하나를 가져와야 할 때는 `.findOne()` 메소드를 사용합니다.

## 정렬 개수 제한

- `.find()` 는 쿼리를 리턴하기 때문에 뒤에 메소드를 **체이닝(chaining)**할 수 있습니다.
- 자주 체이닝하는 메소드는 정렬 메소드인 `.sort()` 와 개수 제한 메소드인 `.limit()` 입니다.

```jsx
app.get('/tasks', async (req, res) => {
    /** 쿼리 파라미터
     *  - sort: 'oldest'인 경우 오래된 태스크 기준, 나머지 경우 새로운 태스크 기준
     *  - count: 태스크 개수
     */
    const sort = req.query.sort;
    const count = Number(req.query.count) || 0;

    const sortOption = { createdAt: sort === 'oldest' ? 'asc' : 'desc' };
    const tasks = await Task.find().**sort**(sortOption).**limit**(count);

    res.send(tasks);
  })
);
```

## 수정 (Upadate)

- 객체를 가져온 후 필드를 수정하고 `.save()` 를 호출하면 됩니다.

```jsx
app.patch('/tasks/:id', async (req, res) => {
  const task = await Task.findById(req.params.id);
  if (task) {
    Object.keys(req.body).forEach((key) => {
      task[key] = req.body[key];
    });
    await task.save();
    res.send(task);
  } else {
    res.status(404).send({ message: 'Cannot find given id.' });
  }
});
```

## 삭제 (Delete)

- `.findByIdAndDelete()` 메소드를 이용해서 객체를 가져오는 것과 동시에 객체를 삭제할 수 있습니다.

```jsx
app.delete('/tasks/:id', async (req, res) => {
  const task = await Task.findByIdAndDelete(req.params.id);
  if (task) {
    res.sendStatus(204);
  } else {
    res.status(404).send({ message: 'Cannot find given id.' });
  }
});
```

## 비동기 오류

- 비동기 코드에서 오류가 나면 서버가 죽기 때문에 이를 따로 처리해 줘야 합니다.
- 모든 핸들러를 감싸는 `asyncHandler()` 같은 함수를 정의하고 안에서 `try`, `catch` 문을 활용할 수 있습니다.

```jsx
function asyncHandler(handler) {
  return async function (req, res) {
    try {
      await handler(req, res);
    } catch (e) {
      // e.name(오류 이름), e.message(오류 메시지) 이용해서 오류 처리
    }
  };
}

// ...

app.get('/tasks', asyncHandler(async (req, res) => { ... }));
```

<br>

# 배포하기

## 서비스 배포

- 배포란 유저들이 실제로 서비스를 쓸 수 있게 하는 것입니다.
- 개인 컴퓨터에서 `npm run dev` 나 `npm start` 를 해야 서버가 실행되는데 개인 컴퓨터에서 실행되는 서버는 다른 컴퓨터에서는 접근할 수 없습니다.

## 배포를 하기전에 해야 하는 작업

### 1. 환경 변수 설정

### 2. CORS 설정

## 환경 변수란?

- 환경 변수 (Environment Variable) 는 프로그램이 실행되는 환경과 관련이 있는 변수들입니다.
- 개발을 할 때와 서비스를 운영할 때는 서로 다른 **데이터베이스**를 사용하기 때문에 환경에 따라  `env.js` 파일에 있는 `DATABASE_URL` 값이 달라집니다.
- **포트 번호**도 마찬가지로 개발을 할 때는 3000번을 많이 쓰지만 서비스를 운영할 때는 http나 https에 해당하는 80번 또는 443번을 사용합니다.
- `dotenv` 라는 라이브러리를 써서 환경 변수를 다룰 수 있습니다.

## `dotenv` 라이브러리 사용하기

- npm으로 `dotenv` 라이브러리를 설치합니다.

```jsx
npm install dotenv
```

- 그리고 `.env` 라는 파일을 만들고 안에 아래와 같이 작성해줍니다.

`.env`

```jsx
DATABASE_URL="mongodb+srv://<username>:<password>@todo-api.l0aepsl.mongodb.net/todo-api?retryWrites=true&w=majority"
PORT=3000
```

- `DATABASE_URL` 은 `env.js` 파일에 있던 값을 그대로 가져오면 됩니다. ( `.env` 파일에서는 큰 따옴 표를 사용합니다.)
- `.env` 파일에서 환경 변수를 가져오려면 코드를 아래와 같이 수정하면 됩니다.

`app.js (수정 전)` 

```jsx
// ...

import { DATABASE_URL } from './env.js';

// ...

mongoose.connect(DATABASE_URL).then(() => console.log('Connected to DB'));

// ...

app.listen(3000, () => console.log('Server Started'));
```

`app.js (수정 후)` 

```jsx
// ...

import * as dotenv from 'dotenv';
dotenv.config();

// ...

mongoose.connect(process.env.DATABASE_URL).then(() => console.log('Connected to DB'));

// ...

app.listen(process.env.PORT || 3000, () => console.log('Server Started'));
```

- **`dotenv` 라이브러리를 import하고 `config()`  함수를 호출하면 `.env` 파일에 있는 변수들이 `process.env` 라는 객체에 로딩됩니다.**
- `dotenv` 를 import 할 때는 위와 같은 방식이 권장됩니다.
- `process.env.PORT || 3000` 이 부분은 만약 `PORT` 환경 변수가 없다면 `3000`  을 사용하는 겁니다.
- `seed.js` 파일도 똑같이 똑같이 바꿔줍니다.

`seed.js (수정 전)`

```jsx
// ...

import { DATABASE_URL } from './env.js';

// ...

mongoose.connect(DATABASE_URL);
```

`seed.js (수정 후)` 

```jsx
// ...

import * as dotenv from 'dotenv';
dotenv.config();

// ...

mongoose.connect(process.env.DATABASE_URL);
```

- `env.js` 같은 일반 자바스크립트 파일 대신 `dotenv` 를 사용하면 몇 가지 장점이 있습니다.

- 첫 번째는 환경 변수를 더 안전하게 관리할 수 있다는 점입니다.
- 환경 변수에는 유저네임이나 비밀번호 같은 민감한 정보가 들어가는 경우가 더 많습니다.
- 코드를 GitHub 같은 온라인 저장소에 올릴 때도 `.env` 파일은 제외되도록 설정합니다.

- 두 번째 장점은 배포 시에 굉장히 편리하다는 점입니다.
- 배포 서비스는 서비스 내에서 환경 변수를 설정할 수 있습니다.
- ⇒> 서비스가 알아서 `.env` 파일을 생성하거나 다른 방식으로 `process.env` 에 환경 변수를 전달합니다.
- 그래서 로컬과 프로덕션 환경에서 완전히 똑같은 코드를 사용해도 각 환경에 맞는 환경 변수 값이 주입됩니다.

## CORS 오류

- 지금 상태로 배포를 하면 프론트엔드에서 배포된 API를 활용할 수 없습니다.
- 프론트엔드 (자바스크립트 코드) 에서 API에 리퀘스트를 보내면 `CORS Error` 라는 게 발생합니다.

```jsx
Access to fetch at <API 주소> from origin <프론트엔드 주소> has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

- 이걸 해결하려면 `CORS (Cross-Origin Resource Sharing)` 을 허용해야 합니다.
- ⇒ 프론트엔드 코드에서 배포된 API를 사용할 수 있게 하려면 `CORS` 를 허용해야 합니다.

## CORS 허용하기

- npm에서 `cors` 라는 패키지를 설치합니다.

```jsx
npm install cors
```

- `app.js` 파일에서 `cors` 를 import하고 `app.use(cors())` 를 추가해줍니다.

`app.js`

```jsx
// ...

**import cors from 'cors';**

// ...

const app = express();

**app.use(cors());**
app.use(express.json());
```

- 이러면 CORS 가 허용돼서 프론트엔드 코드에서 API를 사용할 수 있습니다.
- 참고로 특정 주소에 대해서만 `CORS` 를 허용해도 됩니다.
- 예를 들어 프론트엔드 개발할 때는 [`http://127.0.0.1:3000`](http://127.0.0.1:3000/) , 배포 시에는 [`https://my-todo.com`](https://my-todo.com/) 이라는 주소를 사용한다고 하면 이 두 주소에 대해서만 `CORS` 를 허용하는 게 더 안전합니다.

`app.js`

```jsx
const app = express();

const corsOptions = {
  origin: ['http://127.0.0.1:5500', 'https://my-todo.com'],
};

app.use(cors(corsOptions));
app.use(express.json());
```

- 옵션 객체의 `origin` 프로퍼티에 프론트엔드 주소들을 추가하면 됩니다.

<br>

# 관계형 데이터베이스를 활용한 자바스크립트 서버 만들기

# Prisma 기본기 정리

## Prisma 설치와 초기화

- `Prisma` 를 사용하려면 `prisma` 패키지와 `@prisma/client` 패키지를 설치해야 합니다.

```jsx
npm install prisma --save-dev
npm install @prisma/client
```

- `prisma` 는 Prisma 관련 커맨드를 실행하는 데 필요한 라이브러리이고
- `@prisma/client` 는 Prisma 관련 코드를 실행하기 위해 필요한 라이브러리입니다.
- `prisma` 는 보통 dev dependency로 설치합니다.
- VS Code를 사용한다면 Prisma 익스텐션도 설치해 주는 것이 좋습니다.
- 설치를 완료했으면 아래 커맨드로 초기화를 할 수 있습니다.

```jsx
npx prisma init --datasource-provider db-type
```

- `db-type` 은 `postgresql` , `mysql` , `sqlite` 등으로 설정할 수 있습니다.
- `.env` 파일에 있는 정보를 환경에 맞게 수정해주면 됩니다.
- **`postgresql` 을 사용하려면 macOS에서는 컴퓨터 유저 이름과 비밀번호, Windows에서는 postgres와 설치 시 설정한 비밀번호를 사용하면 됩니다.**

## Prisma Schema

- `schema.prisma` 라는 파일에 필요한 모델들을 정의합니다.

```jsx
// This is your Prisma schema file,
// learn more about it in the docs: https://pris.ly/d/prisma-schema

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id        String   @id @default(uuid())
  email     String   @unique
  firstName String
  lastName  String
  address   String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
}

model Product {
  id          String   @id @default(uuid())
  name        String
  description String?
  category    Category
  price       Float
  stock       Int
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt
}

enum Category {
  FASHION
  BEAUTY
  SPORTS
  ELECTRONICS
  HOME_INTERIOR
  HOUSEHOLD_SUPPLIES
  KITCHENWARE
}
```

- 각 필드는 필드 이름, 필드 타입, 어트리뷰트 (옵셔널) 로 구성되어 있습니다.
- 필트 타입은 `?` 를 사용해서 필수 여부를 정할 수 있고 자주 사용하는 어트리뷰트는 아래와 같습니다.
    - `@id` : 필드가 id라는 것을 나타냄
    - `@unique` : 고유 필드 (값이 중복될 수 없음) 라는 것을 나타냄
    - `@default` : 디폴트 값을 설정할 때 사용
    - `@updatedAt` : 객체가 수정될 때마다 수정 시간을 필드에 기록함
- 위 처럼 필드의 값이 정해진 값 중 하나라면 `enum` 을 사용할 수도 있습니다.

## `enum`

- 유저의 멤버십 (`membership`) 타입을 저장한다고 하면 멤버십은 `BASIC` 과 `PREMIUM` 두 가지 타입이 있습니다.
- 이렇게 필드의 값이 몇 가지 정해진 값 중 하나일때는 **enum (enumerated type)** 을 사용할 수 있습니다.

```jsx
model User {
  // ...
  membership  Membership  @default(BASIC)
}

enum Membership {
  BASIC
  PREMIUM
}
```

- enum 값은 보통 대문자를 사용하고 필드의 타입을 enum 이름으로 설정하면 됩니다.
- `@default`  어트리뷰트도 사용할 수 있습니다.
- enum은 서로 연관된 상수들의 집합입니다.

## `@@unique`

- 여러 필드의 조합이 unique 해야 하는 경우 `@@unique` 어트리뷰트를 사용할 수 있습니다.
- `@@unique` 어트리뷰트는 특정 필드에 종속된 어트리뷰트가 아니기 때문에 모델 아래 부분에 씁니다.

```jsx
model User {
  id        String   @id @default(uuid())
  email     String   @unique
  firstName String
  lastName  String
  address   String
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@unique([firstName, lastName])
}
```

- 위처럼 정의하면 `firstName` 과 `lastName` 의 조합이 unique 해야 합니다.

```jsx
// 삽입 가능

{
  id: "abc",
  firstName: "길동",
  lastName: "홍",
  // ...
},
{
  id: "def",
  firstName: "길동",
  lastName: "박",
  // ...
}

// 삽입 불가능

{
  id: "abc",
  firstName: "길동",
  lastName: "홍",
  // ...
},
{
  id: "def",
  firstName: "길동",
  lastName: "홍",
  // ...
}
```

## Prisma Migration

- `schema.prisma` 파일을 변경하려면 마이그레이션을 해 줘야 합니다.
- **마이그레이션은 스키마 파일의 내용을 데이터베이스에 반영해주는 것입니다.**
- 예를 들어 `schema.prisma` 에 새로운 모델을 정의하고 마이그레이션을 하면 Prisma는 새로운 테이블을 생성하는 SQL 문을 만들어서 실행해 줍니다.

```jsx
npx prisma migrate dev 
```

## Prisma Client

- Prisma Client는 모델 정보를 저장하고 있고 이걸 기반으로 데이터베이스와 상호작용합니다.
- **마이그레이션을 할 때마다 최신 모델 정보를 기반으로 Prisma Client를 생성해줍니다.**
- 모든 데이터베이스 연산은 Prisma Client를 통해 이루어집니다.

```jsx
import { PrismaClient } from '@prisma/client';

const prisma = new PrismaClient();
```

- 연산은 `prisma.model.method()`  형태이며 Promise를 리턴합니다. 이 연산을 ‘쿼리’라고도 부릅니다.

```jsx
const id = 'b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e';

const userData = {
  email: 'yjkim@example.com',
  firstName: '유진',
  lastName: '김',
  address: '충청북도 청주시 북문로 210번길 5',
};;

// 여러 유저 조회
const users = await prisma.user.findMany();

// 유저 1명 조회
const user = await prisma.user.findUnique({
  where: {
    id,
  },
});

// 유저 생성
const user = await prisma.user.create({
  data: userData
});

// 유저 수정
const user = await prisma.user.update({
  where: {
    id,
  },
  data: {
    email: 'yjkim2@example.com',
  },
});

// 유저 삭제
const user = await prisma.user.delete({
  where: {
    id,
  },
});
```

- `where` 는 필터를 할 때 사용하고 `data` 는 데이터를 전달할 때 사용합니다.
- `findMany()` 메소드에는 아래 프로퍼티도 많이 사용됩니다.
    - **`orderBy` : 정렬 기준**
    - **`skip`  : 건너뛸 데이터 개수**
    - **`take` : 조회할 데이터 개수**
- 예를 들어 URL 쿼리 파라미터와 아래와 같이 사용할 수 있습니다.

```jsx
app.get('/users', async (req, res) => {
  const { offset = 0, limit = 10, order = 'newest' } = req.query;
  let **orderBy**;
  switch (order) {
    case 'oldest':
      orderBy = { createdAt: 'asc' };
      break;
    case 'newest':
    default:
      orderBy = { createdAt: 'desc' };
  }
  const users = await prisma.user.findMany({
    orderBy,
    **skip**: parseInt(offset),
    **take**: parseInt(limit),
  });
  res.send(users);
});
```

## 추가적인 CRUD 메소드

### `.findFirst()`

- `id` 로 객체 하나를 조회할 때는 `.findUnique()`  메소드를 사용했었습니다.
- 하지만 `.findUnique()`  메소드는 `id`  필드나 `unique` 한 필드로만 필터를 할 수 있습니다.

```jsx
// OK
const user = await prisma.user.findUnique({
  where: { id: 'b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e' },
});

// 오류(firstName은 unique한 필드가 아님)
const user = await prisma.user.findUnique({
  where: { firstName: '길동' },
});
```

- Unique 하지 않은 필드로 필터를 해서 **객체 하나를 조회**하고 싶을 때는
- **`findFirst()`** 메소드를 사용하면 됩니다.

```jsx
const user = await prisma.user.findFirst({
  where: { firstName: '길동' },
});

console.log(user);
```

```jsx
{
  "id": "b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e",
  "email": "honggd@example.com",
  "firstName": "길동",
  "lastName": "홍",
  "address": "서울특별시 강남구 무실로 123번길 45-6",
  "createdAt": "2023-07-16T09:00:00.000Z",
  "updatedAt": "2023-07-16T09:00:00.000Z"
}
```

- `where` 조건을 만족하는 결과가 여러 개 있을 때 첫 번째 객체를 리턴합니다.
- 데이터베이스가 사용하는 순서를 바꾸고 싶다면 `orderBy` 프로퍼티를 사용하면 됩니다.

### `.upsert()`

- `.upsert()` 메소드는 `where` 조건을 만족하는 객체가 있다면 객체를 업데이트 (update) 하고, 없다면 생성 (insert) 합니다.
- Update와 insert를 합친 메소드라고 생각하시면 됩니다.

```jsx
const user = await prisma.user.upsert({
  where: { email: 'yjkim@example.com' },
  data: {
    email: 'yjkim@example.com',
    firstName: '유진',
    lastName: '김',
    address: '충청북도 청주시 북문로 210번길 5',  
  },
});
```

- 위 예시는 [`yjkim@example.com`](mailto:yjkim@example.com) 이라는 이메일을 가진 유저가 있으면 유저의 정보를 `data` 값으로 업데이트하고,
- [`yjkim@example.com`](mailto:yjkim@example.com) 이메일을 가진 유저가 없으면 `data` 값으로 유저를 생성합니다.

### `.count()`

- 객체의 개수만 필요한 경우 `.count()` 메소드를 사용하면 됩니다.
- `.findMany()`  를 사용해서 배열의 길이를 사용하는 것보다 더 효율적입니다.

```jsx
const count = await prisma.product.count({
  where: {
    category: 'FASHION',
  },
});

console.log(count);
```

```jsx
16
```

## 필터 조건

- `where` 프로퍼티에 필드 이름과 값을 전달하면 기본적으로 일치 (equal) 연산자가 활용됩니다.

```jsx
// category가 'FASHION'인 Product들 필터
const products = await prisma.product.findMany({
  where: {
    category: 'FASHION',
  },
});
```

- 하지만 이 외에도 `not` , `in`  , `contains` , `startsWidth` 같은 다양한 비교 연산자를 사용할 수 있습니다.

```jsx
// category가 'FASHION'이 아닌 Product들 필터
const products = await prisma.product.findMany({
  where: {
    category: {
      not: 'FASHION',
    },
  },
});

// category가 'FASHION', 'SPORTS', 'BEAUTY' 중 하나인 Product들 필터
const products = await prisma.product.findMany({
  where: {
    category: {
      in: ['FASHION', 'SPORTS', 'BEAUTY']
    },
  },
});

// name에 'TV'가 들어가는 Product들 필터
const products = await prisma.product.findMany({
  where: {
    name: {
      contains: 'TV',
    },
  },
});

// name이 'Apple'로 시작하는 Product들 필터
const products = await prisma.product.findMany({
  where: {
    name: {
      startsWith: 'Apple',
    },
  },
});
```

- 그리고 여러 필터를 조합할 수도 있습니다.
- AND (여러 필터 조건을 만족해야 하는 경우) : `where` 안에 프로퍼티를 여러 개 쓰면 됩니다.

```jsx
// category가 'FASHION'이면서 name에 '나이키'가 들어가는 Product들 필터
const products = await prisma.product.findMany({
  where: {
    category: 'FASHION',
    name: {
      contains: '나이키',
    },
  },
});
```

- OR (여러 필터 조건 중 하나만 만족해도 되는 경우) : `OR`  연산자를 사용하면 됩니다.

```jsx
// name에 '아디다스'가 들어가거나 '나이키'가 들어가거는 Product들 필터
const products = await prisma.product.findMany({
  where: {
    OR: [
      {
        name: {
          contains: '아디다스',
        },
      },
      {
        name: {
          contains: '나이키',
        },
      },
    ],
  },
});
```

- NOT (필터 조건을 만족하면 안 되는 경우) : `NOT`  연산자를 사용하면 됩니다.

```jsx
// name에 '삼성'이 들어가지만 'TV'는 들어가지 않는 Product들 필터
const products = await prisma.product.findMany({
  where: {
    name: {
      contains: '삼성',
    },
    NOT: {
      name: {
        contains: 'TV',
      },
    },
  },
});
```

## 데이터베이스 시딩

- Prisma Client 를 이용해서 데이터베이스 시딩도 할 수 있습니다.
- `.createMany()`  메소드를 사용하면 배열 데이터를 한꺼번에 삽입할 수 있습니다.

`seed.js`  

```jsx
import { PrismaClient } from '@prisma/client';
import { USERS, PRODUCTS } from './mock.js';

const prisma = new PrismaClient();

async function main() {
  // 기존 데이터 삭제
  await prisma.user.deleteMany();
  await prisma.product.deleteMany();

  // 목 데이터 삽입
  await prisma.user.createMany({
    data: USERS,
    skipDuplicates: true,
  });

  await prisma.product.createMany({
    data: PRODUCTS,
    skipDuplicates: true,
  });
}

main()
  .then(async () => {
    await prisma.$disconnect();
  })
  .catch(async (e) => {
    console.error(e);
    await prisma.$disconnect();
    process.exit(1);
  });
```

`seed.js`

```jsx
const USERS = [
  // ...
];

const PRODUCTS = [
  // ...
];
```

`package.json`  파일에 **`seed`  커맨드를 정의**하면 Prisma 커맨드로 시딩을 할 수 있습니다.

`package.json` 

```jsx
{
  // ...
  "prisma": {
    **"seed": "node prisma/seed.js"**
  }
}
```

- `prisma/seed.js` 부분은 시드 연산을 실행하는 파일 경로입니다.
- 아래 커맨드로 시딩을 할 수 있습니다.

```jsx
npx prisma db seed
```

## 유효성 검사

- 다양한 방식으로 유효성 검사를 할 수 있지만 `superstruct` 라는 라이브러리로 유효성 검사를 하는 방법
- ⇒ 예상하는 데이터 형식을 정의하고 실제 데이터를 비교하는 방식입니다.
- 먼저 패키지들을 설치해야 합니다.

```jsx
npm install superstruct is-email is-uuid
```

- `is-email` 은 이메일 형식을 확인하고 싶은 경우 설치하면 되고 `is-uuid` 는 UUID 형식을 확인하고 싶은 경우 설치하면 됩니다.
- `superstruct` 라이브러리의 `.string()` , `.number()` , `.integer()` , `.boolean()`  , `.define()`, `.object()` , `.enums()` , `.array()` , `.partial()` 타입들도 틀을 정의하고
- `.size()` , `.min()` , `.max()` 함수로 제약을 추가하면 됩니다.

`structs.js`

```jsx
import * as s from 'superstruct';

const CATEGORIES = [
  'FASHION',
  'BEAUTY',
  'SPORTS',
  'ELECTRONICS',
  'HOME_INTERIOR',
  'HOUSEHOLD_SUPPLIES',
  'KITCHENWARE',
];

export const CreateUser = s.object({
  email: s.define('Email', isEmail),
  firstName: s.size(s.string(), 1, 30),
  lastName: s.size(s.string(), 1, 30),
  address: s.string(),
});

export const PatchUser = s.partial(CreateUser);

export const CreateProduct = s.object({
  name: s.size(s.string(), 1, 60),
  description: s.string(),
  category: s.enums(CATEGORIES),
  price: s.min(s.number(), 0),
  stock: s.min(s.integer(), 0),
});

export const PatchProduct = s.partial(CreateProduct);

```

- 데이터를 비교할 때는 `assert()`  함수를 사용하면 됩니다.

`app.js` 

```jsx
import { assert } from 'superstruct';
import { CreateUser } from './structs.js';

// ...

app.post('/users', async (req, res) => {
  assert(req.body, CreateUser); // CreateUser 형식이 아니라면 오류 발생
  // ...
});
```

## 오류 처리

`app.js`

```jsx
import { PrismaClient, Prisma } from '@prisma/client';

// ...

function asyncHandler(handler) {
  return async function (req, res) {
    try {
      await handler(req, res);
    } catch (e) {
      if (
        e instanceof Prisma.PrismaClientValidationError ||
        e.name === 'StructError'
      ) {
        res.status(400).send({ message: e.message });
      } else if (
        e instanceof Prisma.PrismaClientKnownRequestError &&
        e.code === 'P2025'
      ) {
        res.sendStatus(404);
      } else {
        res.status(500).send({ message: e.message });
      }
    }
  };
}

// ...

app.post('/users', asyncHandler(async (req, res) => {
  assert(req.body, CreateUser); 
  // ...
}));
```

- `e.name === 'StructError'` : Superstruct 객체와 형식이 다를 경우 발생
- `e instanceof Prisma.PrismaClientValidationError` : 데이터를 저장할 때 모델에 정의된 형식과 다른 경우 발생 (Superstruct로 철저히 검사하면 이 상황은 잘 발생하지 않지만 안전성을 위해 둘 다 검사)
- `e instanceof Prisma.PrismaClientKnownRequestError && e.code === 'P2025'` : 객체를 찾을 수 없을 경우 발생

<br>

# Prisma와 관계 정리

## 관계 정의하기

## 일대다 관계

- 일대다 관계는 다음과 같이 정의합니다.

`schema.prisma`

```jsx
model User {
  // ...
  orders  Order[]
}

model Order {
  // ...
  user    User    **@relation(fields: [userId], references: [id])**
  userId  String
}
```

- ‘다’ 에 해당하는 모델에는 아래 필드를 정의합니다.
    - 다른 모델을 가리키는 관계 필드 ( `user` ): `@relation` 어트리뷰트로 foreign key 필드가 무엇이고, 어떤 필드를 **참조**하는지 명시합니다.
    - Foreign key 필드 ( `userId` )
- ‘일’ 에 해당하는 모델에는 아래 필드를 정의합니다.
    - 다른 모델 배열을 저장하는 관계 피드 ( `orders` )

## 일대일 관계

- 일대일 관계는 다음과 같이 정의합니다.

`schema.prisma`  

```jsx
model User {
  // ...
  userPreference  UserPreference?
}

model UserPreference {
  // ...
  user    User    @relation(fields: [userId], references: [id])
  userId  String  @unique
}
```

- Foreign key를 어느 쪽에 정의하든 큰 상관은 없지만, 만약 한쪽 모델이 다른 쪽 모델에 **속해 있다**면 속해 있는 모델에 정의하는 것이 좋습니다.
- Foreign key를 정의하는 모델에는 아래 필드를 정의합니다.
    - 다른 모델을 가리키는 관계 필드 ( `user`) : `@relation`  어트리뷰트로 foreign key 필드가 무엇이고, 어떤 필드를 참조하는지 명시합니다.
    - Foreign key 필드 ( `userId` ) : `@unique` 으로 설정해줘야 합니다.
- 반대쪽 모델에는 아래 필드를 정의합니다.
    - 다른 모델을 가리키는 옵셔널 관계 필드 ( `userPreference` )

## 다대다 관계

- 다대다 관계는 다음과 같이 정의합니다.

`schema.prisma`

```jsx
model User {
  // ...
  savedProducts  Product[]
}

model Product {
  // ...
  savedUsers  User[]
}
```

- 양쪽 모델에 서로의 배열을 저장하는 관계 필드를 정의하면 됩니다.

## 최소 카디널리티

- 최소 카디널리티는 스키마로 완벽히 제어하기 어렵습니다.
- 유일하게 설정할 수 있는 부분은 Foreign Key 와 관계 필드를 옵셔널 ( `?`  ) 하게 만드는 것입니다.
- Foreign key가 있는 모델 반대쪽의 최소 카디널리티가 1이라면 foreign key와 관계 필드를 필수로 만들고 최소 카디널리티가 0이라면 foreign key와 관계 필드를 옵셔널하게 만들면 됩니다.

![image.png](/8thWeek/최소%20카디널리티.jpeg)

```jsx
model User {
  // ...
  orders  Order[]
}

model Order {
  // ...
  user    User    @relation(fields: [userId], references: [id])
  userId  String
}
```

```jsx
model User {
  // ...
  orders  Order[]
}

model Order {
  // ...
  user    User?    @relation(fields: [userId], references: [id])
  userId  String?
}
```

### `onDelete` 옵션

- onDelete 옵션은 연결된 데이터가 삭제됐을 때 기존 데이터를 어떻게 처리할지를 정하는 옵션입니다.

`schema.prisma`

```jsx
model Order {
  // ...
  user    User    @relation(fields: [userId], references: [id], onDelete: ...)
  userId  String
}
```

- `Cascade` : `userId` 가 가리키는 유저가 삭제되면 기존 데이터도 삭제됩니다.
- `Restrict` : `userId` 를 통해 유저를 참조하는 주문이 하나라도 있다면 유저를 삭제할 수 없습니다.
- `SetNull` : `userId` 가 가리키는 유저가 삭제되면 `userId` 를 NULL로 설정합니다. `user` 와 `userId` 모두 옵셔널해야 합니다.
- `SetDefault` : `userId` 가 가리키는 유저가 삭제되면 `userId` 를 디폴트 값으로 설정합니다. `userId` 필드에 `@default()` 를 제공해야 합니다.

⇒ **관계 필드와 foreign key가 필수일 경우 `Restrict` 가 기본값이고 옵셔널할 경우 `SetNull`  이 기본값입니다.**

## 관계 활용하기

- Prisma Client에서는 관계 필드들을 자유롭게 이용할 수 있습니다.

`schema.prisma`

```jsx
model User {
  // ...
  orders  Order[]
}

model Order {
  // ...
  user    User    @relation(fields: [userId], references: [id])
  userId  String
}
```

- 예를 들어 위와 같은 관계가 있을 때 `orders` 필드와 `user`  필드는 실제로 어떤 데이터를 저장하지 않지만 (데이터베이스에서는 `userId` 로 유저의 `id` 만 저장하면 됩니다)

## 관련된 객체 조회하기

`schema.prisma`

```jsx
model User {
  // ...
  userPreference  UserPreference?
}

model UserPreference {
  // ...
  user    User    @relation(fields: [userId], references: [id])
  userId  String  @unique
}
```

- `userPreference` 나 `user` 같은 필드는 기본적으로 조회 결과에 포함되지 않습니다.
- 이런 필드를 같이 조회하려면 `include` 프로퍼티를 사용해야 합니다.

`app.js`

```jsx
const id = '6c3a18b0-11c5-4d97-9019-9ebe3c4d1317';

const user = await prisma.user.findUniqueOrThrow({
  where: { id },
  **include: {
    userPreference: true,
  }**,
});

console.log(user);
```

```jsx
{
  id: '6c3a18b0-11c5-4d97-9019-9ebe3c4d1317',
  email: 'kimyh@example.com',
  firstName: '영희',
  lastName: '김',
  address: '경기도 고양시 봉명로 789번길 21',
  createdAt: 2023-07-16T09:30:00.000Z,
  updatedAt: 2023-07-16T09:30:00.000Z,
  userPreference: {
    id: 'e1c1e5c1-5312-4f7b-a3d6-4cbb2b4f8828',
    receiveEmail: false,
    createdAt: 2023-07-16T09:30:00.000Z,
    updatedAt: 2023-07-16T09:30:00.000Z,
    userId: '6c3a18b0-11c5-4d97-9019-9ebe3c4d1317'
  }
}
```

- 네스팅을 이용해서 관련된 객체의 특정 필드만 조회할 수도 있습니다.

`app.js` 

```jsx
const id = '6c3a18b0-11c5-4d97-9019-9ebe3c4d1317';

const user = await prisma.user.findUniqueOrThrow({
  where: { id },
  include: {
    userPreference: {
      select: {
        receiveEmail: true,
      },
    },
  },
});

console.log(user);
```

```jsx
{
  id: '6c3a18b0-11c5-4d97-9019-9ebe3c4d1317',
  email: 'kimyh@example.com',
  firstName: '영희',
  lastName: '김',
  address: '경기도 고양시 봉명로 789번길 21',
  createdAt: 2023-07-16T09:30:00.000Z,
  updatedAt: 2023-07-16T09:30:00.000Z,
  userPreference: { receiveEmail: false }
}
```

- 관계 필드가 배열 형태여도 똑같이 `include`  를 사용할 수 있습니다.

`schema.prisma`

```jsx
model User {
  // ...
  savedProducts  Product[]
}

model Product {
  // ...
  savedUsers  User[]
}
```

`app.js`

```jsx
const id = '6c3a18b0-11c5-4d97-9019-9ebe3c4d1317';

const user = await prisma.user.findUniqueOrThrow({
  where: { id },
  include: {
    savedProducts: true,
  },
});

res.send(user);
```

```jsx
{
  id: '6c3a18b0-11c5-4d97-9019-9ebe3c4d1317',
  email: 'kimyh@example.com',
  firstName: '영희',
  lastName: '김',
  address: '경기도 고양시 봉명로 789번길 21',
  createdAt: 2023-07-16T09:30:00.000Z,
  updatedAt: 2023-07-16T09:30:00.000Z,
  savedProducts: [
    {
      id: 'f8013040-b076-4dc4-8677-11be9a17162f',
      name: '랑방 샤워젤 세트',
      description: '랑방의 향기로운 샤워젤 세트입니다. 피부를 부드럽게 케어하며, 향기로운 샤워 시간을 선사합니다.',
      category: 'BEAUTY',
      price: 38000,
      stock: 20,
      createdAt: 2023-07-14T10:00:00.000Z,
      updatedAt: 2023-07-14T10:00:00.000Z
    },
    {
      id: '7f70481b-784d-4b0e-bc3e-f05eefc17951',
      name: 'Apple AirPods 프로',
      description: 'Apple의 AirPods 프로는 탁월한 사운드 품질과 노이즈 캔슬링 기능을 갖춘 무선 이어폰입니다. 간편한 터치 컨트롤과 긴 배터리 수명을 제공합니다.',
      category: 'ELECTRONICS',
      price: 320000,
      stock: 10,
      createdAt: 2023-07-14T11:00:00.000Z,
      updatedAt: 2023-07-14T11:00:00.000Z
    },
    {
      id: '4e0d9424-3a16-4a5e-9725-0e9d2f9722b3',
      name: '베르사체 화장품 세트',
      description: '베르사체의 화장품 세트로 화려하고 특별한 분위기를 연출할 수 있습니다. 다양한 아이템으로 구성되어 있으며, 고품질 성분을 사용하여 피부에 부드럽고 안정적인 관리를 제공합니다.',
      category: 'BEAUTY',
      price: 65000,
      stock: 8,
      createdAt: 2023-07-14T11:30:00.000Z,
      updatedAt: 2023-07-14T11:30:00.000Z
    },
    {
      id: 'a4ff201c-48f7-4963-b317-2e9e4e3e43b7',
      name: '랑방 매트 틴트',
      description: '랑방 매트 틴트는 풍부한 컬러와 지속력을 제공하는 제품입니다. 입술에 부드럽게 발리며 오래 지속되는 매트한 마무리를 선사합니다.',
      category: 'BEAUTY',
      price: 35000,
      stock: 20,
      createdAt: 2023-07-14T16:00:00.000Z,
      updatedAt: 2023-07-14T16:00:00.000Z
    }
  ]
}
```

## 관련된 객체 생성, 수정하기

- 객체를 생성하거나 수정할 때 관련된 객체를 동시에 생성하거나 수정할 수 있습니다.

`schema.prisma`

```jsx
model User {
  // ...
  userPreference  UserPreference?
}

model UserPreference {
  // ...
  user    User    @relation(fields: [userId], references: [id])
  userId  String  @unique
}
```

- 데이터를 `data` 프로퍼티로 바로 전달하지 않고 관련된 객체 필드에 `create`  또는 `update` 프로퍼티를 이용해야 합니다.

`app.js`

```jsx
/* create */

const postBody = {
  email: 'yjkim@example.com',
  firstName: '유진',
  lastName: '김',
  address: '충청북도 청주시 북문로 210번길 5',
  userPreference: { 
    receiveEmail: false,
  },
};

const { userPreference, ...userFields } = postBody;

const user = await prisma.user.**create**({
  data: {
    ...userFields,
    userPreference: {
      create: userPreference,
    },
  },
  include: {
    userPreference: true,
  },
});

console.log(user);
```

```jsx
{
  id: 'd2f4a7fe-0831-462f-9b11-baddb0e4aba2',
  email: 'yjkim@example.com',
  firstName: '유진',
  lastName: '김',
  address: '충청북도 청주시 북문로 210번길 5',
  createdAt: 2023-08-25T05:12:00.740Z,
  updatedAt: 2023-08-25T05:12:00.740Z,
  userPreference: {
    id: '8dffa6b8-bb2e-4c6e-82ef-053d69b4face',
    receiveEmail: false,
    createdAt: 2023-08-25T05:12:00.740Z,
    updatedAt: 2023-08-25T05:12:00.740Z,
    userId: 'd2f4a7fe-0831-462f-9b11-baddb0e4aba2'
  }
}
```

```jsx
/* update */

const id = 'b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e';

const patchBody = {
  email: 'honggd2@example.com', 
  userPreference: {
    receiveEmail: false,
  },
};

const { userPreference, ...userFields } = patchBody;

const user = await prisma.user.**update**({
  where: { id },
  data: {
    ...userFields,
    userPreference: {
      update: userPreference,
    },
  },
  include: {
    userPreference: true,
  },
});

console.log(user);
```

```jsx
{
  id: 'b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e',
  email: 'honggd2@example.com',
  firstName: '길동',
  lastName: '홍',
  address: '서울특별시 강남구 무실로 123번길 45-6',
  createdAt: 2023-07-16T09:00:00.000Z,
  updatedAt: 2023-08-25T05:15:06.106Z,
  userPreference: {
    id: '936f5ea4-6e6c-4e5e-91a3-78f5644e1f9a',
    receiveEmail: false,
    createdAt: 2023-07-16T09:00:00.000Z,
    updatedAt: 2023-08-25T05:15:06.106Z,
    userId: 'b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e'
  }
}
```

## 관련된 객체 연결, 연결 해제하기

- 다대다 관계는 보통 두 객체가 이미 존재하고, 그 사이에 관계를 생성하려고 하는 경우가 많습니다.
- 이런 경우 `connect` 프로퍼티를 이용하면 됩니다.

`schema.prisma`

```jsx
model User {
  // ...
  savedProducts  Product[]
}

model Product {
  // ...
  savedUsers  User[]
}
```

`app.js`

```jsx
const userId = 'b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e';
const productId = 'c28a2eaf-4d87-4f9f-ae5b-cbcf73e24253';

const user = await prisma.user.update({
  where: { id: userId },
  data: {
    savedProducts: {
      **connect**: {
        id: productId,
      },
    },
  },
  include: {
    savedProducts: true,
  },
});

console.log(user);
```

```jsx
{
  id: 'b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e',
  email: 'honggd2@example.com',
  firstName: '길동',
  lastName: '홍',
  address: '서울특별시 강남구 무실로 123번길 45-6',
  createdAt: 2023-07-16T09:00:00.000Z,
  updatedAt: 2023-08-25T05:15:06.106Z,
  savedProducts: [
    {
      id: 'c28a2eaf-4d87-4f9f-ae5b-cbcf73e24253',
      name: '쿠진앤에이 오믈렛 팬',
      description: '쿠진앤에이의 오믈렛 팬은 오믈렛을 쉽고 빠르게 만들 수 있는 전용 팬입니다. 내열성이 뛰어나며 논스틱 처리로 편리한 사용과 청소가 가능합니다.',
      category: 'KITCHENWARE',
      price: 25000,
      stock: 8,
      createdAt: 2023-07-15T13:30:00.000Z,
      updatedAt: 2023-07-15T13:30:00.000Z
    }
  ]
}
```

- 반대로 연결을 해제하고 싶다면 disconnect 프로퍼티를 이용하면 됩니다.

```jsx
const userId = 'b8f11e76-0a9e-4b3f-bccf-8d9b4fbf331e';
const productId =

const user = await prisma.user.update({
  where: { id: userId },
  data: {
    savedProducts: {
      **disconnect**: {
        id: productId,
      },
    },
  },
  include: {
    savedProducts: true,
  },
});

console.log(user);
```

