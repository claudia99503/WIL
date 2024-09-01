# 웹 API 디자인
# REST의 제약 조건

## 첫 번째 제약 조건 : Client-Server (클라이언트-서버)

- API를 통해 정보를 교환하는 주체는 클라이언트와 서버 구조를 가져야 한다.
- 클라이언트와 서버의 분리를 통해 서로 의존하지 않는 구조를 가져야 한다. (관심사의 분리)

## 두 번째 제약 조건 : Stateless (무상태성)

- 클라이언트는 상태를 저장하지 않는다.
- 클라이언트의 각 리퀘스트는 서버가 리퀘스트를 이해하는 데에 필요한 모든 정보를 포함해야 한다.

## 세 번째 제약 조건 : Cache (캐시)

- 데이터 복사본을 임시 저장 위치에 저장하여 보다 빠르게 액세스 할 수 있도록 하는 프로세스인 캐싱을 통해 네트워크 효율성을 높인다.
- 리퀘스트에 대한 리스폰스 캐시 가능 및 불가능 여부가 들어 있어야 한다.

## 네 번째 제약 조건 : Uniform Interface (일관된 인터페이스)

- 전체 시스템을 잘 파악할 수 있도록 일관된 인터페이스를 제공해야 한다.
- 이를 통해 구현체가 서비스와 별도로 독립적으로 진화할 가능성을 얻게 도니다. (낮은 결합도)
    - 예를 들어 클라이언트가 업데이트되어도 서버를 함께 업데이트할 필요가 없어진다. (반대의 경우에도 해당)

## 다섯 번째 제약 조건 : Layered System (계층화된 시스템)

- 클라이언트는 서버에 직접 연결 되었는지, 중간 서버를 통해 연결되었는지 알 수 없어야 한다.

## 여섯 번째 제약 조건 : Code on Demand (주문형 코드)

- 서버에서 보낸 코드를 클라이언트에서 실행할 수 있어야 한다. (e.g. JavaScript)
- 선택적 제약 조건이며 지키지 않아도 REST에는 문제가 없다.

- 서버-클라이언트 구조의 웹 서비스에서 HTTP 프로토콜을 사용하여 리퀘스트와 리스폰스를 주고받는다면 **네 번째 제약조건: Uniform Interface (일관된 인터페이스)** 을 제외한 5개의 제약 조건의 대부분이 지켜지게 됩니다.
- 이는 HTTP 프로토콜의 특징 덕분입니다.

## HTTP의 특징이 만족시키는 REST의 제약 조건

- 리퀘스트 (클라이언트) 와 리스폰스 (서버) 의 구조 → **첫 번째 제약 조건 : Client-Server (클라이언트-서버) 만족**
- stateless (무상태성) 과 conneetionless (비연결성) → **두 번째 제약 조건 : Stateless (무상태성) 만족**
- 헤더의 Cache-Control 를 통한 캐시 가능 여부 명시 → **세 번째 제약 조건 : Cache (캐시) 만족**
- 리퀘스트와 리스폰스를 보내는 주체는 중간 계층을 신경 쓰지 않아도 되는 구조 → **다섯 번째 제약 조건 : Layered System (계층화된 시스템) 만족**
- 서버의 코드를 담을 수 있는 Body → **여섯 번째 제약 조건 : Code On Demand (주문형 코드) 만족**

- 그렇다면  **네 번째 제약 조건: Uniform Interface(일관된 인터페이스)**은 어떻게 지킬 수 있을까요?
    - 이 제약 조건은 개발자가 API를 만들 때 가장 영향을 많이 받는 제약 조건입니다.
    - 흔히 "API를 만든다"라고 하면, '어디로 어떻게 무엇을 담아 요청을 보내야 하는지'를 정하게 되는데, 그것을 결정하는 제약 조건이, **네 번째 제약 조건인 Uniform Interface 제약 조건**입니다.
- Unifrom Interface 제약 조건에는 다음과 같은 4가지 하위 제약 조건이 있습니다.
    - identification of resources (자원에 대한 식별)
    - manipulation of resources through representations (표현을 통한 자원에 대한 조작)
    - self-descriptive messages (자기 서술적 메시지)
    - hypermedia as the engine of application state, HATEOAS (하이퍼미디어를 사용한 애플리케이션 상태 표현)

# URI 작성 규칙

- REST의 ‘자원의 식별’ 제약 조건은 접근하고자 하는 자원을 명시하고, 그 자원을 식별할 수 있어야 한다는 내용을 담고 있습니다.
- 웹에서는 URI를 사용하여 자원에 대한 식별을 하고 있습니다.
- URI에 제어하고자 하는 자원에 대해 명시하고, 그 자원을 식별할 수 있는 변하지 않는 ID와 같은 식별자를 URI에 포함하여야 합니다.

## 명사형 사용

- URI는 동작 (동사) 를 가리키는 대신에, 자원 (명사) 을 가리켜야 합니다.
- 자원에는 속성이 있듯이, 명사는 동사가 가지지 않는 속성을 가지기 때문입니다.

### `Bad**❌`**

```jsx
/get-member-list
/create-new-member
```

### **`Good✅`**

```jsx
/members
/articles
```

## 반환 (응답) 하는 자원의 종류에 따라 단수형/복수형 사용하기

- 여러 개의 자원을 반환한다면 복수형, 단 1개의 자원을 반환한다면 단수형을 사용합니다.

```jsx
/members         # 멤버 목록 (복수형)
/members/1       # 멤버 목록 중 (복수형), 1번 멤버 (단수형)
/key             # 1개의 키 (단수형)
```

## 계층 표현을 위하여, 슬래시( `/` ) 사용

- 자원들 간의 계층 표현을 위해서 슬래시 ( `/` ) 을 사용합니다.

```jsx
/articles
/articles/1
/articles/1/comments
/articles/1/comments/2
```

## 마지막 슬래시 ( `/` ) 붙이지 않기

- 마지막에 붙는 슬래시를 트레일링 슬래시 ( Trailling Slash ) 라고 합니다.
- 마지막 슬래시는 REST에서 어떠한 의미도 가지지 않기 때문에 혼동을 줄 수 있습니다.

  ⇒ 따라서 붙이지 않습니다.

### `Bad**❌`**

```jsx
/articles/1/comments/
```

### **`Good✅`**

```jsx
/articles/1/comments
```

## 모두 소문자로 작성 & 띄어쓰기는 대시 ( `-` ) 사용

- 언더 스코어 ( `_` ) 또는 카멜 케이스 ( `CamelCase` )는 가독성을 낮출 수 있기에 사용하지 않습니다.
- 띄어쓰기를 위한 대시 ( `-` ) 와 소문자를 사용합니다.

### `Bad**❌`**

```jsx
/articles/topVoted
/articles/top_voted
```

### **`Good✅`**

```jsx
/articles/top-voted
```

## 파일 확장자 포함하지 않기

- 파일 확장자는 특정 파일을 가리키는지, 아니면 자원의 식별자인지 헷갈릴 수 있기에 넣지 않습니다.
- 만약 HTML, JSON과 같은 미디어 타입을 강조하고 싶은 경우, `Content-Type` 헤더를 사용하여 전달합니다.

### `Bad**❌`**

```jsx
/articles.html
/members.json
```

### **`Good✅`**

```jsx
/articles
/members
```

## 목록에 필터가 필요할 경우, 쿼리 문자열을 사용

```jsx
/articles/1/comments?sort=latest
```

## URI에 동사 사용하지 않기

- HTTP 메소드만으로도 충분히 해당 내용을 표현할 수 있기 때문에, URI에 불필요한 동사를 사용하지 않습니다.

### `Bad**❌`**

```jsx
/members/1/delete
/members/2/update
```

### **`Good✅`**

```jsx
DELETE /members/1
PUT /members/2
```

# POST vs PUT vs PATCH

## POST, PUT, PATCH

- POST, PUT, PATCH는 HTTP 리퀘스트에서 HTTP 메소드가 다른 것 이외의 기술적인 차이는 없습니다.
- 이 3가지를 구분 짓는 기준은 단순한 의미상 (시멘틱, Semantic) 구분입니다.
- **단순 의미상 구분이기에 기술적으로는 POST 리퀘스트로 수정을 구현할 수 있고, PUT 리퀘스트로는 생성을 구현할 수 있다는 의미입니다.**
- 하지만 일반적으로 API를 디자인하거나 개발할 때 항상 그렇게 하지는 않습니다.
- 스펙에 명시된 대로, HTTP 메소드마다 정해진 의미대로 사용하는 것을 권장하기 때문입니다.

 ⇒ **따라서 일반적으로 자원을 생성할 때는 POST, 자원을 수정할 때는 PUT과 PATCH를 사용합니다.**

## PUT vs PATCH

- PUT과 PATCH 모두, 의미적으로 자원을 수정할 때 사용할 수 있는 HTTP 메소드입니다.
- 단, **PUT은 자원의 교체 (replace), PATCH는 부분 수정 (partial update)을 의미한다는 차이**가 있습니다.
- id가 1인 멤버가 다음과 같은 속성을 가지고 있다고 합시다.

```jsx
{
    "username": "harry",
    "age": 30
}
```

- 이 멤버를 각각 `PUT` 과 `PATCH` 를 사용하여 수정해 보도록 하겠습니다.

### `PUT`

- 리퀘스트

```jsx
PUT /members/1

{
    "username": "mike"
}
```

- 리스폰스

```jsx
200 OK

{
    "username": "mike",
    "age": null
}
```

- id가 1인 멤버의 자원을 리퀘스트에 포함된 자원으로 **교체 (replace)** 하였기 때문에, 리퀘스트로 전달되지 않은 자원의 속성은 비어 있는 상태로 수정되며, 자원의 표현 시 `null`  로 표시됩니다.

### `PATCH`

- 리퀘스트

```jsx
PATCH /members/1

{
    "username": "mike"
}
```

- 리스폰스

```jsx
200 OK

{
    "username": "mike",
    "age": 30
}
```

- id가 1인 멤버의 자원을 리퀘스트에 포함된 자원으로 부분 수정 (partial update) 하였기 때문에, 리퀘스트로 전달된 자원의 속성만 반영됩니다.
- 이처럼 두 HTTP 메소드 모두 자원을 수정하는 데 사용할 수 있습니다.
- 하지만 목적이 다르기 때문에 서로 다르게 동작하며, 다르게 동작할 수 있도록 구현에 유의해야합니다.
- 웹 API 디자인 시에는 두 메소드 모두 구현하기도 하고, 상황에 따라 하나면 구현하는 경우도 있습니다.

# 상태 코드 자세히 알아보기

## 100번대 (정보 응답)

- `100 Continue (계속)`
    - 요청의 첫 부분을 받아서 다음 요청을 기다리고 있다는 것을 알려줍니다.
    - 이미 요청을 완료했다면 해당 응답을 무시할 수 있습니다.

## 200번대 (성공 응답)

- `200 OK (성공)`
    - 클라이언트의 요청이 성공적으로 처리되었다는 것을 의미하며 주로 요청한 페이지를 서버가 제공했다는 것을 알려줍니다.
- `201 Created (생성됨)`
    - 요청이 성공적으로 처리되어 새로운 자원을 생성했다는 걸 의미합니다.
- `204 No Content (콘텐츠 없음)`
    - 요청을 성공적으로 처리했으며, 콘텐츠 (body) 를 제공하지 않는다는 것을 의미합니다.

## 300번대 (리다이렉션 메시지)

- `301 Moved Permanently (영구 이동)`
    - 요청한 자원이 새로운 위치로 영구 이동했음을 나타냅니다.
    - 클라이언트는 서버가 전달한 리스폰스의 Location 헤더에 작성된 주소로 이동합니다.
- `302 Fount (임시 이동)`
    - 요청한 자원이 일시적으로 이동했음을 나타냅니다.
    - 클라이언트는 향후 다시 해당 자원을 요청할 때도 동일한 주소로 해야 합니다.
- `304 Not Modified (수정되지 않음)`
    - 마지막 요청 이후 요청한 자원은 수정되지 않았다는 것을 알려주며 서버가 콘텐츠를 전달하지 않습니다.

## 400번대 (클라이언트 에러 응답)

- `400 Bad Request (잘못된 요청)`
    - 클라이언트의 요청을 서버가 이해할 수 없음을 의미합니다.
- `401 Unauthorized (권한 없음)`
    - 클라이언트가 해당 요청에 대한 응답을 받기 위해서는 추가적인 인증이 필요하다는 것을 의미합니다.
- `403 Forbidden (금지됨)`
    - 클라이언트가 요청한 자원에 접근할 권한이 없음을 의미합니다.
    - 401과는 달리 인증된 클라이언트이지만 인가되지 않았음을 의미합니다.
- `404 Not Found (찾을 수 없음)`
    - 클라이언트가 요청한 자원을 서버가 찾을 수 없음을 의미합니다.
- `405 Method Not Allowed (메소드 허용되지 않음)`
    - 클라이언트가 요청한 HTTP 메소드가 허용되지 않았음을 의미합니다.

## 500번대 (서버 에러 응답)

- `500 Internal Server Error (내부 서버 오류)`
    - 서버에서 오류가 발생하여 요청한 작업을 수행할 수 없음을 의미합니다.
- `502 Bad Gateway (잘못된 게이트웨이)`
    - 서버가 요청을 처리하는데 필요한 작업을 수행하던 중, 요청을 처리하는 중간 단계의 서버인 게이트웨이로부터 잘못된 응답을 받았음을 의미합니다.
- `503 Service Unavaliable (서비스 이용 불가)`
    - 서버가 해당 요청을 처리할 준비가 되지 않았음을 의미합니다.
    - 일반적으로 유지 보수를 위해 작동이 중단되거나 과부하가 걸렸을 때 나타나며, 일시적 상황에서 사용됩니다.
- `504 Gateway Timeout (게이트웨이 시간 초과)`
    - 서버가 응답을 제한 시간 안에 줄 수 없는 상태임을 의미합니다.


# 자주 하는 실수

- 웹 API는 잘못 디자인하더라도 오류가 발생하거나 고장 나지 않습니다.
- 즉각적으로 알 수 있는 것이 아니어서 잘못 디자인되는 경우가 종종 있습니다.
- 마찬가지로 REST도 강제성이 있는 건 아니어서, REST 제약 조건을 만족하지 않은 채로 디자인되었더라도 바로 알아차리기가 어렵습니다

## 잘못된 사용된 HTTP 메소드

### 예시 1 : **멤버 목록을 가져오는 API 엔드포인트를 `POST`를 사용하여 구현한 경우**

- '멤버 목록 조회' 리퀘스트

```jsx
POST /members
```

### **예시 2 : 데이터가 포함된 작업을 수행하는 엔드포인트를, `GET`을 사용하여 구현한 경우**

- '새로운 멤버의 생성' 리퀘스트

```jsx
GET /members?username=harry
```

- 문제점
    - GET 리퀘스트의 body를 파싱하지 않는 서버들이 많습니다. 따라서 정상적인 생성이 안될 수 있습니다.
    - GET 리퀘스트의 경우 브라우저 히스토리에 남습니다. 따라서 브라우저의 뒤로 가기나 앞으로 가기 수행시 데이터를 의도와는 관계없이 생성하게 될 수 있습니다.
    - 비밀번호와 같이 기록되지 말아야 할 정보가 쿼리 스트링을 통해 브라우저 히스토리에 기록이 남을 수 있습니다.
    

## 잘못 사용된 상태 코드

### **예시: 필드의 값을 잘못 전달하여 작업 수행에 실패하였지만, 상태 코드는 성공인 경우**

- '새로운 멤버의 생성' 리퀘스트

```jsx
POST /members

{
    "username": "Harry",
    "age": -10,
}
```

- '새로운 멤버의 생성 실패' 리스폰스

```jsx
200 OK

{
    "error": "age must be grater than 0."
}
```

- 문제점
    - 리스폰스의 본문에는 age를 잘못 지정하였다는 오류 메시지가 포함되어 있지만, 상태 코드는 200 OK로 반환하였기 때문에 클라이언트가 정상적인 리퀘스트로 잘못 판단할 수 있습니다.

## 행동을 나타내는 표현이 포함된 URI

- URI는 자원을 가리켜야 합니다. 자원에 대한 행동은 HTTP 메소드를 사용하여 표현할 수 있습니다.

### `Bad**❌`**

```jsx
/create-members
/update-members/1
/delete-members/2
```

### **`Good✅`**

```jsx
POST /members
PUT /members/1
DELETE /members/2
```

## 누락된 MIME Type

- 서버와 클라이언트가 바디에 담긴 콘텐츠의 타입을 잘 이해할 수 있도록 Content-Type 헤더에 올바른 MIME Type을 지정하여 리퀘스트와 리스폰스를 전달하여야 합니다.
- Content-Type 헤더가 없거나 MIME Type을 올바르게 지정하지 않으면 서버와 클라이언트가 내용을 잘못 해석할 수 있으며, 서버에 전달한 리퀘스트가 정상적으로 수행되지 않거나 클라이언트가 받은 리스폰스의 정상적인 확인이 불가능할 수 있습니다.
    - '새로운 멤버의 생성' 리퀘스트
    
    ```jsx
    POST /members
    **Content-Type: application/json**
    
    {
      "name": "Harry"
    }
    ```
    
    - '새로운 멤버의 생성' 리스폰스
    
    ```jsx
    HTTP/1.1 200 OK
    **Content-Type: application/json**
    
    {
      "id": 2,
      "name": "Harry"
    }
    ```
    

# 웹 API 문서화

## API 문서 만들기

- API를 사용하는 개발자들이 어떤 엔드포인트가 있는지, 그리고 어떻게 사용해야 하는지 알 수 있게 하려면 만든 개발자가 엔드포인트와 관련된 내용을 공유해야 합니다.
- 회의를 통해 말로 전달하고 받아 적어도 되겠지만 그렇게 전달하면 시간이 많이 들기도 하고 또 실수로 잘못 전달되거나 하는 비효율이 발생할 수도 있습니다.
- **그래서 일반적으로 API를 만든 개발자는 그 사용 방법을 ‘문서’로 만들어 다른 개발자들이 볼 수 있도록 공유합니다.**

### URI와 사용 가능한 HTTP 메소드

```jsx
POST /members
```

- API 엔드포인트가 어떤 자원에 대한 조작인지를 명시합니다.
- 직관적인 URI는 설명이 필요 없을 수 있지만, 오해가 생기지 않도록 문서에서 보다 명확하게 명시합니다.

### 리퀘스트에 포함해야 하는 바디의 내용 (파라미터)

```jsx
POST /members

{
  "name": "Emily",
  "active": false
}
```

```jsx
name: string (required)
active: boolean (optional, default: true)
```

- 리퀘스트에 사용할 수 있는 모든 파라미터와 그 목적을 상세하게 나열하여야 합니다.
- 각 파라미터의 데이터 타입, 기본값, 필수 여부 등을 명시합니다.

### 리스폰스에서 확인할 수 있는 상태 코드와 바디

```jsx
200 OK

{
  "id": 2,
  "name": "Emily",
  "active": false
}
```

```jsx
400 Bad Request

{
  "message": "field 'name' is required."
}
```

- 리스폰스에서 확인할 수 있는 상태 코드와 바디에는 어떤 것들이 있는지 작성합니다.
- 리스폰스는 리퀘스트에 따라 다르게 올 수 있기 때문에 어떤 리퀘스트 상황에서 발생한 리스폰스인지를 명확하게 구분하여 작성하면 좋습니다.

### 사용 사례 및 예제

```jsx
GET /members
```

```jsx
200 OK

[
  {
    "id": 1,
    "name": "James",
    "active": true
  },
  {
    "id": 2,
    "name": "Emily",
    "active": false
  }
]
```

- 실제 API 사용 사례를 기반으로 구체적인 예제 또는 시나리오를 제공한다면 API를 어떻게 사용하는지 더 잘 전달할 수 있습니다.

```jsx
import requests

API_HOST = 'https://api.example.com'

response = requests.get(f'{API_HOST}/members')
print(response.json())
```

- 나아가 프로그래밍 언어별로 리퀘스트를 어떻게 보낼 수 있는지 간단한 코드를 포함한다면, 다른 개발자들이 더 쉽게 이해할 수 있고 코드의 재사용을 통해 생산성을 높일 수 있습니다.

### 인증 (Authentication)

```jsx
GET /users
Authorization: Bearer {API_KEY}
```

- API를 사용하기 위해 인증이 필요한 경우가 있을 수 있습니다.
- 리퀘스트 횟수의 제한이 있거나 SNS와 같이 개인화된 서비스의 API는 인증이 필요한 경우가 많습니다.
- **인증이 필요한 API의 경우 API 키를 만드는 방법과 API를 사용하기 위한 인증의 방법의 안내도 문서에 포함하여야 합니다.**

## 참고할 만한 실제 API 문서들

- 대부분의 API 문서들이 위에서 살펴본 요소들을 가지고 있습니다.
    - [TMDB API](https://developer.themoviedb.org/reference/intro/getting-started)
    - [GitHub REST API](https://docs.github.com/en/rest?apiVersion=2022-11-28)
    - [Twitter API](https://developer.twitter.com/en/docs/twitter-api)

## 문서화 도구 및 라이브러리

- API 문서는 코딩을 통해 사이트를 직접 만들거나 엑셀이나 노션과 같이 별도의 문서 작성 도구를 통해 직접 정리하여 공유하는 방법도 있겠지만, 아래의 도구들을 사용하면 조금 더 편리하게 API 문서를 작성하고 관리할 수 있습니다.
    - [Swagger](https://swagger.io/) ([API 문서 샘플](https://petstore.swagger.io/))
    - [Redocly](https://redocly.com/) ([API 문서 샘플](https://redocly.github.io/redoc/))
    - [Postman](https://www.postman.com/)의 Documentation 기능

- 기본적으로 API 문서를 웹 페이지로 제작할 수 있는 도구들이지만 Swagger, Redocly와 같은 도구들은 웹 프레임워크와 함께 사용할 수 있는 라이브러리도 있습니다. 별도의 문서 작성 작업 없이 백엔드 코드를 작성하는 것만으로도 자동으로 문서를 생성할 수 있죠.
    - Express 웹 프레임워크: [Swagger UI Express](https://github.com/scottie1984/swagger-ui-express)
    - Django 웹 프레임워크: [Yet another Swagger generator](https://github.com/axnsan12/drf-yasg)
    - Spring 웹 프레임워크: [Springfox](https://github.com/springfox/springfox)
