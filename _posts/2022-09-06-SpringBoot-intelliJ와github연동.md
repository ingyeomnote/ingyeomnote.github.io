---
title:  "[이동준] IntelliJ와 Github 연동하기"
excerpt: "IntelliJ에서 Github를 연동하는 방법에 대해서"

toc: true
toc_sticky: true

categories:
- 스프링 부트와 AWS로 혼자 구현하는 웹 서비스

tags:
  - Java
  - SpringBoot
  - IntelliJ
  - github

last_modified_at: 2022-09-06T22:06:00
---



최근의 개발 상황에서 버전 관리는 뺄 수 없는 요소이다. 그리고 이 버전 고나리는 SVN에서 Git으로 완전히 전환되어 가는 중이며, 실제로 대부분의 IT 서비스 회사는 Git을 통해 버전 관리를 하고 있다.
깃의 원격 저장소 역할을 하는 서비스는 대표적으로 Github  Gitlab이 있다.

# 🚩Github 연동
우리는 여기서 GitHub를 사용하겠다. 인넬리제이에서 단축키로 윈도우에서는 [Ctrl + Shift + A], 맥에서는 [Command + Shift + A]를 사용해 Action 검색착을 열어 share project on github를 검색한다.

![share project on Github](https://velog.velcdn.com/images/ingyeomnote/post/3ec3c8d3-11c3-4097-ac5d-365157254c8c/image.png)

깃허브에에 생성할 저장소 정보를 입력하는 화면이 나온다. [Repository name] 필드에 등록한 이름으로 깃허브에 저장소가 생성된다. 대부분은 프로젝트 이름을 깃허브 저장소와 같은 이름을 사용합니다.

![share project on Github-2](https://velog.velcdn.com/images/ingyeomnote/post/2afd09d1-a7f2-46c2-8909-4d9e06756dff/image.png)

이때, `Share` 버튼을 클릭하면 '계정은 공백일 수 없습니다'라는 경고 메시지가 나타난다. 당황하지 말고 오른쪽의 `계정 추가` 버튼을 클릭하여 `Github를 통해 로그인...`을 선택하자

절차대로 Github에 로그인하여 jetbrain과 연동에 성공하면 계정이 추가된 것을 확인할 수 있다.
![share project on Github-3](https://velog.velcdn.com/images/ingyeomnote/post/1bbceb06-8d7e-4e11-9621-3bc09477b785/image.png)

이제 `공유` 버튼을 클릭하면 프로젝트의 첫 번째 커밋을 위한 팝업창이 등장한다.
![share project on Github-4](https://velog.velcdn.com/images/ingyeomnote/post/b1e124ee-956d-49c8-b68c-4f09150ef364/image.png)

여기서 📂.idea 디렉토리는 커밋하지 않는다. 이는 인텔리제이에서 프로젝트 실행시 자동으로 생성되는 파일들이기 때문에 깃허브에 올리기에는 불필요하다. 커밋할 파일들을 설정했으면 `추가` 버튼을 클릭하고 `푸시` 하자

커밋과 푸시과 성공적으로 수행됐다면 깃허브 계정으로 이동한다. 그럼 다음과 같이 인텔리제이로 만든 프로젝트가 그대로 깃허브에도 생성된 것을 확인할 수 있다.

![share project on Github-5](https://velog.velcdn.com/images/ingyeomnote/post/fc7d2373-3bec-46d7-a31b-9c86402b59d1/image.png)

---

# 🚩.gitignore

깃허브와 동기화가 되었으니 좀 전에 커밋을 하면서 대항에서 제외했던 .idea 폴더를 앞으로의 모든 커밋 대상에서 제외되도록 처리하자 깃에서 특정 파일 혹은 디렉토리를 관리 대상에서 제외할 때는 📝.gitignore 파일을 사용한다. 이 파일 안에 기입된 내용들은 모두 깃에서 관리하지 않겠다는 것을 의미한다.

인텔리제이에서는 이 .gitignore 파일에 대한 기본적인 지원이 없다. 대신 플러그인에서 .gitignore 지원을 하고 있다. .gitignore 플러그인에서 지원하는 기능은 다음과 같다.

- 파일 위치 자동완성
- ignore 처리 여부 확인
- 다양한 ignore 파일 지원(.gitignore, npmignore, dockerignore 등등)

그럼 이제 .ignore 플러그인을 설치를 위해 Action 검색창을 열어 plugins을 검색하면 플러그인 설치 팝업창이 나온다. 

[Marketplace] 탭은 설치 가능한 플러그인 목록을 보여주며, [Installed] 탭은 이미 설치된 플러그인 목록을 나타낸다. 새로운 플러그인을 설치할 것이므로 [Marketplace] 탭에서 .ignore을 검색하고 `Install` 버튼을 클릭하여 설치한다.

![share project on Github-6](https://velog.velcdn.com/images/ingyeomnote/post/64a5ea85-e09c-4d91-aa46-bd34eef0246b/image.png)

이때 반드시 인텔리제이를 다시 시작해야만 설치한 플러그인이 적용되므로 잊지 말고 재시작을 해야 한다. 설치가 다 되었다면 이제 ignore 파일을 한 번 생성해보자

다음 그림과 같이 왼쪽 위의 프로젝트 이름을 선택한 뒤, 마우스 오른쪽 버튼을 누르거나 단축키를 눌러 새로 만들기 탭을 열어보자. 단축키는 윈도우에서는 [Alt + Insert], 맥에서는 [Command + N]이다.

새로 만들기 탭에서 [`.ignore File` -> .gitignore File(Git)]을 선택해서 .gitignore 파일을 생성하겠다.

![share project on Github-7](https://velog.velcdn.com/images/ingyeomnote/post/d76b43a4-579b-41de-9089-abced8978cd6/image.png)

그럼 다음과 같이 .gitignore 생성 화면이 나온다.

![share project on Github-8](https://velog.velcdn.com/images/ingyeomnote/post/cf976667-040e-4f49-a743-064552d1792f/image.png)

`Generate` 화면의 경우 사용자가 미리 만들어 둔 이그노어 템플릿 선택하는 화면이다. 예를 들어 본인이 이미 인텔리제이 프로젝트를 사용할 때는 A라는 디렉토리와 B라는 파일을 이그노어하도록 미리 설정해 둔 것이 있다면 해당 템플릿을 선택하고 `Generate` 버튼을 클릭하면 바로 생성된다. 

미리 만들어 준 것이 없기 때문에 바로 `Generate` 버튼을 클릭해 .gitignore 파일을 생성한다. 생성된 .gitignore파일에 깃 체크 대상에서 제외하고 싶은 이름을 작성하면 된다. 

인틸리제이에서 자동으로 생성되는 파일들을 모두 이그노어 처리하기 위해 .gitignore파일에 다음과 같은 코드를 작성한다.

>.gradle
>.idea


이렇게 이그노어 처리된 것을 깃허브에도 반영해보자. 깃 커밋을 위한 창을 열어야 하는데 윈도우에서는 [Ctrl + K], 맥에서는 [Commnad + K]이다.

![share project on Github-9](https://velog.velcdn.com/images/ingyeomnote/post/d7ff6cb6-86d4-470c-bf7d-ae7d407a42c2/image.png)

.gitignore 파일을 선택하고, 메시지를 작성했다면 [Commit] 버튼을 클릭한다. 그럼 해당 파일이 Commit 상태가 되는데, 바로 푸시까지 진행하겠다. 푸시의 단축키는 윈도우에서는 [Ctrl + Shift + K], 맥에서는 [Command + Shift + K]이다. 

푸시까지 성공적으로 수행했다면 다시 깃허브의 프로젝트로 이동하면 다음과 같이 커밋과 푸시가 성공적으로 반영된 것을 확인할 수 있다.

![share project on Github-10](https://velog.velcdn.com/images/ingyeomnote/post/5d56d1d6-c5fd-4028-b136-a1c30a55ca7d/image.png)

또한, IntelliJ에서 프로젝트 구조에서 파일들의 상태를 확인할 수 있다.

![share project on Github-11](https://velog.velcdn.com/images/ingyeomnote/post/88bdecb5-32a9-43ea-a376-7d2e7791a8a6/image.png)


위처럼 파일들의 색깔이 전부 다른 것을 알 수 있는데, 아래와 같은 의미를 뜻한다.

>
>⬜ **흰색** : 
> &nbsp; &nbsp; 저장소에 올라간 파일과 똑같은 상태
>🟨**노란색** : 
> &nbsp; &nbsp; .gitignore 파일을 통해 제외한 파일들
>🟥**빨간색** : 
> &nbsp; &nbsp; 저장소에 존재하지 않는 파일
>🟦**파란색** : 
> &nbsp; &nbsp; 저장소와 내 컴퓨터에 있는 파일인데 "수정"된 상태. (즉, 저장소에 올라간 파일과 내 컴퓨터에 있는 파일의 내용이 다른 상태)
>🟩**초록색** : 
> &nbsp; &nbsp; Git에 commit만 하고 push는 하지 않은 상태. (push하면 흰색으로 바뀐다.) 

 이제 개발하는 과정에서 커밋과 푸시가 필요하면 인텔리제이에서 바로 진행하고 다시 개발에 돌입할 수 있다.
