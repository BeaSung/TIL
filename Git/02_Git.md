## 저장소 만들기
- 작업할 디렉토리를 만들고(`mkdir`) 생성한 디렉토리로 이동하기(`cd`)
    ```bash
    $ mkdir git-test
    $ cd git-test
    ```

- 현재 디렉토리를 Git 저장소로 만들어 원하는 디렉토리를 기준으로 버전관리를 한다.
  ```bash
  $ git init
  ```
  `git init` 을 입력하면 해당 폴더 기준으로 `.git(로컬 저장소)`가 생성된다. 로컬 저장소에는 버전 정보, 원격 저장소 주소가 저장된다.
  > ⚠️ 이때, 한 폴더에는 하나의 .git(로컬 저장소)을 가져야 한다. 그렇지 않을 경우 충돌이 발생할 수 있다.

<br>

## First commit!

<img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F81ca98f7-2c75-453d-8e38-70b825d1fceb%2Fab14bbe81eaa8a64.png?id=b815462d-a252-45a3-90c8-0b05ce05d860&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">

- `git pull` : 원격 저장소에서 내려받기
- `git add` : 비행기를 타기 위해 공항에 대기중인 상태 -> 추가/수정된 파일들이 대기하는 상태
- `git commit` : 비행기에 탑승! -> 로컬 스토리지에 올라감 (커밋을 하면 version이 생성됨)
- `git push` : 원격 스토리지에 올라감

### 1. 추가하고 커밋하기(`add`, `commit`)
- 파일을 생성(touch), 추가(add)하고 커밋(commit)하기
  ```bash
  $ touch README.md
  $ git status # Untracked 확인
  $ git add README.md
  $ git commit -m "first commit"
  ```

#### 1-1. git이 관리할 대상의 파일 등록하기(`add`)
변경한 파일 목록 중 **스테이지에 올리기 원하는 파일만 선택**한다. <br>
**파일 전체**를 올리고 싶은 경우에는 git add 뒤에 .을 입력한다(`git add .`). 이 때, 스페이스 바가 한 칸 들어간 다는 점을 잊지말자!
```bash
$ git add README.md // 지정 파일 올리기
$ git add . // 파일 전체 올리기
```

#### 1-2. 버전 만들기 (`commit`)
```bash
$ git commit -m "저장 메세지를 입력해주세요"
```

<img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fcb85485d-bce7-4be1-b0d3-071cff132762%2F1_commit.png?id=2ed57324-c156-4bb1-aeac-52a57a985632&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1060&userId=&cache=v2">

### 2. 상태 확인하는 방법 (`status` , `diff` , `log`)
#### 2-1. 파일 상태 확인하기 (`status`)
```bash
$ git status
```

파일의 상태에 따라 Untracked 와 Tracked 로 분류된다.
1) **Untracked**(관리 대상이 아님) : 파일 생성 후 한번도 `git add`하지 않은 상태
2) **Tracked**(관리 대상임) : git이 관리하는 파일임을 의미한다.

- `Unmodified` : 최근의 커밋과 비교했을 때 바뀐 내용이 없는 상태
- `Modified` : 최근 커밋과 비교했을 때 바뀐 내용이 있는 상태
- `Staged` : 파일이 수정되고 나서 스테이지 공간에 올라와 있는 상태이며, `git add` 후의 상태
  <br>

아래의 그림을 통해 로컬 저장소와 원격 저장소 사이의 파일 이동 경로 및 상태를 확인하고 어떤 명령어가 사용되는지 살펴보자!

<img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe47126c2-de4c-48f9-8421-34ccb4de25b2%2Fgit.jpg?id=1f2b2ac8-6ccf-49af-a7b9-6e8a98ccf7af&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1250&userId=&cache=v2">

#### 2-2. 변경사항 확인하기 (`diff`)
`add하기 전` 최근 commit한 내용과 현재 폴더의 변경 사항을 확인할 수 있다.
```bash
$ git diff
```

#### 2-3. 커밋(commit) 히스토리 조회하기 (`log`)
`git log` 명령어를 입력하면, 최근 커밋한 히스토리를 확인할 수 있다.
```bash
$ git log
```

### 3. 저장소에 무시할 파일 설정하는 방법
#### 3-1. 무시할 파일 (gitignore) 추가 하기
1) `.gitignore` 사용하기
push 전  `.gitignore` 파일에 버전 관리에서 제외할 파일을 추가한다.
    ```
    # a comment - 이 줄은 무시한다.
    # 확장자가 .a인 파일 무시
    *.a
    # 윗 줄에서 확장자가 .a인 파일은 무시하게 했지만 lib.a는 무시하지 않는다.
    !lib.a
    # 루트 디렉토리에 있는 TODO파일은 무시하고 subdir/TODO처럼 하위디렉토리에 있는 파일은 무시하지 않는다.
    /TODO
    # build/ 디렉토리에 있는 모든 파일은 무시한다.
    build/
    # `doc/notes.txt`같은 파일은 무시하고 doc/server/arch.txt같은 파일은 무시하지 않는다.
    doc/*.txt
    # `doc` 디렉토리 아래의 모든 .txt 파일을 무시한다.
    doc/**/*.txt
    ```

2) `.gitignore` 자동 생성기 활용하기
`.gitignore` 파일을 직접 생성해야 하는 경우가 종종 있다. 이 때, 라이브러리와 프레임워크를 사용하는 경우에는 어떤 파일을 깃 버전 관리에서 제외 시켜야 하는지(`.gitignore`파일에 넣어야 되는지) 헷갈리는 경우가 있다. 혹은 일일이 작성하기 번거로운 경우도 있다. 이럴 때 편하게 사용할 수 있는 툴인 **gitingnore.io** 를 활용해보자!
- [gitingnore.io](https://www.toptal.com/developers/gitignore)

