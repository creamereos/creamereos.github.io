---  
layout: post
title: ES6 - Async & Await
categories: dev
tags: js
comments: true
---

# async & await

- 2개 이상의 Promise의 업데이트, 코드의 가독성을 높인다. Promised의 then과 catch는 구식. then을 사용하면 then(functio)을 여러번 사용해야하고 코드의 가독성을 떨어뜨린다. 

- async & await를 사용하여 Promise 코드의 가독성을 높인다. async & await는 Promise 밖에서 값을 가져올 수 있는 방법이다.

- **await는 항상 async function 안에서만 사용 가능**하다. 수많은 then을 대체 가능. await는 기본적으로 Promise가 끝날 때 까지 기다린다.

then/catch

```js
const getMoviesPromise = () => {
    fetch("https://yts.am/api/v2/list_movies.json")
    .then(response => {
        console.log(response);
        return response.json();
})
    .then(json => console.log(json))
    .catch(e => console.log(e));
};
```

async/await

```js
// async function getMovies() {} 아래 코드와 같다.
const getMoviesAsync = async() => {
    try{
        const respon = await fetch("https://yts.mx/api/v2/list_movies.json");
        const json = await respon.json();
        console.log(json);
    } catch(e){ //ctach()는 async/await는 일어나는 모든 error를 잡아낸다.
        console.log(e);
    } finally {
        console.log("Finally");
    }
};

getMoviesAsync();
```

# Parallel async/await

- 병렬(동시에 여러개) 실행

```js
const getMoviesAsync = async() => {
    try{
        const [moviesResponse, upcomingResponse] = await Promise.all([
            fetch("https://yts.mx/api/v2/list_movies.json"),
            fetch("https://yts.mx/api/v2/list_upcoming.json")
        ]);
        const [movies, upcoming] = await Promise.all([
            moviesResponse.json(),
            upcomingResponse.json()
        ]);
        console.log(movies, upcoming);
    } catch(e) {
        console.log(e);
    } finally {
        console.log("finally");
    }
};

getMoviesAsync();
```