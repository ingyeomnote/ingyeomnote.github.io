---
title: "[블로그 만들기 #2] Hugo 블로그 구조 완벽 이해하기 - 폴더별 역할 총정리"
date: 2025-11-04T14:30:00+09:00
description: "Hugo 블로그의 폴더 구조, 설정 파일, 레이아웃 시스템을 실제 예시로 쉽게 설명합니다."
categories: ["블로그 만들기"]
tags: ["Hugo", "블로그 구조", "hugo.toml", "layouts"]
draft: false
---

## 들어가며

이전 편에서 Hugo가 무엇인지 알아봤다면, 이번에는 **실제로 Hugo 블로그가 어떻게 구성되어 있는지** 파헤쳐봅니다.

처음 Hugo 프로젝트 폴더를 열면 이런 생각이 들죠:
- "이 폴더들은 뭐하는 건데?"
- "어디를 수정하면 디자인이 바뀌는 거야?"
- "글은 어디에 쓰는 거지?"

하나씩 알아봅시다!

## Hugo 프로젝트 폴더 구조

제 블로그의 실제 폴더 구조입니다:

```
ingyeomnote.github.io/
├── hugo.toml          # 블로그 전체 설정
├── content/           # 블로그 글들 (마크다운)
│   ├── posts/
│   │   ├── welcome.md
│   │   └── hugo-blog-setup.md
│   └── about.md
├── layouts/           # HTML 템플릿들
│   ├── _default/
│   │   ├── baseof.html
│   │   ├── single.html
│   │   └── list.html
│   └── index.html
├── static/            # 이미지, CSS, JS 등
│   ├── css/
│   ├── images/
│   └── cms/
├── archetypes/        # 새 글 템플릿
└── public/            # Hugo가 생성한 결과물 (배포용)
```

### 각 폴더의 역할

| 폴더/파일 | 역할 | 비유 |
|-----------|------|------|
| `hugo.toml` | 블로그 전체 설정 | 건물 설계도 |
| `content/` | 블로그 글 저장소 | 책 내용 |
| `layouts/` | HTML 템플릿 | 책 표지와 레이아웃 |
| `static/` | 정적 파일 (이미지, CSS) | 책 삽화 |
| `public/` | 최종 생성된 웹사이트 | 완성된 책 |

## hugo.toml - 블로그 설정의 모든 것

### 실제 설정 파일 뜯어보기

제 블로그의 `hugo.toml` 파일입니다:

```toml
baseURL = "https://ingyeomnote.github.io/"
languageCode = "ko-kr"
title = "Ingyeom's Tech Blog"
```

#### 기본 설정 설명

**baseURL**: 블로그 주소
```toml
baseURL = "https://ingyeomnote.github.io/"
```
- 이 주소로 모든 링크가 생성됩니다
- GitHub Pages 사용 시: `https://당신ID.github.io/`
- 커스텀 도메인 사용 시: `https://yourdomain.com/`

**languageCode**: 언어 설정
```toml
languageCode = "ko-kr"
```
- `ko-kr`: 한국어
- `en-us`: 영어
- 날짜 표시 형식, SEO 등에 영향

**title**: 블로그 제목
```toml
title = "Ingyeom's Tech Blog"
```
- 브라우저 탭에 표시되는 제목
- RSS 피드 제목

### 파라미터 섹션 (params)

```toml
[params]
  description = "개발, 프로젝트 및 기술 기록을 위한 개인 기술 블로그"
  author = "Ingyeom"
  ShowReadingTime = true
  ShowCodeCopyButtons = true
```

이런 설정들은 **템플릿에서 사용**됩니다:

```html
<!-- layouts에서 이렇게 사용 -->
<meta name="description" content="{{ .Site.Params.description }}">
<p>작성자: {{ .Site.Params.author }}</p>
```

### 메뉴 설정

```toml
[menu]
  [[menu.main]]
    identifier = "posts"
    name = "Posts"
    url = "/posts/"
    weight = 10

  [[menu.main]]
    identifier = "about"
    name = "About"
    url = "/about/"
    weight = 40
```

이렇게 하면 **상단 메뉴**가 자동으로 생성됩니다:

```
[Posts] [Categories] [Tags] [About]
  ↑         ↑          ↑       ↑
weight=10  weight=20  weight=30  weight=40
```

- `weight`: 순서 (숫자가 작을수록 왼쪽)
- `identifier`: 고유 ID
- `name`: 표시될 이름
- `url`: 클릭 시 이동할 주소

### 마크다운 설정

```toml
[markup]
  [markup.highlight]
    codeFences = true
    lineNos = true
    style = "monokai"
```

코드 블록 스타일을 설정합니다:

```python
# 이렇게 표시됩니다
def hello():
    print("Hello World!")
```

## content/ - 블로그 글 저장소

### 폴더 구조 = URL 구조

```
content/
├── posts/
│   ├── welcome.md          → /posts/welcome/
│   └── my-first-post.md    → /posts/my-first-post/
└── about.md                → /about/
```

**중요**: 폴더 구조가 URL로 그대로 매핑됩니다!

### 마크다운 파일 구조

모든 글은 **Front Matter**(메타 데이터) + **본문**으로 구성됩니다:

```markdown
---
title: "제목"
date: 2025-11-04T14:30:00+09:00
categories: ["카테고리1", "카테고리2"]
tags: ["태그1", "태그2"]
draft: false
---

# 여기서부터 본문 시작

내용을 마크다운으로 작성합니다.
```

#### Front Matter 필드 설명

| 필드 | 의미 | 예시 |
|------|------|------|
| `title` | 글 제목 | "Hugo 시작하기" |
| `date` | 작성 날짜 | 2025-11-04T14:30:00+09:00 |
| `categories` | 카테고리 | ["개발", "블로그"] |
| `tags` | 태그 | ["Hugo", "GitHub"] |
| `draft` | 초안 여부 | true (숨김) / false (공개) |
| `description` | 요약 설명 | SEO에 사용 |

### 새 글 만들기

Hugo 명령어로 쉽게 만들 수 있습니다:

```bash
hugo new posts/my-new-post.md
```

이렇게 하면 `content/posts/my-new-post.md`가 생성되고, `archetypes/default.md` 템플릿이 적용됩니다.

## layouts/ - HTML 템플릿의 세계

### Hugo 템플릿 시스템

Hugo는 **Go 템플릿 언어**를 사용합니다:

```html
<!-- 변수 출력 -->
{{ .Title }}

<!-- 조건문 -->
{{ if .Params.tags }}
  <p>태그가 있습니다</p>
{{ end }}

<!-- 반복문 -->
{{ range .Params.tags }}
  <a href="/tags/{{ . }}">{{ . }}</a>
{{ end }}
```

### 주요 템플릿 파일

#### baseof.html - 기본 레이아웃

모든 페이지의 **공통 구조**를 정의합니다:

```html
<!DOCTYPE html>
<html>
<head>
    <title>{{ .Title }} - {{ .Site.Title }}</title>
    <link rel="stylesheet" href="/css/style.css">
</head>
<body>
    <header>
        <nav>
            <!-- 메뉴 -->
        </nav>
    </header>

    <main>
        {{ block "main" . }}{{ end }}
    </main>

    <footer>
        <p>&copy; 2025 {{ .Site.Params.author }}</p>
    </footer>
</body>
</html>
```

#### single.html - 개별 글 페이지

블로그 포스트 하나를 표시합니다:

```html
{{ define "main" }}
<article>
    <header>
        <h1>{{ .Title }}</h1>
        <time>{{ .Date.Format "2006년 1월 2일" }}</time>
    </header>

    <div class="content">
        {{ .Content }}
    </div>

    <!-- 태그 표시 -->
    {{ if .Params.tags }}
    <div class="tags">
        {{ range .Params.tags }}
        <a href="/tags/{{ . | urlize }}">#{{ . }}</a>
        {{ end }}
    </div>
    {{ end }}
</article>
{{ end }}
```

**여기서 중요한 점**:
- `{{ .Title }}`: 글 제목
- `{{ .Date }}`: 작성 날짜
- `{{ .Content }}`: 마크다운이 HTML로 변환된 본문
- `{{ .Params.tags }}`: Front Matter의 tags 필드

#### list.html - 글 목록 페이지

`/posts/`처럼 여러 글을 나열합니다:

```html
{{ define "main" }}
<h1>{{ .Title }}</h1>

{{ range .Pages }}
<article class="post-card">
    <h2>
        <a href="{{ .RelPermalink }}">{{ .Title }}</a>
    </h2>
    <p>{{ .Summary }}</p>
    <time>{{ .Date.Format "2006-01-02" }}</time>
</article>
{{ end }}
{{ end }}
```

- `{{ range .Pages }}`: 모든 페이지를 순회
- `{{ .RelPermalink }}`: 글 링크
- `{{ .Summary }}`: 자동 생성된 요약

#### index.html - 홈 페이지

블로그 첫 페이지입니다:

```html
{{ define "main" }}
<div class="home">
    <h1>{{ .Site.Params.homeInfoParams.Title }}</h1>
    <p>{{ .Site.Params.homeInfoParams.Content }}</p>

    <h2>최근 글</h2>
    {{ range first 5 .Site.RegularPages }}
    <article>
        <a href="{{ .RelPermalink }}">{{ .Title }}</a>
    </article>
    {{ end }}
</div>
{{ end }}
```

- `{{ .Site.RegularPages }}`: 모든 일반 페이지
- `{{ range first 5 ... }}`: 최근 5개만 표시

## static/ - 정적 파일 저장소

### static 폴더의 특징

`static/` 폴더의 파일들은 **그대로 복사**됩니다:

```
static/
├── css/
│   └── style.css       → /css/style.css
├── images/
│   └── logo.png        → /images/logo.png
└── favicon.ico         → /favicon.ico
```

### 템플릿에서 참조하기

```html
<!-- layouts에서 -->
<link rel="stylesheet" href="/css/style.css">
<img src="/images/logo.png" alt="로고">
```

**중요**: `/`로 시작하면 `static/` 폴더 기준입니다!

## public/ - 최종 결과물

### Hugo 빌드 과정

```bash
hugo
```

이 명령어를 실행하면:

```
content/ + layouts/ + static/
           ↓
        [Hugo]
           ↓
       public/
```

`public/` 폴더에 다음이 생성됩니다:
- 모든 HTML 파일
- 복사된 정적 파일
- RSS 피드
- Sitemap

### 배포는 public 폴더만!

GitHub Pages에는 **public 폴더만 배포**하면 됩니다:

```bash
cd public
git add .
git commit -m "사이트 업데이트"
git push
```

또는 GitHub Actions로 자동화할 수 있습니다 (다음 편에서 다룹니다).

## Hugo가 페이지를 만드는 과정

### 전체 흐름

```
1. 글 작성
   content/posts/my-post.md

2. Hugo 빌드
   hugo 명령어 실행

3. 템플릿 적용
   layouts/_default/single.html

4. HTML 생성
   public/posts/my-post/index.html

5. 배포
   GitHub Pages로 push
```

### 구체적인 예시

**1단계: 마크다운 작성**
```markdown
---
title: "Hugo 시작하기"
date: 2025-11-04
---

# 안녕하세요
이것은 첫 글입니다.
```

**2단계: Hugo가 읽음**
- Front Matter 파싱: title, date 추출
- 본문 파싱: 마크다운 → HTML 변환

**3단계: 템플릿 적용**
```html
<article>
    <h1>Hugo 시작하기</h1>
    <time>2025-11-04</time>
    <div>
        <h1>안녕하세요</h1>
        <p>이것은 첫 글입니다.</p>
    </div>
</article>
```

**4단계: 최종 HTML 생성**
```
public/posts/hugo-시작하기/index.html
```

## 실전 팁

### 1. 로컬에서 실시간 미리보기

```bash
hugo server -D
```

- 브라우저에서 `http://localhost:1313` 접속
- 파일 수정하면 자동으로 새로고침
- `-D`: draft인 글도 표시

### 2. 설정 변경 후 재시작

`hugo.toml` 수정 후:
```bash
# 서버 재시작 (Ctrl+C 후)
hugo server
```

### 3. 빌드 전 정리

```bash
# public 폴더 삭제 후 깨끗하게 빌드
rm -rf public
hugo
```

## 다음 편 예고

다음 포스트에서는 **실제로 GitHub Pages에 배포**하는 방법을 알아봅니다:

- GitHub 저장소 설정
- GitHub Actions로 자동 배포 설정
- 커스텀 도메인 연결 (선택사항)
- 배포 후 확인 방법

## 마치며

Hugo의 구조를 이해하면:
- ✅ 어디를 수정해야 할지 알 수 있습니다
- ✅ 에러가 나도 원인을 파악할 수 있습니다
- ✅ 원하는 대로 커스터마이징할 수 있습니다

처음엔 복잡해 보이지만, 실제로는 매우 논리적이고 직관적인 구조입니다!

---

**이전 글:** [1-1] Hugo와 GitHub Pages란?

**다음 글:** [1-3] GitHub Pages에 배포하기

**시리즈 전체 보기:**
- [1-1] Hugo와 GitHub Pages란?
- [1-2] Hugo 블로그 구조와 설정 ← 현재 글
- [1-3] GitHub Pages에 배포하기
- [2] Giscus 댓글 기능 추가
- [3-1] Netlify와 CMS 이해
- [3-2] Netlify CMS 설정
