---
title: "[블로그 만들기 #3] GitHub Pages 자동 배포 설정하기 - GitHub Actions 완벽 가이드"
date: 2025-11-04T15:00:00+09:00
description: "GitHub Actions를 사용해서 Hugo 블로그를 GitHub Pages에 자동으로 배포하는 방법을 단계별로 설명합니다."
categories: ["블로그 만들기"]
tags: ["GitHub Pages", "GitHub Actions", "배포", "CI/CD"]
draft: false
---

## 들어가며

이전 편에서 Hugo의 구조를 이해했다면, 이번에는 **실제로 인터넷에 블로그를 공개**하는 방법을 알아봅니다.

핵심은 간단합니다:
```
로컬에서 글 작성 → Git push → GitHub이 자동으로 빌드 & 배포!
```

이 모든 게 **GitHub Actions** 덕분에 가능합니다.

## GitHub Pages란? (복습)

### 기본 개념

GitHub Pages는 GitHub 저장소의 파일을 **무료로 웹사이트로 호스팅**해주는 서비스입니다.

| 항목 | 일반 호스팅 | GitHub Pages |
|------|-------------|--------------|
| 비용 | 월 5,000원~ | **무료** |
| 도메인 | 별도 구매 | `username.github.io` 제공 |
| 배포 | FTP 업로드 | **Git push** |
| HTTPS | 추가 설정 | **자동 지원** |
| CDN | 유료 | **자동 적용** |

### 두 가지 타입

#### 1. User/Organization 사이트
- 저장소 이름: `username.github.io`
- 접속 주소: `https://username.github.io`
- **계정당 1개만 가능**

#### 2. Project 사이트
- 저장소 이름: 아무거나 (예: `my-blog`)
- 접속 주소: `https://username.github.io/my-blog`
- 여러 개 가능

제 블로그는 **User 사이트**입니다:
- 저장소: `ingyeomnote.github.io`
- 주소: `https://ingyeomnote.github.io`

## GitHub Actions란?

### CI/CD를 쉽게 이해하기

**CI/CD**: Continuous Integration / Continuous Deployment
- 코드 변경 → 자동 테스트 → 자동 배포

**실생활 비유**:
```
[수동 배포]
1. 로컬에서 hugo 명령어 실행
2. public 폴더를 압축
3. 호스팅 서버에 FTP로 업로드
4. 압축 해제
→ 너무 귀찮아! 😫

[자동 배포 with GitHub Actions]
1. git push
→ 끝! 🎉
```

### GitHub Actions 작동 원리

```
1. 코드를 master 브랜치에 push
   ↓
2. GitHub이 이벤트 감지
   ↓
3. GitHub의 서버에서 워크플로우 실행
   - Hugo 설치
   - hugo 명령어 실행
   - public 폴더 생성
   ↓
4. 자동으로 GitHub Pages에 배포
   ↓
5. 1~2분 후 https://username.github.io 업데이트!
```

## 실제 워크플로우 파일 분석

제 블로그의 `.github/workflows/hugo.yml` 파일입니다.

### 전체 구조

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["master"]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Install Hugo CLI
      - name: Checkout
      - name: Build with Hugo
      - name: Upload artifact

  deploy:
    needs: build
    steps:
      - name: Deploy to GitHub Pages
```

하나씩 뜯어봅시다!

### 1. 워크플로우 이름과 트리거

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["master"]
  workflow_dispatch:
```

**해석**:
- `name`: GitHub Actions 탭에서 보이는 이름
- `on.push.branches`: **master 브랜치에 push하면 실행**
- `workflow_dispatch`: GitHub 웹에서 수동으로도 실행 가능

**실전 예시**:
```bash
git push origin master  # ← 이 순간 워크플로우 시작!
```

### 2. 권한 설정

```yaml
permissions:
  contents: read      # 저장소 읽기
  pages: write        # Pages에 쓰기
  id-token: write     # 인증 토큰
```

GitHub Pages에 배포하려면 이 권한들이 필요합니다.

### 3. 빌드 작업 (Build Job)

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.121.1
```

**실행 환경**:
- `ubuntu-latest`: 최신 우분투 서버에서 실행
- `HUGO_VERSION`: 사용할 Hugo 버전 지정

#### Step 1: Hugo 설치

```yaml
- name: Install Hugo CLI
  run: |
    wget -O ${{ runner.temp }}/hugo.deb \
      https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb
    sudo dpkg -i ${{ runner.temp }}/hugo.deb
```

**무슨 일이 일어나나?**
1. Hugo 실행 파일을 다운로드
2. 우분투 서버에 설치

**왜 필요한가?**
- GitHub 서버에는 Hugo가 없으니까!
- 매번 새로운 서버에서 실행되므로 매번 설치 필요

#### Step 2: 코드 체크아웃

```yaml
- name: Checkout
  uses: actions/checkout@v4
  with:
    submodules: recursive
```

**무슨 일이 일어나나?**
- GitHub 저장소의 코드를 서버로 복사

**비유**:
```
GitHub 저장소 (원본) → 복사 → GitHub Actions 서버 (작업 공간)
```

#### Step 3: Hugo로 빌드

```yaml
- name: Build with Hugo
  env:
    HUGO_ENVIRONMENT: production
    HUGO_ENV: production
  run: |
    hugo \
      --minify \
      --baseURL "${{ steps.pages.outputs.base_url }}/"
```

**무슨 일이 일어나나?**
1. `hugo` 명령어 실행
2. `--minify`: HTML/CSS/JS 압축 (로딩 속도 향상)
3. `--baseURL`: 배포 주소로 링크 생성

**결과**:
```
content/ + layouts/ → [hugo] → public/
```

#### Step 4: 결과물 업로드

```yaml
- name: Upload artifact
  uses: actions/upload-pages-artifact@v3
  with:
    path: ./public
```

**무슨 일이 일어나나?**
- `public/` 폴더를 다음 단계로 전달

### 4. 배포 작업 (Deploy Job)

```yaml
deploy:
  environment:
    name: github-pages
    url: ${{ steps.deployment.outputs.page_url }}
  runs-on: ubuntu-latest
  needs: build
  steps:
    - name: Deploy to GitHub Pages
      id: deployment
      uses: actions/deploy-pages@v4
```

**무슨 일이 일어나나?**
1. `needs: build`: 빌드 작업이 끝난 후 실행
2. `public/` 폴더를 GitHub Pages에 배포
3. `https://username.github.io`로 접속 가능!

## 실제 배포 과정 따라하기

### 1단계: 저장소 생성

GitHub에서 새 저장소를 만듭니다:
- 이름: `username.github.io` (본인 GitHub ID 사용)
- Public 저장소여야 함 (무료 계정)

### 2단계: 로컬 Hugo 프로젝트와 연결

```bash
# Hugo 프로젝트 폴더에서
git init
git add .
git commit -m "Initial commit"

# GitHub 저장소와 연결
git remote add origin https://github.com/username/username.github.io.git
git branch -M master
git push -u origin master
```

### 3단계: GitHub Actions 워크플로우 생성

`.github/workflows/hugo.yml` 파일을 만듭니다:

```bash
mkdir -p .github/workflows
```

위에서 본 워크플로우 내용을 복사해서 붙여넣기!

### 4단계: GitHub Pages 설정

1. GitHub 저장소 → Settings → Pages
2. Source: **GitHub Actions** 선택
   - ⚠️ "Deploy from a branch"가 아님!
3. 저장

### 5단계: 배포!

```bash
git add .
git commit -m "Add GitHub Actions workflow"
git push origin master
```

### 6단계: 배포 확인

1. GitHub 저장소 → Actions 탭
2. 워크플로우 실행 상태 확인
3. 초록색 체크 표시가 뜨면 성공! ✅

```
Deploy Hugo site to Pages
├── build (1분 소요)
└── deploy (30초 소요)
```

4. `https://username.github.io` 접속!

## 이후 글 작성 워크플로우

이제부터는 **정말 간단**합니다:

```bash
# 1. 새 글 작성
hugo new posts/my-new-post.md

# 2. 글 작성 (마크다운 에디터)
# ...

# 3. 로컬에서 미리보기
hugo server -D

# 4. 확인되면 배포
git add .
git commit -m "새 글: 제목"
git push

# 5. 1~2분 후 자동으로 블로그 업데이트!
```

## 고급 기능

### 커스텀 도메인 연결

GitHub Pages는 **커스텀 도메인**도 지원합니다.

#### 1. 도메인 구매
- 가비아, 호스팅케이알 등에서 구매 (연간 1만원~)

#### 2. DNS 설정
```
A 레코드:
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153

CNAME 레코드:
www → username.github.io
```

#### 3. GitHub에 도메인 추가
- Settings → Pages → Custom domain
- `yourdomain.com` 입력
- HTTPS 자동 적용!

### 빌드 상태 뱃지 추가

README.md에 추가하면 멋있습니다:

```markdown
[![Deploy Status](https://github.com/username/username.github.io/actions/workflows/hugo.yml/badge.svg)](https://github.com/username/username.github.io/actions/workflows/hugo.yml)
```

결과:
![Deploy Status](https://img.shields.io/badge/deploy-passing-brightgreen)

## 트러블슈팅

### 배포가 실패할 때

#### 1. Actions 탭에서 에러 로그 확인
```
build > Build with Hugo > 에러 메시지
```

#### 2. 흔한 에러들

**Hugo 버전 문제**
```
Error: failed to transform resource
```
→ `hugo.yml`의 `HUGO_VERSION`을 최신 버전으로 업데이트

**baseURL 문제**
```
링크가 깨져서 CSS가 안 먹힘
```
→ `hugo.toml`의 `baseURL` 확인

**권한 문제**
```
Permission denied
```
→ Settings → Actions → Workflow permissions → Read and write 권한 설정

### 배포는 성공했는데 404 에러

**원인**: GitHub Pages 설정 문제
**해결**: Settings → Pages → Source가 **GitHub Actions**인지 확인

### 변경사항이 반영 안 됨

**캐시 문제일 수 있음**:
1. Ctrl+Shift+R (강력 새로고침)
2. 시크릿 모드로 접속
3. 5~10분 정도 기다려보기

## 다음 편 예고

이제 블로그는 완성되었습니다! 다음부터는 **기능 추가** 시리즈입니다.

다음 포스트에서는 **Giscus로 댓글 기능**을 추가합니다:
- GitHub Discussions를 활용한 댓글 시스템
- 무료이면서 광고 없는 댓글
- GitHub 계정으로 로그인

## 마치며

GitHub Actions를 사용한 자동 배포는:
- ✅ **설정은 한 번만**: 이후엔 신경 쓸 필요 없음
- ✅ **무료**: GitHub Actions 무료 한도는 매우 넉넉함 (월 2000분)
- ✅ **빠름**: push 후 1~2분이면 배포 완료
- ✅ **안정적**: GitHub의 인프라 사용

처음 설정할 때만 약간 복잡하지만, 한 번 설정하면 **평생 편하게** 블로그를 운영할 수 있습니다!

---

**이전 글:** [1-2] Hugo 블로그 구조와 설정

**다음 글:** [2] Giscus로 블로그에 댓글 기능 추가하기

**시리즈 전체 보기:**
- [1-1] Hugo와 GitHub Pages란?
- [1-2] Hugo 블로그 구조와 설정
- [1-3] GitHub Pages에 배포하기 ← 현재 글
- [2] Giscus 댓글 기능 추가
- [3-1] Netlify와 CMS 이해
- [3-2] Netlify CMS 설정
