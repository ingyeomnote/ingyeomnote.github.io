---
title: "[블로그 만들기 #6] Netlify CMS 실전 설정 - 웹에서 블로그 글 작성하기"
date: 2025-11-04T16:30:00+09:00
description: "Netlify CMS (Decap CMS)를 Hugo 블로그에 연동하고, 웹 브라우저에서 워드프레스처럼 글을 작성/수정/삭제하는 방법을 실제 코드로 설명합니다."
categories: ["블로그 만들기"]
tags: ["Netlify CMS", "Decap CMS", "Netlify Identity", "Git Gateway"]
draft: false
---

## 들어가며

이제 드디어 **마지막 단계**입니다!

Netlify CMS를 설정하면:
- ✅ 웹 브라우저에서 글 작성 (워드프레스처럼!)
- ✅ 터미널, Git 명령어 없이 글 발행
- ✅ 이미지 업로드도 웹에서
- ✅ 스마트폰에서도 글 작성 가능

이번 편에서 실제로 설정해봅시다!

## Netlify CMS vs Decap CMS

### 이름이 두 개?

- **Netlify CMS**: 원래 이름 (2017~2023)
- **Decap CMS**: 새 이름 (2023~현재)

2023년에 **Netlify에서 독립**하면서 이름을 Decap CMS로 변경했습니다.

**현재 상황**:
- 공식 이름: Decap CMS
- CDN: `decap-cms@^3.0.0`
- 하지만 많은 문서가 여전히 "Netlify CMS"로 검색됨

**이 글에서는**: 편의상 "Netlify CMS"로 부르겠습니다.

## 전체 설정 과정 개요

```
1. Netlify에 사이트 연결
2. Netlify Identity 활성화
3. Git Gateway 활성화
4. CMS 파일들 추가
   - static/cms/index.html
   - static/cms/config.yml
   - static/cms/oauth.html
5. 배포 및 관리자 등록
6. 웹에서 글 작성!
```

## 1단계: Netlify 사이트 연결

### 1-1. Netlify 계정 만들기

1. https://netlify.com 접속
2. "Sign up" 클릭
3. **"Continue with GitHub"** 선택 (중요!)
4. GitHub 권한 승인

### 1-2. 사이트 추가

1. Netlify 대시보드에서 **"Add new site"** 클릭
2. **"Import an existing project"** 선택
3. **"GitHub"** 클릭
4. 저장소 목록에서 `username.github.io` 선택

### 1-3. 빌드 설정

```
Build command: hugo
Publish directory: public
```

**Hugo 버전 지정** (선택사항):
```
Environment variables:
HUGO_VERSION = 0.121.1
```

### 1-4. 배포

**"Deploy site"** 클릭!

```
[빌드 로그]
1. Cloning repository
2. Installing Hugo
3. Building site
4. Deploy success!
```

기본 URL이 생성됩니다:
```
https://random-name-123.netlify.app
```

### 1-5. 사이트 이름 변경 (선택)

1. Site settings → General → Site details
2. **"Change site name"** 클릭
3. 원하는 이름 입력 (예: `ingyeomnote`)
4. URL이 변경됩니다:
   ```
   https://ingyeomnote.netlify.app
   ```

## 2단계: Netlify Identity 활성화

### 2-1. Identity 켜기

1. Netlify 대시보드 → 해당 사이트 선택
2. **"Site settings"** → **"Identity"**
3. **"Enable Identity"** 클릭

### 2-2. Registration 설정

```
Settings → Identity → Registration

Registration preferences:
☑ Invite only (추천)
```

**Invite only vs Open**:

| | Invite only | Open |
|---|-------------|------|
| 가입 | 초대 받은 사람만 | 누구나 |
| 보안 | ✅ 안전 | ⚠️ 스팸 위험 |
| 용도 | 개인 블로그 | 커뮤니티 |

**추천**: Invite only (당신만 글 작성 가능)

### 2-3. External Providers (선택)

```
Settings → Identity → External providers

☑ GitHub
☐ Google
☐ GitLab
☐ Bitbucket
```

GitHub 로그인을 활성화하면 편합니다.

## 3단계: Git Gateway 활성화

### Git Gateway란?

CMS가 GitHub 저장소에 안전하게 접근할 수 있게 해주는 다리입니다.

```
[CMS] → [Git Gateway] → [GitHub 저장소]
           ↑
    (권한 확인 & 인증)
```

### 활성화 방법

1. Site settings → Identity → Services
2. **"Git Gateway"** 섹션 찾기
3. **"Enable Git Gateway"** 클릭

자동으로 GitHub 저장소와 연결됩니다!

## 4단계: CMS 파일 추가

### 폴더 구조 생성

```bash
mkdir -p static/cms
```

최종 구조:
```
static/
└── cms/
    ├── index.html    # CMS 메인 페이지
    ├── config.yml    # CMS 설정 파일
    └── oauth.html    # 인증 콜백 페이지
```

### 4-1. index.html 생성

`static/cms/index.html` 파일을 만듭니다:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Content Manager</title>
  <!-- Netlify Identity Widget -->
  <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
</head>
<body>
  <!-- Decap CMS -->
  <script src="https://unpkg.com/decap-cms@^3.0.0/dist/decap-cms.js"></script>
</body>
</html>
```

**설명**:
- `netlify-identity-widget.js`: 로그인 기능
- `decap-cms.js`: CMS 메인 스크립트

### 4-2. config.yml 생성

`static/cms/config.yml` 파일을 만듭니다.

제 블로그의 실제 설정입니다:

```yaml
backend:
  name: git-gateway
  branch: master

media_folder: "static/images/uploads"
public_folder: "/images/uploads"

locale: 'ko'

collections:
  - name: "posts"
    label: "Posts"
    label_singular: "Post"
    folder: "content/posts"
    create: true
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
    fields:
      - {label: "제목", name: "title", widget: "string"}
      - {label: "발행일", name: "date", widget: "datetime", format: "YYYY-MM-DDTHH:mm:ssZ"}
      - {label: "설명", name: "description", widget: "string", required: false}
      - {label: "카테고리", name: "categories", widget: "list", required: false}
      - {label: "태그", name: "tags", widget: "list", required: false}
      - {label: "Draft", name: "draft", widget: "boolean", default: false}
      - {label: "내용", name: "body", widget: "markdown"}

  - name: "pages"
    label: "Pages"
    label_singular: "Page"
    files:
      - label: "About"
        name: "about"
        file: "content/about.md"
        fields:
          - {label: "제목", name: "title", widget: "string"}
          - {label: "내용", name: "body", widget: "markdown"}
```

#### 설정 항목 자세히 설명

##### backend
```yaml
backend:
  name: git-gateway
  branch: master
```
- `name: git-gateway`: Git Gateway 사용 (Netlify Identity 연동)
- `branch: master`: master 브랜치에 커밋

##### media_folder & public_folder
```yaml
media_folder: "static/images/uploads"
public_folder: "/images/uploads"
```
- `media_folder`: 이미지가 저장될 실제 경로
- `public_folder`: HTML에서 참조할 경로

**예시**:
```markdown
![이미지](http://images/uploads/photo.jpg)
```
↓ Hugo 빌드 후
```html
<img src="/images/uploads/photo.jpg">
```

##### locale
```yaml
locale: 'ko'
```
CMS 인터페이스 언어를 한국어로 설정

##### collections - posts
```yaml
collections:
  - name: "posts"
    label: "Posts"
    folder: "content/posts"
    create: true
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
```

**필드 설명**:
| 필드 | 의미 | 예시 |
|------|------|------|
| `name` | 내부 식별자 | posts |
| `label` | CMS에 표시될 이름 | Posts |
| `folder` | 파일이 저장될 폴더 | content/posts |
| `create` | 새 글 작성 가능 여부 | true |
| `slug` | 파일 이름 패턴 | 2025-11-04-제목 |

##### fields
```yaml
fields:
  - {label: "제목", name: "title", widget: "string"}
  - {label: "발행일", name: "date", widget: "datetime"}
  - {label: "내용", name: "body", widget: "markdown"}
```

**위젯 종류**:
| widget | 설명 | 예시 |
|--------|------|------|
| `string` | 한 줄 텍스트 | 제목 입력 |
| `text` | 여러 줄 텍스트 | 짧은 설명 |
| `markdown` | 마크다운 에디터 | 본문 작성 |
| `datetime` | 날짜/시간 선택 | 2025-11-04 14:30 |
| `boolean` | 체크박스 | draft: true/false |
| `list` | 배열 입력 | tags: [tag1, tag2] |
| `image` | 이미지 업로드 | 대표 이미지 |

### 4-3. oauth.html 생성

`static/cms/oauth.html` 파일을 만듭니다:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Authorizing...</title>
  <style>
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
      background: #f8f9fa;
    }
    .container {
      text-align: center;
      padding: 2rem;
      background: white;
      border-radius: 8px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }
    .spinner {
      border: 3px solid #f3f3f3;
      border-top: 3px solid #12b886;
      border-radius: 50%;
      width: 40px;
      height: 40px;
      animation: spin 1s linear infinite;
      margin: 0 auto 1rem;
    }
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="spinner"></div>
    <h2>Authorizing...</h2>
    <p>인증 처리 중입니다...</p>
  </div>
  <script>
    (function() {
      function getParameterByName(name) {
        name = name.replace(/[\[]/, "\\[").replace(/[\]]/, "\\]");
        var regex = new RegExp("[\\?&]" + name + "=([^&#]*)"),
            results = regex.exec(location.search);
        return results === null ? "" : decodeURIComponent(results[1].replace(/\+/g, " "));
      }

      var code = getParameterByName('code');
      var provider = getParameterByName('provider');

      if (window.opener) {
        window.opener.postMessage(
          'authorization:' + provider + ':success:{"code":"' + code + '"}',
          window.location.origin
        );
        window.close();
      }
    })();
  </script>
</body>
</html>
```

**역할**: GitHub 로그인 후 인증 코드를 CMS로 전달

## 5단계: 배포 및 관리자 초대

### 5-1. Git으로 배포

```bash
git add static/cms/
git commit -m "feat: Add Netlify CMS"
git push origin master
```

Netlify가 자동으로 빌드 & 배포합니다!

### 5-2. 관리자 초대 (나 자신)

1. Netlify 대시보드 → Site settings → Identity
2. **"Invite users"** 클릭
3. **당신의 이메일** 입력
4. **"Send"** 클릭

### 5-3. 초대 수락

1. 이메일 확인 (Netlify에서 온 초대 메일)
2. **"Accept the invite"** 클릭
3. 비밀번호 설정
4. 완료!

## 6단계: CMS 접속 및 사용

### 6-1. CMS 접속

브라우저에서 접속:
```
https://yoursite.netlify.app/cms/
```

또는 (GitHub Pages):
```
https://username.github.io/cms/
```

### 6-2. 로그인

1. **"Login with Netlify Identity"** 클릭
2. 이메일과 비밀번호 입력
3. CMS 대시보드 진입!

### 6-3. CMS 인터페이스

```
┌─────────────────────────────────────┐
│  [Posts]  [Pages]                   │
├─────────────────────────────────────┤
│                                     │
│  📝 Posts (3)                       │
│  ├─ Hugo와 GitHub Pages란?         │
│  ├─ Hugo 블로그 구조               │
│  └─ GitHub Pages 배포              │
│                                     │
│  [New Post]                         │
│                                     │
└─────────────────────────────────────┘
```

### 6-4. 새 글 작성하기

#### 1. "New Post" 클릭

#### 2. 필드 입력
```
제목: 내 첫 CMS 글
발행일: 2025-11-04 16:30
설명: CMS로 작성한 첫 글입니다
카테고리: [테스트]
태그: [CMS, Netlify]
Draft: ☐ (체크 해제 = 공개)
```

#### 3. 본문 작성

마크다운 에디터가 나타납니다:
- 좌측: 마크다운 작성
- 우측: 실시간 미리보기

```markdown
# 안녕하세요!

CMS로 작성한 **첫 글**입니다.

## 기능 테스트

- 리스트
- **굵게**
- *기울임*

```python
def hello():
    print("Hello!")
```
```

#### 4. 이미지 추가

1. 본문에서 **"+"** 버튼 → **"Image"** 클릭
2. 파일 선택 또는 드래그 앤 드롭
3. 자동으로 `static/images/uploads/`에 저장됨
4. 마크다운에 자동 삽입:
   ```markdown
   ![이미지 설명](http://images/uploads/photo.jpg)
   ```

#### 5. 발행

1. 상단 **"Publish"** 클릭
2. **"Publish now"** 확인

**내부 동작**:
```
[CMS]
"Publish" 클릭
     ↓
[Git Gateway]
GitHub API 호출
     ↓
[GitHub 저장소]
새 파일 커밋:
content/posts/2025-11-04-내-첫-cms-글.md
     ↓
[Netlify 자동 빌드]
     ↓
블로그 업데이트!
```

### 6-5. 기존 글 수정

1. Posts 목록에서 글 클릭
2. 내용 수정
3. **"Publish"** → **"Publish changes"**

GitHub에 자동으로 커밋됩니다!

### 6-6. 글 삭제

1. 글 열기
2. 우측 상단 **"Delete"** 클릭
3. 확인

파일이 GitHub 저장소에서 삭제됩니다.

## CMS 워크플로우

### Editorial Workflow (선택사항)

config.yml에 추가:
```yaml
publish_mode: editorial_workflow
```

이렇게 하면 **초안 → 검토 → 발행** 단계를 거칠 수 있습니다:

```
[Drafts]       → [In Review]     → [Ready]
초안 작성        검토 중            발행 준비

각 단계는 GitHub의 다른 브랜치를 사용합니다!
```

**용도**: 여러 명이 협업할 때 유용

## 트러블슈팅

### "Failed to load config.yml"

**원인**: config.yml 문법 오류
**해결**:
1. YAML Linter로 검증: https://www.yamllint.com/
2. 들여쓰기 확인 (탭이 아닌 스페이스 2칸)

### "Error loading the CMS configuration"

**원인**: 파일 경로 오류
**확인**:
```
static/cms/index.html  ← 경로 확인
static/cms/config.yml  ← 경로 확인
```

### 로그인 버튼이 안 보임

**원인**: Netlify Identity 미활성화
**해결**: Site settings → Identity → Enable Identity

### "Not Found" 또는 빈 페이지

**원인 1**: `/cms` 접근 시 404
**해결**: `/cms/` (슬래시 추가) 로 접속

**원인 2**: 배포 안 됨
**해결**: `https://yoursite.netlify.app/cms/` 먼저 테스트

### Git Gateway 연결 오류

**원인**: Git Gateway 미활성화 또는 권한 문제
**해결**:
1. Site settings → Identity → Services → Git Gateway 확인
2. GitHub 저장소가 Public인지 확인
3. Netlify가 GitHub 저장소 접근 권한이 있는지 확인

## 모바일에서 사용하기

### 스마트폰으로 글 작성

1. 모바일 브라우저에서 `https://yoursite.netlify.app/cms/` 접속
2. 로그인
3. 글 작성
4. 발행!

**주의**: 모바일 화면은 좁아서 불편할 수 있습니다.

**추천**: 태블릿이나 노트북 사용

## 고급 설정

### 위젯 커스터마이징

#### 대표 이미지 추가
```yaml
fields:
  - {label: "대표 이미지", name: "image", widget: "image"}
```

#### Select 드롭다운
```yaml
- label: "카테고리"
  name: "category"
  widget: "select"
  options: ["개발", "디자인", "일상"]
```

#### Relation (다른 컬렉션 참조)
```yaml
- label: "작성자"
  name: "author"
  widget: "relation"
  collection: "authors"
  search_fields: ["name"]
  value_field: "name"
```

### 커스텀 미리보기

`static/cms/index.html`에 추가:
```html
<script>
  CMS.registerPreviewStyle("/css/style.css");
  CMS.registerPreviewTemplate("posts", PostPreview);
</script>
```

미리보기 스타일을 블로그와 동일하게 만들 수 있습니다.

## 마치며

축하합니다! 🎉

이제 당신의 블로그는:
- ✅ Hugo로 빠르게 빌드
- ✅ GitHub Pages로 무료 호스팅
- ✅ Netlify로 빠른 배포
- ✅ Giscus로 댓글 기능
- ✅ CMS로 웹에서 글 작성

**완전체**가 되었습니다!

### 블로그 글 작성 방법 2가지

**방법 1: 로컬에서 (개발자 스타일)**
```bash
hugo new posts/my-post.md
vim content/posts/my-post.md
git push
```

**방법 2: CMS에서 (워드프레스 스타일)**
```
1. https://yoursite.netlify.app/cms/ 접속
2. 로그인
3. New Post
4. 작성 후 Publish
```

**둘 다 사용 가능**하니 상황에 맞게 선택하세요!

### 다음 단계

이제 블로그 시스템은 완성되었으니:
- 🎨 디자인 커스터마이징
- 📊 Google Analytics 연동
- 🔍 검색 엔진 최적화 (SEO)
- 📱 소셜 미디어 공유 기능
- 💬 다른 댓글 시스템 비교

등으로 블로그를 더 발전시킬 수 있습니다!

---

**이전 글:** [[3-1] Netlify와 CMS 이해](/posts/2025-11-04-netlify-cms-1-intro/)

**시리즈 전체 보기:**
- [[1-1] Hugo와 GitHub Pages란?](/posts/2025-11-04-hugo-github-pages-1-intro/)
- [[1-2] Hugo 블로그 구조와 설정](/posts/2025-11-04-hugo-github-pages-2-structure/)
- [[1-3] GitHub Pages에 배포하기](/posts/2025-11-04-hugo-github-pages-3-deployment/)
- [[2] Giscus 댓글 기능 추가](/posts/2025-11-04-giscus-comments/)
- [[3-1] Netlify와 CMS 이해](/posts/2025-11-04-netlify-cms-1-intro/)
- **[3-2] Netlify CMS 설정** ← 현재 글 (완결!)

## 시리즈 완료! 🎊

6편의 시리즈를 통해 Hugo 블로그를 처음부터 끝까지 만들어봤습니다.

**이제 당신은**:
- ✅ Hugo와 GitHub Pages의 원리 이해
- ✅ 블로그 구조 파악
- ✅ 자동 배포 설정
- ✅ 댓글 기능 추가
- ✅ CMS로 편하게 글 작성

**모든 것을 할 수 있습니다!**

행복한 블로깅 되세요! 📝✨
