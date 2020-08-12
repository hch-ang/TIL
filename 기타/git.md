# Git

> Git은 분산버전관리시스템(DVCS)이다. Distributed Version Control System
>
> 소스코드의 버전 및 이력을 관리할 수 있다.



## 준비하기

윈도우에서 git을 활용하기 위해서 git bash를 설치한다.

git을 활용하기 위해서 `GUI` 툴인 `source tree`, `github desktop`등을 활용할 수도 있다.

초기 설치를 완료한 이후에 컴퓨터에 author 정보를 입력한다.

```bash
$ git config --global user.name {username}
$ git config --global user.email {user email}
```



## 로컬 저장소(repository)활용하기

### 1. 저장소 초기화

```bash
$ git init
Initialized empty Git repository in C:/Users/.../.git/
```

- `.git`폴더가 생성되며, 여기에 git과 관련된 모든 정보가 저장된다.
- git bash에 `(master)`라고 표시되는데, 이는 현재 폴더가 git으로 관리되고 있다는 뜻이며, `master`라는 branch에 있다는 뜻이다.

### 2. `add`

`working directory`, 즉 작업하고 있는 공간에서 변경된 사항을 이력으로 저장하기 위해서는 반드시 `staging area`를 거쳐야 한다.

```bash
$ git add {파일/폴더}
$ git add markdown.md	# 파일
$ git add images/		# 폴더(슬래시는 필수)
$ git add .				# 현재 디렉토리에 있는 모든 파일과 폴더
```

- markdown.md 파일이 폴더에 새롭게 작성되었으나, 아직 git으로 관리되지 않고, `add`되기 전 상태

```bash
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        markdown.md
        
nothing added to commit but untracked files present (use "git add" to track)
```

- markdown.md 파일이 `add` 후에 `staging area`로 올라간 상태

```bash
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   markdown.md
```

### 3. `commit`

commit은 **이력을 확정짓는 명령어**로, 해당 시점의 스냅샷을 기록한다.

커밋시에는 반드시 메시지를 작성해야 하며, 메시지는 변경사항을 알 수 있도록 명확하게 작성한다.

``` bash
$ git commit -m "마크다운 정리" # "" 안에 원하는 메시지를 입력한다.
[master (root-commit) 68bba0d] 마크다운 정리
 1 file changed, 170 insertions(+)
 create mode 100644 markdown.md
```

커밋 이후에는 아래의 명령어를 통해 지금까지 작성된 커밋 이력을 확인한다.

```bash
$ git log
commit 68bba0ddf48020717fbf9023e9c95b07238a59ee (HEAD -> master)
Author: hch_ang <kkghckd@gmail.com>
Date:   Fri Jul 17 11:48:48 2020 +0900

    마크다운 정리
    
$ git log --oneline # --oneline : 한 줄로 간단하게 표현하는 옵션
68bba0d (HEAD -> master) 마크다운 정리
```

커밋은 중복되지 않는 해시값을 바탕으로 구분된다.

해당 git log의 해시값 : `68bba0ddf48020717fbf9023e9c95b07238a59ee`



## 원격 저장소(remote repository) 활용하기

원격 저장소 기능을 제공하는 다양한 서비스 중에 github을 기준으로 설명한다.

### 0. 준비사항

- Github 회원가입 후 Github에 repository 생성



### 1. 원격 저장소 등록

```bash
$ git remote add origin {github url}
```

- 원격저장소(remote)로 origin이라는 이름으로 github url을 등록(add)한다.
- 등록된 원격 저장소를 보기 위해서는 아래의 명령어를 활용한다.

```bash
$ git remote -v
origin  https://github.com/hch-ang/TIL.git (fetch)
origin  https://github.com/hch-ang/TIL.git (push)
```



### 2. `push` - 원격 저장소로 업로드

```bash
$ git push origin master
```

`origin`이라는 이름의 원격 저장소로 commit 기록들을 업로드한다.

이후 변경사항이 생길 때마다, `add`-`commit`-`push`를 반복한다.



### 3. `pull` - 원격 저장소로부터 불러오기

```bash
$ git pull origin master
```

`origin`이라는 이름의 원격 저장소로부터 새로운 commit 기록들을 불러온다.



## 유형별 git 명령어

### 버전관리

- `add`

- `commit`

- `push`

### 상태확인

- `status`

- `log`

- `git diff`

  파일들의 수정사항을 보여주는 명령어(unstaged 상태의 파일들만 보여주는 명령어로, `git add` 명령어로 stage에 올라간 파일에 대해서는 수정사항을 보여주지 않는다) 

  --> 보통 `git add`하기 전에 수정사항을 확인하기 위해 활용

### 되돌리기(안쓰는 상황이 Best)

- `git restore --staged <file_name>`

  `add`메시지로 stage에 올린 파일을 취소하는 명령어

- `git commit --amend`

  가장 마지막 commit을 수정하는 명령어(직전의 commit 내용 + 지금까지 add한 내용들을 포함하여 commit을 덮어쓴다)

  `i`를 눌러 수정모드로 들어가고 수정사항 수정 후 `esc`로 나간다(편집기능에서 나온다).

  `:wq`를 입력해 저장 후 종료를 한다(`:q`는 저장을 하지 않고 그냥 종료 가능)

  - 가장 마지막 commit만 수정할 수 있으므로 신중하게 commit해야 한다

### 변경사항 무시하기

`.gitignore`파일을 만든 후 `<무시하고 싶은 파일명>/`을 입력한 후 저장한다.

### commit한 파일 취소하기

`git rm -r --cached <파일/폴더명>`으로 삭제 한 후 변경사항을 `add`-`commit`-`push`