---  
layout: post  
title: Git Workflow 21.04.06
subtitle: 
categories: dev
tags: js
comments: true  
--- 

# Git 환경 설정

- Git 최초 설정 (이름, 이메일)

Git init을 한 후 사용자 이름과 이메일 주소를 설정 (Git 커밋 기록에 해당 사용자 이름과 메일 주소가 기록)

--global로 설정하는 것은, 맨처음 단 한번만 해줘된다. 

```
$ git config --global user.name "Kim Coding"
$ git config --global user.email kimcoding@codestates.com
```

**git clone**으로 repo를 local에 가져온 경우 최초 설정이 필요 없다. 

- 에디터 설정

Git에서 커밋 메시지를 기록할 때 (특히 merge commit 확인 메시지가 나올 때) 에디터(vim, nano 등)가 필요하다.

```
$ git config --global core.editor "nano"
```

Git을 설치하고 나면 Git의 사용 환경을 적절하게 설정해 주어야 한다. 환경 설정은 한 컴퓨터에서 한 번만 하면 된다. 설정한 내용은 Git을 업그레이드해도 유지된다. 언제든지 다시 바꿀 수 있는 명령어도 있다.

Git 최초 설정 상세 사항 : https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%B5%9C%EC%B4%88-%EC%84%A4%EC%A0%95

-설정 확인

```
$ git config --list
```

# Git Workflow

0.A와 B가 협업을 한다고 가정해보자.

1.A와 B는 **협업 할 프로젝트의 repository**를 각자의 깃허브에 **fork** 한다.

2.A와 B는 **git clone** 명령어를 통해 **포크한 repository URL을 입력하고 각자의 로컬로 파일을 가져온다.**

```
$ git clone [포크한 레포 URL]
```

3.A와 B는 협업을 위해 서로의 repository를 remote add를 사용하여 연결 

```
$ git remote add [이름 설정] [협업자의 repository URL]

// 잘 연결 되었는지 remote repo 목록 확인
$ git remote -v
```

4.A가 코드를 작성하고 commit하고 자신의 repository에 push

```
$ git add [변경한 파일명.확장자]

$git commit -m '커밋 메시지'

$git push origin master(또는 다른 브랜치 이름)
```

5.방금 A가 작성한 코드를 협업자 B가 가져오는 방법

B는 pull을 사용하여 자신(B)의 로컬로 A가 작성한 코드 가져오기

```
$ git pull [3번에서 설정한 이름] master(또는 다른 브랜치 이름)
```

7.B가 A의 코드를 가져오고 거기에 변경사항을 커밋하고 자신의 repo에 푸쉬

8.A 역시 B가 작성한 코드를 pull을 사용하여 자신(A)의 로컬로 가져오기

위 과정을 반복하면 협업이다.

##### 요약

1. A, B : Co-repo Fork, / A, B local : Clone / A-B : remote add 
2. A :  add / commit / push A
3. B : pull A
4. B : add / commit /push B
5. A : pull B

# 주의 사항

# Push는 항상 자신의 repository에만 가능 !

# Pull은 항상 타인의 repository로 부터!

---

# 충돌(Conflict) 해결 방법

협업을 하는 A와 B가 **코드의 같은 라인을 동시에 고치고 커밋이나 푸쉬할 때 주로 발생.**

- 상황 예시
A가 코드를 수정-커밋-푸쉬(협업을 진행 중인 repo) 를 진행한 상황이다.
B 역시 코드를 수정-커밋-푸쉬(협업을 진행 중인 repo)를 진행하면 rejected 오류가 발생한다. 

B가 이를 해결하기 위해서는 우선 협업을 진행 중인 repo를 fetch 또는 pull을 해와야한다.

```
// 협업 중인 코드 pull
$ git pull origin master
```

하지만 pull을 해와도 CONFLICT 오류가 발생한다. 

**오류가 발생하는 이유 : pull은 협업 중인 코드를 가져오는 것(Fetch) 뿐만 아니라  병합(Merge)까지 하므로 Merge를 시도 할 때 A와 B가 같은 라인을 수정해버려서 발생하는 오류이다.**

*pull(가져와서 병합) = fetch(가져오기) + merge(병합)

- 오류 발생 시 코드 에디터 화면

```js
<<<<<<<< HEAD (Current Change)

B가 변경한 코드

========

A가 변경한 코드(B가 Pull하려고 했던 코드)
>>>>>>>> 5f0asd2134bjsadfhui234ka1283993hu (Incoming Change)
```

병합 충돌(Conflict Merge) 상황에서 3가지 선택 사항이 있다.


- 1. B가 작성한 코드만 적용 : Accept Current Change 클릭

B가 자신이 변경한 코드를 적용하고 싶은 경우 B가 작성한 코드 부분을 뺀 나머지 부분을 지우고 커밋.

```js
B가 변경한 코드
```

- 2. A가 작성한 코드만 적용 : Accept Inciming Change 클릭

B가 A가 변경한 코드를 적용하고 싶은 경우 B가 작성한 코드 부분을 지우고 커밋.

```js
A가 변경한 코드(B가 Pull하려고 했던 코드)
```

- 3. A와 B 모두의 적용하고 싶은 경우 : Accept Both Change 클릭 
(이 경우 코드가 겹쳐 일부가 중첩(삭제)될 수 있으므로 코드를 약간 수정해줘야할 수도 있다.)

```js
B가 변경한 코드
A가 변경한 코드(B가 Pull하려고 했던 코드)
```

위 과정을 진행했다고 해서 Conflict 상황이 끝난 건 아니다. (여전히 vs 코드에서 파일명 옆에 C라는 글자가 보일 것이다.)

git status를 하면 여전히 Conflict 상황임을 확인 할 수 있다.

```
$ git status

You have unmerged paths.
	(fix conflict and run "git commit")
	(use "git merge --abort" to abort the merge)

Unmerged path:  // merge 되지 않은 path
	(use "git add <file>..." to mark resolution) //해결하려면 git add 파일명

	both modified: [동시에 수정하여 merge가 안된 파일명] //빨간 글씨로 나옴
```

**이를 해결하기 위해서는 [동시에 수정한 파읾명]을 git add 해줘야한다.**

```
$ git add '동시에 수정한 파일명'
```

git status로 다시 확인해보면 모든 conflict가 해결 되어 있을 것이다. (빨간 글씨 없음)

**커밋까지 해야 merge가 완료**된다.

**merge까지 완료되었다면 repo에 push**를 해주면 된다.

### pull vs fetch

- fetch : 협업 중인 repo의 최신 이력을 내 로컬 repo로 가져오되 병합(Merge)은 하지는 않음.

- pull : 협업 중인 repo의 최신 소스를 가져와서 로컬 소스에 병합(Merge). 단순히 협업 중인 repo에서 로컬로 소스를 가져온다의 개념보다는 가져와서 합친다의 개념이기 때문에 브랜치끼리도 pull을 통해 코드를 합칠 수 있다. pull은 바로 리모트 서버(협업 중인 repo 등)의 최신 코드를 가져와서 내 로컬 소스에 합쳐버리기 때문에 조금 위험하할 수 있다. (예를 들면 지금 리모트 서버의 최신 소스가 버그가 있는 경우)

# 실제 Git WorkFlow

1.fork : 협업 해야 할 repo(일반적으로 upstream이라고 부른다.)를 자신의 repo로 fork.

2.clone : 자신의 repo를 자신의 로컬로 가져오기

3.reomote add 협업 해야 할 repo(upstream)와 자신의 로컬에 저장 된 repo를 연결

4.pull : 협업 해야 할 repo의 dev(개발용 브랜치)를 내 로컬 repo로 pull하여 로컬의 dev 브랜치를 최신화 여러명이서 작업 중인 협업 해야 할 repo는 계속 업데이트 될 것이므로 협업 해야 할 repo와의 씽크를 계속 맞춰줘야한다. (Dev 브랜치를 관리하는 이유는 아직 배포가 아닌 개발 과정이기 때문)

5.신규 브랜치 생성 : pull을 통해 dev 브랜치를 가지고 왔다면 새로운 브랜치를 생성하고 거기에 새로운 기능을 개발을 위한 코드 작성.

6.새로운 기능이 완료되면 자신의 my repo에 push. 주의해야할 점은 my repo에 다른 브랜치(master, dev)가 아닌 5번에서 만든 새로운 브랜치에 push해야한다.

7.Origin Repo에 pull request !

8.Origin Repo(master)에 관리자는 요청이 온 PR을 확인 후 merge하면 하나의 기능 개발 완료!

9.또 다른 기능을 개발 할 땐 1~5과정 반복

