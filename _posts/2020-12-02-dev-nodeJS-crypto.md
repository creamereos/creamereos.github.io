---  
layout: post
title: nodeJS - built-in module (crypto)
categories: dev
tags: node
comments: true
---

- crypto : 암호화를 도와주는 모듈. (비밀번호)

- 단방향 암호화(해시 함수) : decription(복호화: 암호화된 문자열을 원래 문자열로 되돌려놓는 것을 의미) 할 수 없는 단방향 암호화 방식. 즉 한번 암호화하면 원래 무자열을 찾을 수 없다. (원래 비밀번호는 어디에도 저장되지 않고 암호화된 문자열로만 비교)

- 단방향 암호화 알고리즘은 주로 해시 기법 : 해시 기법이란 어떠한 문자열을 고정된 길이의 다른 문자열로 바꿔버리는 방식. (입력 문자열의 길이는 다르지만 출력 문자열의 길이는 4자리로 고정) 

hash.js

```js
const crypto = require('crypto');

console.log(crypto.createHash('알고리즘').update('문자열').digest('인코딩'));
// createHash(알고리즘): 사용할 해시 알고리즘(sha512)을 넣는다. 나중에 sha512마저도 취약해지면 더 강화된 알고리즘(sha3)으로 바꿔야함.
// update(문자열): 변환할 문자열=비밀번호를 넣는다.
// digest(인코딩): 인코딩할 알고리즘(base64 또는 hex 또는 latin1)을 넣는다. base64가 결과 문자열이 짧아 자주 사용됨. 결과물로 변환된 문자열을 반환.

console.log(crypto.createHash('sha512').update('CREAMer').digest('base64'));
// dDD1g86oi6PERzmdUn/6lSdiX8UmG5+4RkwIt2cpJLOYoRpDUgGIfhlcUVka3fBVb+icuwQvwy60Pccd7Qt/Cg==
```

- pbkdf2 
현재는 주로 pbkdf2(node에서 지원)나 bcrypt, scryp라는 알고리즘으로 비밀번호를 암호화하고 있다. pbkdf2는 간단히 말하면 기존 문자열에 salt라고 불리는 문자열을 붙인 후 해시 알고리즘을 반복해서 적용하는 것이다. pbkdf2는 간단하지만 bcrypt, scrypt보다 취약하므로 더 나은 보안이 필요하다면 bcrypt나 scrypt 방식을 사용하면 된다.

pbkdf2.js

```js
const crypto = require('crypto');

// randomByte() method로 64바이트 길이의 문자열을 만든다. 이것이 salt가 된다. 
crypto.randomBytes(64, (err, buf) => {
    // randomBytes이므로 실행 할 때마다 결과가 달라짐. 따라서 salt를 잘보관하고 있어야 비밀번호도 찾을 수 있다.
    const salt = buf.toString('base64');
    console.log(salt);
    // JhjFD42wr6xXo58acgMGZI6Oh67d81J2n0clH8tRsbBwhDQL+/5VzadV+ZVU+6VM+eqqK45oD7ORq1/5IxuWxg==

    // pbkdf2() method에는 비밀번호, salt, 반복횟수(예시에서는 10만번 반복), 출력 바이드, 해시 알고리즘을 인수로 넣는다.
    // 즉, sha512로 변환된 결괏값을 다시 sha512로 변환하는 과정을 10만번 반복(약 1초 정도 걸림)
    // crypto.randomByte()와 crypto.pbkdf2()는 내부적으로 스레드풀을 사용해 멀티 스레딩으로 동작.
    crypto.pbkdf2('비밀번호', salt, 100000, 64, 'sha512', (err,key) => {
        console.log(key.toString('base64'));
        // j1+IdxUOK0TJHKXgWvw6GQl/wUQxahqKKA5iiR5s9aOfNmlRfKmX1z6fuUaiIo7Yh/kl0/QQsLnRxn2ITYfYrA==
    });
});
```

- 양방향(대칭형) 암호화 : decription(복호화) 가능. 복호화를 할 때 암호화 할 때 사용한 key를 사용.

cipher.js

```js
const algorithmn = 'aes-256-abc';
const key = 'abcdefghijklmnopqrstuvwxyz123456'
const iv = '1234567890123456'

// [암호화]

// crypto.createCipheriv(알고리즘, 키, iv) : 암호화 알고리즘은 aes-256-abc 등을 사용 가능. 
// 사용 가능한 알고리즘 보기 : crypto.getCiphers()
// aes-256-abc 알고리즘의 경우 키는 32바이트, iv는 16바이트여야 한다.
// iv는 암호화 할 때 사용하는 초기화 벡터. (AES 암호화 추가 공부)
const cipher = crypto.createCipheriv(algorithmn, key, iv);

// cipher.update('문자열', '인코딩', '출력');
// 보통 문자열은 utf8 인코딩을 사용하고, 암호화는 base64를 사용한다. 
let result = cipher.update('암호화할 문장', 'utf8', 'base64');

// cipher.final(출력 인코딩) : 출력 결과물의 인코딩을 넣으면 암호화가 완료
result += cipher.final('base64');

console.log(result);
// vl+TiQxMFyN1wKMF92XXbKdwmYx74bmwHS0RAq+yIGA=

// [복호화]

// crypto.createDecipheriv(알고리즘, 키, iv) : 복호화 시 사용. 암호화 할 때 사용했던 알고리즘, 키, iv 값을 그대로 넣어야한다.
const decipher = crypto.createDecipheriv(algorithmn, key, iv);

// decipher.update('문자열', '인코딩', '출력') : cipher.update()에서 utf8, base64 순으로 넣었다면 반대로 base64, utf8 순으로 넣으면 된다.
let result2 = decipher.update(result, 'base64', 'utf8');

// decipher.final(출력 인코딩) : 출력 결과물의 인코딩을 넣으면 복호화가 완료
result2 += decipher.final('utf8');

console.log(result2);
// 암호화할문장
```

상세사항 : https://nodejs.org/api/crypto.html

좀 더 간단하게 암호화를 하고 싶다면 npm 패키지 crypto-js(https://www.npmjs.com/package/crypto-js) 이용
