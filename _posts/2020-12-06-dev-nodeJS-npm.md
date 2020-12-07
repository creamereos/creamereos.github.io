---  
layout: post
title: nodeJS - npm / express
categories: dev
tags: node
comments: true
---

npm(Node Package Manager)

패키지 : npm에 업로드된 **노드 모듈**. 모듈이 다른 모듈을 사용할 수 있는 것처럼 패키지가 다른 패키지를 사용 가능. (의존 관계)

- pakage.json : 설치한 패키지의 버전을 관리하는 파일. **노드 프로젝트를 시작하기 전에는 폴더 내부에 무조건 package.json 파일을 만들고 시작해야한다.** npm은 package.json을 만드는 명령어를 제공한다.

- pakage.json 만들기
우선 콘솔로 프로젝트를 시작할 폴더로 이동한 후 명령어 **$ npm init**를 입력한다. (프로젝트 폴더 이름과 프로젝트 이름이 같으면 안된다.)

```
$ npm init
```

위 명령어를 입력하면 명령어 입력창이 계속 나온다. (일부 입력창은 선택 사항)

- **package name** : 패키지 이름 입력. (package.json의 name 속성에 저장)
- **version** : 패키지 버전 입력.
- **entry point** : 자바스크립트 실행 파일 진입점 입력. 보통 마지막으로 module.exports를 하는 파일을 지정한다. (package.json의 main 속성에 저장)
- **test command** : 코드를 테스할 때 입력할 명령어 입력. (package.json의 scripts 속성안의 test 속성에 저장)
- **git repository** : 코드를 저장해 둔 깃 저장소 주소 입력. 나중에 소스에 문제가 생겼을 때 사용자들이 이 저장소에 방문해 문제를 제기 할 수 있고, 코드 수정본을 올릴 수도 있다. (package.json의 repository 속성에 저장)
- **keywords** : 키워드는 npm 공식 홈페이지(https://npmjsd.com)에서 패키지를 쉽게 찾을 수 있도록 해준다. (package.json의 keywords 속성에 저장)
- **license** : 해당 패키지의 라이센스를 넣으면된다.(오픈 소스라고 해서 모든 패키지를 아무런 제약 없이 사용 할 수 있는 것은 아니다. 라이센스 별로 제한 사항이 있으므로 라이센스를 확인해야한다. 상용 프로그램 개발시 법적 문제 발생할 수도 있음.)

위 사항들을 입력한 후 yes를 누르면 package.json 파일이 생성 완료된다.

package.json

```json
{
  "name": "npmtest",
  "version": "0.0.1",
  "description": "hello package.json",
  "main": "index.js",
// scripts 부분은 npm 명령어를 저장해 두는 부분
// console에서 npm run [스크립트 명령어]를 실행하면 해당 스크립트가 실행
  "scripts": {
    //   $ npm run   test 
    // 위 명령어를 입력하면 "Error: no test specified" && exit 1 실행됨
    "test": "echo \"Error: no test specified\" && exit 1"
    // test 외에도 scripts 속성에 명령어 여러개를 등록하여 사용할 수 있다.
    // 보통 start 명령어에 node [파일명]을 저장해두고 npm start로 실행한다.
    // test나 start 같은 스크립트는 run을 붙이지 않아도 실행됨
  },
  "author": "creamer",
  "license": "ISC"
}
```

- 패키지 설치

express 설치 (pakage.json이 있는 폴더의 콘솔에서 입력)

```
$ npm install express
```

```js
npm notice created a lockfile as package-lock.json. You should commit this file.
// 메시지 중에 WARN이 나오는데 단순한 경고이므로 해결하지 않아도 된다. ERROR라고 나와야 진짜 ERROR. 
npm WARN npmtest@0.0.1 No repository field.

+ express@4.17.1
added 50 packages from 37 contributors and audited 50 packages in 2.928s
// 취약점 검사
found 0 vulnerabilities
```

- 패키지 설치 완료

package.json

```json
{
  "name": "npmtest",
  "version": "0.0.1",
  "description": "hello package.json",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "creamer",
  "license": "ISC",
// 새롭게 추가된 부분
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

추가로 node_modules 폴더도 생성된다. 해당 폴더 안에는 설치한 패키지들이 들어있다. express 하나만 설치했는데 패키지가 여러개 들어있다. 다른 패키지들은 Express가 의존하는 패키지들이다. 패키지 하나가 다른 여러 패키지에 의존하고, 그 패키지들은 또 다른 패키지들에 의존한다. 이렇게 **의존 관계가 복잡하게 얽혀있어 package.json 파일이 필요**하다.

![](/assets/img/post/2020-12-06-18-54-12.png)

package-lock.json 파일도 생성되었다. 내용을 보면 직접 설치한 express 외에도 node_modules에 들어있는 패키지들의 정확한 버전과 의존 관계가 담겨있다. npm으로 패키지를 설치, 수정, 삭제 할때 마다 패키지들 간의 내부 의존 관계를 이 파일에 저장한다.

![](/assets/img/post/2020-12-06-18-55-57.png)

- 모듈 여러개 동시 설치하기

npm install [패키지1] [패키지2] [패키지3] [...]

```
$ npm install morgan cookie-parser express-session
```

```
+ morgan@1.10.0
+ cookie-parser@1.4.5
+ express-session@1.17.1
added 10 packages from 5 contributors and audited 60 packages in 1.532s
found 0 vulnerabilities
```

위에서 설치한 패키지들은 package.json의 dependencies 속성에 기록된다.

package.json

```json
...
  "dependencies": {
    "cookie-parser": "^1.4.5",
    "express": "^4.17.1",
    "express-session": "^1.17.1",
    "morgan": "^1.10.0"
  }
```

- 개발용 패키지 설치 : 실제 배포 시에는 사용되지 않고 개발 중에만 사용되는 패키지들

npm install --save-dev [패키지1] [패키지2] [패키지3] [...]

```
$ npm install --save-dev nodemon
```

개발용 패키지는 package.json의 새로운 속성인 devDependencies 속성에 추가된다. 개발 용 패키지들만 따로 관리 가능

package.json

```json
  "devDependencies": {
    "nodemon": "^2.0.6"
```

- npm 전역(global) 설치 옵션 : 패키지를 현재 폴더(node_modules)에 설치하는 것이 아니라 npm이 설치 되어있는 폴더(맥의 경우 /usr/local/lib/node_modules)에 설치한다. 전역 설치를 했다고 해서 패키지를 모든 곳에서 사용한다는 뜻은 아니고 대부분 콘솔의 명령어로 사용하기 위해 전역 설치를 한다. (전역 설치한 패키지는 package.json에 기록되지 않는다.)

```
$ sudo npm install --global rimraf
rimraf는 rm -rf(지정한 파일이나 폴더를 지우는 명령어)를 사용하게 해준다. 
```

```
/usr/local/bin/rimraf -> /usr/local/lib/node_modules/rimraf/bin.js
+ rimraf@3.0.2
added 12 packages from 4 contributors in 1.609s
```

- 폴더 삭제하기

```
$ rimraf node_modules
```

위 명령어를 실행하면 node_modules에 있는 파일들이 삭제된다. 하지만 package.json에 설치한 패키지 내역이 들어 있으므로 npm install만 하면 알아서 다시 설치된다.

즉, node_modules는 언제든지 npm install로 설치 할 수 있으므로 node_modules는 보관할 필요가 없다. git 같은 버전 관리 프로그램과 같이 사용할때도 node_modules는 커밋하지 않는다. 중요한 파일은 **package.json** 이다.

전역 설치한 패키지는 package.json에 기록되지 않아 다시 설치 할때 어렵다. 이러한 경우 **전역 설치 대신 npx 명령어를 사용**하면된다.

```
$ npm install --save-def rimraf
$ npx rimraf node_modules
```

- npm audit : npm 패키지들이 워낙 많다보니 일부 패키지는 악성 코드를 담고 있다. npm audit을 통해 취약점 검사 할 수 있다.

```
$ npm audit
```

- npm audit fix : npm이 스스로 수정할 수 있는 취약점을 알아서 주기적으로 수정.

```
$ npm audit fix
```

- npm에 없는 패키지 설치하기

모든 패키지가 npm에 등록되어있는 것은 아니다. 일부 패키지는 오픈소스가 아니거나 개발중이므로 깃허브나 nexus 등의 저장소에 보관되어있을 수도있다. 그러한 패키지들도 npm install [저장소 주소] 명령어를 통해 설치할 수 있다.

- 명령어 줄여쓰기

```
npm install = npm i
--save-dev = -D
--global =  -g
```