# Ingyeom's Tech Blog

Hugo 기반의 개인 기술 블로그입니다.

## 기술 스택

- **Static Site Generator**: [Hugo](https://gohugo.io/)
- **Theme**: Custom (기본 레이아웃)
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

```bash
# 새 포스트 생성
hugo new posts/my-new-post.md

# content/posts/my-new-post.md 파일이 생성됩니다
```

## 배포

`main` 브랜치에 푸시하면 GitHub Actions를 통해 자동으로 배포됩니다.

## 디렉토리 구조

```
.
├── archetypes/         # 콘텐츠 템플릿
├── content/            # 블로그 콘텐츠
│   ├── posts/         # 블로그 포스트
│   └── about.md       # About 페이지
├── static/            # 정적 파일
├── themes/            # 테마
│   └── PaperMod/
├── .github/
│   └── workflows/     # GitHub Actions 워크플로우
├── hugo.toml          # Hugo 설정 파일
└── README.md
```

## License

이 블로그의 콘텐츠는 저작권법의 보호를 받습니다.
