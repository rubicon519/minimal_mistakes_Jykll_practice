# Minimal Mistakes Jekyll 테마 — GitHub Pages 제작 가이드

> [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/) 테마를 기준으로 작성된 실전 가이드입니다.

---

## 목차

1. [기본 디렉토리 구조](#1-기본-디렉토리-구조)
2. [이미지 업로드 및 참조](#2-이미지-업로드-및-참조)
3. [포스트 작성하기](#3-포스트-작성하기)
4. [카테고리 & 태그](#4-카테고리--태그)
5. [_pages 파일 완전 정리](#5-_pages-파일-완전-정리)
6. [_config.yml 핵심 설정](#6-_configyml-핵심-설정)
7. [내비게이션 & 사이드바](#7-내비게이션--사이드바)
8. [Front Matter 옵션 전체 레퍼런스](#8-front-matter-옵션-전체-레퍼런스)
9. [빠른 시작 체크리스트](#9-빠른-시작-체크리스트)

---

## 1. 기본 디렉토리 구조

```
my-github-pages/
│
├── _config.yml                   # 사이트 전역 설정
│
├── _data/
│   └── navigation.yml            # 상단 메뉴 & 사이드바 내비게이션
│
├── _pages/                       # 날짜 없는 고정 페이지
│   ├── about.md                  # 소개 페이지
│   ├── category-archive.md       # 전체 카테고리 목록
│   ├── tag-archive.md            # 전체 태그 목록
│   └── year-archive.md           # 연도별 포스트 목록
│
├── _posts/                       # 블로그 포스트 (날짜 필수)
│   └── 2024-01-15-my-post.md
│
├── _drafts/                      # 초안 (빌드 제외, 선택)
│   └── my-draft-post.md
│
└── assets/
    └── images/                   # 모든 이미지
        ├── bio-photo.jpg         # 프로필 사진
        └── posts/                # 포스트용 이미지
            ├── my-header.jpg
            ├── my-teaser.jpg
            └── my-og.jpg
```

---

## 2. 이미지 업로드 및 참조

### 저장 위치

```
assets/images/
├── bio-photo.jpg          # 프로필 사진 → _config.yml author.avatar
├── site-logo.png          # 사이트 로고 → _config.yml logo
└── posts/                 # 포스트 이미지 (날짜 또는 포스트명으로 하위 폴더 구성 권장)
    ├── python-header.jpg  # header.image  — 포스트 상단 배너  (권장 크기: 1280×400↑)
    ├── python-teaser.jpg  # header.teaser — 목록 썸네일      (권장 크기: 500×300)
    └── python-og.jpg      # header.og_image — SNS 공유 이미지 (권장 크기: 1200×630)
```

> 이미지 하나만 준비한다면 **teaser** 우선 — 홈/카테고리/태그 목록에서 보이는 카드 이미지.

### 참조 방법

#### (A) Markdown 본문 삽입

```markdown
![이미지 설명](/assets/images/posts/my-image.jpg)
```

#### (B) 이미지에 링크 걸기

```markdown
[![이미지 설명](/assets/images/posts/my-image.jpg)](https://example.com)
```

#### (C) HTML로 크기 조절

```html
<img src="/assets/images/posts/my-image.jpg" alt="설명" width="400">
```

#### (D) Minimal Mistakes figure helper — 캡션 포함

```liquid
{% include figure image_path="/assets/images/posts/my-image.jpg"
   alt="이미지 설명" caption="출처: [Unsplash](https://unsplash.com)" %}
```

#### (E) Front Matter — 헤더 이미지 & 썸네일

```yaml
---
header:
  image: /assets/images/posts/python-header.jpg    # 포스트 상단 배너
  teaser: /assets/images/posts/python-teaser.jpg   # 목록 썸네일
  og_image: /assets/images/posts/python-og.jpg     # SNS 공유 이미지
---
```

#### (F) 헤더 오버레이 (텍스트 + 버튼 위에 이미지 깔기)

```yaml
---
header:
  overlay_image: /assets/images/posts/python-header.jpg
  overlay_filter: 0.4          # 0~1 사이 값, 어두울수록 텍스트 가독성 ↑
  caption: "Photo: Unsplash"
  actions:
    - label: "GitHub 보기"
      url: "https://github.com/username"
---
```

---

## 3. 포스트 작성하기

### 파일명 규칙

```
_posts/YYYY-MM-DD-영문-제목.md
```

- 날짜는 **파일명과 Front Matter `date`를 일치**시킬 것
- 파일명 한글도 가능하지만, 영문+하이픈이 URL 안전

### 포스트 Front Matter 전체 예시

`_posts/2024-01-15-python-basics.md`

```yaml
---
# ── 기본 정보 ─────────────────────────────────────────
title: "Python 기초 완벽 정리"
date: 2024-01-15
last_modified_at: 2024-01-20        # 수정일 (선택)

# ── 분류 ─────────────────────────────────────────────
# 포스트 내부에만 선언하면 category-archive, tag-archive에 자동 반영됨
categories:
  - 개발
  - Python
tags:
  - python
  - 기초
  - 입문

# ── 미리보기 ──────────────────────────────────────────
excerpt: "변수, 조건문, 반복문 등 Python 핵심 문법을 예제와 함께 정리합니다."

# ── 이미지 ───────────────────────────────────────────
header:
  image: /assets/images/posts/python-header.jpg
  teaser: /assets/images/posts/python-teaser.jpg
  og_image: /assets/images/posts/python-og.jpg

# ── 레이아웃 & UI ─────────────────────────────────────
layout: single          # 포스트는 대부분 single
author_profile: true
read_time: true
comments: true          # _config.yml에 댓글 서비스 설정 필요

# ── 목차 ─────────────────────────────────────────────
toc: true
toc_sticky: true        # 스크롤해도 목차 고정
toc_label: "목차"

# ── 발행 & URL ────────────────────────────────────────
published: true         # false 로 바꾸면 빌드에서 제외
permalink: /python/basics/   # 생략 시 날짜+파일명으로 자동 생성
---

## 1. 변수와 자료형

본문 내용 작성...
```

### permalink 지정 vs 미지정

| 상황 | 파일명 | 실제 URL |
|------|--------|---------|
| 미지정 | `_posts/2024-01-15-python-basics.md` | `/개발/Python/2024/01/15/python-basics/` |
| 지정 | 동일, `permalink: /python/basics/` | `/python/basics/` |

> `_config.yml`의 `permalink: /:categories/:year/:month/:day/:title/` 값을 바꾸면 전체 기본 패턴 변경 가능.

### 초안 관리 2가지 방법

```
# 방법 A — published: false (파일은 _posts/ 에 유지)
_posts/2024-02-01-draft-post.md    ← Front Matter에 published: false

# 방법 B — _drafts/ 폴더 사용 (날짜 없이 파일명만)
_drafts/draft-post.md              ← 빌드 시 자동 제외
                                      로컬 미리보기: jekyll serve --drafts
```

---

## 4. 카테고리 & 태그

### 핵심 원칙

| 항목 | 선언 위치 | 아카이브 자동 반영 | 별도 파일 필요 |
|------|---------|---------------|------------|
| `categories` | 포스트 Front Matter | ✅ 자동 | `category-archive.md` 최초 1회 |
| `tags` | 포스트 Front Matter | ✅ 자동 | `tag-archive.md` 최초 1회 |
| 페이지(About 등) | 해당 없음 | ❌ | `_pages/` 에 직접 파일 생성 |

> **`page`는 카테고리/태그와 다른 개념입니다.**  
> 포스트 Front Matter에 `page`라는 키는 없습니다. About, 소개 등 고정 페이지는 `_pages/`에 별도 파일로 직접 만들어야 합니다.

### Step 1 — 포스트에 선언

```yaml
---
categories:
  - 개발
  - Python
tags:
  - python
  - 기초
---
```

### Step 2 — 아카이브 페이지 (최초 1회 생성)

`_pages/category-archive.md`

```yaml
---
title: "카테고리"
layout: categories      # 전체 카테고리 자동 그룹핑
permalink: /categories/
author_profile: true
---
```

`_pages/tag-archive.md`

```yaml
---
title: "태그"
layout: tags
permalink: /tags/
author_profile: true
---
```

### Step 3 — 특정 카테고리/태그 전용 페이지 (선택)

`_pages/category-python.md`

```yaml
---
title: "Python 관련 글"
layout: category        # 단수형! (categories 아님)
permalink: /categories/python/
taxonomy: Python        # 포스트의 categories 값과 대소문자까지 정확히 일치
---
```

`_pages/tag-jekyll.md`

```yaml
---
title: "Jekyll 관련 글"
layout: tag             # 단수형
permalink: /tags/jekyll/
taxonomy: jekyll
---
```

---

## 5. _pages 파일 완전 정리

`_pages/`는 **날짜 없는 고정 페이지**를 두는 폴더입니다.  
`.md`와 `.html` 모두 사용 가능하며, 기능 차이는 없고 **표현 자유도의 차이**만 있습니다.

- `.md` — Front Matter + 간단한 본문
- `.html` — Liquid 문법(루프, 조건문 등)으로 완전 커스텀 가능

---

### 📋 아카이브 계열 — 자동 목록 생성

포스트를 추가할 때마다 건드릴 필요 없이, 레이아웃이 자동으로 목록을 만들어줍니다.

#### `category-archive.md` — 전체 카테고리 목록

```yaml
---
title: "카테고리"
layout: categories
permalink: /categories/
author_profile: true
---
```

#### `tag-archive.md` — 전체 태그 목록

```yaml
---
title: "태그"
layout: tags
permalink: /tags/
author_profile: true
---
```

#### `year-archive.md` / `all-posts.md` — 연도별 & 전체 포스트 목록

```yaml
# year-archive.md
---
title: "연도별 글"
layout: posts           # 연도 헤더 아래 포스트 자동 정렬
permalink: /posts/
author_profile: true
---
```

```yaml
# all-posts.md — year-archive와 동일한 layout, permalink만 다르게
---
title: "전체 글"
layout: posts
permalink: /all-posts/
---
```

---

### 🗂️ 컬렉션 계열 — `_posts` 외 별도 컬렉션 목록

`_posts/`가 아닌 직접 만든 컬렉션(`_portfolio/`, `_recipes/` 등)을 목록으로 보여줄 때 사용합니다.

#### `portfolio-archive.md` — 단일 컬렉션을 grid 카드형으로

**사용 전 `_config.yml`에 컬렉션 등록 필수:**

```yaml
# _config.yml
collections:
  portfolio:
    output: true
    path: _portfolio    # 실제 파일이 들어갈 폴더
```

`_pages/portfolio-archive.md`

```yaml
---
title: "포트폴리오"
layout: collection      # 특정 컬렉션 하나를 보여줌
permalink: /portfolio/
collection: portfolio   # _config.yml의 collections 키와 일치해야 함
entries_layout: grid    # grid(카드형) 또는 list(기본값)
---
```

각 포트폴리오 파일(`_portfolio/my-project.md`)에 teaser 지정:

```yaml
---
title: "나의 프로젝트"
header:
  teaser: /assets/images/posts/project-teaser.jpg
---
```

#### `collection-archive.html` — 모든 컬렉션을 한 페이지에 통합

`_posts`를 제외한 **모든 컬렉션**을 한 페이지에 모아서 보여줍니다.  
(portfolio, recipes, pets 등 여러 컬렉션을 동시에 운영할 때 유용)

```html
---
layout: archive
title: "컬렉션 전체 보기"
permalink: /collection-archive/
author_profile: true
---

{% for collection in site.collections %}
  {% unless collection.output == false or collection.label == "posts" %}
    <h2>{{ collection.label }}</h2>
    {% for post in collection.docs %}
      {% include archive-single.html %}
    {% endfor %}
  {% endunless %}
{% endfor %}
```

| 파일 | 보여주는 것 | layout | 형태 |
|------|-----------|--------|------|
| `portfolio-archive.md` | `portfolio` 컬렉션 **하나만** | `collection` | grid 카드 |
| `collection-archive.html` | **모든 컬렉션** 통합 | `archive` + Liquid | 리스트 |

---

### 🏠 홈 페이지 계열

#### `home.md` — 최근 포스트 목록형 홈

```yaml
---
layout: home            # 최근 포스트 자동 페이지네이션
permalink: /
---
```

`_config.yml`에 페이지네이션 설정 추가:

```yaml
paginate: 5
paginate_path: /page:num/
```

#### `homepage.md` (splash) — 랜딩 페이지형 홈

```yaml
---
layout: splash
permalink: /
header:
  overlay_image: /assets/images/banner.jpg
  overlay_filter: 0.3
  actions:
    - label: "블로그 보기"
      url: /posts/
excerpt: "개발 공부 기록 블로그입니다."
---
```

| 파일 | layout | 특징 |
|------|--------|------|
| `home.md` | `home` | 최근 포스트 자동 목록 + 페이지네이션 |
| `homepage.md` | `splash` | 히어로 배너 + feature_row 커스텀 |

---

### 🎨 feature_row 계열 — 카드 배너

#### `post-archive-feature-rows.html` — 실제 성격: **데모/테스트 파일**

이 파일은 feature_row의 4가지 정렬 타입(`default`, `left`, `right`, `center`)을 한눈에 확인하는 **샘플 파일**입니다. 내 블로그에 그대로 추가할 파일이 아닙니다.

**실제 블로그에서 feature_row를 쓰는 올바른 방법 — splash 페이지 Front Matter에 직접 정의:**

`_pages/homepage.md` 또는 홈 `index.html`

```yaml
---
layout: splash
permalink: /
feature_row:
  - image_path: /assets/images/posts/python-teaser.jpg
    alt: "Python"
    title: "Python 기초"
    excerpt: "변수, 조건문, 반복문 핵심 정리"
    url: /python/basics/
    btn_label: "읽기"
    btn_class: "btn--primary"
  - image_path: /assets/images/posts/jekyll-teaser.jpg
    alt: "Jekyll"
    title: "Jekyll 시작하기"
    excerpt: "GitHub Pages 세팅 가이드"
    url: /jekyll/start/
    btn_label: "읽기"
    btn_class: "btn--primary"
  - image_path: /assets/images/posts/git-teaser.jpg
    alt: "Git"
    title: "Git 기초 명령어"
    excerpt: "자주 쓰는 Git 명령어 모음"
    url: /git/basics/
    btn_label: "읽기"
    btn_class: "btn--primary"
---

{% include feature_row %}                             <!-- 3열 기본 -->
{% include feature_row id="feature_row" type="left" %}  <!-- 왼쪽 정렬 1개짜리 -->
```

---

### 📋 기타 페이지

#### `archive-layout-with-content.md` — 실제 성격: **데모 파일**

`layout: archive`가 본문과 함께 쓰일 때 어떻게 보이는지 확인하는 **레이아웃 동작 확인용 샘플**입니다.  
일반 블로그에서 이 파일을 복사할 필요는 없습니다.

실제 쓸 때는 어느 `_pages/*.md` 파일에서나 아래처럼 본문과 아카이브를 함께 쓸 수 있습니다:

```yaml
---
title: "공지 & 포스트 목록"
layout: archive
permalink: /notice/
---

## 공지사항

아카이브 목록 위에 이렇게 직접 본문을 추가할 수 있습니다.
```

#### `about.md` — 소개 페이지

```yaml
---
title: "소개"
permalink: /about/
layout: single
author_profile: true
---

안녕하세요! 개발 공부 기록 블로그입니다.

## 관심 분야

- Python / Django
- GitHub Pages / Jekyll
```

---

### _pages 파일 한눈에 정리

| 파일 | layout | 실제 목적 | 내 블로그에 쓸 파일? |
|------|--------|---------|-----------------|
| `category-archive.md` | `categories` | 전체 카테고리 목록 | ✅ 필수 |
| `tag-archive.md` | `tags` | 전체 태그 목록 | ✅ 필수 |
| `year-archive.md` | `posts` | 연도별 포스트 목록 | ✅ 권장 |
| `all-posts.md` | `posts` | 전체 포스트 목록 | ✅ 선택 |
| `portfolio-archive.md` | `collection` | portfolio 컬렉션 grid | ✅ 포트폴리오 운영 시 |
| `collection-archive.html` | `archive` + Liquid | 모든 컬렉션 통합 목록 | ✅ 컬렉션 여러 개 운영 시 |
| `home.md` | `home` | 홈 (포스트 목록형) | ✅ 홈 구성 시 선택 |
| `homepage.md` | `splash` | 홈 (랜딩 페이지형) | ✅ 홈 구성 시 선택 |
| `about.md` | `single` | 소개 페이지 | ✅ 권장 |
| `archive-layout-with-content.md` | `archive` | 레이아웃 데모 | ❌ 참고용만 |
| `post-archive-feature-rows.html` | `archive` | feature_row 정렬 데모 | ❌ 참고용만 |

---

## 6. _config.yml 핵심 설정

```yaml
# ── 사이트 기본 정보 ────────────────────────────────────
title: "나의 기술 블로그"
description: "개발 공부 기록"
url: "https://username.github.io"
baseurl: ""                        # 서브 경로 없으면 빈 문자열

# ── 테마 ────────────────────────────────────────────────
remote_theme: mmistakes/minimal-mistakes   # GitHub Pages 권장
minimal_mistakes_skin: "default"
# 스킨 옵션: default, dark, air, aqua, contrast, dirt, mint, neon, plum, sunrise

# ── 작성자 정보 ──────────────────────────────────────────
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

# ── 포스트 기본값 (개별 포스트에서 override 가능) ──────────
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

# ── 카테고리/태그 아카이브 ───────────────────────────────
category_archive:
  type: liquid
  path: /categories/
tag_archive:
  type: liquid
  path: /tags/

# ── 플러그인 ─────────────────────────────────────────────
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-paginate
  - jekyll-include-cache   # Minimal Mistakes 필수

# ── 페이지네이션 ─────────────────────────────────────────
paginate: 5
paginate_path: /page:num/
```

---

## 7. 내비게이션 & 사이드바

### 상단 메뉴 설정

`_data/navigation.yml`

```yaml
main:
  - title: "홈"
    url: /
  - title: "카테고리"
    url: /categories/
  - title: "태그"
    url: /tags/
  - title: "포트폴리오"
    url: /portfolio/
  - title: "소개"
    url: /about/
```

### 사이드바 내비게이션 (섹션별 목차)

`_data/navigation.yml` 에 키 추가:

```yaml
python-series:
  - title: "Python 시리즈"
    children:
      - title: "기초 문법"
        url: /python/basics/
      - title: "함수와 클래스"
        url: /python/functions/
      - title: "파일 입출력"
        url: /python/file-io/
```

포스트 또는 페이지 Front Matter에서 사용:

```yaml
---
sidebar:
  nav: "python-series"   # navigation.yml의 키와 일치
---
```

### 사이드바에 텍스트 직접 추가

```yaml
---
sidebar:
  - title: "관련 링크"
    text: "[공식 문서](https://docs.python.org/ko/)"
  - title: "작성자"
    image: /assets/images/bio-photo.jpg
    text: "Python 3년차 개발자"
---
```

---

## 8. Front Matter 옵션 전체 레퍼런스

### 기본 정보

| 옵션 | 설명 | 예시 |
|------|------|------|
| `title` | 제목 | `"Python 기초 정리"` |
| `date` | 작성일 | `2024-01-15` |
| `last_modified_at` | 수정일 | `2024-01-20` |
| `excerpt` | 목록 미리보기 텍스트 | `"짧은 요약"` |
| `published` | 발행 여부 | `false` (초안 처리) |
| `permalink` | 고정 URL | `/python/basics/` |

### 분류

| 옵션 | 설명 | 예시 |
|------|------|------|
| `categories` | 카테고리 (복수 가능) | `[개발, Python]` |
| `tags` | 태그 (복수 가능) | `[jekyll, blog]` |

### 레이아웃

| 옵션 | 설명 | 예시 |
|------|------|------|
| `layout` | 레이아웃 종류 | `single`, `splash`, `home`, `archive` |
| `classes` | 추가 CSS 클래스 | `wide` (본문 전체 너비로 확장) |

### UI 요소

| 옵션 | 설명 | 예시 |
|------|------|------|
| `author_profile` | 사이드바 프로필 | `true` |
| `read_time` | 읽기 시간 표시 | `true` |
| `comments` | 댓글 활성화 | `true` |
| `share` | SNS 공유 버튼 | `true` |
| `toc` | 목차 자동 생성 | `true` |
| `toc_sticky` | 목차 스크롤 고정 | `true` |
| `toc_label` | 목차 상단 제목 | `"목차"` |
| `toc_icon` | 목차 아이콘 | `"cog"` (Font Awesome) |

### 이미지

| 옵션 | 설명 | 권장 크기 |
|------|------|---------|
| `header.image` | 포스트 상단 배너 | 1280×400px↑ |
| `header.teaser` | 목록 썸네일 | 500×300px |
| `header.og_image` | SNS 공유 이미지 | 1200×630px |
| `header.overlay_image` | 오버레이 배경 이미지 | 1280×400px↑ |
| `header.overlay_filter` | 오버레이 불투명도 | `0.4` 또는 `rgba(0,0,0,0.5)` |

---

## 9. 빠른 시작 체크리스트

```
□ 1. _config.yml — remote_theme, url, author 정보 입력
□ 2. assets/images/ 폴더 생성 → 프로필 사진(bio-photo.jpg) 업로드
□ 3. _pages/category-archive.md 생성 (layout: categories)
□ 4. _pages/tag-archive.md 생성 (layout: tags)
□ 5. _pages/year-archive.md 생성 (layout: posts)
□ 6. _pages/about.md 작성
□ 7. _data/navigation.yml — 상단 메뉴 설정
□ 8. _posts/YYYY-MM-DD-첫-포스트.md 작성
□ 9. GitHub에 push → Actions 탭에서 배포 확인
```

---

## 참고 링크

- [Minimal Mistakes 공식 문서](https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/)
- [Layouts 상세 설명](https://mmistakes.github.io/minimal-mistakes/docs/layouts/)
- [Helpers (figure, feature_row 등)](https://mmistakes.github.io/minimal-mistakes/docs/helpers/)
- [공식 GitHub 저장소](https://github.com/mmistakes/minimal-mistakes)
- [mm-github-pages-starter (빠른 시작 템플릿)](https://github.com/mmistakes/mm-github-pages-starter)
