## 1. Branch 사용하기
개발 작업을 할 때, 개발자들은 작업 레파지토리에서 소스 코드를 공유한다. 연관성이 없는 기능을 개발한다고 할 때 어떤 개발자는 A 기능을 작업하고 또 다른 개발자는 B 기능을 맡아 작업한다고 가정해보도록 하자.

만약 A 작업이 다 끝난 뒤에 B 작업을 수행한다면 연관성이 없는 기능 단위의 작업을 비효율적으로 하고 있는 것이다. **별도로 작업하여 합치는 방안**이 가장 좋다. Git에서는 **브랜치와 머지**라는 명령으로 이것을 가능하게 한다.

브랜치는 독립적인 작업을 할 수 있는 공간이다. A 기능을 A 브랜치에서 작업하고, B 기능을 B 브랜치에서 작업하면, **서로 다른 독립적인 공간에서 작업하는 것이기 때문에 서로에게 영향을 주지 않고 작업할 수 있다.** 메인 작업 공간의 코드를 복사한 개별적인 작업 공간을 만들고 거기서 각각의 기능 개발을 진행한다. 그리고 작업이 완료되었을 때, 코드를 합치면 된다.

<img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fbb978b3c-63ce-441e-8022-687fa427bc9b%2Fcommit-39.png?id=41c89f41-18fc-49a0-b0e1-0d8f6867ec41&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1250&userId=&cache=v2">

개발자는 기능 브랜치에서 작업한 내용을 기본 브랜치로 바로 커밋을 날릴 수 있고 코드를 병합(merge)하고 업데이트 할 수 있다. 브랜치로 작업을 하면 브랜치 단위로 업데이트가 되기 때문에 충돌이 일어나는 일을 방지하기 좋다.

### 기본 브랜치(main, master)
- **Git** : master
- **GitHub** : main
- Git/GitHub에서의 기본 브랜치
- 기본 브랜치는 git 저장소를 초기화할 때 자동으로 만들어진다.
- 이때, `HEAD`라는 특수한 포인터가 있다. 이 포인터는 **지금 작업 중인 브랜치**를 가리킨다.
    > master라는 단어가 노예제를 떠올리게 한다는 이유로 많은 코드에서 master 대신 main을 채택하고 있다.
    >
    > `git config --global init.defaultBranch main`
    >
    > 명령을 통해 git에서도 GitHub처럼 기본 브랜치를 main으로 사용할 수 있다.

- 하나의 기능 개발이 완료되면 특정 브랜치의 내용들을 main 브랜치에 합친다. 또 새로운 작업을 하려고 하면 main 브랜치에서 다시 새로운 **feature 브랜치**를 생성하고 작업한다.
  <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fefaf8fef-3032-4885-8670-1df00937e0c8%2F16.png?id=5515dd91-06f0-4840-9ca2-bcd37603a857&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">
- 특정 기능을 작업하고 있는데 급하게 버그를 수정해야 하는 경우, 현재의 작업 상태를 임시로 커밋해두고 이전 기능 작업 중인 상태로 브랜치(작업폴더)를 변경할 수 있다.
  - 버그 패치용 브랜치를 만들고 수정 작업을 한다. 이후 작업이 완료되면 버그 패치용 브랜치의 추가된 변경 내역을 main 브랜치에 합칠 수 있다. 이런 과정을 거치면 main 브랜치는 가장 최근에 배포한 소스코드의 커밋을 가리키고 있게 된다.

### 브랜치 사용 방법
- 현재 브랜치 목록과 현재 브랜치 확인
    ```bash
    $ git branch
    ```
  
- branch 만들기
    ```bash
    $ git branch Gary   // Gary 브랜치 생성
    ```

<br>

## 2. Branch 이동/변경, 파일 복원하기
### 2-1. checkout
`checkout`은 **브랜치 변경** 또는 **작업 트리 파일 복원**을 할 수 있다. 하지만 기존의 checkout이 가진 기능이 너무 많았기 때문에 Git 2.23에서 checkout을 대신해 `switch`와 `restore`가 도입되었다.

`checkout`을 통해 **브랜치를 이동하여 사용**할 수 있다. 호텔 체크아웃을 생각해서 브랜치를 떠난다고 생각하면 안된다. 사용할 브랜치를 지정하는 것이 checkout이다.
```bash
$ git checkout Gary   // Gary 브랜치로 이동
```

1. 새로운 내용을 추가하고 커밋
    ```bash
    $ echo 'hello branch' >> branch.txt
    $ git status
    $ git add branch.txt
    $ git commit -m "개리 5"
    ```

2. 상대방이 원격 저장소에서 pull을 받는다.
    ```bash
    $ git pull
    ```

3. 상대방이 브랜치를 생성하고 커밋
    ```bash
    $ git checkout main
    $ git branch binky
    $ git checkout binky
    $ git commit -m "빙키 1"
    ```
   
    <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F7e2a4993-78cd-464a-9a31-3f748790dc2f%2F7.png?id=d0107124-40f1-4aec-8593-36e6a26bdc12&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">
  
    > `git log --all --decorate --oneline --graph` 명령어를 사용하면 모든 컴밋을 그래프로 볼 수 있다.

### 2-2. switch
- 브랜치를 변경한다.
    ```bash
    $ git switch Gary   // Gary 브랜치로 변경
    ```

- 브랜치를 새로 만들어서 변경할 수도 있다.
    ```bash
    $ git switch -c Gary
    ```

### 2-3. restore
`restore`는 파일의 **수정 내용 복원**과 `add` 를 통해 **스테이지에 올린 파일을 빼낼 때** 사용한다.

1. 우선 파일을 생성한 후 커밋을 한다.
    ```bash
    $ touch README.md
    $ vi README.md //i->작성 후->Esc->:wq
    $ git add .
    $ git commit -m "add file"
    $ git status
    ```

2. 이후 해당 파일을 수정한 후에 저장을 하고 상태 확인
    ```bash
    $ vi README.md
    ```
    ```bash
    i -> 수정 후 -> Esc -> :wq   // 아래 순서에 따라 순차적으로 편집
    ```
<br>

#### 파일 변경 내역을 modified 파일에서 unmodified 파일로 되돌리는 방법
1. `restore`을 이용하여 변경 사항을 되돌리기
    ```bash
    $ git restore README.md
    ```
2. `checkout`을 이용하여 파일 변경 내역을 되돌리기 
    ```bash
    $ git checkout -- README.md
    ```
   - `git checkout -- filename `은 해당 파일 내용이 최신 커밋 전으로 돌아가도록 한다. 이때, checkout으로 지워진 내용은 커밋을 하지 않았기 때문에 다시 복구할 수 없다.

<br>

#### 스테이지 올린 것을 빼는 방법
```bash
$ git add .
$ git status
```
1. `restore` 명령어를 이용하여 빼기
    ```bash
    $ git restore --staged README.md
    ```
2. `reset` 명령어를 이용하여 빼기
    ```bash
    $ git reset HEAD README.md
    ```



