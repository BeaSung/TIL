## GitHub 세팅
### 1. Repository 생성하기
레파지토리 이름을 설정하고 `Public(공개)`/`Private(비공개)`를 설정한 후, Add a README file을 클릭하여 README file을 추가한다. 설정이 완료되면 **Create Repository**를 클릭한다.

<img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F24d25a68-ff70-49f5-8bbc-1ddc9df45ed7%2FUntitled.png?id=5c29cb4e-6ff4-458f-8764-0a6b31d1478f&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">
<img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fea739a84-80e3-423f-9738-681b11aaefb3%2FUntitled.png?id=b040937b-4a29-4702-8284-fc97200a23bb&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1060&userId=&cache=v2">

### 2. GitHub에 파일 올리기
#### 2-1. GUI, CLI를 이용하여 GitHub에 올리기
- Add file > Upload File > 001.html
  <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F9eac7284-65df-4744-87a8-27c552173813%2FUntitled.png?id=e9606b44-dc05-4325-b4cb-8496d348bc05&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">
  - 초기 레파지토리 생성시 `README.md` 파일을 생성하지 않았을 경우 해당 화면이 나오지 않고, 아래와 같은 git 명령어가 나오게 된다. 똑같이 git bash에 입력하면 `README.md`파일이 GitHub 레파지토리로 들어가는 것을 확인할 수 있다.
  <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc67558cc-c15f-487e-b542-4956ed4f1db3%2FUntitled.png?id=a0dfd436-ce02-4545-a349-4e57c2e2bdca&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">

#### 2-2. Repository 내 하위폴더 생성하기
프로젝트를 진행하다보면 레파지토리의 갯수가 늘어나게 된다. 이렇게 늘어난 레파지토리를 체계적으로 관리하기 위해서는 하위 폴더의 생성이 필요하다. 하위 폴더를 생성하고자 하는 레파지토리에 들어간 후, 아래 순서를 따라 폴더를 생성해 보자.
1) 우측 상단의 `Add file`을 클릭하고 `Create new file`을 클릭한다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F36e40507-18f6-48a7-ac46-50351a7244bd%2F32CF1492-A4D0-4764-BC14-193AAF6E3AEE_4_5005_c.jpeg?id=5bdfc66e-6911-4cf4-b9d7-b99cba124b49&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=770&userId=&cache=v2">
2) `/` 를 기준으로 폴더가 분리된다. `/{폴더명}` 을 입력해서 폴더를 생성한다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc665f9d5-ef44-4298-a784-a3d97fb9b2d0%2FScreen_Shot_2022-03-01_at_6.44.38_PM.png?id=f6047f26-0f67-4910-9e5b-ecb0ab1d3498&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=960&userId=&cache=v2">
3) 빈 폴더는 생성되지 않는다. 임의의 파일을 생성해주어야 아래와 같이 `commit new file` 버튼이 활성화 된다. `Commit new file` 버튼을 클릭한다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F519fe949-1852-4c73-ba7c-11487f483e5e%2FD16BE856-AC85-4655-9CB8-88EF6DDD4906.png?id=1dca4ac0-51f3-46f1-8984-bddc3a28535b&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1150&userId=&cache=v2">
4) `githubTest` 레파지토리안에 `test` 폴더가 생성되었고 폴더의 하위에 `index.html` 파일이 생성된 것을 확인할 수 있다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fc5ff9e5a-be6e-4601-ae75-53752075efe6%2F%E1%84%8B%E1%85%AA%E1%86%AB%E1%84%89%E1%85%A5%E1%86%BC.jpeg?id=fa237130-5df5-46f8-b89c-835f113666c8&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1150&userId=&cache=v2">

> `index.html` 파일을 각각에 하위 폴더로 넣고 GitHub Page를 운영할 경우 하위 폴더를 `http://{계정명}.github.io/{폴더명}/{폴더명}` 과 같이 URL 구조처럼 사용할 수 있다. <br>
> 예) paullabkorea/githubtest/test → 실제로는 test 폴더에 index.html로 연결된다.

<br>

## git clone
원격 저장소의 코드를 컴퓨터에 받아올 수 있다. 새 작업 디렉토리 만들고(`mkdir` 명령어) 생성한 디렉토리로 이동(`cd` 명령어)한다.
```bash
$ mkdir filename
$ cd filename
$ git clone https://github.com/id/clone-filename.git .
```

> 클론 시 점(.)을 찍는 이유는 현재 폴더에 클론 받기 위함이다. 만약, 점(.)을 찍지 않을 경우 새 폴더를 생성한다.

<br>

## git pull
원격 저장소에 업데이트 된 데이터를 가져오고 병합할 때 사용한다.
```bash
$ git pull origin main
```

- 코드를 수정하고 pull 받으려하니 누군가 이미 코드를 수정했을 경우에 사용하는 명령어다. 여러분이 push를 하려고 했더니 누군가 이미 push를 해서 pull을 받아야 하는 상황이다.
  ```시나리오
  A사람 clone --- push1
  B사람 clone -------------- pull-push2
  C사람 clone ------------------------------- push3(pull 받지 않아 error)
  ```
  1. 로컬 main과 원격 main을 다른 브랜치로 보고 병합한다.
      ```bash
      git pull --no-rebase
      ```
  2. 시간상 순서대로 병합한다.
      ```bash
      git pull --rebase
      ```

<br>

## git add, git commit, git push
### 1. `git add`, `git commit`, `git push`
```bash
$ git status
$ git add .
$ git commit -m "추가 작업 내역입니다."
$ git push origin main
```
push가 완료되면 GitHub에 잘 올라갔는지 확인한다. push를 하게 되면 로컬 저장소에 있는 소스코드 또는 파일들이 GitHub에 올라가게 된다.<br>
GUI 환경에서 히스토리를 확인할 수 있다. 아래처럼 누가 어떤 소스코드를 수정했는지 내역을 확인할 수 있다. 되돌릴 수도 있다.

<img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa44651f6-0a30-4f52-90ac-00cdf86baf0f%2FUntitled.png?id=02e694f1-fc43-4ebb-91a4-fb8e7614fa2b&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1150&userId=&cache=v2">

> 원격 저장소와 로컬 저장소의 싱크가 맞지 않아(예를 들어 컴밋 개수가 다르다던지) 로컬 저장소로 강제로 맞추고 싶을 때 사용하는 명령어다. 이 명령어는 혼자 레파지토리를 사용할 때 사용하고 협업시 절대 사용해서는 안되는 명령어다. <br>
> ```bash
> git push --force
> ```

