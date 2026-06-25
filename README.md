# 나의 AI 블로그 템플릿

n8n + Upstage Solar AI로 블로그 글을 자동 생성·발행하는 Jekyll 블로그 템플릿입니다.

**포함된 것:** Beautiful Jekyll 테마 + narapost 디자인 시스템 + 포춘쿠키 홈 화면 + n8n 연동 구조

---

## 빠른 시작 (3단계)

### 1단계. 템플릿으로 내 레포 만들기

오른쪽 위 **Use this template → Create a new repository** 클릭

| 항목 | 값 |
|---|---|
| Repository name | `n8n-blog` |
| Visibility | **Public** (GitHub Pages 무료 조건) |

### 2단계. `_config.yml` 수정 ← 반드시 해야 함

레포 안의 `_config.yml` 파일을 열고 `your-github-id` 를 **본인 GitHub 아이디**로 교체합니다.

```yaml
github-username: your-github-id   # ← 본인 아이디로 변경

navbar-links:
  홈: ""
  GitHub: "https://github.com/your-github-id/n8n-blog"  # ← 변경
```

> 색상·폰트·디자인은 이미 설정되어 있어 수정 불필요

### 3단계. GitHub Pages 활성화

내 레포 → **Settings → Pages**

- Source: `Deploy from a branch`
- Branch: `main` / `(root)` → **Save**

1~2분 후 `https://[내 GitHub 아이디].github.io/n8n-blog/` 접속 확인

---

## 폴더 구조

```
n8n-blog/
├── _config.yml          # ★ 유일하게 수정 필요한 파일
├── index.md             # 홈 화면 (포춘쿠키 + 포스트 목록)
├── Gemfile              # Jekyll 의존성 (수정 불필요)
├── .gitignore
│
├── _posts/              # n8n이 자동으로 글을 저장하는 폴더
│   └── YYYY-MM-DD-slug.md
│
├── _data/
│   └── posts.yml        # n8n이 자동 관리 (직접 수정 X)
│
└── assets/
    └── css/
        └── style.scss   # 디자인 커스텀 (변경 시 색상·폰트 수정 가능)
```

---

## 디자인 시스템

이 템플릿에 포함된 디자인 요소입니다. 별도 설치 없이 자동 적용됩니다.

| 요소 | 내용 |
|---|---|
| 테마 | Beautiful Jekyll (`daattali/beautiful-jekyll`) |
| Primary 색상 | `#5B67E8` 인디고 |
| 배경 | `#F4F4FA` 연회색 |
| 네비게이션 | 다크 네이비 `#1A1C2E` |
| 폰트 | Noto Sans KR |
| 홈 아이콘 | 🥠 포춘쿠키 float 애니메이션 |
| 포스트 카드 | 라운드 카드 + 호버 효과 |
| 코드 블록 | 다크 배경 + 밝은 텍스트 |

---

## n8n 워크플로우 연결

강사가 제공한 워크플로우 JSON을 n8n에 임포트한 후 아래 4곳을 수정합니다.

| 노드 | 수정 항목 | 변경값 |
|---|---|---|
| GitHub 파일 커밋 | `owner` | 본인 GitHub 아이디 |
| GitHub posts.yml 읽기 | `owner` | 본인 GitHub 아이디 |
| GitHub posts.yml 업데이트 | `owner` | 본인 GitHub 아이디 |
| 모든 GitHub 노드 | Credential | 본인 PAT로 교체 |

> **PAT 발급:** GitHub → Settings → Developer settings → Personal access tokens → `repo` 권한 체크

---

## 포스트 형식

n8n이 자동으로 `_posts/` 에 아래 형식으로 저장합니다.

```markdown
---
layout: post
title: "글 제목"
date: 2026-06-25
categories: [AI]
---

본문 내용 (마크다운)
```

---

## n8n 7노드 흐름

```
[1] Form Trigger          ← 블로그 주제 입력
[2] Solar API             ← AI 글 생성
[3] Code (파싱)            ← 제목·본문·카테고리 추출
[4] GitHub 파일 커밋       ← _posts/ 에 .md 저장
[5] GitHub posts.yml 읽기 ← SHA 값 포함 읽기
[6] Code (yml 업데이트)    ← 새 항목 추가
[7] GitHub posts.yml 업데이트 ← SHA 포함 저장
        ↓
GitHub Pages 자동 빌드 → 블로그 발행 완료
```
