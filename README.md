# Minimal Mistakes Jekyll 테마 GitHub Pages 제작 가이드

## 목차

1. [기본 디렉토리 구조](#1-기본-디렉토리-구조)
2. [이미지 업로드 및 참조](#2-이미지-업로드-및-참조)
3. [카테고리 만들기](#3-카테고리-만들기)
4. [태그 만들기](#4-태그-만들기)
5. [포스트 작성하기](#5-포스트-작성하기)
6. [페이지 작성하기](#6-페이지-작성하기)
7. [_config.yml 핵심 설정](#7-_configyml-핵심-설정)
8. [내비게이션 메뉴 설정](#8-내비게이션-메뉴-설정)
9. [사이드바 설정](#9-사이드바-설정)
10. [자주 쓰는 Front Matter 옵션](#10-자주-쓰는-front-matter-옵션)

---

## 1. 기본 디렉토리 구조

```
my-github-pages/
├── _config.yml          # 사이트 전역 설정
├── _data/
│   └── navigation.yml   # 상단 내비게이션 메뉴
├── _pages/              # 고정 페이지 (About, Category, Tag 등)
│   ├── about.md
│   ├── category-archive.md
│   └── tag-archive.md
├── _posts/              # 블로그 포스트
│   └── 2024-01-01-my-first-post.md
├── assets/
│   └── images/          # 이미지 저장 위치
│       ├── profile.jpg
│       └── posts/
│           └── my-post-thumbnail.jpg
└── index.html           # 홈 페이지
```

---

## 2. 이미지 업로드 및 참조

### 이미지 저장 경로

이미지는 `assets/images/` 폴더에 저장하는 것이 관례입니다.

```
assets/
└── images/
    ├── bio-photo.jpg        # 프로필 사진
    ├── logo.png             # 사이트 로고
    └── posts/               # 포스트별 이미지 (선택)
        ├── 2024-01-01/
        │   ├── thumbnail.jpg
        │   └── figure1.png
        └── my-post-image.jpg
```

### 이미지 참조 방법

#### (A) Markdown에서 직접 삽입

```markdown
![이미지 설명](/assets/images/posts/my-image.jpg)
```

#### (B) 이미지에 링크 걸기

```markdown
[![이미지 설명](/assets/images/thumbnail.jpg)](https://example.com)
```

#### (C) 크기 조절 (HTML 사용)

```html
<img src="/assets/images/posts/my-image.jpg" alt="설명" width="400">
```

#### (D) Minimal Mistakes 전용 — figure 캡션 추가

```markdown
{% include figure image_path="/assets/images/posts/my-image.jpg"
   alt="이미지 설명" caption="이미지 하단에 표시될 캡션" %}
```

#### (E) Front Matter에서 헤더 이미지 / 썸네일 설정

```yaml
---
header:
  image: /assets/images/posts/my-post-header.jpg     # 포스트 상단 넓은 헤더 이미지
  teaser: /assets/images/posts/my-post-thumbnail.jpg # 목록에서 보이는 썸네일
---
```

#### (F) _config.yml에서 기본 프로필 사진 설정

```yaml
author:
  avatar: /assets/images/bio-photo.jpg
```

---

## 3. 카테고리 만들기

Minimal Mistakes에서 카테고리는 **포스트 Front Matter**에 선언하고,  
**카테고리 아카이브 페이지**를 별도로 만들어야 목록이 보입니다.

### Step 1 — 포스트에 카테고리 지정

`_posts/2024-01-15-my-post.md`

```yaml
---
title: "나의 첫 번째 포스트"
categories:
  - 개발
  - Python
---
```

### Step 2 — 카테고리 아카이브 페이지 생성

`_pages/category-archive.md`

```yaml
---
title: "카테고리별 글"
layout: categories
permalink: /categories/
author_profile: true
---
```

→ `/categories/` URL로 접근하면 전체 카테고리 목록이 자동으로 생성됩니다.

### Step 3 — 특정 카테고리 전용 페이지 만들기 (선택)

`_pages/category-python.md`

```yaml
---
title: "Python 관련 글"
layout: category
permalink: /categories/python/
author_profile: true
taxonomy: Python
---
```

> **참고:** `taxonomy` 값은 포스트 Front Matter의 `categories` 값과 **정확히** 일치해야 합니다. (대소문자 구분)

### 결과 URL 구조

| 경로 | 설명 |
|------|------|
| `/categories/` | 모든 카테고리 목록 |
| `/categories/python/` | Python 카테고리 글 목록 |

---

## 4. 태그 만들기

카테고리와 동일한 방식으로 동작합니다.

### 포스트에 태그 지정

```yaml
---
title: "나의 포스트"
tags:
  - jekyll
  - github-pages
  - 블로그
---
```

### 태그 아카이브 페이지 생성

`_pages/tag-archive.md`

```yaml
---
title: "태그별 글"
layout: tags
permalink: /tags/
author_profile: true
---
```

### 특정 태그 전용 페이지 (선택)

`_pages/tag-jekyll.md`

```yaml
---
title: "Jekyll 관련 글"
layout: tag
permalink: /tags/jekyll/
taxonomy: jekyll
---
```

---

## 5. 포스트 작성하기

### 파일명 규칙

```
_posts/YYYY-MM-DD-포스트-제목.md
```

예시: `_posts/2024-01-15-jekyll-시작하기.md`

> **주의:** 파일명에 한글을 써도 되지만, 영문+하이픈 조합이 URL 안전합니다.

### 포스트 예시 (전체)

`_posts/2024-01-15-python-basics.md`

```markdown
---
title: "Python 기초 정리"
date: 2024-01-15
last_modified_at: 2024-01-20
categories:
  - 개발
  - Python
tags:
  - python
  - 기초
  - 입문
excerpt: "Python 기초 문법을 정리한 포스트입니다."  # 목록 미리보기 텍스트
header:
  teaser: /assets/images/posts/python-thumbnail.jpg
  image: /assets/images/posts/python-header.jpg
toc: true           # 오른쪽에 목차 자동 생성
toc_sticky: true    # 스크롤해도 목차 고정
author_profile: true
---

## 1. 변수와 자료형

Python에서 변수는 별도 선언 없이 바로 사용합니다.

```python
name = "홍길동"
age = 25
pi = 3.14
```

## 2. 조건문

```python
if age >= 18:
    print("성인입니다")
else:
    print("미성년자입니다")
```

{% include figure image_path="/assets/images/posts/python-flow.png"
   alt="Python 흐름도" caption="Python 기본 제어 흐름" %}
```

---

## 6. 페이지 작성하기

포스트와 달리, 날짜가 없는 **고정 페이지**는 `_pages/` 폴더에 만듭니다.

### About 페이지 예시

`_pages/about.md`

```markdown
---
title: "소개"
permalink: /about/
layout: single
author_profile: true
---

안녕하세요! 이 블로그는 개발 공부 기록을 위한 공간입니다.

## 관심 분야

- Python / Django
- 데이터 분석
- GitHub Pages / Jekyll
```

---

## 7. _config.yml 핵심 설정

```yaml
# 사이트 기본 정보
title: "나의 기술 블로그"
description: "개발 공부 기록"
url: "https://username.github.io"  # GitHub Pages URL
baseurl: ""                        # 하위 경로가 없으면 빈 문자열

# 테마
remote_theme: mmistakes/minimal-mistakes  # GitHub Pages 권장 방식
minimal_mistakes_skin: "default"          # default, dark, air, aqua, contrast, dirt, mint, neon, plum, sunrise

# 작성자 정보
author:
  name: "홍길동"
  avatar: /assets/images/bio-photo.jpg
  bio: "Python과 Jekyll을 좋아하는 개발자"
  location: "Seoul, Korea"
  links:
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/username"
    - label: "Email"
      icon: "fas fa-fw fa-envelope"
      url: "mailto:your@email.com"

# 포스트 기본 설정 (개별 포스트에서 override 가능)
defaults:
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      toc: true
      toc_sticky: true
      show_date: true

  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      author_profile: true

# 카테고리/태그 아카이브 활성화
category_archive:
  type: liquid
  path: /categories/
tag_archive:
  type: liquid
  path: /tags/

# 플러그인
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-paginate
```

---

## 8. 내비게이션 메뉴 설정

`_data/navigation.yml`

```yaml
main:
  - title: "홈"
    url: /
  - title: "카테고리"
    url: /categories/
  - title: "태그"
    url: /tags/
  - title: "소개"
    url: /about/

# 사이드바 내비게이션 (특정 섹션 내 목차용)
docs:
  - title: "Python 시리즈"
    children:
      - title: "기초 문법"
        url: /python/basics/
      - title: "함수와 클래스"
        url: /python/functions/
```

---

## 9. 사이드바 설정

### 특정 포스트/페이지에 커스텀 사이드바 추가

```yaml
---
title: "Python 기초"
sidebar:
  nav: "docs"   # navigation.yml의 키 이름
---
```

### 사이드바에 소개글 추가 (직접)

```yaml
---
sidebar:
  - title: "Role"
    text: "Python Developer"
  - title: "관련 포스트"
    text: "[다음 글 보기](/python/advanced/)"
---
```

---

## 10. 자주 쓰는 Front Matter 옵션

| 옵션 | 설명 | 예시 |
|------|------|------|
| `title` | 포스트/페이지 제목 | `"나의 첫 글"` |
| `date` | 작성일 | `2024-01-15` |
| `last_modified_at` | 수정일 | `2024-01-20` |
| `categories` | 카테고리 (복수 가능) | `[개발, Python]` |
| `tags` | 태그 (복수 가능) | `[jekyll, blog]` |
| `excerpt` | 미리보기 텍스트 | `"짧은 요약"` |
| `toc` | 목차 활성화 | `true` |
| `toc_sticky` | 목차 고정 | `true` |
| `toc_label` | 목차 제목 | `"목차"` |
| `author_profile` | 사이드바 프로필 표시 | `true` |
| `read_time` | 읽기 시간 표시 | `true` |
| `comments` | 댓글 활성화 | `true` |
| `header.image` | 상단 헤더 이미지 | `/assets/images/header.jpg` |
| `header.teaser` | 목록 썸네일 | `/assets/images/thumb.jpg` |
| `header.og_image` | SNS 공유 이미지 | `/assets/images/og.jpg` |
| `published` | 발행 여부 | `false` (초안) |
| `permalink` | 고정 URL | `/my-custom-url/` |
| `layout` | 레이아웃 | `single`, `splash`, `home` |

---

## 11. Front Matter 옵션별 파일/경로 구조 예시

각 옵션이 실제 프로젝트에서 **어떤 파일에 위치하고, 어떤 경로를 참조하는지** 전체 구조로 보여줍니다.

### 전체 디렉토리 구조 (옵션 연동 기준)

```
my-github-pages/
│
├── _config.yml                        # author_profile, read_time, comments 전역 기본값 설정
│
├── _posts/
│   └── 2024-01-15-python-basics.md   # 아래 Front Matter 예시 파일
│
├── _pages/
│   ├── about.md                       # layout: single, permalink: /about/
│   ├── category-archive.md            # layout: categories, permalink: /categories/
│   └── tag-archive.md                 # layout: tags, permalink: /tags/
│
├── _drafts/                           # published: false 대신 초안 관리용 (선택)
│   └── 2024-02-01-wip-post.md
│
└── assets/
    └── images/
        ├── bio-photo.jpg              # author.avatar → _config.yml에서 참조
        ├── site-logo.png              # title 옆 로고 → _config.yml에서 참조
        └── posts/
            ├── python-header.jpg      # header.image 참조 경로
            ├── python-teaser.jpg      # header.teaser 참조 경로
            └── python-og.jpg          # header.og_image 참조 경로
```

---

### 포스트 Front Matter 전체 예시

`_posts/2024-01-15-python-basics.md`

```yaml
---
# ── 기본 정보 ──────────────────────────────
title: "Python 기초 완벽 정리"           # 브라우저 탭 + 포스트 상단 제목
date: 2024-01-15                          # 파일명의 날짜와 일치시킬 것
last_modified_at: 2024-01-20             # 수정일 (선택)

# ── 분류 ───────────────────────────────────
categories:                              # _pages/category-archive.md layout: categories 와 연동
  - 개발
  - Python
tags:                                    # _pages/tag-archive.md layout: tags 와 연동
  - python
  - 기초
  - 입문

# ── 미리보기 ────────────────────────────────
excerpt: "변수, 조건문, 반복문 등 Python 핵심 문법을 예제와 함께 정리합니다."
                                         # 카테고리/태그 목록에서 제목 아래 표시되는 요약

# ── 이미지 ─────────────────────────────────
header:
  image: /assets/images/posts/python-header.jpg
                                         # 포스트 최상단 와이드 배너 이미지
  teaser: /assets/images/posts/python-teaser.jpg
                                         # 카테고리/태그/홈 목록에서 보이는 썸네일 (300×200 권장)
  og_image: /assets/images/posts/python-og.jpg
                                         # 카카오톡·트위터 등 SNS 공유 시 표시되는 이미지

# ── 레이아웃 & UI ────────────────────────────
layout: single                           # 대부분의 포스트는 single 사용
author_profile: true                     # 왼쪽 사이드바에 작성자 프로필 표시
read_time: true                          # 예상 읽기 시간 표시 ("3 min read")
comments: true                           # 댓글창 활성화 (_config.yml에 댓글 서비스 설정 필요)

# ── 목차 ───────────────────────────────────
toc: true                                # 오른쪽에 목차(Table of Contents) 자동 생성
toc_sticky: true                         # 스크롤 시 목차가 화면에 고정
toc_label: "목차"                         # 목차 상단 제목 (기본값: "On This Page")

# ── 발행 & URL ──────────────────────────────
published: true                          # false 로 바꾸면 빌드에서 제외 (비공개 초안)
permalink: /python/basics/               # 지정 시 이 URL로 고정 (미지정 시 날짜+파일명 자동 생성)
---

본문 내용 작성 시작...
```

---

### permalink 미지정 vs 지정 비교

| 상황 | 파일명 | 실제 접근 URL |
|------|--------|--------------|
| permalink **미지정** | `_posts/2024-01-15-python-basics.md` | `/개발/python/2024/01/15/python-basics/` |
| permalink **지정** | 동일 파일, `permalink: /python/basics/` | `/python/basics/` |

> `_config.yml`에서 `permalink: /:categories/:year/:month/:day/:title/` 형식을 바꾸면 전체 기본 URL 패턴을 변경할 수 있습니다.

---

### published: false — 초안 관리 두 가지 방법

```
# 방법 A: Front Matter로 숨기기 (파일은 _posts/ 에 그대로)
_posts/2024-02-01-draft-post.md   ← published: false 설정

# 방법 B: _drafts/ 폴더 사용 (날짜 없이 파일명 작성)
_drafts/draft-post.md             ← 빌드 시 자동으로 제외
                                     로컬 확인: jekyll serve --drafts
```

---

### header 이미지 3종 역할 비교

```
my-github-pages/
└── assets/images/posts/
    ├── python-header.jpg    # header.image  → 포스트 열었을 때 상단 꽉 채우는 배너
    │                                           권장 크기: 1280×400 이상, 가로 긴 이미지
    ├── python-teaser.jpg    # header.teaser → 목록(홈/카테고리/태그)에서 보이는 카드 썸네일
    │                                           권장 크기: 500×300
    └── python-og.jpg        # header.og_image → SNS 공유 미리보기 이미지
                                                  권장 크기: 1200×630 (Open Graph 표준)
```

이미지를 하나만 준비한다면 **teaser** 우선 — 목록 페이지 시각적 효과가 가장 큼.

---

## 빠른 시작 체크리스트

```
□ 1. remote_theme 또는 gem "minimal-mistakes-jekyll" 설정
□ 2. _config.yml — url, author 정보 입력
□ 3. assets/images/ 폴더 생성 후 프로필 사진 업로드
□ 4. _pages/about.md 작성
□ 5. _pages/category-archive.md 작성
□ 6. _pages/tag-archive.md 작성
□ 7. _data/navigation.yml 메뉴 설정
□ 8. _posts/ 에 첫 포스트 작성 (YYYY-MM-DD-title.md)
□ 9. GitHub에 push → Actions 탭에서 배포 확인
```
