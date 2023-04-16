## 1. Git Pull Request란?
상대방의 저장소를 Fork한 후 원본 저장소에 올리고 싶을 때 어떻게 해야 할까? <br>
이럴 때, 원본 저장소의 권한을 가진 사람에게 두 브랜치를 합치는 것을 허락해 달라고 요청을 보내야 한다. 이것을 **Pull Request** 또는 **PR**이라고 한다. <br>
**PR은 원본 저장소에 보낼 수 있고 포크한 저장소에도 보낼 수 있다.** PR 요청을 사용하면 깃허브의 저장소안에 있는 브랜치에 푸쉬한 변경사항을 다른 사람에게 알릴 수 있다. <br> 
PR이 열리면 팀원과 변경사항을 논의하고 검토할 수 있으며, 변경 사항이 기본 브랜치에 병합되기 전에 후속 커밋을 추가할 수 있다.

<br>

## 2. Git Pull Request의 장점
협업 시에는 최대한 직접 merge하는 것은 피하고 **모든 merge를 pull request를 통해서 하는 것이 좋다.** 상대방이 PR을 보고 **코드 리뷰**가 가능하기 때문이다. <br>
PR에 수정이 필요하면 댓글을 달아 **change request**를 보낼 수 있다. 단, 오픈 소스에 PR을 보낼 때에는 기여 안내문서를 참고해야 한다.

<br>

## 3. Pull Request 보내는 방법
1. 포크한 저장소에 `Contribute`를 클릭하고 `Open pull request` 버튼을 클릭한다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fca3ebf0f-b307-4d2b-ac11-fc9cb3c077d2%2FUntitled.png?id=1b9ba8f2-31dc-4c9f-af0b-628d849cd2b0&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=860&userId=&cache=v2">
2. 원본 저장소로 이동하게 되는데, 어떤 브랜치에 어디로 브랜치 할 것인지 선택한다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F522451e3-575b-428c-a1e0-ab1125f211c3%2FUntitled.png?id=0a0c01d0-cdc7-4854-ba9e-d80adbb29b62&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">
3. 초록색 문구가 나타나면 정상적으로 PR을 할 수 있고 빨간색 문구가 뜬다면 충돌을 해결하고 PR을 한다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F32213813-bcf3-499e-9024-25f4b67b501c%2FUntitled.png?id=e8210b22-d0fd-4bfe-99ec-d14993c94fb3&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">
4. `Create pull request`를 클릭하면 제목과 글을 작성할 수 있다. <br> 작성이 완료되면 `Create pull request` 버튼이 활성화 된다. <br> 상단 `Pull requests`를 클릭하면 상대방이 PR을 보낸 것을 확인할 수 있다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4f8c2c3c-4d3b-4877-8b64-b5b90312515c%2FUntitled.png?id=261c2c22-87b0-495a-81cf-7da433aea99c&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">
5. 이 PR은 내가 **merge**할 수 없고 권한이 있는 사람이 리뷰를 달거나, `Merge pull request` 버튼을 클릭하여 **merge**할 수 있다.
   <img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Feaf068d6-6153-41de-a4a6-b752874c2369%2FUntitled.png?id=03d1634d-048e-4e96-80ed-b60a347f93f1&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=2000&userId=&cache=v2">

<br>

## 4. Pull Request 취소하기
`Pull Request`를 보내고 취소하고 싶다면, `Pull Request`를 요청한 레포지토리로 간 이후, 자신이 보낸 PR을 선택한다. 하단에 `Close pull request` 버튼을 클릭하면 자신이 요청한 PR이 취소된다.
<img src="https://paullabworkspace.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F23186e8e-597f-49d1-81ca-fd6694548ee8%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-03-05_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_12.56.46.png?id=786b9fb9-b7b5-43d7-bdc4-514eafc6b2ac&table=block&spaceId=579fe283-28aa-489d-ae65-d683304becfc&width=1250&userId=&cache=v2">


