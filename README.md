# 나의 AI 블로그

n8n + Upstage Solar AI로 블로그 글을 자동 생성·발행하는 Jekyll 블로그입니다.  
테마: [Beautiful Jekyll](https://github.com/daattali/beautiful-jekyll)

**블로그 주소:** `https://[GitHub 아이디].github.io/n8n-blog/`

---

## 폴더 구조

```
n8n-blog/
├── _config.yml          # 블로그 설정 (테마·색상·네비게이션)
├── index.md             # 홈 화면 (포춘쿠키 + 포스트 목록 자동 출력)
├── Gemfile              # Jekyll 플러그인 의존성
├── .gitignore
│
├── _posts/              # n8n이 자동으로 글을 여기 저장
│   └── YYYY-MM-DD-slug.md
│
├── _data/
│   └── posts.yml        # n8n이 자동으로 관리 (직접 수정 X)
│
└── assets/
    └── css/
        └── style.scss   # 커스텀 디자인 (narapost 스타일)
```

---

## 디자인 시스템

| 항목 | 값 |
|---|---|
| 테마 | Beautiful Jekyll (`daattali/beautiful-jekyll`) |
| Primary | `#5B67E8` 인디고 |
| Dark Navy | `#1A1C2E` |
| Background | `#F4F4FA` |
| 폰트 | Noto Sans KR |
| 네비게이션 | 다크 네이비 바 · 홈 + GitHub 링크 |
| 홈 아이콘 | 🥠 포춘쿠키 float 애니메이션 |

---

## 시작하기

### 1. 이 레포를 내 계정으로 Fork 또는 템플릿 복사

**Use this template → Create a new repository**

- Repository name: `n8n-blog`
- Visibility: **Public** (GitHub Pages 무료 사용 조건)

### 2. GitHub Pages 활성화

내 레포 → **Settings → Pages**

- Source: `Deploy from a branch`
- Branch: `main` / `(root)`
- **Save** 클릭

1~2분 후 `https://[내 GitHub 아이디].github.io/n8n-blog/` 접속 확인

### 3. `_config.yml` 수정

```yaml
title: "나의 AI 블로그"          # 블로그 이름
description: "블로그 설명"
github-username: [내 GitHub 아이디]

navbar-links:
  홈: ""
  GitHub: "https://github.com/[내 GitHub 아이디]/n8n-blog"
```

### 4. n8n 워크플로우 연결

강사가 제공한 워크플로우 JSON을 n8n에 임포트한 후:

1. **GitHub Credential** → `sinnara2021` 대신 본인 계정의 PAT로 교체
2. **GitHub 파일 커밋 노드** → `owner` 값을 본인 GitHub 아이디로 변경
3. **GitHub posts.yml 읽기 노드** → `asBinaryProperty: false` 옵션 확인
4. **Jekyll 파일 생성 노드** → `runOnceForEachItem` 모드 확인

---

## 포스트 형식

n8n이 자동으로 아래 형식으로 글을 만들어 `_posts/`에 저장합니다.

```markdown
---
layout: post
title: "글 제목"
date: 2026-06-25
categories: [AI]
---

본문 내용 (마크다운)
```

파일명 규칙: `YYYY-MM-DD-slug.md`  
예: `2026-06-25-미래-식량-지속-가능한-식탁을-위한-혁신-기술.md`

---

## n8n 워크플로우 구조 (7노드)

```
[1] Form Trigger        ← 블로그 주제 입력
      ↓
[2] Solar API           ← POST api.upstage.ai · model: solar-pro
      ↓
[3] Code (파싱)          ← ##TAG## 파싱 → title / jekyllContent / indexEntry
      ↓
[4] GitHub 파일 커밋     ← _posts/에 .md 파일 생성
      ↓
[5] GitHub posts.yml 읽기 ← content(base64) + sha 수신
      ↓
[6] Code (yml 업데이트)  ← base64 디코딩 → 기존 내용 + 새 항목 합치기
      ↓
[7] GitHub posts.yml 업데이트 ← sha 포함하여 Edit
      ↓
GitHub Pages 자동 빌드 → 블로그 발행 완료
```
