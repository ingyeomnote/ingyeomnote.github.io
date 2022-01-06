git 에러



# Git Push 에러 해결하기([Rejected] Master -> Master (Fetch First) Error: Failed To Push Some Refs To)

 [dal](https://blog.dalso.org/author/dal) 2020년 7월 27일 [7 Comments](https://blog.dalso.org/it/git/14204#comments)

![img](https://blog.dalso.org/wp-content/uploads/2020/07/image-546.png)

깃을 사용중에 위와같은 오류가 나왔을때 해결방법입니다.

**`! [rejected] master -> master (fetch first)error: failed to push some refs to 'https://github.com/dalso~~'hint: Updates were rejected because the remote contains work that you dohint: not have locally. This is usually caused by another repository pushinghint: to the same ref. You may want to first integrate the remote changeshint: (e.g., 'git pull …') before pushing again.hint: See the 'Note about fast-forwards' in 'git push --help' for details.`**

### 원인은 깃의 원격 저장소와 현재 로컬 저장소가 동기화되어있지 않기때문입니다. 해결방법도 동기화 시켜주면 간단하게 해결됩니다.

**`git pull --rebase 원격저장소별칭 master`**

![img](https://blog.dalso.org/wp-content/uploads/2020/07/image-547.png)

위 명령어를 통해 정상적으로 동기화 시켜주면 다시 push하는데 이상없을겁니다.

사이트 리뉴얼중입니다~

서버(Linux, ESXi), NAS(헤놀로지, ESXi 및 IT관련 정보, 기타 등등을 공유하는 커뮤니티 [SVRFORUM](https://svrforum.com/)을 새로 만들었습니다.
많은 가입(?) 부탁드립니다~
[**https://svrforum.com**](https://svrforum.com/)

이전글들은 모두 상단 메뉴의 Blog 글 모음에있습니다!