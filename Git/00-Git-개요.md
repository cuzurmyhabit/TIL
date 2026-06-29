# Git 개요

## Git이란?

Git은 **분산 버전 관리 시스템(DVCS)** 이다.  
코드의 변경 이력을 기록하고, 여러 사람이 동시에 작업해도 충돌을 관리할 수 있게 해준다.

프론트엔드 개발자에게 Git은 선택이 아니라 **필수 도구**다.

- React, Vue 등 프론트 프로젝트는 거의 Git으로 관리
- GitHub / GitLab / Bitbucket에서 코드 리뷰·CI/CD와 연결
- 배포 파이프라인도 보통 특정 브랜치 push를 트리거로 동작

---

## 핵심 개념

### 저장소 (Repository)

프로젝트와 그 **전체 히스토리**가 담긴 폴더.

```text
my-app/          ← 작업 디렉터리 (Working Directory)
└── .git/        ← Git이 이력을 저장하는 숨김 폴더
```

### 커밋 (Commit)

특정 시점의 코드 스냅샷 + **메시지**.

```text
A --- B --- C --- D  (main 브랜치)
      ↑
   "버튼 컴포넌트 추가"
```

### 브랜치 (Branch)

독립적인 작업선. 기능 개발·버그 수정을 main과 분리한다.

```text
main:     A --- B --- C --- F
               \
feature:        D --- E
```

### 원격 저장소 (Remote)

GitHub 등 서버에 있는 저장소. `origin`이 기본 이름.

```text
로컬 PC  ←→  origin (GitHub)
```

---

## Git의 3가지 영역

```text
Working Directory  →  Staging Area  →  Repository
   (작업 폴더)          (스테이징)         (커밋 히스토리)

   파일 수정           git add            git commit
```

| 영역 | 설명 |
| --- | --- |
| **Working Directory** | 지금 편집 중인 파일 |
| **Staging Area** | 다음 커밋에 포함할 변경 목록 |
| **Repository** | 커밋으로 확정된 이력 |

---

## 자주 쓰는 도구

| 도구 | 용도 |
| --- | --- |
| **터미널 (CLI)** | 가장 빠르고 정확. 실무에서 기본 |
| **VS Code Source Control** | GUI로 add/commit/diff |
| **GitHub Desktop** | 초보자 친화적 GUI |
| **gh CLI** | GitHub PR·이슈 관리 |

CLI를 익혀 두면 GUI에서 막히는 상황도 해결할 수 있다.

---

## 프론트엔드 개발 흐름에서 Git의 위치

```text
이슈 확인 (Jira, Linear 등)
    ↓
브랜치 생성 (feature/login-ui)
    ↓
코드 작성 + 커밋
    ↓
push → Pull Request
    ↓
코드 리뷰 + CI (lint, test, build)
    ↓
merge → main 배포
```

---

## 정리

| 개념 | 한 줄 요약 |
| --- | --- |
| Git | 코드 변경 이력 관리 |
| 커밋 | 스냅샷 + 메시지 |
| 브랜치 | 독립 작업선 |
| 원격 | GitHub 등 공유 저장소 |
| 3영역 | 작업 → 스테이징 → 커밋 |

---

## 다음 단계

→ [01-기본-설정과-저장소.md](./01-기본-설정과-저장소.md)에서 Git 설치·설정과 저장소 만드는 방법을 다룬다.
