# Ingyeom's Tech Blog

Hugo 기반의 개인 기술 블로그입니다.

## 기술 스택

- **Static Site Generator**: [Hugo](https://gohugo.io/)
- **Theme**: Custom (velog 스타일)
- **CMS**: [Decap CMS](https://decapcms.org/) (웹 기반 콘텐츠 관리)
- **Comments**: [Giscus](https://giscus.app/) (GitHub Discussions 기반)
- **Hosting**: GitHub Pages
- **CI/CD**: GitHub Actions

## 로컬 개발

### 요구사항

- Hugo Extended (v0.121.1 이상)

### 설치 및 실행

```bash
# 저장소 클론
git clone --recurse-submodules https://github.com/ingyeomnote/ingyeomnote.github.io.git
cd ingyeomnote.github.io

# 로컬 서버 실행
hugo server -D

# 빌드
hugo
```

## 새 포스트 작성

### 방법 1: 웹 CMS 사용 (추천)

1. https://ingyeomnote.github.io/cms/ 접속
2. GitHub 계정으로 로그인
3. 웹 에디터에서 포스트 작성/수정/삭제

### 방법 2: 로컬에서 마크다운 작성

```bash
# 새 포스트 생성
hugo new posts/my-new-post.md

# content/posts/my-new-post.md 파일이 생성됩니다
# 편집 후 Git commit & push
```

## 배포

`master` 브랜치에 푸시하면 GitHub Actions를 통해 자동으로 배포됩니다.

## 기능

### ✨ Decap CMS
- 웹 UI에서 포스트 작성/수정/삭제
- 마크다운 에디터 제공
- 이미지 업로드 지원
- GitHub OAuth 인증

### 💬 댓글 시스템 (Giscus)
- GitHub Discussions 기반
- 반응(reaction) 지원
- 한국어 지원
- 스팸 필터링

## 디렉토리 구조

```
.
├── archetypes/         # 콘텐츠 템플릿
├── content/            # 블로그 콘텐츠
│   ├── posts/         # 블로그 포스트
│   └── about.md       # About 페이지
├── layouts/           # 커스텀 레이아웃
│   ├── _default/      # 기본 레이아웃
│   └── index.html     # 홈페이지 레이아웃
├── static/            # 정적 파일
│   ├── cms/          # Decap CMS 관리 페이지
│   └── images/       # 이미지 파일
├── .github/
│   └── workflows/     # GitHub Actions 워크플로우
├── hugo.toml          # Hugo 설정 파일
└── README.md
```

## License

이 블로그의 콘텐츠는 저작권법의 보호를 받습니다.
