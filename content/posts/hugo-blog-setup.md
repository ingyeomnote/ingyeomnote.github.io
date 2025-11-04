---
title: "Hugo로 GitHub Pages 블로그 만들기"
date: 2025-11-04T01:00:00+09:00
draft: false
tags: ["hugo", "github-pages", "블로그", "튜토리얼"]
categories: ["개발"]
description: "Hugo와 GitHub Pages를 사용하여 정적 블로그를 구축하는 방법을 알아봅니다."
---

## Hugo란?

Hugo는 Go 언어로 작성된 정적 사이트 생성기(Static Site Generator)입니다.
빠른 빌드 속도와 간편한 사용법으로 개발자들에게 인기가 많습니다.

## Hugo의 장점

1. **빠른 빌드 속도**: Go 언어 기반으로 매우 빠른 빌드 성능
2. **간편한 설치**: 단일 바이너리 파일로 설치 가능
3. **풍부한 테마**: 다양한 무료 테마 제공
4. **마크다운 지원**: 마크다운으로 쉽게 콘텐츠 작성
5. **활발한 커뮤니티**: 지속적인 업데이트와 문서화

## Hugo 프로젝트 구조

```
.
├── archetypes/     # 콘텐츠 템플릿
├── content/        # 블로그 포스트 및 페이지
├── data/           # 데이터 파일
├── layouts/        # HTML 템플릿
├── static/         # 정적 파일 (이미지, CSS, JS 등)
├── themes/         # 테마
└── hugo.toml       # 설정 파일
```

## PaperMod 테마

PaperMod는 깔끔하고 빠른 Hugo 테마로, 기술 블로그에 적합합니다.

### 주요 기능

- 반응형 디자인
- 다크/라이트 모드 지원
- 코드 하이라이팅
- 검색 기능
- 소셜 미디어 아이콘
- SEO 최적화

## GitHub Actions로 자동 배포

GitHub Actions를 사용하면 푸시할 때마다 자동으로 빌드하고 배포할 수 있습니다.

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: recursive
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true
      - name: Build
        run: hugo --minify
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

## 마치며

Hugo와 GitHub Pages를 조합하면 무료로 빠르고 안정적인 블로그를 운영할 수 있습니다.
지속적으로 좋은 콘텐츠를 작성해 나가겠습니다!
