## 1. Fork란? 
빙키와 개리가 만든 저장소가 있다. 이 저장소에 없는 기능을 알리가 만들고 싶어한다. 하지만 저장소의 권한은 빙키와 개리에게만 있고 알리에게는 없다. 이때, 저장소의 권한을 얻어내기 위해 알리는 기여자 등록을 해야 할까? 

기여자 등록을 따로 하지 않아도 Fork기능을 사용하여 원본 저장소를 복사해 내 저장소에서 **commit** > **push** 하실 수 있다. 기능 생성 후, 내 저장소 브랜치와 빙키와 개리의 저장소의 브랜치에 merge를 하면 된다. <br>
물론 허락을 맡아야 한다!

<br>

## 2. Fork 하는 방법
1. 복사하고자 하는 Github 레파지토리에 들어가서 오른쪽 상단에 Fork를 클릭한다. Fork가 완료되면 내 레파지토리에 생성된 것을 볼 수 있다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe43799c5-aa83-41fd-a16f-cc7e9187aa79%2FUntitled.png?id=3dbcef55-716c-4abd-84f7-db56bdc39020&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1250&userId=&cache=v2">
2. 이제 포크한 저장소를 클론하기 위해 주소 복사 버튼을 클릭한다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Faca6e0db-bbd3-4b85-9889-fe59e2245dfa%2FUntitled.png?id=c0d6527a-bcb0-4ed2-99cc-45571c124c30&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=860&userId=&cache=v2">
3. 저장하고 싶은 곳으로 이동한 후, 클론한다. 이때, 개리와 빙키의 원본 저장소의 변경 이력을 볼 수 없다. 원본 저장소와 알리의 저장소는 다르기 때문이다. 원본 저장소의 이력을 보고 싶은 경우에는 원본 저장소를 원격 저장소에 추가해야 한다.
    ```bash
    $ cd 저장하고_싶은_디렉토리
    $ git clone 복사한_git주소 .
    ```
4. 원격 저장소 이름들을 가지고 옵니다.
    ```bash
    $ git remote
5. 새로운 원격 저장소를 추가한다.
    ```bash
    $ git remote add 새로운_원격저장소_이름 fork한_git주소
6. 로컬 저장소에는 없지만 원본 저장소에 있는 데이터를 가져오려면 fetch 명령어를 입력한다.
    ```bash
    $ git fetch 새로운_원격저장소_이름
    ```

<br>

## 3. 브랜치와 포크의 차이는? 
브랜치와 포크는 두가지 모두 코드를 협업하기 위해 분기점을 나누는 방식이지만 특성이 다르므로 프로젝트에 맞게 사용해야 한다.

| 브랜치                    | 포크                                  |
|------------------------|-------------------------------------|
| 하나의 저장소에서 브랜치를 나누어 쓴다. | 여러 저장소를 만들고 브랜치를 만들어 사용한다.          |
| 코드 커밋 이력을 쉽게 볼 수 있다.   | 원본 저장소에 영향을 미치지 않으므로 자유롭게 수정할 수 있다. |
| 소수인원 작업 시사용하는 것이 좋다.   | 원본 저장소의 이력을 보려면 주소를 추가해야 한다.        |
|                        | 불특정 다수의 사람의 작업 시 사용하는 것이 좋다.        |

