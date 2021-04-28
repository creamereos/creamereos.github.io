---  
layout: post  
title: fetch + HTTP method 21.04.29
subtitle: 
categories: dev
tags: js
comments: true  
--- 

# 비동기 처리 POST 요청 모듈화 : POST는 업데이트 시 자주 사용되므로

function + fetch + HTTP POST method + async & await = 비동기 처리 + 모듈화

```js
// function + fetch + HTTP POST method + async & await = 비동기 처리 + 모듈화

// 비동기 처리를 위해 async 사용
// parameter에는 host, path, body, headers가 들어오며 headers의 default 값은 빈 객체
async function post(host, path, body, headers = {}) {
    // url 선언
    const url = `https://${host}/${path}`
    const options = {
        method : 'POST',
        headers : {
            // headers default value : json 형식임을 표시
            "Content-Type": "application/json",
            // headers에 인자가 있다면 spread syntax로 분해
            ...headers,
        },
        // 인자로 받은 body 직렬화.(데이터 효율성?)
        body: JSON.stringify(body),
    }
    // fetch로 url 응답 객체 받아오기
    const res = await fetch(url, options)
    // 받아온 응답 객체를 데이터로 사용하기 위해 json 화 
    const data = await res.json()
    
    // 만약 응답 객체의 상태가 ok면 
    if (res.ok) {
        // data return
        return data
    } else {
        // 그렇지 않다면 에러 출력
        throw Error(data)
    }
}

// 위 모듈화 함수 실행
post("jsonplaceholder.typicode.com", "posts", {
  title: "Test",
  body: "I am testing!",
  userId: 1,
}).then(data => console.log(data))
  .then(err => console.log(err))
//   {title: "Test", body: "I am testing!", userId: 1, id: 101}
```

### [HTTP 요청 메서드]

- GET : 조회(Read) : 특정 리소스의 표시를 요청. 오직 데이터를 받기만 한다.
- HEAD : GET과 동일한 응답을 요구하지만, 응답 본문을 포함하지 않는다.
- POST : 추가(Create) : 특정 리소스에 엔티티를 제출 할 때 사용. 서버의 상태 변화나 부작용 일으킬 수 도 있다.
- PUT : 리소스의 전체 업데이트
- PATCH : 리소스의 부분만을 수정할때 사용
- DELETE : 삭제 (Delete) : 특정 리소스 삭제

# Get 호출 : API에 있는 데이터 가져오기 + fetch = 비동기 요청

REST API 리소스 샘플 제공 : https://jsonplaceholder.typicode.com/posts

### syntax

```js
// default 값은 HTTP method GET
fetch(URL 주소)
    // fetch로 받아온 값은 응답 객체(object reponse)이므로 .json()으로 파싱 필요.
    .then(res => res.json())
    .then(data => {
        // 응답 객체를 파싱한 데이터를 받고 원하는 작업 실행.
        // data는 보통 객체 배열 형식.
    })
```

##### New Terms

- 응답 객체(object reponse) : 서버가 클라이언트의 요청에 응답하는 정보를 담고 있는 객체

```js
// object reponse ex
Response {type: "cors", url: "https://jsonplaceholder.typicode.com/posts", redirected: false, status: 200, ok: true, …}
```

- 파싱(parsing) : 다른 형식으로 저장된 데이터를 원하는 형식의 데이터로 변환

### GET + then + ES6 (비동기 처리 X)

```js
fetch("https://jsonplaceholder.typicode.com/posts")
    .then(res => res.json())
    .then(data => console.log(data));
```

- 비동기 처리 X : 아래 코드는 fetch가 data를 가져오기전에 console.log(text)가 실행되므로 text 변수에 적절한 data가 저장되지 않는다.

```js
let text = '비동기 처리 안됨';

fetch("https://jsonplaceholder.typicode.com/posts")
    .then(res => res.json())
    .then(data => {
    text = data;
    });

console.log(text); 
// 비동기 처리 안됨
```

- 비동기 처리 : 시간이 소요되는 작업이 완료될 때까지 계속 기다리지 않고 따로 처리하며, 일단 다른 코드들도 먼저 실행할 수 있게끔 하는 작업

### GET + fetch + async & await = 비동기 처리

async/await는 여러 API를 호출하여 그 값들을 이용해야 할 때 진가를 발휘

```js
// 파싱된 data를 return 하는 함수
function getData(){
    const resp = fetch("https://jsonplaceholder.typicode.com/posts");
    return resp.then(res => res.json());
}

// 비동기 처리 함수
async function exec() {
    let text;
    try {
        // getTitl()이 데이터를 가져와올 때까지 기다림
        // await는 객체를 return하는 부분 앞에만 붙일 수 있다.
        // fetch API를 통해 받은 response 데이터는 Promise 객체라서 사용 가능
        text = await getData();
        console.log(text[0].title)
    }
    catch(err) {
        console.log(err);
    }
}

exec();
// sunt aut facere repellat provident occaecati excepturi optio reprehenderit
```

# POST 호출 : 원격 API에서 관리하고 있는 데이터를 생성

### POST + fetch

```js
fetch("https://jsonplaceholder.typicode.com/posts", {
  // HTTP method를 POST로 지정
  method: "POST",
  // headers 부분은 JSON 포맷 사용 지정
  headers: {
    "Content-Type": "application/json",
  },
  // 가장 중요! body는 직렬화(stringify)
  body: JSON.stringify({
    title: "Test",
    body: "I am testing!",
    userId: 1,
  }),
}).then(resp => resp.json()) // 다시 파싱
  .then(data => console.log(data))
//  {title: "Test", body: "I am testing!", userId: 1, id: 101}
```

### POST + fetch + Async / Await

```js
let data1 = { title: "Test",
             body: "I am testing!",
             userId: 1
            }

function postData() {
    const resp = fetch("https://jsonplaceholder.typicode.com/posts", {
    // HTTP method를 POST로 지정
    method: "POST",
    // headers 부분은 JSON 포맷 사용 지정
    headers: {
        "Content-Type": "application/json",
    },
    // 가장 중요! body는 직렬화(stringify)
    body: JSON.stringify({
        // 내부 데이터는 밖에서 변수로 선언도 가능
        // data1
        title: "Test",
        body: "I am testing!",
        userId: 1,
    }),
    })
    return resp.then(data => data.json())
}

async function exec() {
    let data = await postData()
    console.log(data);
}

exec()
// {title: "Test", body: "I am testing!", userId: 1, id: 101}
```

# Delete : 데이터 삭제. headers와 body 옵션 필요 없음

```js
fetch("https://jsonplaceholder.typicode.com/posts/1", {
    method: "DELETE",
    })
    .then(resp => resp.json())
    .then(data => console.log(data))
// {}
```
