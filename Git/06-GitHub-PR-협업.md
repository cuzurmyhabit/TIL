# GitHub PR 협업

프론트엔드 팀에서 Git의 최종 목적지는 보통 **Pull Request(PR)** 이다.  
코드 리뷰, CI 검사, merge가 한 흐름으로 이어진다.

---

## Pull Request란?

**"내 브랜치 변경을 main에 합쳐달라"** 는 요청.

```text
feature/login-ui  ──PR──→  main
```

PR에는 다음이 포함된다.

- 변경된 파일 diff
- 커밋 목록
- 리뷰어 코멘트
- CI 체크 결과 (lint, test, build)

---

## PR 생성 흐름

```bash
# 1. 브랜치에서 작업 완료
git push -u origin feature/login-ui

# 2. GitHub에서 "Compare & pull request" 클릭
# 3. 제목·본문 작성
# 4. 리뷰어 지정
# 5. Create pull request
```

### gh CLI로 PR 생성

```bash
gh pr create --title "feat: 로그인 UI 구현" --body "$(cat <<'EOF'
## Summary
- 로그인 폼 UI 추가
- 이메일/비밀번호 유효성 검사

## Test plan
- [ ] 로그인 성공 케이스
- [ ] 잘못된 이메일 에러 표시
- [ ] 모바일 반응형 확인
EOF
)"
```

---

## ⭐ 좋은 PR 작성법

### 제목

```text
feat: 로그인 페이지 UI 및 폼 유효성 검사 추가
fix: Safari에서 모달 스크롤 막히는 문제
```

커밋 메시지와 같은 Conventional Commits 스타일을 권장.

### 본문 템플릿

```markdown
## Summary
- 무엇을, 왜 변경했는지 2~3줄

## Changes
- LoginForm 컴포넌트 추가
- useLogin 훅으로 API 연동

## Screenshots (UI 변경 시 필수!)
| Before | After |
|--------|-------|
| ![before](url) | ![after](url) |

## Test plan
- [ ] 로그인 성공
- [ ] 에러 메시지 표시
- [ ] 모바일 375px 확인
```

프론트엔드 PR에는 **스크린샷·영상**이 있으면 리뷰 속도가 크게 빨라진다.

---

## 코드 리뷰

### 리뷰어가 보는 것

- UI/UX 의도가 맞는지
- 컴포넌트 분리·재사용성
- 접근성 (a11y)
- 불필요한 리렌더·성능
- 타입 안전성 (TypeScript)
- 테스트 유무

### 리뷰 코멘트 유형 (GitHub)

| 유형 | 의미 |
| --- | --- |
| **Comment** | 의견, 반드시 수정 X |
| **Approve** | 승인, merge 가능 |
| **Request changes** | 수정 요청 |

### 리뷰 반영 후

```bash
# 추가 커밋
git add .
git commit -m "refactor: 리뷰 반영 - 에러 핸들링 분리"
git push
# PR에 자동 반영됨
```

---

## Draft PR

작업 중이지만 **미리 피드백**을 받고 싶을 때:

```bash
gh pr create --draft --title "WIP: 결제 페이지"
```

- CI는 돌아갈 수 있음
- merge 버튼 비활성화
- "Ready for review"로 전환하면 정식 리뷰 시작

---

## CI/CD와 PR

프론트엔드 프로젝트에서 PR마다 흔히 돌아가는 체크:

```text
✅ ESLint / Prettier
✅ TypeScript (tsc --noEmit)
✅ Unit test (Vitest / Jest)
✅ Build (vite build / next build)
✅ E2E (선택, Playwright / Cypress)
```

CI가 실패하면 merge가 막힌다. 로컬에서 먼저 확인:

```bash
npm run lint
npm run test
npm run build
```

---

## PR merge 후 정리

```bash
# main 최신화
git checkout main
git pull

# 로컬 feature 브랜치 삭제
git branch -d feature/login-ui

# 원격 브랜치는 GitHub "Delete branch" 버튼 또는
git push origin --delete feature/login-ui
```

GitHub 설정에서 **merge 후 자동 삭제**를 켜 두면 편하다.

---

## 이슈와 PR 연결

```text
Closes #42
Fixes PROJ-123
```

PR 본문에 이슈 번호를 넣으면 merge 시 이슈가 자동으로 닫힌다.

---

## Fork → 기여 (오픈소스)

```bash
# 1. GitHub에서 Fork
# 2. 내 fork clone
git clone git@github.com:myname/react-lib.git

# 3. upstream 등록
git remote add upstream git@github.com:original/react-lib.git

# 4. 작업 후 내 fork에 push → PR to upstream
```

---

## 정리

| 단계 | 내용 |
| --- | --- |
| push | feature 브랜치를 원격에 올림 |
| PR 생성 | 제목·본문·스크린샷·테스트 플랜 |
| 리뷰 | Approve 후 merge |
| 정리 | main pull + 브랜치 삭제 |

---

## 다음 단계

→ [07-되돌리기와-응급-처치.md](./07-되돌리기와-응급-처치.md)에서 실수했을 때 되돌리는 방법을 다룬다.
