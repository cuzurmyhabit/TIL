# React 개요

## React란?

React는 **사용자 인터페이스(UI)를 컴포넌트 단위로 만들기 위한 JavaScript 라이브러리**다.

- Facebook(Meta)에서 개발
- SPA(Single Page Application)에서 UI를 효율적으로 구성할 때 널리 사용
- 웹뿐 아니라 React Native로 모바일 앱도 만들 수 있음

---

## 왜 React를 쓰는가?

### 1. 컴포넌트 기반 UI

화면을 **재사용 가능한 조각(컴포넌트)** 으로 나눠서 조립한다.

```text
App
├── Header
├── Sidebar
└── Main
    ├── PostList
    │   └── PostItem (반복)
    └── Footer
```

버튼 하나, 카드 하나도 컴포넌트로 만들 수 있다.

### 2. 선언적 UI

**"어떻게(How)" DOM을 조작할지**가 아니라, **"무엇을(What)" 보여줄지**를 코드로 작성한다.

```javascript
// 명령형 (vanilla JS) — DOM을 직접 조작
const el = document.getElementById('count');
el.textContent = count;

// 선언형 (React) — 상태에 따라 UI를 기술
function Counter({ count }) {
  return <p>{count}</p>;
}
```

### 3. Virtual DOM과 효율적인 업데이트

상태가 바뀌면 React가 **Virtual DOM**을 비교(diff)해서 **실제로 변경된 부분만** DOM에 반영한다.

---

## React 앱의 기본 구조

```jsx
import { createRoot } from 'react-dom/client';

function App() {
  return <h1>Hello, React!</h1>;
}

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

| 요소 | 역할 |
| --- | --- |
| `App` | 최상위 컴포넌트 |
| `createRoot` | React 18+ 루트 생성 |
| `root.render()` | 컴포넌트를 DOM에 마운트 |

---

## React 생태계 한눈에 보기

| 도구 | 역할 |
| --- | --- |
| **Vite / CRA** | 프로젝트 생성·번들링 |
| **React Router** | 페이지(라우트) 관리 |
| **Zustand / Redux** | 전역 상태 관리 |
| **TanStack Query** | 서버 데이터 캐싱·동기화 |
| **Next.js** | SSR, 라우팅, 풀스택 프레임워크 |

React 자체는 **UI 라이브러리**에 가깝고, 라우팅·상태·데이터 페칭은 별도 라이브러리와 함께 쓰는 경우가 많다.

---

## 핵심 개념 미리보기

| 개념 | 한 줄 요약 |
| --- | --- |
| **컴포넌트** | UI를 나눈 재사용 단위 |
| **JSX** | JavaScript 안에서 UI를 작성하는 문법 |
| **Props** | 부모 → 자식으로 전달하는 읽기 전용 데이터 |
| **State** | 컴포넌트 내부에서 변하는 데이터 |
| **렌더링** | 상태·Props 변경 시 UI를 다시 계산하는 과정 |
| **Hooks** | 함수 컴포넌트에서 상태·부수 효과를 다루는 API |

---

## 다음 단계

→ [01-컴포넌트.md](./01-컴포넌트.md)에서 컴포넌트의 종류와 작성 방법을 살펴본다.
