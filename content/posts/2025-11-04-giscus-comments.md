---
title: "[블로그 만들기 #4] Giscus로 무료 댓글 기능 추가하기 - GitHub Discussions 활용"
date: 2025-11-04T15:30:00+09:00
description: "GitHub Discussions 기반의 무료 댓글 시스템 Giscus를 Hugo 블로그에 연동하는 방법을 단계별로 설명합니다."
categories: ["블로그 만들기"]
tags: ["Giscus", "댓글", "GitHub Discussions", "Hugo"]
draft: false
---

## 들어가며

정적 사이트의 단점 중 하나는 **댓글 기능이 기본으로 없다**는 것입니다.

하지만 Giscus를 사용하면:
- ✅ 완전 무료
- ✅ 광고 없음
- ✅ GitHub 계정으로 로그인
- ✅ 마크다운 지원
- ✅ 반응(👍, ❤️ 등) 지원

이런 댓글 시스템을 **5분 만에** 추가할 수 있습니다!

## 댓글 시스템 비교

### 기존 솔루션들

| 서비스 | 비용 | 광고 | 단점 |
|--------|------|------|------|
| **Disqus** | 무료/유료 | ✅ (무료 버전) | 느리고 무거움 |
| **Facebook Comments** | 무료 | ❌ | Facebook 필수 |
| **Utterances** | 무료 | ❌ | GitHub Issues 사용 |
| **Giscus** | 무료 | ❌ | GitHub 계정 필요 |

### 왜 Giscus인가?

Giscus는 **Utterances의 후속작**으로:
- GitHub Issues 대신 → **GitHub Discussions** 사용
- 더 많은 기능: 반응, 답글, 카테고리
- 개발이 활발함

## Giscus란?

### GitHub Discussions 기반 댓글

**실생활 비유**:
```
[일반 댓글 시스템]
블로그 ← 별도 댓글 데이터베이스
            (서버 비용 발생)

[Giscus]
블로그 ← GitHub Discussions
            (GitHub이 무료 제공!)
```

### 작동 원리

```
1. 사용자가 블로그 글에 댓글 작성
   ↓
2. Giscus가 GitHub Discussions에 자동으로 토론 생성
   ↓
3. 댓글이 Discussions에 저장됨
   ↓
4. 블로그에서 Discussions의 댓글을 불러와 표시
```

**핵심**: 댓글 데이터는 **GitHub 저장소의 Discussions**에 저장됩니다!

### GitHub Discussions란?

GitHub에서 제공하는 **커뮤니티 토론 기능**입니다.

```
저장소 탭 구조:
[Code] [Issues] [Pull Requests] [Discussions] ← 여기!
```

**Issues vs Discussions**:

| | Issues | Discussions |
|---|--------|-------------|
| 용도 | 버그, 작업 관리 | 질문, 토론, 공지 |
| 구조 | 단순 스레드 | 카테고리 분류 |
| 댓글용 | Utterances | Giscus |

## Giscus 설정 단계별 가이드

### 1단계: GitHub Discussions 활성화

#### 1-1. 저장소 Settings로 이동
```
GitHub 저장소 → Settings → General
```

#### 1-2. Features 섹션에서 Discussions 체크
```
☐ Wikis
☐ Issues
☑ Discussions  ← 체크!
```

#### 1-3. 확인
저장소 탭에 **Discussions**가 생겼습니다!

```
[Code] [Issues] [Pull requests] [Discussions] ← 추가됨!
```

### 2단계: Giscus 앱 설치

#### 2-1. Giscus 사이트 방문
https://giscus.app/ko

#### 2-2. "giscus 설치" 클릭
페이지 중간쯤에 있는 링크를 클릭하면 GitHub으로 이동합니다.

#### 2-3. 저장소 선택
```
☑ Only select repositories
   └─ username/username.github.io  ← 선택
```

**또는**:
```
☑ All repositories  ← 모든 저장소에 적용
```

#### 2-4. Install 클릭
권한을 확인하고 설치합니다.

### 3단계: Giscus 설정

다시 https://giscus.app/ko로 돌아갑니다.

#### 3-1. 저장소 입력
```
저장소:
username/username.github.io

✅ 성공! 이 저장소는 모든 조건을 만족합니다.
```

**조건**:
- ✅ Public 저장소
- ✅ giscus 앱 설치됨
- ✅ Discussions 기능 활성화됨

#### 3-2. Discussion 카테고리 선택
```
카테고리:
└─ General (추천)
   또는
└─ Announcements (공지용)
```

**추천**: General 카테고리 사용
- 모든 사용자가 댓글 작성 가능
- Announcements는 관리자만 토론 생성 가능

#### 3-3. 페이지 ↔️ Discussions 매핑

```
매핑 방식:
☑ pathname (추천)
```

**매핑 방식 설명**:

| 방식 | 설명 | 예시 |
|------|------|------|
| pathname | URL 경로로 매핑 | `/posts/my-post/` |
| URL | 전체 URL로 매핑 | `https://...` |
| title | 페이지 제목으로 매핑 | "내 첫 글" |

**pathname을 추천하는 이유**:
- URL이 바뀌어도 괜찮음 (도메인 변경 시)
- 제목 변경해도 댓글 유지됨

#### 3-4. 기능 설정

```
☑ Discussion 제목이 검색 결과에 포함되도록 <title> 태그 포함

☑ 반응 활성화
   └─ 댓글에 👍, ❤️, 🎉 등 반응 가능

입력창 위치:
☑ 댓글 상단에 배치 (추천)
```

#### 3-5. 테마 선택

```
테마:
└─ light (밝은 테마)
└─ dark (어두운 테마)
└─ preferred_color_scheme (사용자 설정 따름)
```

**추천**: `light` 또는 블로그 디자인에 맞춰 선택

### 4단계: 생성된 코드 복사

설정이 끝나면 페이지 하단에 **스크립트 코드**가 생성됩니다:

```html
<script src="https://giscus.app/client.js"
        data-repo="username/username.github.io"
        data-repo-id="R_kgDOxxxxxxxx"
        data-category="General"
        data-category-id="DIC_kwDOxxxxxxxx"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="top"
        data-theme="light"
        data-lang="ko"
        data-loading="lazy"
        crossorigin="anonymous"
        async>
</script>
```

**중요**: 이 코드를 복사해둡니다!

## Hugo 블로그에 통합하기

### 방법 1: single.html에 직접 추가 (제가 사용한 방법)

#### 1. single.html 파일 열기
```bash
layouts/_default/single.html
```

#### 2. 본문 아래에 댓글 섹션 추가

제 블로그의 실제 코드입니다:

```html
{{ define "main" }}
<article>
    <header>
        <h1>{{ .Title }}</h1>
        <div class="post-meta">
            <time>{{ .Date.Format "2006년 1월 2일" }}</time>
        </div>
    </header>

    <div class="content">
        {{ .Content }}
    </div>

    <!-- 여기부터 댓글 섹션 -->
    <div class="comments-section">
        <h3>댓글</h3>
        <script src="https://giscus.app/client.js"
                data-repo="ingyeomnote/ingyeomnote.github.io"
                data-repo-id="R_kgDOQOxxvA"
                data-category="General"
                data-category-id="DIC_kwDOQOxxvM4Cxa1M"
                data-mapping="pathname"
                data-strict="0"
                data-reactions-enabled="1"
                data-emit-metadata="0"
                data-input-position="top"
                data-theme="light"
                data-lang="ko"
                data-loading="lazy"
                crossorigin="anonymous"
                async>
        </script>
    </div>
</article>
{{ end }}
```

#### 3. CSS 스타일 추가 (선택사항)

```css
.comments-section {
    margin-top: 4rem;
    padding-top: 2rem;
    border-top: 1px solid #e0e0e0;
}

.comments-section h3 {
    margin-bottom: 1.5rem;
    font-size: 1.5rem;
}
```

### 방법 2: Partial 템플릿 사용 (더 깔끔)

#### 1. Partial 파일 생성
```bash
layouts/partials/comments.html
```

#### 2. Giscus 스크립트 작성
```html
<!-- layouts/partials/comments.html -->
<div class="comments-section">
    <h3>댓글</h3>
    <script src="https://giscus.app/client.js"
            data-repo="{{ .Site.Params.giscus.repo }}"
            data-repo-id="{{ .Site.Params.giscus.repoId }}"
            data-category="{{ .Site.Params.giscus.category }}"
            data-category-id="{{ .Site.Params.giscus.categoryId }}"
            data-mapping="pathname"
            data-strict="0"
            data-reactions-enabled="1"
            data-emit-metadata="0"
            data-input-position="top"
            data-theme="{{ .Site.Params.giscus.theme | default "light" }}"
            data-lang="ko"
            data-loading="lazy"
            crossorigin="anonymous"
            async>
    </script>
</div>
```

#### 3. hugo.toml에 설정 추가
```toml
[params.giscus]
  repo = "username/username.github.io"
  repoId = "R_kgDOxxxxxxxx"
  category = "General"
  categoryId = "DIC_kwDOxxxxxxxx"
  theme = "light"
```

#### 4. single.html에서 불러오기
```html
{{ define "main" }}
<article>
    <!-- ... 글 내용 ... -->
    {{ .Content }}

    {{ partial "comments.html" . }}
</article>
{{ end }}
```

**장점**:
- 설정을 한 곳에서 관리
- 여러 템플릿에서 재사용 가능
- 수정이 쉬움

## 배포 및 확인

### 1. 로컬에서 테스트

```bash
hugo server -D
```

브라우저에서 `http://localhost:1313` 접속 → 글 페이지에서 댓글 위젯 확인

**주의**: 로컬에서는 실제 댓글 작성이 안 될 수 있습니다 (localhost 주소라서).

### 2. GitHub에 배포

```bash
git add .
git commit -m "feat: Add Giscus comments"
git push origin master
```

### 3. 실제 블로그에서 확인

`https://username.github.io`에서 글을 열고:
1. 댓글 위젯이 로드되는지 확인
2. "GitHub로 로그인" 버튼이 보이는지 확인

### 4. 테스트 댓글 작성

1. "GitHub로 로그인" 클릭
2. 권한 허용
3. 테스트 댓글 작성
4. GitHub 저장소 → Discussions → General 확인
   → 자동으로 토론이 생성되었습니다!

## Giscus의 멋진 기능들

### 1. 마크다운 지원

댓글에 **마크다운**을 사용할 수 있습니다:

```markdown
**굵게**
*기울임*
`코드`
[링크](https://example.com)
```

### 2. 코드 블록

````markdown
```python
def hello():
    print("Hello!")
```
````

### 3. 반응 (Reactions)

댓글에 이모지로 반응할 수 있습니다:
👍 ❤️ 😄 🎉 😕 🚀 👀

### 4. 답글 (Replies)

댓글에 답글을 달 수 있습니다:
```
├─ 댓글 1
│  ├─ 답글 1-1
│  └─ 답글 1-2
└─ 댓글 2
```

### 5. 알림

GitHub 알림으로 댓글 알림을 받을 수 있습니다!

## 트러블슈팅

### 댓글 위젯이 안 보여요

#### 1. 콘솔 에러 확인
브라우저 F12 → Console 탭에서 에러 확인

#### 2. 저장소 설정 확인
- Public 저장소인가?
- Discussions 활성화되었나?
- giscus 앱 설치되었나?

#### 3. repo-id 확인
```html
data-repo="username/username.github.io"  ← 정확한가?
data-repo-id="R_kgDOxxxxxxxx"  ← 정확한가?
```

### "Error: Not Found" 메시지

**원인**: 저장소 이름이나 ID가 틀림
**해결**: https://giscus.app/ko에서 다시 설정 생성

### 댓글 작성이 안 돼요

**원인 1**: Discussions 카테고리 권한 문제
- Announcements 카테고리 사용 중이라면 → General로 변경

**원인 2**: giscus 앱 권한 문제
- GitHub Settings → Applications → giscus → 권한 확인

### 댓글이 다른 글에도 보여요

**원인**: data-mapping 설정 문제
**해결**: `data-mapping="pathname"` 확인

## 고급 활용

### 테마를 블로그와 연동

다크 모드 토글이 있다면:

```javascript
// 테마 변경 시 Giscus 테마도 변경
function setGiscusTheme(theme) {
    const iframe = document.querySelector('iframe.giscus-frame');
    if (iframe) {
        iframe.contentWindow.postMessage(
            { giscus: { setConfig: { theme } } },
            'https://giscus.app'
        );
    }
}

// 다크 모드 버튼 클릭 시
darkModeToggle.addEventListener('click', () => {
    const newTheme = isDark ? 'light' : 'dark';
    setGiscusTheme(newTheme);
});
```

### 댓글 수 표시

글 목록에 댓글 수를 표시할 수 있습니다:

```html
<a href="{{ .RelPermalink }}">
    {{ .Title }}
    <span class="comment-count" data-url="{{ .Permalink }}"></span>
</a>
```

```javascript
// Giscus API로 댓글 수 가져오기
// (자세한 내용은 Giscus 문서 참조)
```

## 다음 편 예고

다음 포스트에서는 **Netlify와 Netlify CMS**를 알아봅니다:
- Netlify가 뭔지
- Netlify로 어떻게 블로그에 접근하는지
- Netlify CMS로 웹에서 글 작성/수정하기

## 마치며

Giscus는 정말 훌륭한 댓글 솔루션입니다:
- ✅ 완전 무료
- ✅ 광고 없음
- ✅ GitHub이라는 안정적인 플랫폼 사용
- ✅ 개발자 친화적

특히 기술 블로그라면 **GitHub 계정으로 로그인**하는 것이 오히려 장점입니다!

블로그에 댓글 기능을 추가해서 독자들과 소통해보세요! 🎉

---

**이전 글:** [1-3] GitHub Pages에 배포하기

**다음 글:** [3-1] Netlify와 CMS의 이해

**시리즈 전체 보기:**
- [1-1] Hugo와 GitHub Pages란?
- [1-2] Hugo 블로그 구조와 설정
- [1-3] GitHub Pages에 배포하기
- [2] Giscus 댓글 기능 추가 ← 현재 글
- [3-1] Netlify와 CMS 이해
- [3-2] Netlify CMS 설정
