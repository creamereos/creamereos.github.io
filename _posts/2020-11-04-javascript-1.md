---  
layout: post
title: Node JS - Forntend JS
subtitle:
categories: dev
tags: javascript
comments: true  
--- 

# 프론트엔드 자바스크립트

### AJAX (Asynchronous Javascript And XML)
**페이지 이동 없이 서버에 요청(jQuery / axios)을 보내고 응답을 받는 기술.**

- 비동기적 웹 서비스를 개발할 떄 사용하는 기법.
- JSON 주로 사용

##### GET 요청
~~~
[front.html]

<script src="https://unpkg.com/axios/dist/sxios.min.js"></script>
<script>
    axios.get('https://wwww.zerocho.com/api/get')
        .then((result) => {
            console.log(result);
            console.log(result.data); //{}
        })
        .catch((error) => {
            console.error(error);
        });
</script>
~~~

프로미스이므로 async/await 방식으로 변경 가능. 익명 할수라서 즉시 실행을 위해 소괄호로 감싸서 호출.

~~~
(async () => {
  try {
    const result = await axios.get('https://wwww.zerocho.com/api/get');
    console.log(result);
    console.log(result.data); // {}
    } catch (error) {
        console.error(error);
      }
  })();
~~~

##### POST 요청
데이터를 서버로 보낼 수 있음.

~~~
(async () => {
  try {
    const result = await axios.post('https://wwww.zerocho.com/api/post' , {
      name: 'zerocho',
      birth: 1994,
    });
    console.log(result);
    console.log(result.data); // {}
    } catch (error) {
        console.error(error);
      }
  })();
~~~

##### FormData
서버에 폼데이터를 보내는 경우.

HTML form 태그의 데이터를 동적으로 제어할 수 있는 기능.

~~~
const formData = new FormData();

- 생성된 객체의 append 메서드로 키-값 형식의 데이터 저장 가능
formData.append('name', 'zerocho');
formData.append('item', 'orange');
formData.append('item', 'melon');

- has 메서드는 주어진 키에 해당하는 값이 있는지 여부를 알림.
formData.has('item');
// true
formData.has('money');
// false

- get 메서드는 주어진 키에 해당하는 값 하나를 가져옴.
formData.get('item');
// "orange"

- getAll 메서드는 주어진 키에 해당하는 값을 모두 가져옴.
formData.getAll('item');
// (2) ["orange", "melon"]

- append 메서드를 여러번 사용해서 키 하나에 여러개의 값을 추가 가능
formData.append('test', ['hi','zero']);

formData.get('test');
// "hi,zero"

- Delete 메서드는 현재 키를 제거
formData.delete('test');

formData.get('test');
// null

- set 메서드는 현재 키를 수정.
formData.set('item', 'apple');

formData.getAll('item');
// ["apple"]

~~~

- axios로 폼데이터를 서버에 보내기

~~~
(async () => {
  try {
    const formData = new FormData();
    formData.append('name', 'zerocho');
    formData.append('birth', 1994);
    const result = await axios.post('https://wwww.zerocho.com/api/post/formdata' , formData);
    console.log(result);
    console.log(result.data);
    } catch (error) {
        console.error(error);
      }
  })();
~~~

##### endcodeURIComponent, decodeURIComponent
서버가 한글 주소를 이해하지 못하는 경우가 있는데 이럴 때  endcodeURIComponent를 사용. (한글 주소 부분만 endcodeURIComponent 메서드로 감싸면 된다.)

- 브라우저와 노드에서 사용 가능
~~~
(async () => {
  try {
    const result = await axios.get('https://wwww.zerocho.com/api/search/ ${endcodeURIComponent('노드')});
    console.log(result);
    console.log(result.data); // {}
    } catch (error) {
        console.error(error);
      }
  })();

endcodeURIComponent로 감싸서 노드라는 한글 주소를 변환.
~~~

받는 쪽에서는 decodeURIComponent를 사용하면 된다.


##### 데이터 속성과 dataset
데이터를 JS 변수에 저장해도 되지만 HTML5에도 HTML과 관련된 데이터를 저장하는 공식적인 방법.

~~~
<ul>
    <li data-id="1" data-user-job="programmer">Zero</li>
    <li data-id="2" data-user-job="designer">Nero</li>
    <li data-id="3" data-user-job="programmer">Hero</li>
    <li data-id="4" data-user-job="ceo">Kero</li>
</ul>
<script>    
    console.log(document.querySelector('li').dataset);
</script>
~~~

data- 로 시작하는 것들이 데이터 속성. 웹 화면에 표시되지 않지만 웹 애플리케이션 구동에 필요한 데이터들. 나중에 이 데이터들을 사용해 서버에 요청을 보내게 됨.

- 데이터 속성의 장점 : 자바스크립트로 쉽게 접근 가능. (위 코드에서는 dataset 속성을 통해 li 태그에 접근.)

- 반대로 dataset에 데이터를 넣어도 HTML 태그에 반영.

~~~
dataset.monthSalary = 1000000000;을 넣으면 data-month-salary = "1000000000"라는 속성이 생긴다.
~~~

노드를 웹서버로 사용하는 경우, 클라이언트(프론트엔드)와 자주 데이터를 주고 받는다. 서버에서 보내준 데이터를 프론트엔드 어디에 넣어야할지 고민해야한다.

[ 프론트엔드에 데이터를 내려보낼 때 고려할 점]
보안 : 프론트엔드에 민감한 데이터(비밀번호 등)를 넣으면 안된다.
보안과 무관한 데이터들은 자유롭게 프론트엔드로 보내도 된다.
